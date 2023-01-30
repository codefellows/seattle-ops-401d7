# Lecture Notes: Log Analysis with Splunk

## Log Analysis

- **Why** (5 min)
  - Why is log analysis important?
    - Piece together events that already took place
    - Usable for threat hunting (proactively looking for threats)
    - If maintained properly, admissible as legal evidence
  - What are we discussing today?
    - Core concepts necessary to complete today's lab, a BOTS workshop

- **What** (10 min)
  - An **advanced persistent threat (APT)** is “a skilled and determined cyber criminal [who] can use multiple vectors and entry points to navigate around defenses, breach your network in minutes and evade detection for months. APTs present a challenge for organizational cyber security efforts.” -FireEye
  - **Web site defacement** is typically a form of hacktivism, where the attacker infiltrates a web server and alters the front page to send a message
  - A **hacktivist** uses offensive security skills to influence policy and attempt to bring about social change
    - Example: Anonymous, a decentralized international activist/hacktivist movement
  - What is the Cyber Kill Chain?
    - A **cyber kill chain** is a security model outlining the sequential phases of a cyber attack
    - Used by defenders to plan ways to interrupt or “break” the cyber kill chain and hopefully thwart the attack entirely
    - Originally developed by Lockheed Martin
    - Today’s lab will be structured around the sequential phases of a cyber kill chain
      - Stages of the Lockheed Martin Cyber Kill Chain:
        - Reconnaissance
          - involves observation, research and planning of and into potential targets that satisfy the needs or the mission of the attackers.
          - Attackers gather as much information of their targets, including researching about them in publicly available sources and sites (Twitter, Facebook, Instagram, LinkedIn, etc.)
          - At this stage they start to look for vulnerabilities within the network which they can exploit (applications, targets networks, etc.) and start mapping out the areas where they can take advantage
          - Once they figure out which defenses are in place, they choose which weapon is best for their needs to exploit the network (bribing an employee, e-mail attachments with viruses, phishing attacks)
        - Weaponization
          - Preparing for and staging the attack
        - Delivery
          - Launching the attack
          - Attackers breach the organization's network and install malware or any other viruses or a reverse shell program to gain access to the target
          - Common tactics
            - Ransomware
            - Spear phishing
            - Password attacks
        - Exploitation
          - Attackers start an exploit against weaknesses found in the network system
          - At this stage, the system is compromised and the organization's data is at risk
        - Installation
          - At this stage the attacker makes sure that they can maintain continued control over the compromised network
          - Attackers will install the malware in order to conduct further operations (maintain access and escalate privileges)
        - Command and Control
          - If the compromise remains undetected at this stage, the attackers will be able to take complete control over the organization's network
          - At this stage, attackers will establish a command channel in order to pass back the data between the infected devices and their own infrastructure
        - Actions on Objectives
          - Final stage where the attacker executes the final stage of their mission (data exfiltration, destruction of critical infrastructure, defacing web property, creating fear of any means of extortion)
  - What is the `metadata` command and how does it work? ([Documentation](https://docs.splunk.com/Documentation/Splunk/latest/SearchReference/Metadata))
    - The `metadata` command returns a list of sources, sourcetypes, or hosts from a specified index or distributed search peer. The metadata command returns information accumulated over time. You can view a snapshot of an index over a specific timeframe, such as the last 7 days, by using the time range picker.
    - Syntax
      - `| metadata type=<metadata-type> [<index-specifier>]... [splunk_server=<wc-string>] [splunk_server_group=<wc-string>]...`
    - Example
      - `| metadata type=hosts`
  - What is a web vulnerability scanner?
    - **Web vulnerability scanners** are automated tools that scan web applications to look for security vulnerabilities such as cross-site scripting (XSS), SQL injection (SQLi), and cross-site request forgery (CSRF).
  - We can use open source intelligence (OSINT) resources to help us analyze the data that we find in our logs.
    - [Threatminer](https://www.threatminer.org/)
    - [VirustTotal](https://www.virustotal.com/gui/)
    - [HybridAnalysis](https://www.hybrid-analysis.com/)
  - What is Dynamic DNS?
    - **Dynamic DNS (DDNS)** is a service that automatically updates a DNS with a web property's correct IP, even if that IP address changes.
      - **Advantages**
        - Saves time required by static addresses updates manually when network configuration changes
        - Saves space as the number of addresses are used as required at one time rather than using one for all the possible users of the IP address
        - Very comfortable for users point of view as IP address changes will not affect any of their activities
        - Does not affect accessibility as changed IP addresses are configured automatically against URL's
      - **Disadvantages**
        - Less reliable due to lack of static IP addresses and domain name mappings
        - Can't guarantee about the device your are attempting to connect is actually your own
      - **Uses**
        - Used for internet access devices such as routers
        - Used for security appliance manufacturers and even required for IP-based security appliances like DVRs

- **How** (30 min)
  - Ref. [DEMO.md](DEMO.md)

# Lecture Notes: Python ipaddress module

Python's `ipaddress` module which provides the ability to create, manipulate and operate on IPv4 and IPv6 addresses and networks. This comes inbuilt with python 3.3+ so you do not need to install it. If you need to install it, you can use `pip install ipaddress`

**IPv4Address**

The IPv4Address objects have a lot of attributes for IPv4 address. `ipaddress.IPv4Address('address')` construct an IPv4Address object representing IPv4 address 'address'

Attributes:

- max_prefixlen: Return the total number of bits in the IP address represented by IPv4Address object (32 for IPv4 and 128 for IPv6).
- is_multicast: Return True if the address is reserved for multicast use
- is_private: Return True if the address is allocated for private networks
- is_global: Return True if the address is allocated for public networks
- is_unspecified: Return True if the address is unspecified
- is_reserved: Return True if the address is otherwise IETF reserved
- is_loopback: Return True if this is a loopback address
- is_link_local: Return True if the address is reserved for link-local usage.

Examples:

```python
import ipaddress
# Creating an object of IPv4Address class and
# initializing it with an IPv4 address.
ip = ipaddress.IPv4Address('112.79.234.30')

# Print total number of bits in the ip.
print("Total no of bits in the ip:", ip.max_prefixlen)

# Print True if the IP address is reserved for multicast use.
print("Is multicast:", ip.is_multicast)

# Print True if the IP address is allocated for private networks.
print("Is private:", ip.is_private)

# Print True if the IP address is global.
print("Is global:", ip.is_global)

# Print True if the IP address is unspecified.
print("Is unspecified:", ip.is_unspecified)

# Print True if the IP address is otherwise IETF reserved.
print("Is reversed:", ip.is_reserved)

# Print True if the IP address is a loopback address.
print("Is loopback:", ip.is_loopback)

# Print True if the IP address is Link-local
print("Is link-local:", ip.is_link_local)

# next ip address
ip1 = ip + 1
print("Next ip:", ip1)

# previous ip address
ip2 = ip - 1
print("Previous ip:", ip2)

# Print True if ip1 is greater than ip2
print("Is ip1 is greater than ip2:", ip1 > ip2)
```

**IPv4Network**

IPv4Network objects are used to inspect and define IP network.

Attributes:

- network_address: Return the network address for the network
- broadcast_address: Return the broadcast address for the network. Packets sent to the broadcast address should be received by every host on the network
- netmask: Return network mask of the network.
with_netmask: Return a string representation of the network, with the mask in netmask notation
- with_hostmask: Return a string representation of the network, with the mask in host mask notation.
- prefixlen: Return the length of the network prefix in bits.
- num_addresses: Return the total number of the address of this network.
- hosts(): Returns an iterator over the usable hosts in the network. The usable hosts are all the IP addresses that belong to the network, except the network address itself and the network broadcast address.
- overlaps(other): Return True if this network is partly or wholly contained in other or other is wholly contained in this network.
- subnets(prefixlen_diff): Return the subnets that join to make the current network definition, depending on the argument values. The prefixlen_diff parameter is the integer that indicates the amount our prefix length should be increased by.
- supernet(prefixlen_diff): Return the supernet containing this network definition, the prefixlen_diff is the amount our prefix length should be decreased by.
- subnet_of(other): Return True if this network is a subnet of other (new in python 3.7).
- supernet_of(other): Return True if this network is a supernet of other (new in python 3.7).
- compare_networks(other): Compare ip network with the other IP network. In this comparison only the network addresses are considered, host bits aren’t. It returns either -1, 0, or 1.

Examples:

```python
import ipaddress

# Initializing an IPv4 Network.
network = ipaddress.IPv4Network("192.168.1.0/24")

# Print the network address of the network.
print("Network address of the network:", network.network_address)

# Print the broadcast address
print("Broadcast address:", network.broadcast_address)

# Print the network mask.
print("Network mask:", network.netmask)

# Print with_netmask.
print("with netmask:", network.with_netmask)

# Print with_hostmask.
print("with_hostmask:", network.with_hostmask)

# Print Length of network prefix in bits.
print("Length of network prefix in bits:", network.prefixlen)

# Print the number of hosts under the network.
print("Total number of hosts under the network:", network.num_addresses)

# Print if this network is under (or overlaps) 192.168.0.0/16
print("Overlaps 192.168.0.0/16:", network.overlaps(ipaddress.IPv4Network("192.168.0.0/16")))

# Print the supernet of this network
print("Supernet:", network.supernet(prefixlen_diff=1))

# Print if the network is subnet of 192.168.0.0/16.
print("The network is subnet of 192.168.0.0/16:",
	network.subnet_of(ipaddress.IPv4Network("192.168.0.0/16")))

# Print if the network is supernet of 192.168.0.0/16.
print("The network is supernet of 192.168.0.0/16:",
	network.supernet_of(ipaddress.IPv4Network("192.168.0.0/16")))

# Compare the ip network with 192.168.0.0/16.
print("Compare the network with 192.168.0.0/16:",
	network.compare_networks(ipaddress.IPv4Network("192.168.0.0/16")))
```
