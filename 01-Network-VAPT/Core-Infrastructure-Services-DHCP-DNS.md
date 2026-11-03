# Core Infrastructure Services (DHCP & DNS)
<img width="1536" height="1024" alt="DHCP   DNS Infrastructure Guide" src="https://github.com/user-attachments/assets/cc899c36-1bde-4287-8f29-1a59bc2da382" />

The infographic explains two essential services used in enterprise networks:
- DHCP: Automatically assigns network configuration to devices.
- DNS: Translates human-readable domain names into IP addresses.
It covers the DHCP DORA process, DNS records, recursive and iterative queries, and an internal services deployment lab.
1. DHCP — Dynamic Host Configuration Protocol
DHCP automatically provides network configuration to devices when they connect to a network.

Instead of manually configuring every computer, a DHCP server can assign:
- IP address
- Subnet mask
- Default gateway
- DNS server address
- Lease duration
For example, when a laptop connects to the company network, DHCP can assign 192.168.10.100 automatically.
DHCP DORA process
Four steps of DHCP
1. Discover
   
   The client broadcasts a message asking whether a DHCP server is available.
2. Offer
   
   The DHCP server offers an available IP address and configuration.
3. Request
   
   The client requests the offered IP address.
4. Acknowledge
   
   The server confirms the lease and provides the network configuration.

This process is called DORA: Discover, Offer, Request, Acknowledge.

DHCP leases have a lifetime. Clients can renew their leases before they expire, allowing them to continue using their assigned addresses.
2. DNS — Domain Name System
DNS translates human-readable hostnames into IP addresses.

For example:

web.internal.local → 192.168.10.20

Without DNS, users would need to remember IP addresses instead of hostnames.
Common DNS record types
Record	Purpose	Example
A	Maps a hostname to an IPv4 address	web.internal.local → 192.168.10.20
AAAA	Maps a hostname to an IPv6 address	IPv6 address
CNAME	Creates an alias for another hostname	www → web.internal.local
MX	Identifies mail servers	mail.internal.local
TXT	Stores text information	SPF verification

3. Recursive vs. iterative DNS queries
Recursive query

The client asks a DNS resolver to find the final answer. The resolver performs the necessary lookup and returns the result.

Iterative query

A DNS server returns the best available referral to another DNS server. The resolver follows those referrals until it finds the answer.

In a typical environment, a client sends a recursive query to its configured DNS resolver. The resolver may use iterative queries to find the authoritative answer.
4. Hands-on project: Internal Services Deployment







rumy29.tistory.com


Objective: Deploy a local DHCP server and DNS server so endpoints automatically receive network configuration and can resolve internal hostnames.
Example network design
Component	Configuration
Network	192.168.10.0/24
Gateway	192.168.10.1
DHCP server	192.168.10.10
DNS server	192.168.10.10
DHCP allocation pool	192.168.10.100–192.168.10.200
Internal DNS zone	internal.local
Web server	192.168.10.20

Project implementation
1. Build a network with a router, switch, DHCP server, DNS server and client computers.
2. Configure DHCP with an address pool, subnet mask, gateway, DNS server and lease duration.
3. Configure DNS with a forward lookup zone named internal.local.
4. Add DNS records such as web.internal.local and www.internal.local.
5. Configure clients to obtain their IP configuration automatically.
6. Test DHCP assignment and internal hostname resolution.
Example DHCP configuration
A Linux ISC DHCP server configuration might look like:
subnet 192.168.10.0 netmask 255.255.255.0 {
    range 192.168.10.100 192.168.10.200;
    option routers 192.168.10.1;
    option domain-name-servers 192.168.10.10;
    option domain-name "internal.local";
    default-lease-time 28800;
    max-lease-time 86400;
}


Example DNS records
$TTL 86400
@ IN SOA ns1.internal.local. admin.internal.local. (
    2026110301
    3600
    1800
    604800
    86400
)

@       IN NS     ns1.internal.local.
ns1     IN A      192.168.10.10
web     IN A      192.168.10.20
www     IN CNAME  web.internal.local.
mail    IN A      192.168.10.30
@       IN MX 10  mail.internal.local.
@       IN TXT    "v=spf1 ip4:192.168.10.30 -all"


These are illustrative configurations. The DHCP and DNS server software must be installed and configured before they can serve clients.
Verification commands
On Windows:
ipconfig /all
ipconfig /release
ipconfig /renew
nslookup web.internal.local


On Linux:
ip addr
ip route
resolvectl status
dig web.internal.local
dig -t MX internal.local
dig -t TXT internal.local


Expected results:
- Clients receive IP addresses from the DHCP pool.
- Clients receive the correct gateway and DNS server.
- web.internal.local resolves to 192.168.10.20.
- The CNAME, MX and TXT records return the configured values.
