# Device Configuration Guide

## Router Configuration

### Basic Configuration
```
enable
configure terminal
hostname Office-Router
no ip domain-lookup
enable secret cisco123

! Console password
line console 0
password cisco
login
logging synchronous

! VTY password for Telnet/SSH
line vty 0 4
password cisco
login
transport input all
exit

! Banner
banner motd #Unauthorized access is prohibited#
```

### Interface Configuration
```
! WAN Interface (to ISP)
interface GigabitEthernet0/0
description WAN Connection to ISP
ip address 203.0.113.2 255.255.255.252
no shutdown

! LAN Interface (to Distribution Switch)
interface GigabitEthernet0/1
description LAN Connection to Distribution Switch
no ip address
no shutdown

! Sub-interfaces for VLAN routing (Router-on-a-stick)
interface GigabitEthernet0/1.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
description Management VLAN

interface GigabitEthernet0/1.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
description Sales VLAN

interface GigabitEthernet0/1.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
description IT VLAN

interface GigabitEthernet0/1.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
description HR VLAN

interface GigabitEthernet0/1.50
encapsulation dot1Q 50
ip address 192.168.50.1 255.255.255.0
description Guest VLAN

interface GigabitEthernet0/1.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
description Server VLAN
```

### NAT Configuration
```
! Define inside and outside interfaces
interface GigabitEthernet0/0
ip nat outside

interface GigabitEthernet0/1.10
ip nat inside

interface GigabitEthernet0/1.20
ip nat inside

interface GigabitEthernet0/1.30
ip nat inside

interface GigabitEthernet0/1.40
ip nat inside

interface GigabitEthernet0/1.50
ip nat inside

interface GigabitEthernet0/1.99
ip nat inside

! Access list for NAT
access-list 1 permit 192.168.0.0 0.0.255.255

! NAT overload (PAT)
ip nat inside source list 1 interface GigabitEthernet0/0 overload
```

### DHCP Configuration
```
! DHCP for Management VLAN
ip dhcp pool VLAN10-POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.99.10 8.8.8.8

! DHCP for Sales VLAN
ip dhcp pool VLAN20-POOL
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.99.10 8.8.8.8

! DHCP for IT VLAN
ip dhcp pool VLAN30-POOL
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 192.168.99.10 8.8.8.8

! DHCP for HR VLAN
ip dhcp pool VLAN40-POOL
network 192.168.40.0 255.255.255.0
default-router 192.168.40.1
dns-server 192.168.99.10 8.8.8.8

! DHCP for Guest VLAN
ip dhcp pool VLAN50-POOL
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1
dns-server 8.8.8.8

! Excluded addresses (gateways and servers)
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp excluded-address 192.168.50.1 192.168.50.10
ip dhcp excluded-address 192.168.99.1 192.168.99.20
```

### Routing Configuration
```
! Default route to ISP
ip route 0.0.0.0 0.0.0.0 203.0.113.1
```

### Access Control Lists (Security)
```
! Guest VLAN ACL - restrict access to internal networks
ip access-list extended GUEST-RESTRICT
deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.99.0 0.0.0.255
permit ip any any

! Apply to Guest VLAN interface
interface GigabitEthernet0/1.50
ip access-group GUEST-RESTRICT in
```

### Save Configuration
```
end
write memory
```

---

## Distribution Switch Configuration

### Basic Configuration
```
enable
configure terminal
hostname Dist-Switch
no ip domain-lookup
enable secret cisco123

line console 0
password cisco
login
logging synchronous

line vty 0 15
password cisco
login
exit

banner motd #Unauthorized access is prohibited#
```

### VLAN Configuration
```
! Create VLANs
vlan 10
name Management
vlan 20
name Sales
vlan 30
name IT
vlan 40
name HR
vlan 50
name Guest
vlan 99
name Servers
exit
```

### Trunk Configuration
```
! Trunk to Router
interface GigabitEthernet0/1
description Trunk to Router
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Trunk to Access Switch 1
interface GigabitEthernet0/2
description Trunk to Access-Switch-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Trunk to Access Switch 2
interface FastEthernet0/1
description Trunk to Access-Switch-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Trunk to Access Switch 3
interface FastEthernet0/2
description Trunk to Access-Switch-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
```

### Save Configuration
```
end
write memory
```

---

## Access Switch Configuration

### Access Switch 1 Configuration
```
enable
configure terminal
hostname Access-Switch-1
no ip domain-lookup
enable secret cisco123

line console 0
password cisco
login
logging synchronous

line vty 0 15
password cisco
login
exit

! Create VLANs
vlan 10
name Management
vlan 20
name Sales
vlan 30
name IT
vlan 40
name HR
vlan 50
name Guest
vlan 99
name Servers

! Trunk port to Distribution Switch
interface GigabitEthernet0/1
description Trunk to Distribution Switch
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Access ports for Management VLAN
interface range FastEthernet0/1-5
description Management Department
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown

! Access ports for Sales VLAN
interface range FastEthernet0/6-10
description Sales Department
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown

end
write memory
```

### Access Switch 2 Configuration
```
enable
configure terminal
hostname Access-Switch-2
no ip domain-lookup
enable secret cisco123

line console 0
password cisco
login
logging synchronous

line vty 0 15
password cisco
login
exit

! Create VLANs
vlan 10
name Management
vlan 20
name Sales
vlan 30
name IT
vlan 40
name HR
vlan 50
name Guest
vlan 99
name Servers

! Trunk port to Distribution Switch
interface FastEthernet0/24
description Trunk to Distribution Switch
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Access ports for IT VLAN
interface range FastEthernet0/1-8
description IT Department
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown

! Access ports for Server VLAN
interface range FastEthernet0/9-12
description Server Farm
switchport mode access
switchport access vlan 99
no shutdown

end
write memory
```

### Access Switch 3 Configuration
```
enable
configure terminal
hostname Access-Switch-3
no ip domain-lookup
enable secret cisco123

line console 0
password cisco
login
logging synchronous

line vty 0 15
password cisco
login
exit

! Create VLANs
vlan 10
name Management
vlan 20
name Sales
vlan 30
name IT
vlan 40
name HR
vlan 50
name Guest
vlan 99
name Servers

! Trunk port to Distribution Switch
interface FastEthernet0/24
description Trunk to Distribution Switch
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown

! Access ports for HR VLAN
interface range FastEthernet0/1-8
description HR Department
switchport mode access
switchport access vlan 40
switchport port-security
switchport port-security maximum 2
switchport port-security violation shutdown
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown

! Access ports for Guest VLAN
interface range FastEthernet0/9-16
description Guest Network
switchport mode access
switchport access vlan 50
switchport port-security
switchport port-security maximum 3
spanning-tree portfast
no shutdown

end
write memory
```

---

## Server Configuration

### DNS Server (192.168.99.10)
- **IP Address**: 192.168.99.10
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.99.1
- **DNS Service**: Enable DNS service
- **Domain**: office.local

### File Server (192.168.99.11)
- **IP Address**: 192.168.99.11
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.99.1
- **Services**: File sharing (FTP/HTTP)

### Web Server (192.168.99.12)
- **IP Address**: 192.168.99.12
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.99.1
- **Services**: HTTP/HTTPS

---

## End Device Configuration (Example)

### PC Configuration (DHCP)
- **IP Configuration**: DHCP
- **DNS**: Automatic from DHCP

### PC Configuration (Static - for testing)
- **IP Address**: 192.168.X.X (based on VLAN)
- **Subnet Mask**: 255.255.255.0
- **Default Gateway**: 192.168.X.1
- **DNS Server**: 192.168.99.10, 8.8.8.8
