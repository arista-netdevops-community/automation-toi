# Lab 4: Create EVPN services and configure leaf server ports

### Description

In this lab, we will configure L2 EVPN Services and the corresponding VLANs on the leaf server ports:
- `VLAN 134` is local to `POD1`.
- `VLAN 234` is local to `POD2`.
- `VLAN 112` is stretched across `POD1` and `POD2`.
- `s1-host1` and `s1-host2` have Layer 3 interfaces in `VLAN 134` and `VLAN 112`.
- `s2-host1` and `s2-host2` have Layer 3 interfaces in `VLAN 234` and `VLAN 112`.

At the end if this lab, hosts will be able to ping each other using the VLANs described above.

### Instructions

#### EVPN Services

1. Go to the `Provisioning` menu and access the `Studios` tab. Click on the `Create Workspace` button to create a workspace and give it a meaningful name like `Create VLAN 112, 134 and 234`.
2. Click on the `EVPN Services` studio. Only assign leaves to this Studio using the query `Role:Leaf` in the `Device Selection` section (use `Tag Query`).
3. Add a tenant called `Tenant-A`, go to the tenant configuration by clicking on the arrow.
4. Create 3 VLANs: `112`, `134` and `234`.
5. In the `VLAN 112` configuration, modify the following configuration:
   - `Name` must have the value `L2_Stretched_POD1-POD2`.
   - VLAN must be `Bridged`.
   - `VTEPs` must have the value `Role:Leaf AND ( DC-Pod:POD1 OR DC-Pod:POD2 )` to select all VTEPs in `POD1` and `POD2`.
6. In the `VLAN 134` configuration, modify the following configuration:
   - `Name` must have the value `L2_Intra_POD1`.
   - VLAN must be `Bridged`.
   - `VTEPs` must have the value `Role:Leaf AND DC-Pod:POD1` to select only VTEPs in `POD1`.
7. In the `VLAN 234` configuration, modify the following configuration:
   - `Name` must have the value `L2_Intra_POD2`.
   - VLAN must be `Bridged`.
   - `VTEPs` must have the value `Role:Leaf AND DC-Pod:POD2` to select only VTEPs in `POD2`.
8. Click on the `Review Workspace` button on the top right corner. You will be taken to the `Workspace` screen where the inputs will be validated, the configlet generated and the configuration validated by the devices.
9. Review the configuration changes for each device but **do not submit** the workspace yet, we will also include the leaf server ports configuration in the change control.

#### Leaf Server Ports

1. Go to the `Provisioning` menu and access the `Studios` tab.
2. Click on the `Interface Configuration` studio. Only assign leaves to this Studio using the query `Role:Leaf` in the `Device Selection` section (use `Tag Query`).
3. Create 2 `Profiles` named `Host POD1` and `Host POD2`. Since we have 4 hosts but only 2 different port configurations, 2 profiles will suffice.
4. In the `Host POD1` configuration, modify the following configuration:
   - `Profile Description` must have the value `$1` to reuse the interface description. When defining a profile per host, it is a good practice to set the attached host name here as this description will be used on the Port-Channel interface.
   - `Switchport Mode` must have the value `trunk`.
   - `Allowed VLANs` must have the value `112,134`.
   - `Port Channel Group` must have the value `1`.
   - `Enable MLAG` must be `Yes`.
   - `Enable LACP` must be `Yes`.
5. In the `Host POD2` configuration, modify the following configuration:
   - `Profile Description` must have the value `$1`.
   - `Switchport Mode` must have the value `trunk`.
   - `Allowed VLANs` must have the value `112,234`.
   - `Port Channel Group` must have the value `1`.
   - `Enable MLAG` must be `Yes`.
   - `Enable LACP` must be `Yes`.
6. Go back to the studio configuration and select device `s1-leaf1`. On the interface `Ethernet4`, select `Yes` for the `Enabled` value, `Host POD1` for the `Profile` value and set `s1-host1` as `Description`.
7. Similarly, configure the following leaf interfaces:
   - `Ethernet4` on `s1-leaf2` with the `Host POD1` profile and the `s1-host1` description.
   - `Ethernet4` on `s1-leaf3` with the `Host POD1` profile and the `s1-host2` description.
   - `Ethernet4` on `s1-leaf4` with the `Host POD1` profile and the `s1-host2` description.
   - `Ethernet4` on `s2-leaf1` with the `Host POD2` profile and the `s2-host1` description.
   - `Ethernet4` on `s2-leaf2` with the `Host POD2` profile and the `s2-host1` description.
   - `Ethernet4` on `s2-leaf3` with the `Host POD2` profile and the `s2-host2` description.
   - `Ethernet4` on `s2-leaf4` with the `Host POD2` profile and the `s2-host2` description.
8. Click on the `Review Workspace` button on the top right corner. You will be taken to the `Workspace` screen where the inputs will be validated, the configlet generated and the configuration validated by the devices.
9. Review the configuration changes for each device and click on `Submit Workspace` then `View Change Control`.
10. You will be taken to the `Change Control` tab in the newly created change control. Review, Approve and Execute the change control.
11. Hosts are now able to ping each other using the VLANs we have just configured.
