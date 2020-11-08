---
title: "CentOS and KVM: Single interface, multiple VLANs for Guests"
date: 2020-11-08T01:55:00-08:00
---

KVM on Linux is a nice way to run isolated services on your home network;
in my case: MySQl, a Unifi controller, various Docker containers, etc.
However: if you want to run these services on different VLANs so they can be
in isolated networks, the process is less straightforward. This document covers
how to get CentOS network interfaces set up so multiple VLANs can co-exist
on a single interface in a way that's transparent to KVM guests.

**Note:** This is mostly notes for my own memory, I may come back later and
clean this up into more of a "how-to" article. As it stands, this will probably
get you going if you are as stuck as I was. This document covers CentOS 8.2 but
should work on Red Hat, Fedora, etc.

# Network Scripts

Create a network configuration definition for your primary interface; this will
be named after the interface name, which you can retrieve with an
`ip link show`; in my case, it's `enp0s25`. Replace those file and interface
names with whatever interface you'll be using.

Configure the bridge interface; use your own network information as appropriate.
This will be the primary IP address of your CentOS server:

```
# /etc/sysconfig/network-scripts/ifcfg-br0
DEVICE=br0
TYPE=Bridge
IPADDR=172.16.18.10
NETMASK=255.255.255.0
GATEWAY=172.16.18.254
DNS1=172.168.18.1
ONBOOT=yes
DEFROUTE=yes
BOOTPROTO=static
DELAY=0
DOMAIN="lan.mydomain.example"
```

Configure your interface to use the newly created `br0`:

```
# /etc/sysconfig/network-scripts/ifcfg-enp0s25
DEVICE="enp0s25" # replace with your interface name
TYPE=Ethernet
HWADDR=ff:ff:ff:ff:ff:ff  # replace with your hardware address
BOOTPROTO=none
ONBOOT=yes
BRIDGE=br0
```

Configure a bridge for each VLAN you wish to use; the `127` you see here is an
example and should be replaced with the number of your VLAN:

```
# /etc/sysconfig/network-scripts/ifcfg-br0.127
DEVICE=br0.127
TYPE=Bridge
IPADDR=172.16.19.10
NETMASK=255.255.255.0
ONBOOT=yes
BOOTPROTO=static
DELAY=0
VLAN=yes
```

Now create an interface for that VLAN and bridge:

```
# /etc/sysconfig/network-scripts/ifcfg-enp0s25.127
DEVICE="enp0s25.127"
BOOTPROTO=none
ONBOOT=yes
BRIDGE=br0.127
USERCTL=no
VLAN=yes
```

Now reboot your system; when it comes back up, you should have the new
interfaces you configured available, and it should be pingable at the primary
and VLAN addresses. Do this for as many VLANs as you need to cover.

When configuring new guests via the CentOS "Virtual Machine" manager, you should
be able to select the VLAN bridge as the host interface. When your guest boots,
it will now be on that selected VLAN without any additional configuration
necessary—and will support DHCP and all expected network features.
