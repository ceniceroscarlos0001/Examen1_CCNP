## Equipos Red
### R1
```
conf t
interface fa0/0
ip address 1.1.1.2 255.255.255.0
no shutdown
end
wr
```
### CSR1000v
```
conf t
interface GigabitEthernet2
ip address 1.1.1.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet4
ip address dhcp
no shutdown
end
conf t
ip route 0.0.0.0 0.0.0.0 <DHCP-GW> #192.168.42.27
end
wr
```
### ASA
```
conf t
interface management0/0
nameif MGMT
security-level 100
ip address dhcp
no shutdown
exit
same-security-traffic permit inter-interface
end
conf t
route MGMT 0 0 <DHCP-GW> #192.168.42.27
end
wr
```

## Hosts
### WebTerm
```
auto eth0
iface eth0 inet static
	address 2.2.2.2
	netmask 255.255.255.0
	gateway 2.2.2.1
```

### WordPress (archivo interfaces)
```
auto eth0
iface eth0 inet static
    address 3.3.3.2
    netmask 255.255.255.0
    gateway 3.3.3.1
```
