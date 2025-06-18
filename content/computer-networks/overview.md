[GeeksforGeeks - Networking Interview Questions](https://www.geeksforgeeks.org/networking-interview-questions/)


# Computer Networks

## Demilitarised Zone (DMZ)

A demilitarized zone (DMZ) in networking is a physical or logical subnetwork that sits between a private internal network (like a company's LAN) and an untrusted external network, typically the internet.

It acts as a buffer, hosting public-facing services like web servers, email servers, and DNS servers, while isolating them from the more sensitive internal network.

API Gateway Example: \
An API gateway, can be deployed in a DMZ, where it can handle tasks like request authentication, authorization, logging, and caching before forwarding requests to the backend services。

## VPN - Virtual Private Network

connect two offices in remote locations

(via cloud computing)

A Virtual Private Network is a way to extend a private network using a public network such as the Internet.

It makes use of tunneling protocols to establish a secure connection. 

### IPSsec -  Internet Protocol Security

a protocol suite that provides secure communication over IP networks, primarily through encryption and authentication of network traffic

to establish Virtual Private Networks (**VPNs**) and protect sensitive data transmitted across unsecured networks

#### Tunnel mode

Tunnel mode is most commonly used between gateways, acting as a **proxy** for the hosts behind it

- Transport Mode: Encrypts the payload of the IP packet and authenticates the original IP header. 
- Tunnel Mode: Encapsulates the entire IP packet within another IP packet, providing a higher level of security

## OSI Model 

Software layers:
- Application layer (layer 7)
- Presentation layer (layer 6)
- Session layer (layer 5)

Transport layer (Layer 4)

Hardware layers:
- Network layer (Layer 3)
- Datalink layer (Layer 2)
- Physical layer (Layer 1)

### Application layer



**Protocols**

- HTTP
- FTP
- DNS



### In which OSI layer is the header and trailer added?

In the OSI model, as a data packet moves from the upper to lower layers, **headers are added**. This header contains useful information.

Data link layer **trailer** is added

At layer 6,5,4,3 added header

### Layer 7: Application
- **Protocol**: HTTP, FTP, DNS
- **Message Type**: Data
- **Function**: End-user interface, application services

**Services**

- Mail services
- Directory services
- File transfer
- Access management
- Network virtual terminal

**DNS Server**

Domain Name Server: It translates Internet domains and hostnames to IP addresses and vice versa.

---

### Layer 6: Presentation
- **Protocol**: JPEG, MPEG
- **Message Type**: Data
- **Function**: Data translation, encryption, compression

---

### Layer 5: Session
- **Protocol**: RPC, PPTP
- **Message Type**: Data
- **Function**: Establish, manage, terminate sessions

---

### Layer 4: Transport
- **Protocol**: TCP, UDP, TLS
- **Message Type**: Segments (TCP), Datagrams (UDP)
- **Function**: Reliable data transfer, flow control

---

### Layer 3: Network
- **Protocol**: IP, ICMP
- **Device**: Router
- **Message Type**: Packets
- **Function**: Routing, logical addressing

**ICMP - Internet Control Message Protocol**

a network layer protocol used for sending error messages and operational information indicating success or failure when communicating with another IP address
- ping
- traceroute

**IP - Internet Protocol**

fundamental protocol that handles addressing and routing of data packets, ensuring they reach the correct destination

---

### Layer 2: Data Link
- **Protocol**: Ethernet, PPP
- **Device**: Switch, Bridge
- **Message Type**: Frames
- **Function**: Physical addressing, error detection

---

### Layer 1: Physical
- **Protocol**: USB, Ethernet (Physical)
- **Device**: Hub, Repeater
- **Message Type**: Bits
- **Function**: Physical medium, signal transmission


## zone-based firewall

source IP address, destination IP address, source port number, and destination port number are recorded.

only the replies are allowed i.e. if the traffic is Generated from inside the network then only the replies (of inside network traffic) coming from outside the network are allowed.

AWS Access Control List (ACL) & AWS Network Firewall

| AWS ACL       | AWS Network Firewall                    |
|-----------------|---------------------------------------|
| Subnet level    | VPC level   |
| Stateless  -- direction (inbound and outbound) rules     | Stateful -- considering the context of established connections      |
| Simple       | More advanced features and config options            |
| Faster           | can impact performance |
| Basic subnet security           | managing traffic across multiple VPCs, protecting against sophisticated attacks              |


## user authentication

- Something you know: Password
- Something you are: biometrics
- Something you have: token, physical key

## Common protocol and default ports

| Protocol      | Port Number | Transport Layer Protocol | Description                          |
|---------------|-------------|---------------------------|--------------------------------------|
| HTTP          | 80          | TCP                       | Web traffic (unencrypted)            |
| HTTPS         | 443         | TCP                       | Secure web traffic (SSL/TLS)         |
| FTP           | 21          | TCP                       | File Transfer Protocol (control)     |
| FTP (Data)    | 20          | TCP                       | FTP data transfer                    |
| SFTP          | 22          | TCP                       | Secure File Transfer (SSH-based)     |
| SSH           | 22          | TCP                       | Secure Shell                          |
| Telnet        | 23          | TCP                       | Remote terminal access (insecure)    |
| SMTP          | 25          | TCP                       | Simple Mail Transfer Protocol        |
| SMTPS         | 465         | TCP                       | SMTP over SSL                        |
| IMAP          | 143         | TCP                       | Internet Message Access Protocol     |
| IMAPS         | 993         | TCP                       | IMAP over SSL                        |
| POP3          | 110         | TCP                       | Post Office Protocol v3              |
| POP3S         | 995         | TCP                       | POP3 over SSL                        |
| DNS           | 53          | UDP/TCP                   | Domain Name System                   |
| DHCP (Client) | 68          | UDP                       | Dynamic Host Configuration Protocol  |
| DHCP (Server) | 67          | UDP                       | Assigns IP addresses                 |
| TFTP          | 69          | UDP                       | Trivial File Transfer Protocol       |
| SNMP          | 161         | UDP                       | Simple Network Management Protocol   |
| LDAP          | 389         | TCP/UDP                   | Lightweight Directory Access Protocol|
| RDP           | 3389        | TCP                       | Remote Desktop Protocol              |
| NTP           | 123         | UDP                       | Network Time Protocol                |
| BGP           | 179         | TCP                       | Border Gateway Protocol              |


## Multicast

Multicast is a method of group communication where the sender sends data to multiple receivers or nodes present in the network simultaneously

One-to-many communication

## TCP vs UDP

### ✅ TCP Connection Establishment

**TCP uses a 3-way handshake:**

1. **SYN** – Client sends a SYN to the server to initiate connection.
2. **SYN-ACK** – Server replies with SYN-ACK to acknowledge.
3. **ACK** – Client sends ACK to confirm.  
✅ Connection is now established.

---

### ✅ Difference Between TCP and UDP

| Feature         | TCP                            | UDP                          |
|----------------|----------------------------------|-------------------------------|
| Type           | Connection-oriented              | Connectionless                |
| Reliability    | Reliable (acknowledgments, retransmission) | Unreliable (no guarantee)     |
| Order          | Ensures ordered delivery         | No ordering guaranteed        |
| Speed          | Slower due to overhead           | Faster due to minimal overhead|
| Overhead       | Higher                           | Lower                         |

---

### ✅ Use Cases

**When to use TCP:**
- Web browsing (HTTP, HTTPS)
- Email (SMTP, IMAP, POP3)
- File transfer (FTP, SFTP)

**When to use UDP:**
- Live video/audio streaming (e.g. Zoom, YouTube Live)
- Online gaming
- DNS queries
- VoIP (e.g. Skype)

## 🔒 Reserved Private IP Ranges:

| IP Range              | Subnet Mask         | Class | Typical Use                |
|-----------------------|---------------------|-------|-----------------------------|
| 10.0.0.0 – 10.255.255.255 | 255.0.0.0 ( /8 )     | A     | Large enterprise networks   |
| 172.16.0.0 – 172.31.255.255 | 255.240.0.0 ( /12 ) | B     | Medium-sized networks       |
| 192.168.0.0 – 192.168.255.255 | 255.255.0.0 ( /16 ) | C     | Home/small office networks  |

🧠 Why Private IPs?

- **Cost-effective**: No need to buy public IPs for every device.
- **Secure**: Not accessible from the public internet directly.
- **Used with NAT** (Network Address Translation) to access the internet.