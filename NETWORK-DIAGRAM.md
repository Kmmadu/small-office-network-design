# Network Diagram

## Logical Topology

```
                                 INTERNET
                                    |
                              (ISP Cloud)
                                    |
                            203.0.113.1/30
                                    |
                        +-----------+-----------+
                        |    Office Router      |
                        |   (Cisco 2911)        |
                        | 203.0.113.2 (WAN)     |
                        | Router-on-a-Stick     |
                        +-----------+-----------+
                                    | G0/1 (Trunk)
                                    | VLANs: 10,20,30,40,50,99
                        +-----------+-----------+
                        | Distribution Switch   |
                        |   (Cisco 3560)        |
                        |   Layer 2 Switch      |
                        +-----------+-----------+
                                    |
                    +---------------+---------------+
                    |               |               |
                 Trunk           Trunk           Trunk
                    |               |               |
        +-----------+---+ +---------+--------+ +---+-----------+
        | Access-SW-1   | | Access-SW-2      | | Access-SW-3   |
        | (Cisco 2960)  | | (Cisco 2960)     | | (Cisco 2960)  |
        +-------+-------+ +-------+----------+ +-------+-------+
                |                 |                    |
         +------+------+   +------+------+     +------+------+
         |             |   |             |     |             |
    VLAN 10       VLAN 20  VLAN 30   VLAN 99   VLAN 40   VLAN 50
    (Mgmt)        (Sales)  (IT)      (Servers) (HR)      (Guest)
         |             |   |             |     |             |
      +--+--+       +--+--+ +--+--+   +--+--+  +--+--+    +--+--+
      | PC  |       | PC  | | PC  |   |File |  | PC  |    |Guest|
      +-----+       +-----+ +-----+   |Serv |  +-----+    | PC  |
      | PC  |       | PC  | | PC  |   +-----+  | PC  |    +-----+
      +-----+       +-----+ +-----+   | DNS |  +-----+    |Guest|
                    |Print| |Print|   |Serv |  |Print|    | PC  |
                    +-----+ +-----+   +-----+  +-----+    +-----+
                                      | Web |
                                      |Serv |
                                      +-----+
```

## Physical Topology Details

### Network Segments

#### Core/Edge Layer
- **Router**: Office-Router (Cisco 2911)
  - G0/0: WAN connection to ISP
  - G0/1: Trunk to Distribution Switch
  - Subinterfaces for inter-VLAN routing

#### Distribution Layer
- **Distribution Switch**: Dist-Switch (Cisco 3560)
  - G0/1: Trunk to Router
  - G0/2: Trunk to Access-Switch-1
  - Fa0/1: Trunk to Access-Switch-2
  - Fa0/2: Trunk to Access-Switch-3

#### Access Layer
- **Access-Switch-1**: Management and Sales
  - Fa0/1-5: VLAN 10 (Management)
  - Fa0/6-10: VLAN 20 (Sales)
  
- **Access-Switch-2**: IT and Servers
  - Fa0/1-8: VLAN 30 (IT)
  - Fa0/9-12: VLAN 99 (Servers)
  
- **Access-Switch-3**: HR and Guest
  - Fa0/1-8: VLAN 40 (HR)
  - Fa0/9-16: VLAN 50 (Guest)

## VLAN Assignment Map

| VLAN ID | Name | Subnet | Gateway | Connected Switch | Ports |
|---------|------|--------|---------|------------------|-------|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | Access-SW-1 | Fa0/1-5 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 | Access-SW-1 | Fa0/6-10 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | Access-SW-2 | Fa0/1-8 |
| 40 | HR | 192.168.40.0/24 | 192.168.40.1 | Access-SW-3 | Fa0/1-8 |
| 50 | Guest | 192.168.50.0/24 | 192.168.50.1 | Access-SW-3 | Fa0/9-16 |
| 99 | Servers | 192.168.99.0/24 | 192.168.99.1 | Access-SW-2 | Fa0/9-12 |

## IP Address Assignments

### Network Devices

| Device | Interface | IP Address | Description |
|--------|-----------|------------|-------------|
| Office-Router | G0/0 | 203.0.113.2/30 | WAN to ISP |
| Office-Router | G0/1.10 | 192.168.10.1/24 | Management Gateway |
| Office-Router | G0/1.20 | 192.168.20.1/24 | Sales Gateway |
| Office-Router | G0/1.30 | 192.168.30.1/24 | IT Gateway |
| Office-Router | G0/1.40 | 192.168.40.1/24 | HR Gateway |
| Office-Router | G0/1.50 | 192.168.50.1/24 | Guest Gateway |
| Office-Router | G0/1.99 | 192.168.99.1/24 | Server Gateway |
| ISP Gateway | - | 203.0.113.1 | ISP Router |

### Server Assignments

| Server | IP Address | Subnet Mask | Gateway | Services |
|--------|------------|-------------|---------|----------|
| DNS Server | 192.168.99.10 | 255.255.255.0 | 192.168.99.1 | DNS |
| File Server | 192.168.99.11 | 255.255.255.0 | 192.168.99.1 | FTP, File Sharing |
| Web Server | 192.168.99.12 | 255.255.255.0 | 192.168.99.1 | HTTP/HTTPS |

### End Devices (Example Assignments)

| Department | Sample IPs | DHCP Range | Static Reserved |
|------------|------------|------------|-----------------|
| Management | 192.168.10.11-20 | 192.168.10.11-254 | .1-.10 |
| Sales | 192.168.20.11-20 | 192.168.20.11-254 | .1-.10 |
| IT | 192.168.30.11-20 | 192.168.30.11-254 | .1-.10 |
| HR | 192.168.40.11-20 | 192.168.40.11-254 | .1-.10 |
| Guest | 192.168.50.11-20 | 192.168.50.11-254 | .1-.10 |

## Cable Types

| Connection Type | Cable Used | Purpose |
|-----------------|------------|---------|
| Router to Distribution Switch | Copper Straight-Through | Trunk Link |
| Switch to Switch | Copper Straight-Through | Trunk Link |
| Switch to PC | Copper Straight-Through | Access Link |
| Router to ISP | Serial DCE/DTE or Copper | WAN Link |
| PC to PC (direct) | Copper Cross-Over | Direct Connection |

## Port Configurations

### Trunk Ports (802.1Q)
- Router G0/1 ↔ Dist-Switch G0/1
- Dist-Switch G0/2 ↔ Access-SW-1 G0/1
- Dist-Switch Fa0/1 ↔ Access-SW-2 Fa0/24
- Dist-Switch Fa0/2 ↔ Access-SW-3 Fa0/24

### Access Ports
- All end-device connections
- Configured with specific VLAN assignment
- Port security enabled on most ports

## Traffic Flow Examples

### Example 1: PC in Sales VLAN accessing Web Server
```
PC (VLAN 20) → Access-SW-1 → Dist-Switch → Router (Inter-VLAN) 
→ Dist-Switch → Access-SW-2 → Web Server (VLAN 99)
```

### Example 2: PC in Management accessing Internet
```
PC (VLAN 10) → Access-SW-1 → Dist-Switch → Router (NAT) 
→ ISP → Internet
```

### Example 3: Guest PC attempting to access File Server (Blocked)
```
Guest PC (VLAN 50) → Access-SW-3 → Dist-Switch → Router 
→ ACL Blocks → No access to VLAN 99
```

## Security Zones

```
┌─────────────────────────────────────────────────┐
│         DMZ / Public Zone (Future)              │
└─────────────────────────────────────────────────┘
                      ↑
┌─────────────────────────────────────────────────┐
│              Trusted Zone                        │
│  - Management VLAN (10)                         │
│  - Sales VLAN (20)                              │
│  - IT VLAN (30)                                 │
│  - HR VLAN (40)                                 │
│  - Server VLAN (99)                             │
└─────────────────────────────────────────────────┘
                      ↑
┌─────────────────────────────────────────────────┐
│          Restricted Zone                        │
│  - Guest VLAN (50) - Internet Only              │
└─────────────────────────────────────────────────┘
```

## Scalability Plan

### Phase 1: Current Implementation (30-40 users)
- Single router with router-on-a-stick
- 3 access switches
- 6 VLANs

### Phase 2: Growth (50-75 users)
- Add additional access switches
- Consider Layer 3 switch for distribution
- Add redundancy with second router

### Phase 3: Expansion (100+ users)
- Implement hierarchical three-layer design
- Add core layer switches
- Implement redundant links and HSRP
- Add wireless access points and controller

## Legend

```
[Router]     = Layer 3 routing device
[Switch]     = Layer 2 switching device  
[PC]         = End-user workstation
[Server]     = Server device
――――――――      = Physical connection
- - - - -     = Logical/Virtual connection (VLAN)
(Trunk)      = 802.1Q trunk carrying multiple VLANs
(Access)     = Access port for single VLAN
```
