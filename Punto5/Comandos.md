# CLI
## CSR1000v
```
conf t
router bgp 2
network 10.10.10.0 mask 255.255.255.0
network 1.1.1.0 mask 255.255.255.0
neighbor 10.10.10.1 remote-as 1
neighbor 10.10.10.1 password CISCO-BGP
neighbor 10.10.10.1 activate
no auto-summary
end
wr
```
## R2
```
conf t
interface fa0/0
ip address 10.10.20.2 255.255.255.0
no shutdown
exit
interface fa0/1
ip address 2.2.2.1 255.255.255.0
no shutdown
exit
router ospf 1
network 10.10.20.0 0.0.0.255 area 0
network 2.2.2.0 0.0.0.255 area 0
exit
interface fa0/0
ip ospf authentication message-digest
ip ospf message-digest-key 1 md5 CISCO-O
end
wr
```
## R3
```
conf t
interface fa0/0
ip address 10.10.30.2 255.255.255.0
no shutdown
exit
interface fa0/1
ip address 3.3.3.1 255.255.255.0
no shutdown
exit
key chain EIGRP-KEYS
key 1
key-string CISCO-EIGRP
exit
exit
router eigrp 1
network 10.10.30.0 0.0.0.255
network 3.3.3.0 0.0.0.255
no auto-summary
exit
interface fa0/0
ip authentication mode eigrp 1 md5
ip authentication key-chain eigrp 1 EIGRP-KEYS
end
wr
```
# ASDM 
## OSPF
```
router ospf 1
 log-adj-changes
 area 0
 area 0 authentication message-digest
 network 10.10.20.0 255.255.255.0 area 0
interface GigabitEthernet0/1
 ospf authentication message-digest
 ospf cost 10
 ospf message-digest-key 1 md5 CISCO-O
```
## EIRGP
```
router eigrp 1
router eigrp 1
 network 10.10.30.0 255.255.255.0
interface GigabitEthernet0/2
 authentication mode eigrp 1 md5
 authentication key eigrp 1 CISCO-EIGRP key-id 1
```
## BGP
```
router bgp 1
 address-family ipv4
  network 10.10.10.0 mask 255.255.255.0
  neighbor 10.10.10.2 remote-as 2
  neighbor 10.10.10.2 password CISCO-BGP
  exit-address-family
```
## Redistribución
```
router eigrp 1
router eigrp 1
 redistribute ospf 1 metric 1544 15 255 255 1500
 redistribute bgp 1 metric 1544 15 255 255 1500
 redistribute static metric 1544 15 255 255 1500
 redistribute connected metric 1544 15 255 255 1500
router ospf 1
 redistribute eigrp 1 metric 1 subnets
 redistribute static metric 1 subnets
 redistribute connected metric 1 subnets
 redistribute bgp 1 metric 1 subnets
router bgp 1
 address-family ipv4
  redistribute static metric 20
  redistribute connected metric 20
  redistribute eigrp 1 metric 20
  redistribute ospf 1 metric 20
  exit-address-family
```
