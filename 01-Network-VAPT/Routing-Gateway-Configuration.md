# Routing & Gateway Configuration
<img width="1536" height="1024" alt="Routing   Gateway Configuration Infographic" src="https://github.com/user-attachments/assets/aecef9b5-6131-4e06-b1de-4569a6c32159" />

The infographic explains how routers connect different networks, how default gateways work, how routing tables select paths, and how static and dynamic routing operate. It also covers inter-VLAN routing and a multi-router OSPF practical lab.
*1. What is routing?*
Routing is the process of forwarding IP packets between different networks.

A switch primarily forwards Ethernet frames within a Layer 2 network. A router forwards IP packets between different IP networks using its routing table.

For example:
- Network A: 192.168.10.0/24
- Network B: 192.168.20.0/24
A router allows devices in these two networks to communicate, subject to routing and security policies.
2. Default gateway
A default gateway is the router's IP address that a device uses to reach destinations outside its local subnet.

Example:
Device setting	Value
Host IP	192.168.10.10
Subnet mask	255.255.255.0
Default gateway	192.168.10.1
Destination	192.168.20.10

When the host wants to reach 192.168.20.10, it recognizes that the destination is outside its local subnet and forwards the packet to its default gateway.
3. Routing table
A routing table stores information about known destination networks and how to reach them.

Example:
Route type	Destination	Next hop
Connected	192.168.10.0/24	Direct
Connected	192.168.20.0/24	Direct
Static	192.168.30.0/24	10.0.0.2
OSPF	172.16.0.0/16	10.0.0.2

Common route codes:
- C — Connected
- S — Static
- O — OSPF
Routers select routes using the longest-prefix-match rule first. When routes have the same prefix length, route preference and metrics help determine which route is selected.
4. Static routing
Static routing means manually configuring a route on a router.

Example Cisco command:
ip route 192.168.30.0 255.255.255.0 10.0.0.2


This tells the router to reach 192.168.30.0/24 through next-hop address 10.0.0.2.

Advantages: Simple, predictable and useful for small or stable networks.

Limitations: Administrators must update routes manually when the network changes.
5. Dynamic routing — OSPF basics
OSPF stands for Open Shortest Path First.

It is a dynamic, link-state routing protocol that allows routers to exchange network information and calculate paths.

Important concepts:
- OSPF routers discover neighbors using Hello packets.
- They exchange link-state information.
- They build a link-state database.
- They use the Shortest Path First (SPF) algorithm to calculate routes.
- Area 0 is the backbone area.
Unlike static routing, OSPF can learn routes automatically and adapt when network topology changes.
6. Inter-VLAN routing
VLANs separate devices into different Layer 2 broadcast domains. Inter-VLAN routing allows devices in different VLANs to communicate.

Two common methods:

Router-on-a-Stick (ROAS)
- Uses one physical router interface.
- Creates multiple logical subinterfaces.
- Uses 802.1Q VLAN tagging on the trunk.
- Each subinterface acts as a gateway for its VLAN.
Layer 3 switch
- Uses Switch Virtual Interfaces (SVIs).
- Performs routing directly on the switch.
- Common in enterprise networks.
7. Hands-on project: Multi-Router Interconnection







gallardo.work


Objective: Build a simulated enterprise network with multiple VLANs, inter-VLAN routing and OSPF between branch routers.
Example network design
Location	Network
Branch A	192.168.10.0/24
Headquarters	192.168.20.0/24
Branch B	192.168.30.0/24
R1–R2 link	10.0.12.0/30
R2–R3 link	10.0.23.0/30

Router-on-a-Stick example
Assume R1 connects to SW1 using G0/0, and VLANs 10, 20 and 30 are configured on the switch.
enable
configure terminal

interface gigabitEthernet 0/0
 no shutdown

interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.10.1 255.255.255.0

interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.20.1 255.255.255.0

interface gigabitEthernet 0/0.30
 encapsulation dot1Q 30
 ip address 192.168.30.1 255.255.255.0

end


These are example commands; the actual interfaces and IP addresses must match your topology.
OSPF configuration example
On a router, use:
enable
configure terminal

router ospf 1
 network 192.168.10.0 0.0.0.255 area 0
 network 10.0.12.0 0.0.0.3 area 0

end


Configure the corresponding connected networks on each router. The OSPF network statements identify the interfaces that participate in OSPF.
Verification commands
show ip route
show ip ospf neighbor
show ip ospf database
show ip interface brief
ping <destination-ip>
traceroute <destination-ip>


Expected results:
- VLAN devices can reach their configured gateways.
- Devices in different VLANs can communicate when routing and policies permit.
- Routers establish OSPF neighbor relationships.
- Remote networks appear in routing tables.
- End-to-end connectivity works across the simulated branches.
