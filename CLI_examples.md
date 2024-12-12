# CLI Examples

This document holds the translation of the last CP packet tacer questions to CLI 

## Key Things
If this is your first time using packet tracer you use the CLI you must open the thing that is going to be configed and then the terminal 


CS Distribution Router 1:

·       Host name - CSDistributionRouter1

·       IP addressing:

§  Interface Gig 1/0/1 – 172.16.0.6/30

§  Interface Gig 1/0/2 – 172.16.0.13/30

§  Interface VLAN 1:

-        Network address – 172.16.0.16

-        Number of hosts available – 14

-        This interface should use the first available address in the subnet

§  Interface VLAN 2:

-        Network address – 172.16.0.32

-        Number of hosts available - 6

-        This interface should use the first available address in the subnet

§  Interface VLAN 3:

-        Interface address 192.168.0.1

-        Number of hosts available – 2046

·       Routing:

§  Routing to the other company networks and the internet is accomplished by two static floating default routes.

-        Main route should have a default gateway of 172.16.0.5

-        Secondary route should have a default gateway of 172.16.0.14 and distance of 254

·       Switching:

§  VLANs 1-3 should exist on the switch

§  Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Gig 1/0/5 and Gig 1/0/6should be bundled together as an LACP etherchannel named port-channel 2

§  Both Port Channels should be dot1q trunks


1. Central Site (CS) - Internet Router Configuration (CSInternetRouter)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSInternetRouter
interface Gig0/0
  ip address 16.34.34.2 255.255.255.0
interface Gig0/1
  ip address 155.74.10.2 255.255.255.252
interface Gig0/2
  ip address 172.16.0.1 255.255.255.252
router ospf 1
  network 16.34.34.0 0.0.0.255 area 0
  network 155.74.10.0 0.0.0.3 area 0
  network 172.16.0.0 0.0.0.3 area 0
  timers hello 5
  timers dead 20
  ip ospf cost 30
end
write memory
```

2. Central Site Core Router Configuration (CSCoreRouter)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSCoreRouter
interface Gig0/0
  ip address 172.16.0.2 255.255.255.252
interface Gig0/1
  ip address 172.16.0.5 255.255.255.252
interface Gig0/2
  ip address 172.16.0.9 255.255.255.252
router ospf 1
  network 172.16.0.0 0.0.0.3 area 0
  redistribute static metric 1 subnets
end
write memory
```
3. Central Site Distribution Router 1 Configuration (CSDistributionRouter1)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSDistributionRouter1
interface Gig1/0/1
  ip address 172.16.0.6 255.255.255.252
interface Gig1/0/2
  ip address 172.16.0.13 255.255.255.252
interface vlan 1
  ip address 172.16.0.16 255.255.255.240
interface vlan 2
  ip address 172.16.0.32 255.255.255.252
interface vlan 3
  ip address 192.168.0.1 255.255.255.0
router ospf 1
  default-information originate
  network 172.16.0.0 0.0.0.3 area 0
  network 192.168.0.0 0.0.0.255 area 0
  ip ospf cost 10
  ip ospf priority 100
ip route 0.0.0.0 0.0.0.0 172.16.0.5
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
end
write memory
```
4. Central Site Distribution Router 2 Configuration (CSDistributionRouter2)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSDistributionRouter2
interface Gig1/0/1
  ip address 172.16.0.10 255.255.255.252
interface Gig1/0/2
  ip address 172.16.0.14 255.255.255.252
interface vlan 1
  ip address 172.16.0.16 255.255.255.240
interface vlan 2
  ip address 172.16.0.32 255.255.255.252
interface vlan 3
  ip address 192.168.0.2 255.255.255.0
router ospf 1
  default-information originate
  network 172.16.0.0 0.0.0.3 area 0
  network 192.168.0.0 0.0.0.255 area 0
  ip ospf cost 10
  ip ospf priority 100
ip route 0.0.0.0 0.0.0.0 172.16.0.9
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
end
write memory
```
5. Central Site Switch 1 Configuration (CSSwitch1)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSSwitch1
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
interface port-channel 3
  switchport mode trunk
interface Fast0/1
  switchport mode access
  switchport access vlan 1
interface Fast0/2
  switchport mode access
  switchport access vlan 2
interface Fast0/3
  switchport mode access
  switchport access vlan 3
end
write memory
```
6. Central Site Switch 2 Configuration (CSSwitch2)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname CSSwitch2
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
interface port-channel 3
  switchport mode trunk
interface Fast0/1
  switchport mode access
  switchport access vlan 1
interface Fast0/2
  switchport mode access
  switchport access vlan 2
interface Fast0/3
  switchport mode access
  switchport access vlan 3
end
write memory
```
7. Remote Site 1 (RS1) - Internet Router Configuration (RS1InternetRouter)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1InternetRouter
interface Gig0/0
  ip address 16.34.35.2 255.255.255.0
interface Gig0/1
  ip address 155.74.10.6 255.255.255.252
interface Gig0/2
  ip address 172.16.1.1 255.255.255.252
router ospf 1
  network 16.34.35.0 0.0.0.255 area 0
  network 155.74.10.0 0.0.0.3 area 0
  network 172.16.1.0 0.0.0.3 area 0
  timers hello 5
  timers dead 20
  ip ospf cost 30
end
write memory
```
8. Remote Site 1 Core Router Configuration (RS1CoreRouter)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1CoreRouter
interface Gig0/0
  ip address 172.16.1.2 255.255.255.252
interface Gig0/1
  ip address 172.16.1.5 255.255.255.252
interface Gig0/2
  ip address 172.16.1.9 255.255.255.252
router ospf 1
  network 172.16.1.0 0.0.0.3 area 0
  redistribute static metric 1 subnets
end
write memory
```
9. Remote Site 1 Switch 1 Configuration (RS1Switch1)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1Switch1
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
interface port-channel 3
  switchport mode trunk
interface Fast0/1
  switchport mode access
  switchport access vlan 1
interface Fast0/2
  switchport mode access
  switchport access vlan 2
interface Fast0/3
  switchport mode access
  switchport access vlan 3
end
write memory
```
10. Remote Site 1 Switch 2 Configuration (RS1Switch2)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1Switch2
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
interface port-channel 3
  switchport mode trunk
interface Fast0/1
  switchport mode access
  switchport access vlan 1
interface Fast0/2
  switchport mode access
  switchport access vlan 2
interface Fast0/3
  switchport mode access
  switchport access vlan 3
end
write memory
```
