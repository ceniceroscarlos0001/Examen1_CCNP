# Punto 2: Direccionamiento y acceso desde fuera de GNS3
## R1
conf t
interface fa0/0
ip address 1.1.1.2 255.255.255.0
no shutdown
end
wr
## CSR1000v
conf t
interface GigabitEthernet2
ip address 1.1.1.1 255.255.255.0
no shutdown
exit
interface GigabitEthernet4
ip address dhcp
no shutdown
end

Ruta por defecto (con el gateway obtenido por DHCP):
conf t
ip route 0.0.0.0 0.0.0.0 <DHCP-GW>
end
wr
```

Documentación:
```
show ip interface brief
show ip route
```

## ASA

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
```

Ruta por defecto (con el gateway obtenido por DHCP):
```
conf t
route MGMT 0 0 <DHCP-GW>
end
wr
```

Documentación:
```
show ip
show route
show runn same-security-traffic
```

## Hosts

### WebTerm
| Parámetro | Valor |
|---|---|
| Interface | eth0 |
| Use DHCP | no |
| IP Address | 2.2.2.2 |
| Network Mask | 255.255.255.0 |
| Broadcast | 2.2.2.255 |
| Gateway | 2.2.2.1 |
| NameServers | 8.8.8.8 |

### WordPress (archivo interfaces)
```
auto eth0
iface eth0 inet static
    address 3.3.3.2
    netmask 255.255.255.0
    gateway 3.3.3.1
```

## Acceso desde fuera de GNS3

### VM de GNS3 (desactivar iptables)
```
sudo -i
iptables -P INPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT
iptables -t mangle -F
iptables -F
iptables -X
```

### Computadora física (CMD como administrador)
```
route add <RED-DHCP-NAT> mask 255.255.255.0 <IP-VM-GNS3>
```

### Validación de conectividad
```
ping <IP-DHCP-CSR>
ping <IP-DHCP-ASA>
```
```

Reemplaza `<DHCP-GW>`, `<RED-DHCP-NAT>`, `<IP-VM-GNS3>`, `<IP-DHCP-CSR>` e `<IP-DHCP-ASA>` con tus valores reales. Verifica también que las IPs de R1, CSR, WebTerm y WordPress coincidan con tu PDF.
