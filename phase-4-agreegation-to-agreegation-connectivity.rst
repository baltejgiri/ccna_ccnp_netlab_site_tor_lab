---
### YAML fontmatter for static site generator is created to publish this file as a blog post.
title: Enterprise Network Lab: Phase 4 - Aggregation to Aggregation Connectivity
author: "Baltej Giri"
date: 2025-11-13
category: Networking, CCNA, CCNP
tags: [ccna, ccnp, eve-ng]
layout: post  # Example metadata for a static site generator
---

## Phase 4 - Aggregation to Aggregation Connectivity

Phase 4 focuses on establishing Layer 3 connectivity between the two aggregation layer switches, TOR-AGGR-SW1 and TOR-AGGR-SW2, using a routed Port-Channel (EtherChannel) with LACP protocol. This setup enhances redundancy and bandwidth between the aggregation switches. As always, validate configurations with verification commands before saving the configuration on switches.

### Switch 1 - TOR-AGGR-SW1
!Create Layer 3 Port-Channel 1
interface Port-channel1
!Set description
description L3_EtherChannel_to_TOR-AGGR-SW2
!Set port channel to routed port
no switchport
!Set ip address for port channel 1
ip address 10.10.150.1 255.255.255.252
no shutdown
!
!Configure Physical member interfaces using range command
!
interface range GigabitEthernet2/2-3
no switchport
channel-protocol lacp
channel-group 1 mode active
no shutdown
!
interface GigabitEthernet2/2
description Member_of_Po1_to_TOR-AGGR-SW2_Gig2/2
!
interface GigabitEthernet2/3
description Member_of_Po1_to_TOR-AGGR-SW2_Gig2/3
!
end
!

### Switch 2 - TOR-AGGR-SW2

!
configure terminal
!
!Create Layer 3 Port-Channel 1
interface Port-channel1
!Set description
description L3_EtherChannel_to_TOR-AGGR-SW1
!Set port channel to routed port
no switchport
!Set ip address for port channel 1
ip address 10.10.150.2 255.255.255.252
no shutdown
!
!Configure Physical member interfaces using range command
!
interface range GigabitEthernet2/2-3
no switchport
channel-protocol lacp
channel-group 1 mode active
no shutdown
!
interface GigabitEthernet2/2
description Member_of_Po1_to_TOR-AGGR-SW1_Gig2/2
!
interface GigabitEthernet2/3
description Member_of_Po1_to_TOR-AGGR-SW1_Gig2/3
!
end
!

### Verification Commands for both switches

```cisco
!
!To see Port-Channel summary
show etherchannel summary
!
!To see LACP neighbor details
show lacp neighbor
!
show ip interface brief | include Port-channel
!
ping 10.10.150.2
!
!ping 10.10.150.1 from TOR-AGGR-SW2
!
```

### Save Configuration
```cisco
!
write memory
!
```
---
End of Phase 4 - Aggregation to Aggregation Connectivity.
---

