# CLI Examples

This document holds the translation of the last CP packet tacer questions to CLI 

## Key Things
If this is your first time using packet tracer you use the CLI you must open the thing that is going to be configed and then the terminal 




CS Internet  Router:

·      Host name - CSInternetRouter

·       IP Addressing:

§  Interface Gig 0/0 – 16.34.34.2/24

§  Interface Gig 0/1 – 155.74.10.2/30 

§  Interface Gig 0/2 – 172.16.0.1/30

·       Routing:

§  OSPF should be configured under process 1 and all network statements in area 0

§  Connection to ISP 1 should have OSPF enabled

§  Connection to ISP 2 should have OSPF enabled

§  Interface Gig 0/2 should have OSPF enabled

§  ISP 1 and 2 would like us to use values that are half the default value for hello and dead intervals. Internal networks should stay at default.

§  Use the network address for the subnet and matching wildcard mask in the network statement for the connection to ISP 2. Use the specific interface address and the required mask in the network statement for the connection to ISP 1 and on Gig 0/2.

§  Configure the link to ISP2 with an OSPF cost value of 30 so the ISP1 path is preferred for internet traffic when both links are up.

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
CS Core Router:

·       Host name - CSCoreRouter

·       IP addressing:

§  Interface Gig 0/0 – 172.16.0.2/30

§  Interface Gig 0/1 – 172.16.0.5/30

§  Interface gig 0/2 – 172.16.0.9/30

·       Routing:

§  OSPF should be configured under process 1 and all network statements in area 0

§  OSPF should be enabled for int Gig 0/0

§  Use the specific interface address and the required mask in the network statement for Gig 0/0

§  Configure the following command in the OSPF process “redistribute static metric 1 subnets”

§  Routing to the downstream networks on the distribution routers should be via static routes (These networks are the VLAN interfaces on the CS distribution routers. You can find these networks in the instructions for the CS distribution routers). Because VLAN 3 has many more hosts than the other VLANs we want to dedicate one of the two paths to that VLAN. The other two VLANs will use the other link. However, we want redundancy in case of a failure so we will use floating static routes.

-        VLAN 3 main route next hop should point to the directly connected interface on Distribution Router 2 

-        VLAN 3 secondary route next hop should point to the directly connected interface on Distribution Router 1

-        VLAN 1 and 2 main route next hop should point to the directly connected interface on Distribution Router 1

-        VLAN 1 and 2 secondary route next hop should point to the directly connected interface on Distribution Router 2

-        Secondary routes should have distance of 254

 

 
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
CS Distribution Router 2:

·      Host Name - CSDistributionRouter2

·       IP addressing:

§  Interface Gig 1/0/1 – 172.16.0.10/30

§  Interface Gig 1/0/2 – 172.16.0.14/30

§  Interface VLAN 1:

-        Network address – 172.16.0.16

-        Number of hosts available – 14

-        This interface should use the second available address in the subnet

§  Interface VLAN 2:

-        Network address – 172.16.0.32

-        Number of hosts available - 6

-        This interface should use the second available address in the subnet

§  Interface VLAN 3:

-        Interface address 192.168.0.2

-        Number of hosts available – 2046

·       Routing:

§  Routing to the other company networks and the internet is accomplished by two static floating default routes.

-        Main route should have a default gateway of 172.16.0.9

-        Secondary route should have a default gateway of 172.16.0.13 and distance of 254

·       Switching:

§  VLANs 1-3 should exist on the switch

§  Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2

§  Both Port Channels should be dot1q trunks

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

CS Switch 1:

·       Host name - CSSwitch1

·       Switching:

§  Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2

§  Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3

§  All port channels should be trunks

§  All PC ports should be in access mode

§  Interface Fast 0/7 – VLAN 1

§  Interface Fast 0/8 – VLAN 2

§  Interface Fast 0/8 – VLAN 3

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
CS Switch 2:

·       Host name CSSwitch2

·       Switching:

§  Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2

§  Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3 

§  All port channels should be trunks

§  All PC ports should be in access mode

§  Interface Fast 0/7 – VLAN 1

§  Interface Fast 0/8 – VLAN 2

§  Interface Fast 0/9 – VLAN 3
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

RS1 Internet Router:

·       Host name - RS1InternetRouter

·       IP addressing:

§  Interface Gig 0/0 – 16.34.35.2/24

§  Interface Gig 0/1 – 155.74.10.6/30

§  Interface Gig 0/2 – 172.16.1.1/30

·       Routing:

§  OSPF should be configured under process 1, and all network statements in area 0

§  Connection to ISP 1 should have OSPF enabled.

§  Connection to ISP 2 should have OSPF enabled.

§  Interface Gig 0/2 should have OSPF enabled.

§  ISP 1 and 2 would like us to use values that are half the default value for hello and dead intervals. Internal networks should stay at default.

§  Use the network address for the subnet and matching wildcard mask
 in the network statement for the connection to ISP 2. Use the specific interface address and the required mask in the network statement for the connection to ISP 1 and Gig 0/2.

§  Using a cost value of 30 configure the router so that it will always prefer ISP 1 when both ISP connections are up.

 

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
RS1 Core Router:

·       Host name RS1CoreRouter

·       IP addressing:

§  Interface Gig 0/0 – 172.16.1.2/30

§  Interface Gig 0/1 – 172.16.1.5/30

§  Interface gig 0/2 – 172.16.1.9/30

·       Routing:

§  OSPF should be configured under process 1 and all network statements in area 0

§  OSPF should be enabled on int Gig 0/0

§  Use the specific interface address and the required mask in the network statement for Gig 0/0

§  Configure the following command in the OSPF process “redistribute static metric 1 subnets”

§  Routing to the downstream networks on the distribution routers should be via static routes (These networks are the VLAN interfaces on the RS1 distribution routers. You can find these networks in the instructions for the RS1 distribution routers). There should be two routes for each network. One with a next hop of the directly connected interface on Distribution Router 1 and the second with a next hop of the directly connected interface on Distribution Router 2. 

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
RS1 Distribution Router 1:

·       Host name RS1DistributionRouter1

·       IP addressing:

§  Interface Gig 1/0/1 – 172.16.1.6/30

§  Interface Gig 1/0/2 – 172.16.1.13/30  

§  Interface VLAN 1:

-        Network address – 172.16.1.16

-        Number of hosts available – 14

-        This interface should use the first available address in the subnet

§  Interface VLAN 2:

-        Network address – 172.16.1.32

-        Number of hosts available - 30

-        This interface should use the first available address in the subnet

§  Interface VLAN 3:

-        Network address – 172.16.1.64

-        Number of hosts available – 6

-        This interface should use the first available address in the subnet

·       Routing:

§  Routing to the other company networks and the internet is accomplished by two static floating default routes.

-        Main route should have a next hop of 172.16.1.5

-        The secondary route should have a next hop of 172.16.1.14 and distance of 254

·       Switching:

§  VLANs 1-3 should exist on the switch

§  Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2

§  Both Port Channels should be dot1q trunks

9. RS1 Distribution Router 1 Configuration (RS1DistributionRouter1)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1DistributionRouter1
interface Gig1/0/1
  ip address 172.16.1.6 255.255.255.252
interface Gig1/0/2
  ip address 172.16.1.13 255.255.255.252
interface vlan 1
  ip address 172.16.1.17 255.255.255.240
interface vlan 2
  ip address 172.16.1.32 255.255.255.224
interface vlan 3
  ip address 172.16.1.64 255.255.255.248
router static
  0.0.0.0 0.0.0.0 172.16.1.5
  0.0.0.0 0.0.0.0 172.16.1.14 254
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
end
write memory
```
RS1 Distribution Router 2:

·       Host name - RS1DistributionRouter2

·       IP addressing:

§  Interface Gig 1/0/1 – 172.16.1.10/30

§  Interface Gig 1/0/2 – 172.16.1.14/30

§  Interface VLAN 1:

-        Network address – 172.16.1.16

-        Number of hosts available – 14

-        This interface should use the second available address in the subnet

§  Interface VLAN 2:

-        Network address – 172.16.1.32

-        Number of hosts available - 30

-        This interface should use the second available address in the subnet

§  Interface VLAN 3:

-        Network address – 172.16.1.64

-        Number of hosts available – 6

-        This interface should use the second available address in the subnet

·       Routing:

§  Routing to the other company networks and the internet is accomplished by two static floating default routes.

-        Main route should have a next hop of 172.16.1.9

-        Secondary route should have a next hop of 172.16.1.13 and distance of 254

·       Switching:

§  VLANs 1-3 should exist on the switch

§  Interfaces Gig 1/0/3 and Gig 1/0/4 should be bundled together as an LACP etherchannel named port-channel 1

§  Interfaces Gig 1/0/5 and Gig 1/0/6 should be bundled together as an LACP etherchannel named port-channel 2

§  Both Port Channels should be dot1q trunks

 
10. RS1 Distribution Router 2 Configuration (RS1DistributionRouter2)
```
enable
enable secret C1sc0R0cks
configure terminal
hostname RS1DistributionRouter2
interface Gig1/0/1
  ip address 172.16.1.10 255.255.255.252
interface Gig1/0/2
  ip address 172.16.1.14 255.255.255.252
interface vlan 1
  ip address 172.16.1.18 255.255.255.240
interface vlan 2
  ip address 172.16.1.32 255.255.255.224
interface vlan 3
  ip address 172.16.1.64 255.255.255.248
router static
  0.0.0.0 0.0.0.0 172.16.1.9
  0.0.0.0 0.0.0.0 172.16.1.13 254
interface port-channel 1
  switchport mode trunk
interface port-channel 2
  switchport mode trunk
end
write memory
```
RS1 Switch 1:

·       Host name - RS1Switch1

·       Switching:

§  Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1.

§  Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2.

§  Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3.

§  All port channels should be trunks

§  All PC ports should be in access mode

§  Interface Fast 0/7 – VLAN 1

§  Interface Fast 0/8 – VLAN 2

§  Interface Fast 0/8 – VLAN 3
11. Remote Site 1 Switch 1 Configuration (RS1Switch1)
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
RS1 Switch 2:

·       Host name - RS1Switch2

·       Switching:

§  Interfaces Fast 0/1 and Fast 0/2 should be bundled together as an LACP etherchannel named port-channel 1.

§  Interfaces Fast 0/3 and Fast 0/4 should be bundled together as an LACP etherchannel named port-channel 2.

§  Interfaces Fast 0/5 and Fast 0/6 should be bundled together as an LACP etherchannel named port-channel 3. 

§  All port channels should be trunks

§  All PC ports should be in access mode

§  Interface Fast 0/7 – VLAN 1

§  Interface Fast 0/8 – VLAN 2

§  Interface Fast 0/9 – VLAN 3 

12. Remote Site 1 Switch 2 Configuration (RS1Switch2)
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
