# Lab 2: Provision POD2

### Description

In this lab, we will use Arista Validated Design (AVD) to generate `POD2` underlay and overlay configuration and CloudVision to deploy it.
This time, `eos_designs` variables need to be created.
At the end of this lab, `POD1` and `POD2` will be 2 separate EVPN domains: all leaf can only reach the `Loopback0` of other leaves of their respective pods.

<p align="center">
<img src="../images/Pod2.png"  width="512" height="354">
</p>

### Instructions

1. Go to the folder `/home/coder/project/persist/avd-toi`.
2. Open `inventory.yml` and **uncomment** all lines in the file.
3. Create `group_vars/POD2.yml` and fill it with required information.
    
    > Use `group_vars/POD1.yml` as example.

    > Use [this documentation](https://avd.sh/en/stable/roles/eos_designs/docs/input-variables.html#node-type-settings) to know what to modify.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/group_vars/POD2.yml">here</a>.
    </details>

4. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
5. Run the playbook `deploy.yml` with the command `ansible-playbook deploy.yml`.
6. Go to the CloudVision instance or your Arista Test Drive lab and open the `Provisioning` menu.
7. Go to the `Tasks` tab, select the tasks that have been created by the playbook and create a change control. At this point, you can select the Parallel arrangement to have the tasks executed in parallel within the change control.
8. You will be taken to the `Change Control` tab in the newly created change control. You can rename the `Change Control` to give it a meaningful name. Review, Approve and Execute the change control.
9. When the change control is completed, underlay and overlay of `POD2` will be configured. You can eventually access the devices via SSH and run the `show running-config` command to verify the configuration.
10. Connect to a spine and check the underlay and EVPN BGP sessions:
   ```cli
   s2-spine1#show ip bgp summary
   BGP summary information for VRF default
   Router identifier 172.16.2.1, local AS number 65200
   Neighbor Status Codes: m - Under maintenance
   Description              Neighbor  V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
   s2-leaf1_Ethernet2       10.0.0.1  4 65201             12        16    0    0 00:05:21 Estab   3      3
   s2-leaf2_Ethernet2       10.0.0.5  4 65201             15        15    0    0 00:05:21 Estab   3      3
   s2-leaf3_Ethernet2       10.0.0.9  4 65202             13        15    0    0 00:05:20 Estab   3      3
   s2-leaf4_Ethernet2       10.0.0.13 4 65202             12        16    0    0 00:05:20 Estab   3      3
   s2-brdr1_Ethernet2       10.0.0.17 4 65203             14        16    0    0 00:05:21 Estab   3      3
   s2-brdr2_Ethernet2       10.0.0.21 4 65203             14        16    0    0 00:05:20 Estab   3      3
   s2-spine1#show bgp evpn summary
   BGP summary information for VRF default
   Router identifier 172.16.2.1, local AS number 65200
   Neighbor Status Codes: m - Under maintenance
   Description              Neighbor   V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
   s2-leaf1                 172.16.0.1 4 65201             10        10    0    0 00:05:28 Estab   0      0
   s2-leaf2                 172.16.0.2 4 65201             10        10    0    0 00:05:27 Estab   0      0
   s2-leaf3                 172.16.0.3 4 65202             11        11    0    0 00:05:27 Estab   0      0
   s2-leaf4                 172.16.0.4 4 65202             11        11    0    0 00:05:27 Estab   0      0
   s2-brdr1                 172.16.0.5 4 65203             10        10    0    0 00:05:26 Estab   0      0
   s2-brdr2                 172.16.0.6 4 65203             10        10    0    0 00:05:27 Estab   0      0
   s2-spine1#
   ```