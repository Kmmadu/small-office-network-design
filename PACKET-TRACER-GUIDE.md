# Packet Tracer Implementation Guide

This guide provides step-by-step instructions for building the small office network in Cisco Packet Tracer.

## Prerequisites
- Cisco Packet Tracer 7.3 or later installed
- Basic familiarity with Packet Tracer interface
- Understanding of basic networking concepts

## Part 1: Initial Setup

### Step 1: Start New Project
1. Launch Cisco Packet Tracer
2. Click on **File** > **New** to create a new project
3. Save the project as `small-office-network.pkt`

### Step 2: Set Up Workspace
1. Maximize the Packet Tracer window
2. Clear the workspace area
3. Prepare to add devices

## Part 2: Adding Network Devices

### Step 3: Add the Router
1. Click on the **Routers** icon (bottom left)
2. Select **2911** router
3. Drag and drop it to the upper-middle of workspace
4. Label it by clicking on the router name and typing: `Office-Router`

### Step 4: Add Distribution Switch
1. Click on the **Switches** icon
2. Select **3560-24PS** switch (or 3560 model available)
3. Drag and drop it below the router
4. Label it: `Dist-Switch`

### Step 5: Add Access Switches
1. Click on the **Switches** icon
2. Select **2960-24TT** switch
3. Drag and drop three switches below the distribution switch, spaced evenly
4. Label them from left to right:
   - `Access-Switch-1`
   - `Access-Switch-2`
   - `Access-Switch-3`

### Step 6: Add ISP Cloud
1. Click on **WAN Emulation** icon
2. Select **Cloud-PT**
3. Place it above the router
4. Label it: `ISP`

### Step 7: Add PCs (Access-Switch-1)
For Management VLAN (VLAN 10):
1. Click on **End Devices** icon
2. Select **PC-PT**
3. Add 3 PCs below Access-Switch-1 (left side)
4. Label them: `Mgmt-PC1`, `Mgmt-PC2`, `Mgmt-PC3`

For Sales VLAN (VLAN 20):
1. Add 3 PCs below Access-Switch-1 (right side)
2. Label them: `Sales-PC1`, `Sales-PC2`, `Sales-PC3`
3. Add 1 Printer: `Sales-Printer`

### Step 8: Add PCs (Access-Switch-2)
For IT VLAN (VLAN 30):
1. Add 3 PCs below Access-Switch-2 (left side)
2. Label them: `IT-PC1`, `IT-PC2`, `IT-PC3`
3. Add 1 Printer: `IT-Printer`

For Server VLAN (VLAN 99):
1. Click on **End Devices** icon
2. Select **Server-PT**
3. Add 3 servers below Access-Switch-2 (right side)
4. Label them: `DNS-Server`, `File-Server`, `Web-Server`

### Step 9: Add PCs (Access-Switch-3)
For HR VLAN (VLAN 40):
1. Add 3 PCs below Access-Switch-3 (left side)
2. Label them: `HR-PC1`, `HR-PC2`, `HR-PC3`
3. Add 1 Printer: `HR-Printer`

For Guest VLAN (VLAN 50):
1. Add 3 PCs below Access-Switch-3 (right side)
2. Label them: `Guest-PC1`, `Guest-PC2`, `Guest-PC3`

## Part 3: Connecting Devices

### Step 10: Connect Router to ISP
1. Click on the **Connections** icon (lightning bolt)
2. Select **Copper Straight-Through** cable
3. Click on ISP Cloud → Select **Ethernet 0**
4. Click on Router → Select **GigabitEthernet0/0**
5. Wait for link lights to turn green

### Step 11: Connect Router to Distribution Switch
1. Select **Copper Straight-Through** cable
2. Click on Router → Select **GigabitEthernet0/1**
3. Click on Dist-Switch → Select **GigabitEthernet0/1**
4. Wait for link lights to turn green

### Step 12: Connect Distribution Switch to Access Switches
1. **Dist-Switch to Access-Switch-1**:
   - Cable: Copper Straight-Through
   - From: Dist-Switch **GigabitEthernet0/2**
   - To: Access-Switch-1 **GigabitEthernet0/1**

2. **Dist-Switch to Access-Switch-2**:
   - Cable: Copper Straight-Through
   - From: Dist-Switch **FastEthernet0/1**
   - To: Access-Switch-2 **FastEthernet0/24**

3. **Dist-Switch to Access-Switch-3**:
   - Cable: Copper Straight-Through
   - From: Dist-Switch **FastEthernet0/2**
   - To: Access-Switch-3 **FastEthernet0/24**

### Step 13: Connect PCs to Access-Switch-1
Management VLAN PCs:
- Mgmt-PC1 → Access-Switch-1 **FastEthernet0/1**
- Mgmt-PC2 → Access-Switch-1 **FastEthernet0/2**
- Mgmt-PC3 → Access-Switch-1 **FastEthernet0/3**

Sales VLAN devices:
- Sales-PC1 → Access-Switch-1 **FastEthernet0/6**
- Sales-PC2 → Access-Switch-1 **FastEthernet0/7**
- Sales-PC3 → Access-Switch-1 **FastEthernet0/8**
- Sales-Printer → Access-Switch-1 **FastEthernet0/9**

### Step 14: Connect PCs to Access-Switch-2
IT VLAN devices:
- IT-PC1 → Access-Switch-2 **FastEthernet0/1**
- IT-PC2 → Access-Switch-2 **FastEthernet0/2**
- IT-PC3 → Access-Switch-2 **FastEthernet0/3**
- IT-Printer → Access-Switch-2 **FastEthernet0/4**

Server VLAN devices:
- DNS-Server → Access-Switch-2 **FastEthernet0/9**
- File-Server → Access-Switch-2 **FastEthernet0/10**
- Web-Server → Access-Switch-2 **FastEthernet0/11**

### Step 15: Connect PCs to Access-Switch-3
HR VLAN devices:
- HR-PC1 → Access-Switch-3 **FastEthernet0/1**
- HR-PC2 → Access-Switch-3 **FastEthernet0/2**
- HR-PC3 → Access-Switch-3 **FastEthernet0/3**
- HR-Printer → Access-Switch-3 **FastEthernet0/4**

Guest VLAN devices:
- Guest-PC1 → Access-Switch-3 **FastEthernet0/9**
- Guest-PC2 → Access-Switch-3 **FastEthernet0/10**
- Guest-PC3 → Access-Switch-3 **FastEthernet0/11**

## Part 4: Device Configuration

### Step 16: Configure ISP Cloud
1. Click on the **ISP** cloud
2. Go to **Config** tab
3. Select **Ethernet 0** interface
4. Set IP address: `203.0.113.1`
5. Set Subnet Mask: `255.255.255.252`
6. Click **Add** to save

### Step 17: Configure Router
1. Click on **Office-Router**
2. Click on **CLI** tab
3. Press Enter to get command prompt
4. Enter the following commands (copy from `CONFIGURATIONS.md`):

```
enable
configure terminal
hostname Office-Router
no ip domain-lookup
enable secret cisco123

! WAN Interface
interface GigabitEthernet0/0
description WAN Connection to ISP
ip address 203.0.113.2 255.255.255.252
no shutdown
exit

! LAN Interface with Subinterfaces
interface GigabitEthernet0/1
no shutdown
exit

interface GigabitEthernet0/1.10
encapsulation dot1Q 10
ip address 192.168.10.1 255.255.255.0
exit

interface GigabitEthernet0/1.20
encapsulation dot1Q 20
ip address 192.168.20.1 255.255.255.0
exit

interface GigabitEthernet0/1.30
encapsulation dot1Q 30
ip address 192.168.30.1 255.255.255.0
exit

interface GigabitEthernet0/1.40
encapsulation dot1Q 40
ip address 192.168.40.1 255.255.255.0
exit

interface GigabitEthernet0/1.50
encapsulation dot1Q 50
ip address 192.168.50.1 255.255.255.0
exit

interface GigabitEthernet0/1.99
encapsulation dot1Q 99
ip address 192.168.99.1 255.255.255.0
exit

! NAT Configuration
interface GigabitEthernet0/0
ip nat outside
exit

interface GigabitEthernet0/1.10
ip nat inside
exit

interface GigabitEthernet0/1.20
ip nat inside
exit

interface GigabitEthernet0/1.30
ip nat inside
exit

interface GigabitEthernet0/1.40
ip nat inside
exit

interface GigabitEthernet0/1.50
ip nat inside
exit

interface GigabitEthernet0/1.99
ip nat inside
exit

access-list 1 permit 192.168.0.0 0.0.255.255
ip nat inside source list 1 interface GigabitEthernet0/0 overload

! Default Route
ip route 0.0.0.0 0.0.0.0 203.0.113.1

! DHCP Pools
ip dhcp excluded-address 192.168.10.1 192.168.10.10
ip dhcp excluded-address 192.168.20.1 192.168.20.10
ip dhcp excluded-address 192.168.30.1 192.168.30.10
ip dhcp excluded-address 192.168.40.1 192.168.40.10
ip dhcp excluded-address 192.168.50.1 192.168.50.10
ip dhcp excluded-address 192.168.99.1 192.168.99.20

ip dhcp pool VLAN10-POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 192.168.99.10
exit

ip dhcp pool VLAN20-POOL
network 192.168.20.0 255.255.255.0
default-router 192.168.20.1
dns-server 192.168.99.10
exit

ip dhcp pool VLAN30-POOL
network 192.168.30.0 255.255.255.0
default-router 192.168.30.1
dns-server 192.168.99.10
exit

ip dhcp pool VLAN40-POOL
network 192.168.40.0 255.255.255.0
default-router 192.168.40.1
dns-server 192.168.99.10
exit

ip dhcp pool VLAN50-POOL
network 192.168.50.0 255.255.255.0
default-router 192.168.50.1
dns-server 8.8.8.8
exit

! Guest VLAN ACL
ip access-list extended GUEST-RESTRICT
deny ip 192.168.50.0 0.0.0.255 192.168.10.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.20.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.30.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.40.0 0.0.0.255
deny ip 192.168.50.0 0.0.0.255 192.168.99.0 0.0.0.255
permit ip any any
exit

interface GigabitEthernet0/1.50
ip access-group GUEST-RESTRICT in
exit

end
write memory
```

### Step 18: Configure Distribution Switch
1. Click on **Dist-Switch**
2. Click on **CLI** tab
3. Enter the configuration:

```
enable
configure terminal
hostname Dist-Switch
no ip domain-lookup

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

! Trunk to Router
interface GigabitEthernet0/1
description Trunk to Router
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! Trunk to Access-Switch-1
interface GigabitEthernet0/2
description Trunk to Access-Switch-1
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! Trunk to Access-Switch-2
interface FastEthernet0/1
description Trunk to Access-Switch-2
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! Trunk to Access-Switch-3
interface FastEthernet0/2
description Trunk to Access-Switch-3
switchport trunk encapsulation dot1q
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

end
write memory
```

### Step 19: Configure Access-Switch-1
1. Click on **Access-Switch-1**
2. Click on **CLI** tab
3. Enter the configuration:

```
enable
configure terminal
hostname Access-Switch-1

vlan 10
name Management
vlan 20
name Sales
exit

! Trunk port
interface GigabitEthernet0/1
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! Management VLAN ports
interface range FastEthernet0/1-3
switchport mode access
switchport access vlan 10
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown
exit

! Sales VLAN ports
interface range FastEthernet0/6-9
switchport mode access
switchport access vlan 20
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown
exit

end
write memory
```

### Step 20: Configure Access-Switch-2
1. Click on **Access-Switch-2**
2. Click on **CLI** tab
3. Enter the configuration:

```
enable
configure terminal
hostname Access-Switch-2

vlan 30
name IT
vlan 99
name Servers
exit

! Trunk port
interface FastEthernet0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! IT VLAN ports
interface range FastEthernet0/1-4
switchport mode access
switchport access vlan 30
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown
exit

! Server VLAN ports
interface range FastEthernet0/9-11
switchport mode access
switchport access vlan 99
no shutdown
exit

end
write memory
```

### Step 21: Configure Access-Switch-3
1. Click on **Access-Switch-3**
2. Click on **CLI** tab
3. Enter the configuration:

```
enable
configure terminal
hostname Access-Switch-3

vlan 40
name HR
vlan 50
name Guest
exit

! Trunk port
interface FastEthernet0/24
switchport mode trunk
switchport trunk allowed vlan 10,20,30,40,50,99
no shutdown
exit

! HR VLAN ports
interface range FastEthernet0/1-4
switchport mode access
switchport access vlan 40
switchport port-security
switchport port-security maximum 2
switchport port-security mac-address sticky
spanning-tree portfast
no shutdown
exit

! Guest VLAN ports
interface range FastEthernet0/9-11
switchport mode access
switchport access vlan 50
spanning-tree portfast
no shutdown
exit

end
write memory
```

## Part 5: Configure End Devices

### Step 22: Configure Servers
**DNS Server (192.168.99.10)**:
1. Click on DNS-Server
2. Go to **Desktop** > **IP Configuration**
3. Select **Static**
4. IP Address: `192.168.99.10`
5. Subnet Mask: `255.255.255.0`
6. Default Gateway: `192.168.99.1`
7. Go to **Services** > **DNS** and turn DNS service **On**

**File Server (192.168.99.11)**:
1. Click on File-Server
2. Go to **Desktop** > **IP Configuration**
3. Select **Static**
4. IP Address: `192.168.99.11`
5. Subnet Mask: `255.255.255.0`
6. Default Gateway: `192.168.99.1`

**Web Server (192.168.99.12)**:
1. Click on Web-Server
2. Go to **Desktop** > **IP Configuration**
3. Select **Static**
4. IP Address: `192.168.99.12`
5. Subnet Mask: `255.255.255.0`
6. Default Gateway: `192.168.99.1`
7. Go to **Services** > **HTTP** and turn HTTP service **On**

### Step 23: Configure PCs with DHCP
For all PCs (Management, Sales, IT, HR, Guest):
1. Click on each PC
2. Go to **Desktop** > **IP Configuration**
3. Select **DHCP**
4. Wait for IP address to be assigned
5. Verify the IP address is in the correct VLAN range

### Step 24: Configure Printers
For all printers:
1. Click on each printer
2. Go to **Config** > **FastEthernet0**
3. Select **DHCP** for automatic configuration

## Part 6: Testing

### Step 25: Test Basic Connectivity
1. From Mgmt-PC1, go to **Desktop** > **Command Prompt**
2. Test ping to another Management PC:
   ```
   ping 192.168.10.X
   ```
3. Should receive replies

### Step 26: Test Inter-VLAN Routing
1. From Mgmt-PC1, ping Sales PC:
   ```
   ping 192.168.20.X
   ```
2. Should receive replies

### Step 27: Test Guest Isolation
1. From Guest-PC1, try pinging Management PC:
   ```
   ping 192.168.10.X
   ```
2. Should timeout (blocked by ACL)

### Step 28: Test Internet Connectivity
1. From any PC, ping ISP:
   ```
   ping 203.0.113.1
   ```
2. Should receive replies

### Step 29: Verify DHCP
1. Check IP configuration on any PC:
   - Go to **Desktop** > **Command Prompt**
   - Type: `ipconfig`
2. Verify IP, subnet mask, gateway, and DNS are correct

### Step 30: Final Verification
1. Test all connectivity scenarios from `TESTING.md`
2. Save the project: **File** > **Save**

## Part 7: Finishing Touches

### Step 31: Add Labels and Notes
1. Use the **Note** tool (sticky note icon) to add documentation
2. Add VLAN labels near device groups
3. Add IP range labels

### Step 32: Organize Layout
1. Arrange devices neatly
2. Use straight cable paths where possible
3. Group devices by department/VLAN

### Step 33: Save Final Project
1. Click **File** > **Save**
2. Create backup copy
3. Export topology (optional)

## Troubleshooting Common Issues

### Link Lights Not Turning Green
- Wait 30 seconds for STP convergence
- Check interface status: `show ip interface brief`
- Verify `no shutdown` on interfaces

### DHCP Not Working
- Verify DHCP pools configured on router
- Check excluded addresses don't overlap with pool
- Verify trunk is carrying VLAN

### Inter-VLAN Routing Failed
- Check router subinterface configuration
- Verify VLAN encapsulation (dot1Q)
- Check trunk between switch and router

### Guest ACL Not Working
- Verify ACL is applied to correct interface
- Check ACL direction (in/out)
- Test with `show access-lists`

## Next Steps

After completing the basic setup:
1. Review all documentation
2. Perform comprehensive testing per `TESTING.md`
3. Experiment with different configurations
4. Try the suggested future enhancements

## Tips for Success

- Save your work frequently
- Use copy-paste for configurations to avoid typos
- Test incrementally after each major configuration
- Use Packet Tracer's simulation mode to visualize traffic
- Document any custom changes you make

## Resources

- Refer to `CONFIGURATIONS.md` for complete command reference
- Check `TESTING.md` for all test procedures
- Review `DESIGN.md` for design rationale
- See `NETWORK-DIAGRAM.md` for topology details
