# Lab 3: Configure the border leafs to extend the underlay and EVPN domain

### Description

In this lab, we will configure Layer 3 point-to-point links between `s1-brdr1` and `s2-brdr1` and between `s1-brdr2` and `s2-brdr2`, create underlay eBGP sessions on these links and EVPN overlay sessions to extend the EVPN domains.
At the end of this lab, `POD1` and `POD2` will share the same underlay and form a single EVPN domain: all `Loopback0` interfaces must be reachable from any leafs.

### Instructions

1. Connect first to a border leaf and check the underlay and EVPN BGP sessions.
   
```cli
s1-brdr2#show ip bgp summary
BGP summary information for VRF default
Router identifier 172.16.0.6, local AS number 65103
Neighbor Status Codes: m - Under maintenance
  Description              Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  s1-spine1_Ethernet8      10.0.0.20   4 65100             22        21    0    0 00:10:33 Estab   7      7
  s1-spine2_Ethernet8      10.0.0.22   4 65100             22        19    0    0 00:10:33 Estab   7      7
  s1-brdr1                 169.254.0.0 4 65103             20        21    0    0 00:10:30 Estab   10     10
s1-brdr2#
show bgp evpn summary
BGP summary information for VRF default
Router identifier 172.16.0.6, local AS number 65103
Neighbor Status Codes: m - Under maintenance
  Description              Neighbor   V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  s1-spine1                172.16.1.1 4 65100             16        16    0    0 00:10:38 Estab   0      0
  s1-spine2                172.16.1.2 4 65100             16        16    0    0 00:10:39 Estab   0      0
s1-brdr2#
```

2. Go to `Provisioning` menu and access the `Studios` tab. Click on the `Create Workspace` button to create a workspace and give it a meaningful name like `Create POD1 and POD2 border leafs configuration`.
3. Click on the `Enterprise Routing` studio, add a network called `Datacenter1-POD1_POD2`, go to the network configuration by clicking on the arrow and assign the devices `s1-brdr1`, `s1-brdr2`, `s2-brdr1` and `s2-brdr2` to this network using the `Assigned Devices` field on at the top of the screen.
4. Next to the `Global Underlay Routing` section, modify the following configuration:
   - `Underlay Routing` must have the value `BGP`.
   - `BGP Peer Group Name` must have the value `IPv4-DCI-PEERS`.
5. Go to the `Edge Router Domains` table and add domain named `POD1` and `POD2`.
6. Go to `POD1` domain configuration and assign devices `s1-brdr1` and `s1-brdr2`.
7. Under `BGP Settings`:
   - Enable `No BGP Default IPv4 Unicast`
   - Add the following CLI commands to the `BGP Defaults CLI` table:
     - `neighbor IPv4-DCI-PEERS maximum-routes 12000`
     - `neighbor IPv4-DCI-PEERS send-community`
8. Go back to the domain configuration and create a BGP Peer Group named `EVPN-DCI-PEERS`.
9.  Under this BGP peer group configuration, modify the following configuration:
   - `Address Family` must have the value `evpn`.
   - `Maximum Accepted Routes` must have the value `0`.
   - `eBGP Multihop TTL` must have the value `3`.
   - `BFD` must have the value `Yes`.
   - `Update Source` must have the value `Loopback0`.
   - `Next Hop` must have the value `Unchanged`.
10. Go back to the domain configuration and navigate to the `s1-brdr1` Edge Router Domain Member configuration to modify the following fields:
   - Under `Underlay Interfaces`, add `Ethernet4` with description `P2P_LINK_TO_s2-brdr1_Ethernet4`, Local IPv4 Address `10.255.0.0` and Peer ASN `65203`.
   - Under `BGP Peers`, add `172.16.2.7` with Peer ASN `65203` and Peer Group `EVPN-DCI-PEERS` and Description `s2-brdr1`.
11. Navigate to the `s1-brdr2` Edge Router Domain Member configuration to modify the following fields:
   - Under `Underlay Interfaces`, add `Ethernet5` with description `P2P_LINK_TO_s2-brdr2_Ethernet5`, Local IPv4 Address `10.255.0.2` and Peer ASN `65203`.
   - Under `BGP Peers`, add `172.16.2.8` with Peer ASN `65203` and Peer Group `EVPN-DCI-PEERS` and Description `s2-brdr2`.
12. Repeat steps 5 to 8 for `POD2` domain configuration and assigned devices `s2-brdr1` and `s2-brdr2` to this domain.
13. Navigate to the `s2-brdr1` Edge Router Domain Member configuration to modify the following fields:
   - Under `Underlay Interfaces`, add `Ethernet4` with description `P2P_LINK_TO_s1-brdr1_Ethernet4`, Local IPv4 Address `10.255.0.1` and Peer ASN `65103`.
   - Under `BGP Peers`, add `172.16.1.7` with Peer ASN `65103` and Peer Group `EVPN-DCI-PEERS` and Description `s1-brdr1`.
14. Navigate to the `s2-brdr2` Edge Router Domain Member configuration to modify the following fields:
   - Under `Underlay Interfaces`, add `Ethernet5` with description `P2P_LINK_TO_s1-brdr2_Ethernet5`, Local IPv4 Address `10.255.0.3` and Peer ASN `65103`.
   - Under `BGP Peers`, add `172.16.1.8` with Peer ASN `65103` and Peer Group `EVPN-DCI-PEERS` and Description `s1-brdr2`.
15. Click on the `Review Workspace` button on the top right corner. You will be taken to the `Workspace` screen where the inputs will be validated, the configlet generated and the configuration validated by the devices.
16. Review the configuration changes for each device and click on `Submit Workspace` then `View Change Control`.
17. You will be taken to the `Change Control` tab in the newly created change control. Review, Approve and Execute the change control.
18. The border leafs are now configured to extend and EVPN domain services between `POD1` and `POD2`.
19. Connect again to a border leaf and check the underlay and EVPN BGP sessions to check the new sessions between the border leafs.

```cli
s1-brdr2#show ip bgp summary
BGP summary information for VRF default
Router identifier 172.16.1.8, local AS number 65103
Neighbor Status Codes: m - Under maintenance
  Description              Neighbor    V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  s1-spine1_Ethernet8      10.0.0.20   4 65100            595       599    0    0 01:30:09 Estab   7      7
  s1-spine2_Ethernet8      10.0.0.22   4 65100            592       590    0    0 01:30:09 Estab   7      7
                           10.255.0.3  4 65203             47        53    0    0 00:31:34 Estab   11     11
  s1-brdr1                 169.254.0.0 4 65103            591       577    0    0 01:30:09 Estab   21     21
s1-brdr2#
s1-brdr2#show bgp evpn summary
BGP summary information for VRF default
Router identifier 172.16.1.8, local AS number 65103
Neighbor Status Codes: m - Under maintenance
  Description              Neighbor   V AS           MsgRcvd   MsgSent  InQ OutQ  Up/Down State   PfxRcd PfxAcc
  s1-spine1                172.16.1.1 4 65100            561       560    0    0 01:30:14 Estab   0      0
  s1-spine2                172.16.1.2 4 65100            560       560    0    0 01:30:13 Estab   0      0
                           172.16.2.8 4 65203             61        61    0    0 00:21:22 Estab   0      0
s1-brdr2#
```