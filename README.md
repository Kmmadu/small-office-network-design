# Small Office Network Design

This project provides a complete small office network design and implementation using Cisco Packet Tracer. It demonstrates best practices for network segmentation, security, and scalability for a business with 30-40 employees.

## Overview

This network design includes:
- **Multi-VLAN segmentation** for department isolation
- **Router-on-a-stick** configuration for inter-VLAN routing
- **NAT/PAT** for Internet connectivity
- **DHCP** for automatic IP address assignment
- **Port security** to prevent unauthorized access
- **Guest network isolation** from internal resources
- **Hierarchical design** supporting future growth

## Network Architecture

The network uses a hierarchical design with:
- **1 Edge Router** (Cisco 2911) - Provides Internet connectivity, NAT, and inter-VLAN routing
- **1 Distribution Switch** (Cisco 3560) - Central aggregation point
- **3 Access Switches** (Cisco 2960) - End-user connectivity
- **Multiple VLANs** - Department segmentation (Management, Sales, IT, HR, Guest, Servers)

## Quick Start

1. **Install Cisco Packet Tracer** (version 7.3 or later)
2. **Download** the `.pkt` file (when available) or build using the configuration files
3. **Review** the documentation in this repository:
   - `DESIGN.md` - Detailed network design specifications
   - `CONFIGURATIONS.md` - Device configuration commands
   - `NETWORK-DIAGRAM.md` - Network topology and IP addressing
   - `TESTING.md` - Testing procedures and troubleshooting
   - `PACKET-TRACER-GUIDE.md` - Step-by-step build instructions

## Network Specifications

### VLANs and Subnets

| VLAN | Department | Network | Purpose |
|------|------------|---------|---------|
| 10 | Management | 192.168.10.0/24 | Network administrators and IT management |
| 20 | Sales | 192.168.20.0/24 | Sales department staff |
| 30 | IT/Technical | 192.168.30.0/24 | IT staff and developers |
| 40 | HR | 192.168.40.0/24 | Human resources department |
| 50 | Guest | 192.168.50.0/24 | Visitors (isolated from internal network) |
| 99 | Servers | 192.168.99.0/24 | File, DNS, and Web servers |

### Key Features

- **Security**: 
  - VLANs isolate departments
  - Guest VLAN blocked from accessing internal networks
  - Port security prevents unauthorized devices
  - Access Control Lists (ACLs) control traffic flow

- **Scalability**:
  - Room for 254 hosts per VLAN
  - Easy to add new switches and VLANs
  - Hierarchical design supports growth

- **Services**:
  - DHCP for automatic IP configuration
  - DNS for name resolution
  - NAT for Internet access
  - Inter-VLAN routing for department communication

## Documentation

- **[DESIGN.md](DESIGN.md)** - Complete network design documentation including requirements, topology, IP addressing, and security policies
- **[CONFIGURATIONS.md](CONFIGURATIONS.md)** - Detailed device configurations for router, switches, and servers
- **[NETWORK-DIAGRAM.md](NETWORK-DIAGRAM.md)** - Visual topology, VLAN assignments, and traffic flow
- **[TESTING.md](TESTING.md)** - Comprehensive testing procedures and troubleshooting guide
- **[PACKET-TRACER-GUIDE.md](PACKET-TRACER-GUIDE.md)** - Step-by-step instructions for building in Packet Tracer

## Building the Network

### Option 1: Use Pre-built File
Download and open `small-office-network.pkt` in Cisco Packet Tracer (when available)

### Option 2: Build from Scratch
Follow the step-by-step instructions in `PACKET-TRACER-GUIDE.md` to:
1. Place devices in the workspace
2. Connect devices with appropriate cables
3. Configure each device using commands from `CONFIGURATIONS.md`
4. Test connectivity using procedures in `TESTING.md`

## Testing the Network

The `TESTING.md` file includes 11 comprehensive tests:
1. Basic connectivity within same VLAN
2. Inter-VLAN routing
3. Guest VLAN isolation
4. DHCP functionality
5. Internet connectivity (NAT)
6. DNS resolution
7. Trunk links
8. VLAN configuration
9. Port security
10. Routing table verification
11. End-to-end connectivity

## Use Cases

This network design is suitable for:
- Small to medium businesses (30-100 employees)
- Educational demonstrations of enterprise networking
- Network certification training (CCNA level)
- Understanding VLAN segmentation and security
- Learning router and switch configuration

## Technologies Demonstrated

- **VLANs** (Virtual Local Area Networks)
- **802.1Q Trunking**
- **Router-on-a-Stick** inter-VLAN routing
- **DHCP** (Dynamic Host Configuration Protocol)
- **NAT/PAT** (Network Address Translation / Port Address Translation)
- **ACLs** (Access Control Lists)
- **Port Security**
- **Hierarchical Network Design**

## Requirements

- **Software**: Cisco Packet Tracer 7.3 or later (free from Cisco Networking Academy)
- **Knowledge Level**: Basic to intermediate networking concepts
- **Time**: 2-4 hours to build from scratch, 30 minutes to test

## Learning Objectives

After completing this project, you will understand:
- How to design a hierarchical network
- How to implement VLANs for network segmentation
- How to configure inter-VLAN routing
- How to set up DHCP and NAT services
- How to implement basic network security
- How to test and troubleshoot network connectivity

## Future Enhancements

Potential expansions for this design:
- Add wireless access points and controller
- Implement redundancy with HSRP/VRRP
- Add Layer 3 switch for inter-VLAN routing
- Configure dynamic routing (OSPF/EIGRP)
- Implement VPN for remote access
- Add intrusion detection/prevention
- Set up centralized logging and monitoring

## Contributing

Feel free to submit issues or pull requests if you have suggestions for improving this network design.

## License

This project is for educational purposes.

## Author

Network design and documentation created as a demonstration of small office network best practices.

## References

- Cisco Networking Academy
- Cisco CCNA Certification Guide
- Network Design Best Practices
