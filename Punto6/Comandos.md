# ASDM Políticas
```
access-list INSIDE_access_in line 1 remark INSIDE-TO-ANY
access-list INSIDE_access_in line 2 extended permit ip host 2.2.2.2 any
access-list DMZ_access_in line 1 remark DMZ-TO-OUTSIDE
access-list DMZ_access_in line 2 extended permit tcp host 3.3.3.2 host 1.1.1.2 eq ssh
access-list MGMT-IN line 2 remark PC-TO-MGMT
access-list MGMT-IN line 3 extended permit tcp host 192.168.107.1 host 10.10.10.2 eq ssh
access-list MGMT-IN line 4 remark PC-TO-MGMT
access-list MGMT-IN line 5 extended permit tcp host 192.168.107.1 host 3.3.3.2 eq ssh
access-list MGMT-IN line 6 remark PC-TO-MGMT
access-list MGMT-IN line 7 extended permit tcp host 192.168.107.1 host 3.3.3.2 eq https
access-group DMZ_access_in in interface DMZ
access-group INSIDE_access_in in interface INSIDE
```
