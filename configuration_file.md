https://www.dell.com/en-ca/shop/ipovw/networking-z-series


---------------------------------------------------------
---------------------------------------------------------
-              ACCESS LAYER CONFIGURATIONS              -
---------------------------------------------------------
---------------------------------------------------------
#SW1
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
##TRUNKING TO AGRREGATION LAYER
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
-----------------------------------------------
-----------------------------------------------
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
##TRUNKING TO AGRREGATION LAYER
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