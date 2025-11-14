---
# YAML fontmatter for static site generator is created to publish this file as a blog post.
title: "Enterprise Network Phase 2 - Aggregation Layer Basic Configuration"
author: "Baltej Giri"
date: 2025-11-13
category: Networking, CCNA, CCNP
tags: [ccna, ccnp, eve-ng]
layout: post  # Example metadata for a static site generator
---

## Aggregation Layer Basic Configuration

Aggregation layer configuration for two switches "TOR-AGGR-SW1" and "TOR-AGGR-SW2". Both switches will have VLANs 10,20,30,40 created, and layer 3 boundary for VLANs using switched virtual interfaces. Interfaces connecting to access layer switches will be configured with all vlans to form trunks.

### Tasks to be done:
- Create VLANs
- Create SVI's
- Setup trunking on interfaces those connects to access layer switches.
- Verify VLANs
- Verify trunking
- Verify switch virtual interfaces
- Test inter-vlan connectivity
- Save configuration on both switches

### Aggregation Layer Switch "TOR-AGGR-1" configuration

```cisco
configure terminal
!
hostname TOR-AGGR-SW1
!
line console 0
logging synchronous
!
vlan 10
!
vlan 20
!
vlan 30
!
vlan 40
!
exit
!
interface range gig0/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-ACCE-SW1_gig0/0
!
interface gig0/1
description to_TOR-ACCE-SW2_gig0/0
exit
!
interface gig0/2
description to_TOR-ACCE-SW3_gig0/1
!
interface gig0/3
description to_TOR-ACCE-SW4_gig0/1
!
exit
!
interface vlan 10
ip address 10.10.10.2 255.255.255.0
description SVI_VLAN_10
no shutdown
!
interface vlan 20
ip address 10.10.20.2 255.255.255.0
description SVI_VLAN_20
no shutdown
!
interface vlan 30
ip address 10.10.30.2 255.255.255.0
description SVI_VLAN_30
no shutdown
!
interface vlan 40
ip address 10.10.40.2 255.255.255.0
description SVI_VLAN_40
no shutdown
!
end
!
```

### Aggregation Layer Switch "TOR-AGGR-2 configuration

```cisco
!
configure terminal
!
hostname TOR-AGGR-SW2
!
line console 0
logging synchronous
!
vlan 10
!
vlan 20
!
vlan 30
!
vlan 40
!
exit
!
interface range gig0/0-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-ACCE-SW3_gig0/0
!
interface gig0/1
description to_TOR-ACCE-SW4_gig0/0
exit
!
interface gig0/2
description to_TOR-ACCE-SW1_gig0/1
!
interface gig0/3
description to_TOR-ACCE-SW2_gig0/1
!
exit
!
interface vlan 10
ip address 10.10.10.3 255.255.255.0
description SVI_VLAN_10
no shutdown
!
interface vlan 20
ip address 10.10.20.3 255.255.255.0
description SVI_VLAN_20
no shutdown
!
interface vlan 30
ip address 10.10.30.3 255.255.255.0
description SVI_VLAN_30
no shutdown
!
interface vlan 40
ip address 10.10.40.3 255.255.255.0
description SVI_VLAN_40
no shutdown
!
end
!
```

### Verification

```cisco
show vlan brief
!
show interface trunks
```

### Configuration saving

```cisco
!
write memory
!
```

---
End of Phase 2 - Aggregation layer configuration
---
