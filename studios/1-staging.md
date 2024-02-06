# Lab 1: Staging

> TODO: configuration validation process and `show logging`.

### Description

In this lab, we will use **`static configlets`** to stage the Arista Test Drive topology for the next lab.
First, we will configure the 4 hosts to use a LACP Port Channel and Layer 3 subinterfaces to connect to the fabric,
then we will shutdown Ethernet interfaces in the core network to only have 2 links between the pods and avoid relying on Spanning Tree Protocol, as if `s1-brdr1` is directly connected to `s2-brdr1` and `s1-brdr2` is directly connected to `s2-brdr2`.

### Instructions

#### Hosts configuration

1. Go to the `Provisioning` menu and access the `Configlets` tab. Click on the `EVPN-SINGLE_s1-host1` configlet and edit it using the button on the top right corner.
2. On the `Port-Channel1.112` interface, change the `10.111.112.201/24` IP to `10.111.112.101/24`. Change the configlet name to `EVPN-SINGLE_s1-host1-updated` (this is important to not have this change overwritten by the Arista Test Drive provisioning when the lab restarts).
3. Similarly, edit the `EVPN-SINGLE_s1-host2` configlet and change the `10.111.112.202/24` IP to `10.111.112.102/24` on the `Port-Channel1.112` interface. Also update the configlet name to `EVPN-SINGLE_s1-host2-updated`.
4. Go to the `Network Provisioning` tab, right click on `s1-host1` and select Manage -> Configlet. Assign the configlet `EVPN-SINGLE_s1-host1-updated` to the device, validate and save the changes.
5. Repeat step 4 and assign configlet `EVPN-SINGLE_s1-host2-updated` to `s1-host2`, configlet `EVPN-SINGLE_s2-host1` to `s2-host1` and configlet `EVPN-SINGLE_s2-host2` to `s2-host2`.
6. Go to the `Tasks` tab, select the 4 tasks that have been created and create a change control. At this point, you can select the Parallel arrangement to have the 4 tasks executed in parallel within the change control.
7. You will be taken to the `Change Control` tab in the newly created change control. You can rename the `Change Control` to give it a meaningful name. Review, Approve and Execute the change control.
8. When the change control is completed, the 4 hosts will be configured. You can eventually access the hosts via SSH and run the `show running-config` command to verify the configuration.

#### Core network configuration

1. Go to `Provisioning` menu and access the `Configlets` tab. Create a new configlet using the `+` button on the top right corner and select `Configlets`.
2. Name the configlet `DCI_s1-core1` and write the following configuration:
    ```
    interface Ethernet 1,3,6
        shutdown
    ```
3. Repeat step 1 and 2 to create the following configlets:
   - Configlet named `DCI_s2-core1` with configuration:
     ```
     interface Ethernet 1,3,6
         shutdown
     ```
   - Configlets named `DCI_s1-core2` and `DCI_s2-core2` with configuration:
     ```
     interface Ethernet 1-2,6
         shutdown
     ```
4. Go to the `Network Provisioning` tab, right click on `s1-core1` and select Manage -> Configlet. Assign the configlet `DCI_s1-core1` to the device, validate and save the changes.
5. Repeat step 4 and assign configlet `DCI_s1-core2` to `s1-core2`, configlet `DCI_s2-core1` to `s2-core1` and configlet `DCI_s2-core2` to `s2-core2`.
6. Go to the `Tasks` tab, select the 4 tasks that have been created and create a change control. At this point, you can select the Parallel arrangement to have the 4 tasks executed in parallel within the change control.
7. You will be taken to the `Change Control` tab in the newly created change control. You can rename the `Change Control` to give it a meaningful name. Review, Approve and Execute the change control.
8. When the change control is completed, the 4 core nodes will be configured. You can eventually access the hosts via SSH and run the `show running-config` command to verify the configuration.
