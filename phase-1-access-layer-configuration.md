---
# YAML fontmatter for static site generator is created to publish this file as a blog post.
title: "Enterprise Network Phase 1 - Access Layer Configuration"
author: "Baltej Giri"
date: 2025-11-13
category: Networking, CCNA, CCNP
tags: [ccna, ccnp, eve-ng]
layout: post  # Example metadata for a static site generator
---

## Access Layer Configuration

Access layer configuration for four switches "TOR-ACCE-SW1", "TOR-ACCE-SW2", "TOR-ACCE-SW3", "TOR-ACCE-SW4". Each switch will have VLANs 10,20,30,40 created. Each switch will have one access port assigned to a specific VLAN and trunking configured to the aggregation layer switches. At the end, VLANs and trunking will be verified and configuration will be saved.

### Tasks to be done:
- Create VLANs 10,20,30,40
- Assign Access port to VLAN 10
- Configure Trunking to Aggregation Layer Switches
- Verify VLANs and Trunking
- Save Configuration

### Switch 1 "TOR-ACCE-SW1" Configuration

```cisco
configure terminal
!
line con 0
logging sync
!
hostname TOR-ACCE-SW1
exit
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
int gig0/2
switchport mode access
switchport access vlan 10
no shutdown
!
interface range gig0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-AGRR-SW1_gig0/0
!
interface gig0/1
description to_TOR-AGRR-SW2_gig0/2
exit
!
do show int trunk
!
```
### Switch 2 "TOR-ACCE-SW2" Configuration

```cisco
configure terminal
!
line con 0
logging sync
!
hostname TOR-ACCE-SW2
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
int gig0/2
switchport mode access
switchport access vlan 20
no shutdown
!
interface range gig0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-AGRR-SW1_gig0/1
!
interface gig0/1
description to_TOR-AGRR-SW2_gig0/3
end
!
show vlan brief
!
show int trunk
!
write memory
!
```
### Switch 3 "TOR-ACCE-SW3" Configuration
#### Tasks to be done:
- Create VLANs 10,20,30,40
- Assign Access port to VLAN 30
- Configure Trunking to Aggregation Layer Switches
- Verify VLANs and Trunking
- Save Configuration

```cisco                        
configure terminal
!
line con 0
logging sync
!
hostname TOR-ACCE-SW3
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
int gig0/2
switchport mode access
switchport access vlan 30
no shutdown
!
exit
!
interface range gig0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-AGRR-SW2_gig0/0
!
interface gig0/1
description to_TOR-AGRR-SW1_gig0/2
end
!
show vlan brief
!
show int trunk
!
write memory
!
```
### Switch 4 "TOR-ACCE-SW4" Configuration
#### Tasks to be done:
- Create VLANs 10,20,30,40
- Assign Access port to VLAN 30
- Configure Trunking to Aggregation Layer Switches
- Verify VLANs and Trunking
- Save Configuration

```cisco
configure terminal
!
line con 0
logging sync
!
hostname TOR-ACCE-SW4
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
int gig0/2
switchport mode access
switchport access vlan 30
no shutdown
!
end
!
show vlan brief
##TRUNKING TO AGRREGATION LAYER
!
interface range gig0/0-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 1,10,20,30,40
no shutdown
!
interface gig0/0
description to_TOR-AGRR-SW2_gig0/1
!
interface gig0/1
description to_TOR-AGRR-SW1_gig0/3
end
!
show vlan brief
!
show int trunk
!
write memory
!
```

---
End of Phase 1 - Access Layer Configuration
---

