# Lab 4: Create EVPN services and configure leaf server ports

### Description

In this lab, we will configure L2 and L3 EVPN Services and the corresponding VLANs on the leaf server ports:
- `VLAN 134` is local to `POD1` and part of VRF `Blue`. The Anycast Gateway in this VLAN is `10.111.134.1/24`.
- `VLAN 234` is local to `POD2` and part of VRF `Blue`. The Anycast Gateway in this VLAN is `10.222.234.1/24`.
- `VLAN 112` is a L2VNI stretched across `POD1` and `POD2`.
- `s1-host1` and `s1-host2` have Layer 3 interfaces in `VLAN 134` and `VLAN 112`. A static route to reach `VLAN 234` needs to be created on these hosts.
- `s2-host1` and `s2-host2` have Layer 3 interfaces in `VLAN 234` and `VLAN 112`. A static route to reach `VLAN 134` needs to be created on these hosts.

At the end if this lab, hosts will be able to ping each other using `VLAN 112` (VXLAN bridged traffic) but also through `VLAN 134` and `VLAN 234` (VXLAN routed traffic).

### Instructions

1. Go to the folder `/home/coder/project/persist/avd-toi`.
2. Create `group_vars/CONNECTED_ENDPOINTS.yml`. Use the [Connected Endpoints](https://avd.sh/en/stable/roles/eos_designs/docs/input-variables.html#connected-endpoints-settings) model to create the configuration to attach `s1-host1`, `s1-host2` , `s2-host1` and `s2-host2` to the fabric.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/group_vars/CONNECTED_ENDPOINTS.yml">here</a>.
    </details>

3. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
4. Create `group_vars/NETWORK_SERVICES.yml`. Use the [Network Services](https://avd.sh/en/stable/roles/eos_designs/docs/input-variables.html#network-services) model to create the `L2 VLAN 112` across `POD1` and `POD2`. Create a VRF called `Blue` that will have 2 SVIs in `VLAN 134` on `POD1` only and in `VLAN 234` on `POD2` only. You can make use of tags to filter on which leaf the configuration is deployed.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/group_vars/NETWORK_SERVICES.yml">here</a>.
    </details>

5. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
6. Run the playbook `deploy.yml` with the command `ansible-playbook deploy.yml`.
7. Go to the CloudVision instance or your Arista Test Drive lab and open the `Provisioning` menu.
8.  Go to the `Tasks` tab, select the tasks that have been created by the playbook and create a change control. At this point, you can select the Parallel arrangement to have the tasks executed in parallel within the change control.
9.  You will be taken to the `Change Control` tab in the newly created change control. You can rename the `Change Control` to give it a meaningful name. Review, Approve and Execute the change control.
10. When the change control is completed, the border leafs will be configured to extend the EVPN domain. You can eventually access the devices via SSH and run the `show running-config` command to verify the configuration.