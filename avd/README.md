# Arista Validated Design Labs

In the labs below, we will use Arista Validated Design (AVD) to create the device configuration and CloudVision provisioning capabilities to push the configuration to the fabric.

## Environement

These labs has been built using Arista Test Drive with:

- The dual datacenter topology
- CVP version 2023.2.0
- EOS Version 4.31.1F
- Arista Validated Design 4.5.0
- The AVD Project [arista-netdevops-community/avd-toi](https://github.com/arista-netdevops-community/avd-toi)

## Set-Up

Access to the Visual Studio Code Server by clicking on `Programmability IDE` on the left side of the Arista Test Drive home screen.
Open a Terminal (Menu on the top left corner -> Terminal -> New Terminal) and type the following commands:

``` bash
cd persist
git clone https://github.com/arista-netdevops-community/avd-toi.git
ansible-galaxy collection install -U arista.avd
```

Make sure that the state of the Arista Test Drive devices is the one at the end of the lab [CloudVision Studios Labs - Lab 1: Staging](../studios/1-staging.md). We want the hosts and core devices to be provisioned using static configlets: these configuration will not be managed by AVD.

## Table of contents

- [Lab 1: Provision POD1](1-provision-pod1.md)
- [Lab 2: Provision POD2](2-provision-pod2.md)
- [Lab 3: Configure the border leafs to extend the underlay and EVPN domain](3-border-leafs.md)
- [Lab 4: Create EVPN services and configure leaf server ports](4-services-attachments.md)
