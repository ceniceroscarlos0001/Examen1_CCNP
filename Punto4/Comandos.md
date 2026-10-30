# CLI
## CSR1000v

```
conf t
restconf
ip http server
ip http authentication local
ip http secure-server
ip http client source-interface gi4
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
access-list MGMT-IN extended permit ip any host <ASA-DHCP-IP> -> 192.168.42.126
access-group MGMT-IN in interface MGMT
end
wr
```
# Postman

## CSR1000v
### PATCH https://192.168.42.183/restconf/data/Cisco-IOS-XE-native:native/interface/
```
<interface>
  <GigabitEthernet>
    <name>1</name>
    <description>CSR-TO-ASA</description>
    <shutdown xmlns:nc="urn:ietf:params:xml:ns:netconf:base:1.0" nc:operation="delete"/>
    <ip>
      <address>
        <primary>
          <address>10.10.10.2</address>
          <mask>255.255.255.0</mask>
        </primary>
      </address>
    </ip>
  </GigabitEthernet>
</interface>
```

## ASA
### POST https://192.168.42.126/api/cli
```
{
  "commands": [
    "interface GigabitEthernet0/0",
    "nameif OUTSIDE",
    "security-level 0",
    "ip address 10.10.10.1 255.255.255.0",
    "no shutdown",
    "exit",
    "interface GigabitEthernet0/1",
    "nameif INSIDE",
    "security-level 100",
    "ip address 10.10.20.1 255.255.255.0",
    "no shutdown",
    "exit",
    "interface GigabitEthernet0/2",
    "nameif DMZ",
    "security-level 50",
    "ip address 10.10.30.1 255.255.255.0",
    "no shutdown"
  ]
}
```
