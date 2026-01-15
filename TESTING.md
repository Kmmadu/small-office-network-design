# Setup and Testing Instructions

## Prerequisites
- Cisco Packet Tracer (version 7.3 or later)
- Basic understanding of networking concepts
- Familiarity with Cisco IOS commands

## Setup Instructions

### Step 1: Open the Network File
1. Launch Cisco Packet Tracer
2. Open the `small-office-network.pkt` file
3. Review the network topology

### Step 2: Verify Device Connections
1. Check all cable connections (green indicators)
2. Verify trunk links are established
3. Ensure all devices are powered on

### Step 3: Access Device Configuration
To access any network device:
1. Click on the device in the topology
2. Select the "CLI" tab
3. Press Enter to access the command line
4. Use the configurations from `CONFIGURATIONS.md`

## Testing Procedures

### Test 1: Basic Connectivity Within Same VLAN

**Objective**: Verify devices in the same VLAN can communicate

**Steps**:
1. Open a PC in VLAN 10 (Management)
2. Go to Desktop > Command Prompt
3. Ping another PC in VLAN 10:
   ```
   ping 192.168.10.X
   ```
4. **Expected Result**: Reply from destination with 0% packet loss

**Repeat for all VLANs**: 20, 30, 40, 50, 99

### Test 2: Inter-VLAN Routing

**Objective**: Verify devices in different VLANs can communicate (except Guest VLAN)

**Steps**:
1. Open a PC in VLAN 10 (Management)
2. Go to Desktop > Command Prompt
3. Ping a PC in VLAN 20 (Sales):
   ```
   ping 192.168.20.X
   ```
4. **Expected Result**: Reply from destination with 0% packet loss

**Test Cases**:
- Management (VLAN 10) → Sales (VLAN 20) ✓
- Management (VLAN 10) → IT (VLAN 30) ✓
- Management (VLAN 10) → HR (VLAN 40) ✓
- Sales (VLAN 20) → IT (VLAN 30) ✓
- Any VLAN → Server (VLAN 99) ✓

### Test 3: Guest VLAN Isolation

**Objective**: Verify Guest VLAN cannot access internal networks

**Steps**:
1. Open a PC in VLAN 50 (Guest)
2. Try to ping devices in other VLANs:
   ```
   ping 192.168.10.2    (Management)
   ping 192.168.20.2    (Sales)
   ping 192.168.30.2    (IT)
   ping 192.168.40.2    (HR)
   ping 192.168.99.10   (Server)
   ```
3. **Expected Result**: Request timed out (100% packet loss)

4. Try to ping external addresses:
   ```
   ping 8.8.8.8
   ```
5. **Expected Result**: Reply from destination (Guest has Internet access)

### Test 4: DHCP Functionality

**Objective**: Verify DHCP is assigning IP addresses correctly

**Steps**:
1. Open a new PC or reset existing PC IP configuration
2. Set IP configuration to DHCP:
   - Desktop > IP Configuration > DHCP
3. Wait for IP address assignment
4. Verify assigned IP is in correct range:
   - VLAN 10: 192.168.10.11-254
   - VLAN 20: 192.168.20.11-254
   - VLAN 30: 192.168.30.11-254
   - VLAN 40: 192.168.40.11-254
   - VLAN 50: 192.168.50.11-254

5. Check IP configuration:
   ```
   ipconfig
   ```
6. **Expected Result**: 
   - IP Address: Within VLAN range
   - Subnet Mask: 255.255.255.0
   - Default Gateway: 192.168.X.1
   - DNS Server: 192.168.99.10

### Test 5: Internet Connectivity (NAT)

**Objective**: Verify internal devices can access the Internet through NAT

**Steps**:
1. Open a PC in any VLAN (except Guest, test separately)
2. Go to Desktop > Command Prompt
3. Ping the ISP gateway:
   ```
   ping 203.0.113.1
   ```
4. **Expected Result**: Reply from destination

5. Verify on Router:
   ```
   show ip nat translations
   ```
6. **Expected Result**: NAT translations showing internal IPs mapped to external IP

### Test 6: DNS Resolution

**Objective**: Verify DNS service is working

**Steps**:
1. Configure DNS server (192.168.99.10) with domain names
2. From any PC, use DNS:
   ```
   nslookup fileserver.office.local
   ```
3. **Expected Result**: Resolves to 192.168.99.11

### Test 7: Trunk Links

**Objective**: Verify trunk links are passing all VLANs

**Steps**:
1. Access Distribution Switch via CLI
2. Check trunk status:
   ```
   show interfaces trunk
   ```
3. **Expected Result**: 
   - Trunking mode: on
   - Allowed VLANs: 10,20,30,40,50,99
   - Active VLANs: 10,20,30,40,50,99

4. Repeat for all trunk links

### Test 8: VLAN Configuration

**Objective**: Verify VLANs are properly configured

**Steps**:
1. Access any switch via CLI
2. Check VLAN configuration:
   ```
   show vlan brief
   ```
3. **Expected Result**: All VLANs (10,20,30,40,50,99) listed with correct names and ports

### Test 9: Port Security

**Objective**: Verify port security is preventing MAC address violations

**Steps**:
1. Access Access Switch via CLI
2. Check port security:
   ```
   show port-security interface FastEthernet0/1
   ```
3. **Expected Result**: 
   - Port Security: Enabled
   - Maximum MAC Addresses: 2
   - Violation Mode: Shutdown

4. Simulate violation by connecting multiple devices beyond limit
5. **Expected Result**: Port shuts down (err-disabled)

### Test 10: Routing Table

**Objective**: Verify routing is configured correctly

**Steps**:
1. Access Router via CLI
2. Check routing table:
   ```
   show ip route
   ```
3. **Expected Result**: 
   - Static default route: 0.0.0.0/0 via 203.0.113.1
   - Connected routes for all VLANs
   - All subnets reachable

### Test 11: End-to-End Connectivity

**Objective**: Complete end-to-end test from internal network to Internet

**Steps**:
1. From a PC in VLAN 20 (Sales)
2. Traceroute to external address:
   ```
   tracert 8.8.8.8
   ```
3. **Expected Result**: 
   - Hop 1: Default gateway (192.168.20.1)
   - Hop 2: Router external interface
   - Hop 3: ISP gateway (203.0.113.1)
   - Subsequent hops: Internet routers

## Troubleshooting Guide

### Issue: No connectivity within VLAN

**Checks**:
1. Verify cable connections
2. Check VLAN assignment on switch port:
   ```
   show vlan brief
   show interfaces switchport
   ```
3. Verify device IP configuration

**Solution**:
- Reassign port to correct VLAN
- Check for port security violations
- Verify IP addresses are in same subnet

### Issue: No inter-VLAN routing

**Checks**:
1. Verify router is receiving traffic:
   ```
   show ip interface brief
   ```
2. Check subinterface configuration:
   ```
   show running-config interface g0/1.X
   ```
3. Verify trunk between switch and router:
   ```
   show interfaces trunk
   ```

**Solution**:
- Reconfigure router subinterfaces
- Verify trunk encapsulation (dot1q)
- Check VLAN allowed list on trunk

### Issue: No Internet connectivity

**Checks**:
1. Ping default gateway
2. Ping router external interface (203.0.113.2)
3. Ping ISP gateway (203.0.113.1)
4. Check NAT configuration:
   ```
   show ip nat translations
   show ip nat statistics
   ```

**Solution**:
- Verify default route on router
- Check NAT inside/outside interface configuration
- Verify ACL for NAT

### Issue: Guest VLAN can access internal networks

**Checks**:
1. Verify ACL configuration on router:
   ```
   show access-lists
   ```
2. Check ACL application:
   ```
   show ip interface g0/1.50
   ```

**Solution**:
- Reconfigure ACL to deny Guest VLAN access
- Apply ACL in correct direction (inbound on interface)

### Issue: DHCP not working

**Checks**:
1. Verify DHCP pool configuration:
   ```
   show ip dhcp pool
   ```
2. Check DHCP bindings:
   ```
   show ip dhcp binding
   ```
3. Verify excluded addresses:
   ```
   show running-config | include dhcp
   ```

**Solution**:
- Reconfigure DHCP pool
- Check network address and gateway
- Verify excluded address range

## Performance Verification

### Bandwidth Testing
1. Use Simulation mode in Packet Tracer
2. Generate traffic between devices
3. Monitor packet flow and delays

### Latency Testing
1. Ping with timestamp
2. Measure round-trip time (RTT)
3. **Acceptable RTT**: < 10ms within LAN

### Throughput Testing
1. Transfer large file between devices
2. Monitor transfer speed
3. Verify no bottlenecks in network

## Documentation Checklist

After setup and testing, ensure:
- [ ] All device configurations are saved
- [ ] Network diagram is updated
- [ ] IP address assignments are documented
- [ ] All tests passed successfully
- [ ] Known issues are documented
- [ ] Backup configurations are stored

## Maintenance Schedule

**Daily**:
- Monitor network performance
- Check for security violations

**Weekly**:
- Review DHCP bindings
- Check NAT translations
- Verify trunk status

**Monthly**:
- Backup all configurations
- Review and update ACLs
- Update documentation

**Quarterly**:
- Security audit
- Performance review
- Capacity planning
