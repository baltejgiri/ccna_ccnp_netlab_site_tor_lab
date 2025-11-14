---
# YAML fontmatter for static site generator is created to publish this file as a blog post.
title: Enterprise Network Lab: Phase 3 - Aggregation Layer Advanced Configuration
author: "Baltej Giri"
date: 2025-11-13
category: Networking, CCNA, CCNP
tags: [ccna, ccnp, eve-ng]
layout: post  # Example metadata for a static site generator
---

# Phase 3 - Aggregation Layer Advanced Configuration

This configuration focuses on enabling IP routing and setting up HSRP (Hot Standby Router Protocol) on both aggregation layer switches "TOR-AGGR-SW1" and "TOR-AGGR-SW2". HSRP will provide high availability for the default gateways of VLANs 10, 20, 30, and 40. 

The configuration includes setting up active and passive roles for each VLAN across the two switches, along with authentication for security. Finally, verification commands are included to check the HSRP status.

---

## Tasks to be done:
- Enable IP routing on both aggregation switches.
- Configure HSRP on SVIs for VLANs 10, 20, 30, and 40 on both switches.
- Verify HSRP configuration and status.
- Save configuration on both switches.

### Switch 1: TOR-AGGR-SW1

```cisco
!
!Enable routing on layer 3 switch
configure terminal
ip routing
!
#SVI 10 - HSRP GROUP 10 is ACTIVE
interface vlan 10
!Create a standby group with number and assign a VIP.
standby 10 ip 10.10.10.1
!Set priority number higher than default (100)
standby 10 priority 110
!Set group to preemption so switch can reclaim active role.
standby 10 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 10 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 10 authentication md5 key-string SecureHSRP10
no shutdown
!
#SVI 20 - HSRP GROUP 20 is PASSIVE
interface vlan 20
!Create a standby group with number and assign a VIP.
standby 20 ip 10.10.20.1
!Set priority number higher than default (100)
standby 20 priority 100
!Set group to preemption so switch can reclaim active role.
standby 20 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 20 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 20 authentication md5 key-string SecureHSRP20
no shutdown
!
#SVI 30 - HSRP GROUP 30 is ACTIVE
interface vlan 30
!Create a standby group with number and assign a VIP.
standby 30 ip 10.10.30.1
!Set priority number higher than default (100)
standby 30 priority 110
!Set group to preemption so switch can reclaim active role.
standby 30 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 30 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 30 authentication md5 key-string SecureHSRP30
no shutdown
!
#SVI 40 - HSRP GROUP 40 is PASSIVE
interface vlan 40
!Create a standby group with number and assign a VIP.
standby 40 ip 10.10.40.1
!Set priority number higher than default (100)
standby 40 priority 100
!Set group to preemption so switch can reclaim active role.
standby 40 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 40 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 40 authentication md5 key-string SecureHSRP40
no shutdown
!
end
!
```

### Switch 2: TOR-AGGR-SW2

```cisco
!
!Enable routing on layer 3 switch
configure terminal
ip routing
!
configure terminal
!
#SVI 10 - HSRP GROUP 10 is PASSIVE
interface vlan 10
!Create a standby group with number and assign a VIP.
standby 10 ip 10.10.10.1
!Set priority number lower than Active if this switch has passive role for VLAN 10.
standby 10 priority 100
!Set group to preemption so switch can reclaim active role.
standby 10 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 10 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 10 authentication md5 key-string SecureHSRP10
no shutdown
!
# SVI 20 - HSRP GROUP 20 is ACTIVE
interface vlan 20
!Create a standby group with number and assign a VIP.
standby 20 ip 10.10.20.1
!Set priority number higher than default (100)
standby 20 priority 110
!Set group to preemption so switch can reclaim active role.
standby 20 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 20 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 20 authentication md5 key-string SecureHSRP20
no shutdown
!
# SVI 30 - HSRP GROUP 30 is PASSIVE
interface vlan 30
!Create a standby group with number and assign a VIP.
standby 30 ip 10.10.30.1
!Set priority number lower than Active if this switch has passive role for VLAN 30.
standby 30 priority 100
!Set group to preemption so switch can reclaim active role.
standby 30 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 30 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 30 authentication md5 key-string SecureHSRP30
no shutdown
!
# SVI 40 - HSRP GROUP 40 is ACTIVE
interface vlan 40
!Create a standby group with number and assign a VIP.
standby 40 ip 10.10.40.1
!Set priority number higher than default (100)
standby 40 priority 110
!Set group to preemption so switch can reclaim active role.
standby 40 preempt
!Set custom hello timer to 1 and hold timer to 3 sec for faster conversion.
standby 40 timers 1 3
!Set standby group key and match it on the passive switch, eliminates rough devices joining HSRP group.
standby 40 authentication md5 key-string SecureHSRP40
no shutdown
!
end
!

```

### Verification

```cisco
show standby all
!
show standby brief
!
```

### Configuration save

```cisco
write memory
```

---
End of Phase 3 - Aggregation Layer Advanced Configuration
---