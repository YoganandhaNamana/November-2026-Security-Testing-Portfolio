Switching & Layer 2 Technologies
<img width="1536" height="1024" alt="Layer 2 Switching and VLAN Trunking Infographic" src="https://github.com/user-attachments/assets/bbccd463-5e00-4f23-8cd2-29c1747c9d15" />

The infographic explains the fundamentals of Layer 2 switching, MAC address tables, ARP, broadcast and collision domains, VLAN segmentation, and IEEE 802.1Q trunking.
1. Layer 2 switching
Layer 2 is the Data Link layer of the OSI model.

A network switch connects devices within a local network and forwards Ethernet frames using MAC addresses.

When a frame arrives, the switch:
1. Reads the source MAC address.
2. Learns which port that MAC address came from.
3. Checks its MAC address table for the destination MAC.
4. Forwards the frame through the appropriate port.
5. Floods unknown unicast and broadcast frames within the relevant VLAN.
2. MAC address table
A MAC address table stores the relationship between a device's MAC address and the switch port through which it can be reached.

Example:
MAC address	VLAN	Switch port
AA:AA:AA:AA:AA:01	10	Fa0/1
BB:BB:BB:BB:BB:02	20	Fa0/2
CC:CC:CC:CC:CC:03	30	Fa0/3

This helps the switch forward traffic efficiently instead of sending every frame to every connected device.
3. ARP — Address Resolution Protocol
ARP resolves an IPv4 address into the corresponding MAC address on a local network.

Example:
- Host A wants to communicate with 192.168.10.20.
- It checks its ARP cache.
- If the mapping is unknown, it broadcasts an ARP request: "Who has 192.168.10.20?"
- Host B responds with its MAC address.
- Host A stores the mapping in its ARP cache.
Important: ARP is used for IPv4. IPv6 uses Neighbor Discovery Protocol (NDP), not ARP.
4. Broadcast domains vs. collision domains
Broadcast domain: A group of devices that receive Layer 2 broadcast traffic.
- A VLAN creates a separate broadcast domain.
- Broadcast traffic from VLAN 10 does not normally reach VLAN 20 without routing.
Collision domain: A network segment where simultaneous transmissions could collide.
- Each switch port creates a separate collision domain.
- Modern full-duplex Ethernet eliminates ordinary Ethernet collisions.
5. VLAN — Virtual Local Area Network
A VLAN logically separates one physical network into multiple isolated Layer 2 networks.

Example:
VLAN	Department	Network
10	HR	192.168.10.0/24
20	IT	192.168.20.0/24
30	Guest	192.168.30.0/24

Even if devices connect to the same physical switch, they can belong to different broadcast domains.

VLAN separation alone does not provide complete security. Communication between VLANs requires Layer 3 routing and appropriate access-control policies.
6. IEEE 802.1Q trunking
IEEE 802.1Q is the standard used to carry multiple VLANs over a single Ethernet trunk link.

For example, two switches can connect through one trunk cable carrying VLANs 10, 20 and 30.

The switch adds a VLAN tag to frames transmitted over the trunk. The receiving switch uses that tag to identify the VLAN.
- Access port: Normally carries traffic for one VLAN.
- Trunk port: Carries traffic for multiple VLANs.

7. Hands-on project: VLAN Segmentation & Trunking Lab







dev.to


Objective: Configure VLANs on switches and establish a trunk link between them.
Lab topology
You will use:
- Two switches: SW1 and SW2.
- Six PCs: HR, IT and Guest on each switch.
- One trunk link between the switches.
VLAN allocation
VLAN ID	Department	IP network
10	HR	192.168.10.0/24
20	IT	192.168.20.0/24
30	Guest	192.168.30.0/24

Practical configuration
On both switches, create the VLANs:
enable
configure terminal

vlan 10
 name HR

vlan 20
 name IT

vlan 30
 name GUEST

end


Assign access ports according to your topology. Example:
configure terminal

interface fastethernet 0/1
 switchport mode access
 switchport access vlan 10

interface fastethernet 0/2
 switchport mode access
 switchport access vlan 20

interface fastethernet 0/3
 switchport mode access
 switchport access vlan 30

end


Configure the inter-switch trunk port (replace g0/1 with the actual connected port):
configure terminal

interface gigabitethernet 0/1
 switchport mode trunk

end


On Cisco Packet Tracer switch models that support it, this enables 802.1Q trunking. Some older models require an encapsulation command; many modern models use 802.1Q by default.
Verification commands
show vlan brief
show interfaces trunk
show mac address-table
show interfaces


Expected results:
- HR devices on both switches should communicate within VLAN 10.
- IT devices on both switches should communicate within VLAN 20.
- Guest devices on both switches should communicate within VLAN 30.
- Devices in different VLANs should not communicate without Layer 3 routing.
