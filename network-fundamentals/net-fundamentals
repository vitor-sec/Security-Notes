# Networking Fundamentals — Study Notes

> Personal reference covering the OSI and TCP/IP models, core protocols, and the structural concepts that underpin both offensive and defensive network work.

These are study notes, not a tutorial. They exist to be referenced later, not read linearly.

---

## Network Types

| Type | Scope | Example |
|------|-------|---------|
| LAN  | Local area | Home, office |
| WAN  | Wide area | The Internet |
| MAN  | Metropolitan | Citywide |
| PAN  | Personal | Bluetooth, USB tethering |
| WLAN | Wireless LAN | Wi-Fi |

## Network Components

- **Router** — routes packets between networks, handles NAT and basic firewalling at the edge
- **Switch** — forwards frames within a network based on MAC tables; intelligent unicast forwarding
- **Hub** — legacy device; floods every frame to every port (no longer used in practice)
- **Modem** — converts carrier signal (DSL, cable, fiber) to Ethernet
- **Firewall** — enforces traffic policy at L3/L4 (stateful) or L7 (NGFW)
- **Server** — provides services (HTTP, mail, DNS)
- **Client** — consumes services

## Topologies

- **Bus** — single shared backbone; single point of failure
- **Star** — all hosts connect to a central switch; modern default
- **Ring** — closed loop, less common today
- **Mesh** — every node connects to every other; resilient, expensive

---

## Core Protocols

| Protocol | Full Name | Function |
|----------|-----------|----------|
| HTTP/HTTPS | HyperText Transfer Protocol (Secure) | Web traffic |
| FTP/SFTP | File Transfer Protocol / SSH FTP | File transfer |
| TCP | Transmission Control Protocol | Reliable, connection-oriented transport |
| UDP | User Datagram Protocol | Connectionless transport |
| IP | Internet Protocol | Addressing and routing (v4 / v6) |
| DNS | Domain Name System | Name → IP resolution |
| DHCP | Dynamic Host Configuration Protocol | Automatic IP assignment |
| SMTP / IMAP / POP3 | Mail protocols | Sending / fetching mail |
| ARP | Address Resolution Protocol | IP → MAC within a segment |
| ICMP | Internet Control Message Protocol | Diagnostics (ping, traceroute) |
| SSH | Secure Shell | Encrypted remote shell |
| LDAP | Lightweight Directory Access Protocol | Directory services (AD) |
| SNMP | Simple Network Management Protocol | Device monitoring |
| NTP | Network Time Protocol | Clock synchronization |

---

## OSI Model

| Layer | Name | Function | Examples |
|-------|------|----------|----------|
| 7 | Application | User-facing protocols | HTTP, FTP, DNS |
| 6 | Presentation | Encoding, encryption | TLS, character sets |
| 5 | Session | Session management | Session establishment/teardown |
| 4 | Transport | End-to-end delivery | TCP, UDP |
| 3 | Network | Routing, addressing | IP, ICMP |
| 2 | Data Link | Frame delivery on local segment | Ethernet, Wi-Fi, MAC |
| 1 | Physical | Bits on the wire | Copper, fiber, radio |

**Reading order for packet analysis:** bottom-up. Frames carry packets, packets carry segments, segments carry application data. Wireshark mirrors this hierarchy.

## TCP/IP Model

Condensed version of OSI, what is actually implemented in stacks:

1. **Application** — HTTP, HTTPS, DNS, SMTP
2. **Transport** — TCP, UDP
3. **Internet** — IP, ICMP
4. **Network Interface** — Ethernet, Wi-Fi

## TCP vs UDP

- **TCP** — stateful, ordered, retransmission on loss; used for web, mail, file transfer
- **UDP** — stateless, fire-and-forget; used for DNS, voice, gaming, QUIC (HTTP/3)

## Stateful vs Stateless Firewalls

- **Stateful** — tracks connection state; permits return traffic for established sessions
- **Stateless** — evaluates each packet independently against ACLs; faster but coarser

---

## DNS

### TLD Categories

- **gTLD** — generic (`.com`, `.org`, `.net`)
- **ccTLD** — country code (`.br`, `.uk`, `.us`)

### Common Record Types

| Type | Function |
|------|----------|
| A | Hostname → IPv4 |
| AAAA | Hostname → IPv6 |
| CNAME | Alias to another hostname |
| MX | Mail server (with priority) |
| TXT | Arbitrary text (SPF, DKIM, domain verification) |
| NS | Authoritative nameservers for a zone |
| PTR | Reverse lookup (IP → hostname) |

### Resolution Path

1. Local cache (browser, OS)
2. Recursive resolver (typically ISP or `1.1.1.1` / `8.8.8.8`)
3. Root servers
4. TLD servers
5. Authoritative servers for the zone

---

## Addressing

### IPv4 Private Ranges (RFC 1918)

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

Anything outside these ranges is publicly routable.

### Common Ports

| Port | Service |
|------|---------|
| 22 | SSH |
| 25 / 587 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 445 | SMB |
| 3306 | MySQL |
| 3389 | RDP |

---

## HTTP

### URL Structure

```
https://user:password@example.com:443/path?id=1&name=test#section
└─┬─┘   └──────┬─────┘ └───┬─────┘└┬┘└─┬─┘└──────┬──────┘└──┬───┘
scheme    credentials    host   port path     query      fragment
```

### Methods

| Method | Purpose |
|--------|---------|
| GET | Retrieve a resource |
| POST | Submit data (create) |
| PUT | Replace a resource (update) |
| PATCH | Partial update |
| DELETE | Remove a resource |
| HEAD | Same as GET, headers only |
| OPTIONS | Discover supported methods |

### Important Headers

**Request:**
- `Host` — virtual host targeting
- `User-Agent` — client identity
- `Cookie` — session state
- `Authorization` — credentials
- `Accept-Encoding` — supported compressions

**Response:**
- `Set-Cookie` — server sets client state
- `Content-Type` — payload MIME type
- `Cache-Control` — caching directives
- `Content-Encoding` — compression used
- `Location` — redirect target

---

## Subnetting

A subnet partitions a network into smaller segments for:
- Broadcast domain control
- Security segmentation
- Routing efficiency

Three address roles per subnet:
- **Network address** — first address (e.g., `192.168.1.0/24`)
- **Host addresses** — assignable to devices
- **Broadcast address** — last address (e.g., `192.168.1.255`)
- **Default gateway** — router interface used for off-subnet traffic

---

## DHCP Lease Process (DORA)

1. **Discover** — client broadcasts looking for a DHCP server
2. **Offer** — server proposes an IP lease
3. **Request** — client formally requests that lease
4. **Acknowledge** — server confirms; client uses the address

The server maintains a `MAC → IP` table. Each interface (each MAC) is treated as a separate client, which is why multi-interface hosts receive multiple leases.

---

## ARP

Resolves IP → MAC within a local segment.

1. Host needs to send to `192.168.1.50` but has no MAC for it
2. Broadcasts: "Who has `192.168.1.50`? Tell `192.168.1.10`"
3. The target replies with its MAC
4. Result is cached locally (the ARP cache)

ARP is unauthenticated — anyone on the segment can reply. This is the basis for ARP spoofing / MITM attacks.

---

## Frame vs Packet

- **Frame** — Layer 2 unit, carries MAC addresses, lives on a single segment
- **Packet** — Layer 3 unit, carries IP addresses, traverses networks

A packet is encapsulated in a new frame at every hop. The packet's IPs stay the same end-to-end; the frame's MACs change at each router.

---

## References

- [RFC 791](https://datatracker.ietf.org/doc/html/rfc791) — Internet Protocol
- [RFC 1918](https://datatracker.ietf.org/doc/html/rfc1918) — Private address allocation
- [RFC 2131](https://datatracker.ietf.org/doc/html/rfc2131) — DHCP
- [RFC 9110](https://datatracker.ietf.org/doc/html/rfc9110) — HTTP Semantics
- Tanenbaum, A. *Computer Networks* (5th ed.)
- Stevens, W. R. *TCP/IP Illustrated, Vol 1*
