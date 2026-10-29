## R1

```
clock set 10:00:00 29 October 2026
conf t
ntp master
ntp authentication-key 10 md5 cisco123
ntp trusted-key 10
ip domain-name ca.fi.unam.mx
ip http server
crypto key generate rsa general-keys label CAKEYS modulus 2048 exportable
crypto pki trustpoint CA
rsakeypair CAKEYS
exit
crypto pki server CA
issuer-name cn=ca.fi.unam.mx
grant auto
database archive pem
no shutdown
Password: cisco123
Re-enter password: cisco123
end
wr
```

## CSR1000v

```
conf t
ntp authenticate
ntp authentication-key 10 md5 cisco123
ntp trusted-key 10
ntp server 1.1.1.2 key 10
crypto pki trustpoint CA
enrollment url http://1.1.1.2:80
subject-name cn=csr1000.fi.unam.mx
exit
crypto pki authenticate CA
crypto pki enroll CA
Password: cisco123
Re-enter password: cisco123
end
wr
```
