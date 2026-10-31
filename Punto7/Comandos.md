# CLI
## R1
```
conf t
username cisco privilege 15 password cisco123
crypto key generate rsa general-keys label SSH-KEYS modulus 1024
ip ssh rsa keypair-name SSH-KEYS
ip ssh version 2
line vty 0 4
transport input ssh
login local
exit
ip route 0.0.0.0 0.0.0.0 1.1.1.1
end
wr
```
## CSR
```
conf t
ip domain name fi.unam.mx
username cisco privilege 15 password cisco123
crypto key generate rsa general-keys label SSH-KEYS modulus 2048
ip ssh rsa keypair-name SSH-KEYS
ip ssh version 2
line vty 0 15
transport input ssh
login local
end
wr
```
## R2
```
conf t
ip route 0.0.0.0 0.0.0.0 10.10.20.1
end
wr
```
## ASA
```
conf t
crypto key generate rsa general-keys modulus 2048
aaa authentication ssh console LOCAL
ssh version 2
ssh 0.0.0.0 0.0.0.0 MGMT
ssh key-exchange group dh-group14-sha1
object network OBJ-CSR-GI1
host 10.10.10.2
exit
object network OBJ-WORDPRESS
host 3.3.3.2
exit
object service OBJ-TCP-1010
service tcp destination eq 1010
exit
object service OBJ-TCP-1020
service tcp destination eq 1020
exit
object service OBJ-TCP-1030
service tcp destination eq 1030
exit
object service OBJ-TCP-22
service tcp destination eq 22
exit
object service OBJ-TCP-443
service tcp destination eq 443
exit
nat (MGMT,OUTSIDE) source dynamic any interface destination static interface OBJ-CSR-GI1 service OBJ-TCP-1010 OBJ-TCP-22
nat (MGMT,DMZ) source static any any destination static interface OBJ-WORDPRESS service OBJ-TCP-1020 OBJ-TCP-22
nat (MGMT,DMZ) source static any any destination static interface OBJ-WORDPRESS service OBJ-TCP-1030 OBJ-TCP-443
object network OBJ-PAT-INSIDE
subnet 0.0.0.0 0.0.0.0
nat (INSIDE,MGMT) dynamic interface
exit
object network OBJ-PAT-DMZ
subnet 0.0.0.0 0.0.0.0
nat (DMZ,MGMT) dynamic interface
end
wr
```
