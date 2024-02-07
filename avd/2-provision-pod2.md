# Lab 2: Provision POD2

### Description

In this lab, we will use Arista Validated Design (AVD) to generate `POD2` underlay and overlay configuration and CloudVision to deploy it.
This time, `eos_designs` variables need to be created.
At the end of this lab, `POD1` and `POD2` will be 2 separate EVPN domains: all leaf can only reach the `Loopback0` of other leaves of their respective pods.

### Instructions

1. Go to the folder `/home/coder/project/persist/avd-toi`.
2. Open `inventory.yml` and uncomment all lines in the file.
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