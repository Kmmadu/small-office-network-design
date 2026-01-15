# Quick Reference Guide

## Network Summary

### VLANs and IP Ranges

| VLAN | Name | Network | Gateway | DHCP Range |
|------|------|---------|---------|------------|
| 10 | Management | 192.168.10.0/24 | 192.168.10.1 | .11-.254 |
| 20 | Sales | 192.168.20.0/24 | 192.168.20.1 | .11-.254 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 | .11-.254 |
| 40 | HR | 192.168.40.0/24 | 192.168.40.1 | .11-.254 |
| 50 | Guest | 192.168.50.0/24 | 192.168.50.1 | .11-.254 |
| 99 | Servers | 192.168.99.0/24 | 192.168.99.1 | Static |

### Device Credentials
- **Enable Secret**: cisco123
- **Console Password**: cisco
- **VTY Password**: cisco

### Server IP Addresses
- **DNS Server**: 192.168.99.10
- **File Server**: 192.168.99.11
- **Web Server**: 192.168.99.12

### WAN Connection
- **Router External IP**: 203.0.113.2/30
- **ISP Gateway**: 203.0.113.1

## Essential Commands

### Router Commands
```bash
# Access privileged mode
enable

# Enter configuration mode
configure terminal

# View running configuration
show running-config

# View IP interfaces
show ip interface brief

# View routing table
show ip route

# View NAT translations
show ip nat translations

# View DHCP bindings
show ip dhcp binding

# View access lists
show access-lists

# Save configuration
write memory
# or
copy running-config startup-config
```

### Switch Commands
```bash
# View VLAN configuration
show vlan brief

# View trunk interfaces
show interfaces trunk

# View specific interface
show interfaces fastethernet 0/1

# View port security
show port-security interface fastethernet 0/1

# View MAC address table
show mac address-table

# View spanning tree
show spanning-tree
```

### Troubleshooting Commands
```bash
# Ping test
ping [destination-ip]

# Extended ping (from router)
ping
# Follow prompts for source, destination, etc.

# Traceroute
traceroute [destination-ip]

# View interface status
show ip interface brief

# Debug (use carefully)
debug ip packet
# Turn off with: no debug all or undebug all

# View logs
show logging
```

## Common Configuration Tasks

### Add a New PC to VLAN 20 (Sales)
1. Connect PC to available port on Access-Switch-1 (Fa0/11-23)
2. Configure switch port:
   ```
   configure terminal
   interface fastethernet 0/11
   switchport mode access
   switchport access vlan 20
   switchport port-security
   switchport port-security maximum 2
   switchport port-security mac-address sticky
   spanning-tree portfast
   no shutdown
   end
   write memory
   ```
3. Configure PC for DHCP
4. Verify connectivity with ping

### Add a New VLAN
1. On Router, create subinterface:
   ```
   configure terminal
   interface gigabitethernet 0/1.60
   encapsulation dot1Q 60
   ip address 192.168.60.1 255.255.255.0
   ip nat inside
   exit
   ```
2. Create DHCP pool:
   ```
   ip dhcp excluded-address 192.168.60.1 192.168.60.10
   ip dhcp pool VLAN60-POOL
   network 192.168.60.0 255.255.255.0
   default-router 192.168.60.1
   dns-server 192.168.99.10
   exit
   ```
3. On all switches, create VLAN:
   ```
   configure terminal
   vlan 60
   name NewDepartment
   exit
   ```
4. Update trunk allowed VLANs
5. Assign ports to new VLAN

### Change Device Password
```bash
enable
configure terminal

# Change enable secret
enable secret new_password

# Change console password
line console 0
password new_password
login
exit

# Change VTY password
line vty 0 4
password new_password
login
exit

end
write memory
```

### Reset Port Security Violation
```bash
enable
configure terminal
interface fastethernet 0/1

# Shutdown and re-enable the interface
shutdown
no shutdown

# Or clear port-security
exit
clear port-security sticky interface fastethernet 0/1
```

## Testing Quick Commands

### Test Same VLAN Connectivity
```bash
# From PC Command Prompt
ping 192.168.X.Y
```

### Test Inter-VLAN Connectivity
```bash
# From PC in VLAN 10
ping 192.168.20.2
```

### Test Internet Connectivity
```bash
# Ping ISP
ping 203.0.113.1

# Ping external DNS
ping 8.8.8.8
```

### Check IP Configuration
```bash
# Windows/PC Command Prompt
ipconfig
ipconfig /all

# Release and renew DHCP
ipconfig /release
ipconfig /renew
```

## Security Notes

### Guest VLAN Restrictions
- Guest VLAN (50) **CANNOT** access:
  - Management VLAN (10)
  - Sales VLAN (20)
  - IT VLAN (30)
  - HR VLAN (40)
  - Server VLAN (99)
- Guest VLAN **CAN** access:
  - Internet only

### Port Security Settings
- **Maximum MAC addresses**: 2 per port
- **Violation mode**: Shutdown
- **MAC address learning**: Sticky
- **Recovery**: Manually re-enable port after violation

## Performance Benchmarks

### Expected Ping RTT
- Same VLAN: < 1 ms
- Different VLAN: < 5 ms
- To Router: < 5 ms
- To ISP: < 10 ms

### Troubleshooting Decision Tree

```
Connection Problem?
│
├─ Can't get DHCP IP?
│  ├─ Check cable connection
│  ├─ Verify VLAN assignment
│  ├─ Check DHCP pool on router
│  └─ Verify excluded addresses
│
├─ Can ping gateway but not other devices?
│  ├─ Check routing configuration
│  ├─ Verify inter-VLAN routing
│  └─ Check ACLs
│
├─ Can ping same VLAN but not different VLAN?
│  ├─ Check router subinterfaces
│  ├─ Verify trunk between switch and router
│  └─ Check VLAN exists on all devices
│
├─ Can ping internal devices but not Internet?
│  ├─ Check default route
│  ├─ Verify NAT configuration
│  └─ Ping ISP gateway
│
└─ Port security violation?
   ├─ Check connected MAC addresses
   ├─ Clear violation: clear port-security
   └─ Re-enable port: shutdown, no shutdown
```

## File Organization

```
small-office-network-design/
├── README.md                  # Project overview
├── DESIGN.md                  # Detailed network design
├── CONFIGURATIONS.md          # Device configurations
├── NETWORK-DIAGRAM.md         # Topology and diagrams
├── PACKET-TRACER-GUIDE.md     # Step-by-step build guide
├── TESTING.md                 # Testing procedures
├── QUICK-REFERENCE.md         # This file
├── .gitignore                 # Git ignore rules
└── small-office-network.pkt   # Packet Tracer file (when created)
```

## Useful Packet Tracer Shortcuts

| Shortcut | Action |
|----------|--------|
| **Ctrl + Z** | Undo |
| **Ctrl + Y** | Redo |
| **Delete** | Delete selected device |
| **Ctrl + R** | Realtime mode |
| **Ctrl + D** | Simulation mode |
| **Alt + Click** | Select specific interface |
| **Ctrl + S** | Save project |

## Common Packet Tracer Symbols

- **Green Triangle**: Interface is up/up
- **Red Triangle**: Interface is down
- **Orange Triangle**: Interface is coming up
- **Lightning Bolt**: Connection tool
- **Magnifying Glass**: Inspect tool
- **Hand**: Move tool

## Next Steps After Setup

1. ✅ Verify all devices are connected
2. ✅ Configure all network devices
3. ✅ Test basic connectivity
4. ✅ Test inter-VLAN routing
5. ✅ Test security features
6. ✅ Document any custom changes
7. 📝 Consider implementing enhancements:
   - Wireless access points
   - Redundant router (HSRP)
   - Layer 3 switch for routing
   - VPN configuration
   - QoS policies

## Getting Help

- Review `TESTING.md` for comprehensive test procedures
- Check `CONFIGURATIONS.md` for complete command reference
- See `DESIGN.md` for design rationale and specifications
- Follow `PACKET-TRACER-GUIDE.md` for build instructions

## Important Reminders

⚠️ **Always save configurations after changes**: `write memory`

⚠️ **Test incrementally**: Don't configure everything before testing

⚠️ **Use simulation mode**: To visualize packet flow in Packet Tracer

⚠️ **Backup your work**: Save multiple copies of the .pkt file

⚠️ **Document changes**: Keep notes of any custom configurations
