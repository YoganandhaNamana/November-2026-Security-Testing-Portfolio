# IP Addressing, Subnetting, and CIDR
<img width="1536" height="1024" alt="IP Addressing and Subnetting Blueprint" src="https://github.com/user-attachments/assets/2de505d7-9caf-4a34-a3d4-8827027b4a4f" />

The infographic above explains IPv4 and IPv6 addressing, network and host bits, subnet masks, CIDR notation, VLSM, public and private IP ranges, and the Enterprise Subnetting Blueprint.
1. IP addressing fundamentals
An IP address (Internet Protocol address) is a logical address assigned to a device so it can communicate over a network.

There are two main versions:
- IPv4: 32-bit address, written in four decimal octets. Example: 192.168.1.10
- IPv6: 128-bit address, written in hexadecimal groups. Example: 2001:db8::1
IPv4 has a limited address space, while IPv6 provides a vastly larger address space.
2. Network bits vs. host bits
An IP address has two logical parts:
- Network bits: Identify which network the device belongs to.
- Host bits: Identify the individual device within that network.
For example:

192.168.1.10/24
- /24 means 24 bits identify the network.
- The remaining 8 bits identify the host.
- Network address: 192.168.1.0
- Usable host range: 192.168.1.1 – 192.168.1.254
- Broadcast address: 192.168.1.255
3. Subnet mask and CIDR
A subnet mask separates the network portion from the host portion of an IPv4 address.

CIDR stands for Classless Inter-Domain Routing. The /24 notation is called a prefix length.
CIDR	Subnet mask	Total addresses	Usable hosts*
/24	255.255.255.0	256	254
/25	255.255.255.128	128	126
/26	255.255.255.192	64	62
/27	255.255.255.224	32	30
/28	255.255.255.240	16	14
/29	255.255.255.248	8	6
/30	255.255.255.252	4	2

For ordinary IPv4 subnets, excluding the network and broadcast addresses.

Important formulas:
\[
\text{Total addresses}=2^{(32-\text{prefix})}
\]

\[
\text{Usable hosts}=2^{(32-\text{prefix})}-2
\]

4. VLSM — Variable Length Subnet Masking
VLSM allows you to divide one network into multiple subnets of different sizes.

For example, a company has different departments:
- Administration: 100 devices
- Sales: 50 devices
- IT: 30 devices
- Finance: 20 devices
- Guest: 10 devices
Instead of allocating the same size subnet to everyone, VLSM allocates an appropriate subnet to each department. This helps avoid wasting IP addresses.
5. Public vs. private IPv4 addresses
Private IP addresses are used inside private networks and are not directly routable over the public internet.

The three private IPv4 ranges are:
Private range	CIDR
10.0.0.0 – 10.255.255.255	/8
172.16.0.0 – 172.31.255.255	/12
192.168.0.0 – 192.168.255.255	/16

Public IP addresses are globally routable addresses allocated through internet address management systems.
6. Hands-on project: Enterprise Subnetting Blueprint

The project requirements:
Department	VLAN	Required hosts
Administration	10	100
Sales	20	50
IT	30	30
Finance	40	20
Guest	50	10

You will use VLSM to allocate subnets and calculate:
1. Network address.
2. CIDR prefix and subnet mask.
3. Total addresses and usable hosts.
4. First and last usable IP addresses.
5. Broadcast address.
6. VLAN assignment.
The infographic includes one example allocation using 192.168.10.0/24 as the starting network.
