# Small Office Network Design Documentation

## 1. Network Requirements

This small office network is designed for a business with approximately 30-40 employees across different departments. The network requirements include:

- **Security**: Separate VLANs for different departments
- **Connectivity**: Internet access for all users
- **Scalability**: Room for expansion
- **Reliability**: Redundancy where critical
- **Management**: Centralized network management

## 2. Network Topology

### 2.1 Network Architecture
The network follows a hierarchical three-layer design:
- **Core Layer**: Central router connecting to ISP
- **Distribution Layer**: Layer 3 switch for inter-VLAN routing
- **Access Layer**: Layer 2 switches for end-user connectivity

### 2.2 Physical Layout
```
Internet (ISP)
     |
  Router
     |
Layer 3 Switch (Distribution)
     |
   / | \
  /  |  \
Switch1 Switch2 Switch3 (Access Layer)
  |      |      |
Users  Users  Users
```

## 3. IP Addressing Scheme

### 3.1 Network Segments

| VLAN | Department | Network | Subnet Mask | Gateway | Host Range |
|------|------------|---------|-------------|---------|------------|
| 10 | Management | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1 | 192.168.10.2-254 |
| 20 | Sales | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1 | 192.168.20.2-254 |
| 30 | IT/Technical | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1 | 192.168.30.2-254 |
| 40 | HR | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1 | 192.168.40.2-254 |
| 50 | Guest | 192.168.50.0/24 | 255.255.255.0 | 192.168.50.1 | 192.168.50.2-254 |
| 99 | Server | 192.168.99.0/24 | 255.255.255.0 | 192.168.99.1 | 192.168.99.2-254 |

### 3.2 WAN Connection
- **ISP Connection**: 203.0.113.0/30
- **Router External IP**: 203.0.113.2
- **ISP Gateway**: 203.0.113.1

## 4. Device Specifications

### 4.1 Network Devices

#### Router (Edge Router)
- **Model**: Cisco 2911 (or equivalent in Packet Tracer)
- **Purpose**: Connect to ISP, NAT, DHCP relay, ACLs
- **Interfaces**:
  - GigabitEthernet0/0: WAN (ISP Connection)
  - GigabitEthernet0/1: LAN (to Distribution Switch)

#### Layer 3 Switch (Distribution Switch)
- **Model**: Cisco 3560 (or equivalent in Packet Tracer)
- **Purpose**: Inter-VLAN routing, VLAN management
- **VLANs**: 10, 20, 30, 40, 50, 99

#### Access Layer Switches (3 units)
- **Model**: Cisco 2960 (or equivalent in Packet Tracer)
- **Purpose**: End-user connectivity
- **Configuration**: Access ports for end devices

### 4.2 End Devices
- **PCs**: Windows/Linux workstations
- **Laptops**: Mobile devices for employees
- **Printers**: Network printers in each department
- **Servers**: File server, Web server, DNS server

## 5. Network Services

### 5.1 DHCP
- DHCP pools configured on the router for each VLAN
- Static IP assignments for servers and network devices

### 5.2 DNS
- Internal DNS server: 192.168.99.10
- External DNS: 8.8.8.8 (Google DNS)

### 5.3 NAT
- PAT (Port Address Translation) configured on the edge router
- Internal networks translated to external IP

### 5.4 Security Features
- VLANs for network segmentation
- Access Control Lists (ACLs) to restrict inter-VLAN traffic
- Port security on access switches
- Guest VLAN isolated from internal network

## 6. VLAN Configuration

### 6.1 VLAN Assignments
- **VLAN 10 (Management)**: Network administrators, IT management
- **VLAN 20 (Sales)**: Sales department staff
- **VLAN 30 (IT/Technical)**: IT staff, developers
- **VLAN 40 (HR)**: Human resources department
- **VLAN 50 (Guest)**: Visitors and guest devices
- **VLAN 99 (Server)**: File servers, web servers, DNS

### 6.2 Trunk Links
- Router to Distribution Switch: Trunk (802.1Q)
- Distribution Switch to Access Switches: Trunk (802.1Q)

## 7. Routing Configuration

### 7.1 Static Routing
- Default route on router pointing to ISP
- Static routes for specific subnets if needed

### 7.2 Inter-VLAN Routing
- Configured on Layer 3 switch
- Each VLAN has an SVI (Switch Virtual Interface)

## 8. Security Policies

### 8.1 Access Control
- Guest VLAN cannot access internal VLANs
- Management VLAN has access to all VLANs for administration
- HR VLAN has restricted access to other departments
- Server VLAN accessible from internal networks only

### 8.2 Port Security
- MAC address limiting on access ports
- Sticky MAC address learning enabled
- Violation mode: shutdown

## 9. Testing and Verification

### 9.1 Connectivity Tests
- Ping tests between devices in same VLAN
- Ping tests between devices in different VLANs
- Internet connectivity test
- DNS resolution test

### 9.2 Security Tests
- Verify VLAN isolation
- Test ACL rules
- Verify NAT functionality

## 10. Maintenance and Troubleshooting

### 10.1 Common Commands
```
show ip interface brief
show vlan brief
show ip route
show running-config
show interfaces trunk
ping [destination]
traceroute [destination]
```

### 10.2 Documentation Updates
- Keep configuration backups
- Document all changes
- Maintain network diagram
