# Lab 3: Configure the border leafs to extend the underlay and EVPN domain

### Description

In this lab, we will configure Layer 3 point-to-point links between `s1-brdr1` and `s2-brdr1` and between `s1-brdr2` and `s2-brdr2`, create underlay eBGP sessions on these links and EVPN overlay sessions to extend the EVPN domains.
At the end of this lab, `POD1` and `POD2` will share the same underlay and form a single EVPN domain: all `Loopback0` interfaces must be reachable from any leafs.

### Instructions

1. Go to the folder `/home/coder/project/persist/avd-toi`.
2. Open `group_vars/FABRIC.yml`. Use the [L3 Edge](https://avd.sh/en/stable/roles/eos_designs/docs/input-variables.html#l3-edge-and-dci-settings) model to create the point-to-point links between `s1-brdr1` and `s2-brdr1` and between `s1-brdr2` and `s2-brdr2`. Include these interfaces in the underlay protocol.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/group_vars/FABRIC.yml#L57">here</a>.
    </details>

3. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
4. We now want to create the overlay EVPN sessions between the border leafs. Create `host_vars/s1-brdr1.yml`. Use [Custom Structured Configuration](https://avd.sh/en/stable/roles/eos_designs/docs/how-to/custom-structured-configuration.html) to define `eos_cli_config_gen` variables for `s1-brdr1`. Use the [router_bgp](https://avd.sh/en/stable/roles/eos_cli_config_gen/docs/input-variables.html#router-bgp) model of `eos_cli_config_gen`. Define one neighbot with the correct IP, ASN, Description and Peer Group.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/host_vars/s1-brdr1.yml">here</a>.
    </details>

5. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
6. Repeat step 3 for `s1-brdr2`, `s2-brdr1` and `s2-brdr2`.

    <details close>
    <summary>Solution: Don't cheat, use your brain and RTFM</summary>
        One possible solution is accessible  <a href="https://github.com/arista-netdevops-community/avd-toi/blob/solution/host_vars">here</a>.
    </details>

7. Run the playbook `build.yml` with the command `ansible-playbook build.yml`. Verify the configurations in `intended/configs`.
8. Run the playbook `deploy.yml` with the command `ansible-playbook deploy.yml`.
9. Go to the CloudVision instance or your Arista Test Drive lab and open the `Provisioning` menu.
10. Go to the `Tasks` tab, select the tasks that have been created by the playbook and create a change control. At this point, you can select the Parallel arrangement to have the tasks executed in parallel within the change control.
11. You will be taken to the `Change Control` tab in the newly created change control. You can rename the `Change Control` to give it a meaningful name. Review, Approve and Execute the change control.
12. When the change control is completed, the border leafs will be configured to extend the EVPN domain. You can eventually access the devices via SSH and run the `show running-config` command to verify the configuration.