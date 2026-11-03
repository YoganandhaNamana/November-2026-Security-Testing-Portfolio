# Perimeter Security, NAT, and Access Control
<img width="1536" height="1024" alt="Perimeter Network Security Infographic" src="https://github.com/user-attachments/assets/25677787-477c-4467-94bb-976dc9bf9099" />

The infographic covers how organizations protect the boundary between internal networks and the internet using NAT, port forwarding, firewalls and Access Control Lists (ACLs).
1. Perimeter security
Perimeter security protects an organization's internal network from unauthorized external access.

A typical network perimeter contains:
- Internal LANs and VLANs.
- A perimeter firewall or router.
- NAT/PAT for address translation.
- ACLs and firewall rules to control traffic.
- Public-facing services, often placed in a DMZ.
The main objective is to allow legitimate communication while blocking unauthorized traffic.
2. NAT — Network Address Translation
NAT translates IP addresses as packets move between networks.

For example, a computer inside a company might use a private IP address:

192.168.10.10

When it accesses the internet, the perimeter router can translate its private IP to a public IP.

Common NAT types:
Type	Purpose
Static NAT	One private IP maps to one public IP
Dynamic NAT	Private IPs map temporarily to addresses from a public pool
PAT (NAT Overload)	Multiple internal devices share one public IP using different port numbers

PAT is commonly used in home and corporate networks because it allows many devices to share a public IPv4 address.
3. Port forwarding
Port forwarding directs incoming traffic received on a particular public IP address and port to a specific internal server.

Example:
- Public IP: 203.0.113.10
- Public port: 80
- Internal server: 192.168.40.10
- Internal port: 80
When an external client connects to 203.0.113.10:80, the router forwards the connection to the internal web server.

Port forwarding should expose only the services that genuinely need external access.
4. Stateful vs. stateless packet filtering
Stateless filtering
- Examines individual packets independently.
- Applies rules based on IP addresses, ports and protocols.
- Does not remember previous connection states.
Stateful filtering
- Tracks active connections.
- Maintains connection information in a state table.
- Can allow legitimate response traffic belonging to an established connection.
Stateful inspection is commonly used in modern firewalls.
5. ACL — Access Control List
An ACL is a collection of rules that permits or denies network traffic.

There are two common Cisco ACL types:

Standard ACL
- Primarily filters traffic using the source IPv4 address.
- Useful for basic source-based restrictions.
Extended ACL
- Can filter using source IP, destination IP, protocol and port numbers.
- Provides more granular traffic control.
Example:
access-list 110 permit tcp 192.168.10.0 0.0.0.255 any eq 443
access-list 110 deny ip any any


This example permits HTTPS traffic from the specified source network and denies other traffic matched by that ACL. ACL placement, direction and other required rules must be considered before applying it.

6. Hands-on project: Firewall Perimeter Defense







figviz.com


Objective: Configure NAT/PAT, port forwarding and ACLs to control traffic between internal VLANs and external networks.
Example enterprise network
Network	Purpose
VLAN 10 — 192.168.10.0/24	HR
VLAN 20 — 192.168.20.0/24	IT
VLAN 30 — 192.168.30.0/24	Guest
VLAN 40 — 192.168.40.0/24	DMZ servers
Public IP — 203.0.113.10	Example external address

The 203.0.113.0/24 range is reserved for documentation examples; it is not a real public internet address for deployment.
Project implementation
1. Build internal VLANs for HR, IT and Guest.
2. Configure the perimeter router or firewall with inside and outside interfaces.
3. Configure source NAT/PAT for internal internet access.
4. Configure destination NAT or port forwarding for an authorized public-facing web service.
5. Write ACL rules to restrict unauthorized cross-VLAN communication.
6. Allow only required traffic to the DMZ web server.
7. Test permitted and denied traffic.
Example Cisco IOS configuration concepts
Source NAT/PAT:
interface gigabitEthernet 0/0
 ip nat inside

interface gigabitEthernet 0/1
 ip nat outside

access-list 1 permit 192.168.10.0 0.0.0.255
access-list 1 permit 192.168.20.0 0.0.0.255
access-list 1 permit 192.168.30.0 0.0.0.255

ip nat inside source list 1 interface gigabitEthernet 0/1 overload


Destination NAT example for an internal web server:
ip nat inside source static tcp 192.168.40.10 80 203.0.113.10 80


Example extended ACL:
ip access-list extended VLAN-POLICY
 permit tcp 192.168.10.0 0.0.0.255 host 192.168.40.10 eq 443
 deny ip 192.168.30.0 0.0.0.255 192.168.10.0 0.0.0.255
 deny ip 192.168.30.0 0.0.0.255 192.168.20.0 0.0.0.255
 permit ip any any


These are illustrative snippets, not a complete secure production configuration. ACL direction, interface placement, return traffic, NAT processing order and routing must all be checked. The final permit ip any any is intentionally broad and would need to be replaced with an appropriate least-privilege policy.
Verification commands
show ip nat translations
show ip nat statistics
show access-lists
show ip interface
show ip route


Expected observations:
- Internal addresses are translated when using PAT.
- Authorized public traffic reaches the intended internal service.
- Guest traffic cannot access restricted HR or IT networks.
- Required traffic is permitted.
- ACL counters show which rules matched.
