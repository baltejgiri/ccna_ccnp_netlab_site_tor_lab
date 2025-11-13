https://www.dell.com/en-ca/shop/ipovw/networking-z-series



# ACCESS LAYER CONFIGURATIONS
---
## SW1
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
## TRUNKING TO AGRREGATION LAYER
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
---
SW2
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
!
end
!
show vlan brief
!
##TRUNKING TO AGRREGATION LAYER
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
exit
!
do show int trunk
!
-----------------------------
-----------------------------
SW3
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
end
!
show vlan brief
!
##TRUNKING TO AGRREGATION LAYER
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
exit
!
do show int trunk
!
-----------

SW4
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
exit
!
do show int trunk
!
---------------------------------------------------------
---------------------------------------------------------
-              AGRREGATION LAYER CONFIGURATIONS         -
---------------------------------------------------------
---------------------------------------------------------
------------------
-  TOR-AGRR-SW1  -
------------------
configure terminal
!
hostname TOR-AGRR-SW1
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
##TRUNKING TO AGRREGATION LAYER
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
------------------
-  TOR-AGRR-SW2  -
------------------
configure terminal
!
hostname TOR-AGRR-SW2
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
do show vlan brief
!
## TRUNKING TO AGRREGATION LAYER
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
exit
!
## ENABLE IP ROUTING ON BOTH AGRREGATED SWITCHES
### TOR-AGRR-SW1
!
configure terminal
ip routing
!
### TOR-AGRR-SW2
!
configure terminal
ip routing
!
------------------------------------------
## ENABLE HSRP ON BOTH AGRREGATED SWITCHES
----------------------------
### SWITCH 2 -> TOR-AGRR-SW2
----------------------------
!
configure terminal
!
# SVI 10 - HSRP GROUP 10 is ACTIVE
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
# SVI 20 - HSRP GROUP 20 is PASSIVE
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
# SVI 30 - HSRP GROUP 30 is ACTIVE
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
# SVI 40 - HSRP GROUP 40 is PASSIVE
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
show standby all
!
show standby brief
!
----------------------------
### SWITCH 2 -> TOR-AGRR-SW2
----------------------------
!
configure terminal
!
# SVI 10 - HSRP GROUP 10 is PASSIVE
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
show standby all
!
show standby brief
!


