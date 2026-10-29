# Punto 4 (CLI): Habilitar CSR1000v y ASA para administración con Postman

## CSR1000v

```
conf t
restconf
ip http server
ip http authentication local
ip http secure-server
ip http client source-interface GigabitEthernet4
username cisco privilege 15 password cisco123
end
wr
```

## ASA

```
conf t
rest-api image boot:/asa-restapi-132-lfbff-k8.SPA
rest-api agent
username cisco password cisco123 privilege 15
aaa authentication http console LOCAL
http server enable
http 0 0 MGMT
access-list MGMT-IN extended permit ip any host <ASA-DHCP-IP>
access-group MGMT-IN in interface MGMT
end
wr
```
