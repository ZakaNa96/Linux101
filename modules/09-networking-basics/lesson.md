# Module 9: Basic Networking

## 🎯 Learning Goal

By the end of this module, you will understand fundamental networking concepts, be able to view and troubleshoot network configurations, test connectivity, use SSH for remote access, and download files from the internet using command-line tools.

---

## Table of Contents

1. [Networking Fundamentals](#1-networking-fundamentals)
2. [Viewing Network Configuration](#2-viewing-network-configuration)
3. [Testing Network Connectivity](#3-testing-network-connectivity)
4. [DNS and Name Resolution](#4-dns-and-name-resolution)
5. [Routing Basics](#5-routing-basics)
6. [Network Statistics and Connections](#6-network-statistics-and-connections)
7. [Downloading Files](#7-downloading-files)
8. [Remote Connections with SSH](#8-remote-connections-with-ssh)
9. [Network Configuration Files](#9-network-configuration-files)
10. [Firewall Basics](#10-firewall-basics)
11. [Useful Network Tools on Kali](#11-useful-network-tools-on-kali)
12. [Troubleshooting Network Issues](#12-troubleshooting-network-issues)

---

## 1. Networking Fundamentals

### What is a Network?

A **network** is a collection of computers and devices connected together to share resources and communicate with each other. Networks can be as small as two computers connected directly, or as large as the internet, which connects billions of devices worldwide.

### IP Addresses (IPv4 Basics)

An **IP address** (Internet Protocol address) is a unique numerical label assigned to each device on a network. Think of it like a postal address for computers.

**IPv4 addresses** consist of four numbers separated by dots, each ranging from 0 to 255:

```
192.168.1.100
10.0.0.5
172.16.254.1
```

Each device on a network needs a unique IP address to communicate.

### Public vs Private IP Addresses

**Private IP addresses** are used within local networks (like your home or office). These addresses are not routable on the internet:

| Range | Class | Common Use |
|-------|-------|------------|
| 10.0.0.0 - 10.255.255.255 | Class A | Large networks |
| 172.16.0.0 - 172.31.255.255 | Class B | Medium networks |
| 192.168.0.0 - 192.168.255.255 | Class C | Home/small networks |

**Public IP addresses** are unique addresses assigned by Internet Service Providers (ISPs) that are routable on the internet.

💡 **Tip:** Your computer likely has a private IP address. Your router has both a private address (facing your network) and a public address (facing the internet).

### Subnet Masks

A **subnet mask** determines which part of an IP address identifies the network and which part identifies the host (device).

Common subnet masks:
- `255.255.255.0` (or /24) - 256 addresses, 254 usable hosts
- `255.255.0.0` (or /16) - 65,536 addresses
- `255.0.0.0` (or /8) - 16.7 million addresses

For example, with IP `192.168.1.100` and subnet `255.255.255.0`:
- Network: `192.168.1.0`
- Host: `100`

### Ports and Protocols

**Ports** are like doors on a computer that different services use to communicate. They're numbered from 0 to 65535.

**Protocols** are rules that define how data is transmitted. The two main transport protocols are:

| Protocol | TCP | UDP |
|----------|-----|-----|
| Full Name | Transmission Control Protocol | User Datagram Protocol |
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower but reliable | Faster but may lose data |
| Use Cases | Web, email, file transfer | Streaming, gaming, DNS |

### Common Ports

As a system administrator, you should memorize these common ports:

| Port | Service | Protocol | Description |
|------|---------|----------|-------------|
| 20, 21 | FTP | TCP | File Transfer Protocol |
| 22 | SSH | TCP | Secure Shell (remote access) |
| 23 | Telnet | TCP | Unencrypted remote access (deprecated) |
| 25 | SMTP | TCP | Email sending |
| 53 | DNS | TCP/UDP | Domain Name System |
| 80 | HTTP | TCP | Web traffic (unencrypted) |
| 443 | HTTPS | TCP | Web traffic (encrypted) |
| 3306 | MySQL | TCP | MySQL database |
| 5432 | PostgreSQL | TCP | PostgreSQL database |

⚠️ **Warning:** Ports below 1024 are "privileged ports" and require root access to bind to.

---

## 2. Viewing Network Configuration

### The `ip` Command (Modern)

The `ip` command is the modern standard for network configuration in Linux.

**Show IP addresses:**
```bash
ip addr
# or shortened:
ip a
```

Example output:
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    inet 192.168.1.100/24 brd 192.168.1.255 scope global eth0
```

**Show network interfaces:**
```bash
ip link
# or shortened:
ip l
```

### Understanding Interface Names

| Interface | Description |
|-----------|-------------|
| `lo` | Loopback interface (localhost, 127.0.0.1) |
| `eth0`, `eth1` | Traditional Ethernet interface names |
| `enp0s3` | Predictable Ethernet name (en=ethernet, p0=bus, s3=slot) |
| `wlan0` | Traditional wireless interface name |
| `wlp2s0` | Predictable wireless name |
| `virbr0` | Virtual bridge (virtualization) |
| `docker0` | Docker network interface |

💡 **Tip:** In VirtualBox, your network interface is typically named `eth0` or something like `enp0s3`.

### The `ifconfig` Command (Legacy)

The older `ifconfig` command is still found on many systems:

```bash
ifconfig
```

To see all interfaces (including down ones):
```bash
ifconfig -a
```

⚠️ **Warning:** `ifconfig` is deprecated in favor of `ip`. However, you'll still encounter it in scripts and documentation, so it's good to know.

### Quick IP Display

For a quick display of your IP addresses:

```bash
hostname -I
```

This shows all IP addresses assigned to your machine, space-separated.

---

## 3. Testing Network Connectivity

### The `ping` Command

`ping` sends ICMP echo requests to test if a host is reachable:

```bash
ping google.com
```

This will continue indefinitely until you press `Ctrl+C`.

**Limited number of pings:**
```bash
ping -c 4 google.com
```

Example output:
```
PING google.com (142.250.185.78) 56(84) bytes of data.
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=1 ttl=117 time=12.3 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=2 ttl=117 time=11.8 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=3 ttl=117 time=12.1 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=4 ttl=117 time=11.9 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
rtt min/avg/max/mdev = 11.800/12.025/12.300/0.183 ms
```

### Understanding Ping Output

| Field | Meaning |
|-------|---------|
| `64 bytes` | Size of the reply packet |
| `icmp_seq=1` | Sequence number (order of packets) |
| `ttl=117` | Time To Live (max hops before packet dies) |
| `time=12.3 ms` | Round-trip time in milliseconds |
| `packet loss` | Percentage of packets that didn't return |
| `rtt` | Round-trip time statistics |

### Testing Loopback

Test your local network stack:

```bash
ping -c 4 localhost
# or
ping -c 4 127.0.0.1
```

If this fails, there's a serious problem with your network configuration.

### Why Ping Might Fail

If ping doesn't work, consider these reasons:

1. **Host is down** - The target machine is offline
2. **Firewall blocking** - Many servers block ICMP (ping) requests
3. **No route to host** - Network path doesn't exist
4. **DNS failure** - Can't resolve hostname (try pinging IP instead)
5. **Network disconnected** - Your network cable or WiFi is down

💡 **Tip:** If `ping google.com` fails but `ping 8.8.8.8` works, you have a DNS problem, not a connectivity problem.

---

## 4. DNS and Name Resolution

### What is DNS?

**DNS (Domain Name System)** translates human-readable domain names into IP addresses. It's like a phone book for the internet.

When you type `google.com`:
1. Your computer asks a DNS server "What's the IP for google.com?"
2. DNS server responds with `142.250.185.78`
3. Your computer connects to that IP address

### Local Name Resolution: `/etc/hosts`

The `/etc/hosts` file provides local name resolution that bypasses DNS:

```bash
cat /etc/hosts
```

Example contents:
```
127.0.0.1       localhost
127.0.1.1       kali
::1             localhost ip6-localhost ip6-loopback
```

You can add custom entries:
```bash
sudo nano /etc/hosts
```

Add a line like:
```
192.168.1.50    myserver
```

Now you can use `myserver` instead of the IP address.

💡 **Tip:** This is useful for development, testing, or blocking websites (point them to 127.0.0.1).

### DNS Configuration: `/etc/resolv.conf`

This file specifies your DNS servers:

```bash
cat /etc/resolv.conf
```

Example:
```
nameserver 8.8.8.8
nameserver 8.8.4.4
```

⚠️ **Warning:** On modern systems, this file is often managed by NetworkManager or systemd-resolved. Manual changes may be overwritten.

### DNS Lookup Commands

**`host` - Simple DNS lookup:**
```bash
host google.com
```

Output:
```
google.com has address 142.250.185.78
google.com has IPv6 address 2a00:1450:4001:829::200e
google.com mail is handled by 10 smtp.google.com.
```

**`dig` - Detailed DNS information:**
```bash
dig google.com
```

This shows much more detail including:
- Query time
- DNS server used
- TTL (Time To Live)
- Record types

**Query specific record types:**
```bash
dig google.com MX      # Mail records
dig google.com NS      # Name servers
dig google.com TXT     # Text records
dig google.com +short  # Brief output
```

**`nslookup` - Interactive DNS query:**
```bash
nslookup google.com
```

Or query a specific DNS server:
```bash
nslookup google.com 8.8.8.8
```

---

## 5. Routing Basics

### What is Routing?

**Routing** is the process of selecting paths in a network to send data from source to destination. Your computer uses a **routing table** to determine where to send packets.

### Viewing the Routing Table

```bash
ip route
# or shortened:
ip r
```

Example output:
```
default via 192.168.1.1 dev eth0 proto dhcp metric 100
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.100 metric 100
```

### Understanding the Routing Table

| Entry | Meaning |
|-------|---------|
| `default via 192.168.1.1` | Default gateway (where to send unknown destinations) |
| `dev eth0` | Use interface eth0 |
| `192.168.1.0/24` | Local network (direct connection, no gateway needed) |
| `metric 100` | Priority (lower = preferred) |

### Default Gateway

The **default gateway** is the router that forwards traffic to other networks (including the internet). It's usually your home router.

To find your default gateway:
```bash
ip route | grep default
```

### Tracing Network Path

**`traceroute` - Show the path packets take:**
```bash
traceroute google.com
```

Example output:
```
traceroute to google.com (142.250.185.78), 30 hops max
 1  _gateway (192.168.1.1)  0.724 ms
 2  10.0.0.1 (10.0.0.1)  10.274 ms
 3  isp-router.example.com (203.0.113.1)  15.332 ms
 ...
```

Each line represents a "hop" (router) along the path.

**`tracepath` - Alternative without root:**
```bash
tracepath google.com
```

💡 **Tip:** `traceroute` may require installation: `sudo apt install traceroute`

⚠️ **Warning:** Some routers don't respond to traceroute packets, showing `* * *` instead.

---

## 6. Network Statistics and Connections

### The `ss` Command (Modern)

`ss` (socket statistics) is the modern replacement for `netstat`:

**Show all connections:**
```bash
ss
```

**Show listening TCP and UDP ports:**
```bash
ss -tuln
```

Options explained:
| Option | Meaning |
|--------|---------|
| `-t` | TCP sockets |
| `-u` | UDP sockets |
| `-l` | Listening sockets only |
| `-n` | Show numbers (don't resolve names) |
| `-p` | Show process using socket |
| `-a` | All sockets (listening and non-listening) |

**Show listening ports with process names (requires root):**
```bash
sudo ss -tulnp
```

Example output:
```
Netid  State   Recv-Q  Send-Q  Local Address:Port  Peer Address:Port  Process
tcp    LISTEN  0       128     0.0.0.0:22          0.0.0.0:*          users:(("sshd",pid=1234))
tcp    LISTEN  0       511     0.0.0.0:80          0.0.0.0:*          users:(("apache2",pid=5678))
```

### The `netstat` Command (Legacy)

`netstat` is older but still widely used:

**Show listening ports:**
```bash
netstat -tuln
```

**Show with process names:**
```bash
sudo netstat -tulnp
```

**Show all connections:**
```bash
netstat -an
```

💡 **Tip:** If `netstat` isn't installed: `sudo apt install net-tools`

### The `lsof` Command

`lsof` (list open files) can show network connections:

**Show all network connections:**
```bash
sudo lsof -i
```

**Show what's using a specific port:**
```bash
sudo lsof -i :22
```

**Show connections to a specific host:**
```bash
sudo lsof -i @google.com
```

---

## 7. Downloading Files

### `wget` - Download Files

`wget` is a powerful command-line downloader:

**Basic download:**
```bash
wget https://example.com/file.zip
```

**Save with a different name:**
```bash
wget -O newname.zip https://example.com/file.zip
```

**Download in background:**
```bash
wget -b https://example.com/largefile.iso
```

**Resume interrupted download:**
```bash
wget -c https://example.com/largefile.iso
```

**Download quietly (no output):**
```bash
wget -q https://example.com/file.zip
```

**Limit download speed:**
```bash
wget --limit-rate=1m https://example.com/file.zip
```

### `curl` - Transfer Data

`curl` is more versatile than `wget`:

**Display webpage content:**
```bash
curl https://example.com
```

**Download file (original name):**
```bash
curl -O https://example.com/file.zip
```

**Download with custom name:**
```bash
curl -o newname.zip https://example.com/file.zip
```

**Follow redirects:**
```bash
curl -L https://example.com/redirect
```

**Show response headers:**
```bash
curl -I https://example.com
```

**Download silently:**
```bash
curl -s https://example.com/file.zip -o file.zip
```

### `wget` vs `curl`

| Feature | wget | curl |
|---------|------|------|
| **Primary use** | Download files | Transfer data |
| **Recursive download** | Yes (`wget -r`) | No |
| **Resume downloads** | Yes (`wget -c`) | Yes (`curl -C -`) |
| **Protocols** | HTTP, HTTPS, FTP | Many (HTTP, FTP, SFTP, SCP, etc.) |
| **Output** | Saves to file by default | Outputs to screen by default |
| **API testing** | Limited | Excellent |
| **Follow redirects** | Yes (by default) | Needs `-L` flag |

💡 **Tip:** Use `wget` for downloading files and websites. Use `curl` for API testing and one-off requests.

---

## 8. Remote Connections with SSH

### What is SSH?

**SSH (Secure Shell)** is a protocol for secure remote access to another computer. It encrypts all traffic, including your password.

### Basic SSH Connection

```bash
ssh username@hostname
```

Example:
```bash
ssh admin@192.168.1.50
ssh root@myserver.com
```

**Connect on a custom port:**
```bash
ssh -p 2222 user@hostname
```

### First Connection (Host Key)

The first time you connect to a new server, you'll see:

```
The authenticity of host 'server (192.168.1.50)' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type `yes` to accept. The key is saved in `~/.ssh/known_hosts`.

⚠️ **Warning:** If you see this message for a server you've connected to before, someone might be intercepting your connection (man-in-the-middle attack). Verify before proceeding!

### SSH Key Authentication

SSH keys are more secure than passwords:

**Generate an SSH key pair:**
```bash
ssh-keygen -t ed25519
```

This creates:
- `~/.ssh/id_ed25519` - Private key (keep secret!)
- `~/.ssh/id_ed25519.pub` - Public key (share this)

**Copy public key to server:**
```bash
ssh-copy-id user@hostname
```

Now you can log in without a password!

### Secure Copy with `scp`

`scp` copies files securely over SSH:

**Copy file TO remote server:**
```bash
scp localfile.txt user@host:/remote/path/
```

**Copy file FROM remote server:**
```bash
scp user@host:/remote/file.txt ./local/path/
```

**Copy directory recursively:**
```bash
scp -r localdir/ user@host:/remote/path/
```

**Using custom port:**
```bash
scp -P 2222 file.txt user@host:/path/
```

💡 **Tip:** Note that `scp` uses uppercase `-P` for port, while `ssh` uses lowercase `-p`.

### SSH Configuration

Create `~/.ssh/config` for easier connections:

```
Host myserver
    HostName 192.168.1.50
    User admin
    Port 22

Host devbox
    HostName dev.example.com
    User developer
    Port 2222
    IdentityFile ~/.ssh/dev_key
```

Now you can simply type:
```bash
ssh myserver
ssh devbox
```

---

## 9. Network Configuration Files

### Debian/Ubuntu: `/etc/network/interfaces`

Traditional network configuration file:

```bash
cat /etc/network/interfaces
```

Example static IP configuration:
```
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

Example DHCP configuration:
```
auto eth0
iface eth0 inet dhcp
```

### NetworkManager (Modern Systems)

Most modern Linux distributions use NetworkManager:

**Check NetworkManager status:**
```bash
systemctl status NetworkManager
```

**NetworkManager CLI (`nmcli`):**

```bash
# Show all connections
nmcli connection show

# Show device status
nmcli device status

# Show detailed connection info
nmcli connection show "Wired connection 1"

# Connect to a network
nmcli device connect eth0

# Disconnect
nmcli device disconnect eth0
```

**Create a static IP connection:**
```bash
nmcli connection add type ethernet con-name "Static" ifname eth0 \
    ipv4.addresses 192.168.1.100/24 \
    ipv4.gateway 192.168.1.1 \
    ipv4.dns "8.8.8.8" \
    ipv4.method manual
```

### DHCP vs Static IP

| Aspect | DHCP | Static IP |
|--------|------|-----------|
| **Configuration** | Automatic | Manual |
| **IP Address** | May change | Always the same |
| **Use Case** | Workstations, laptops | Servers, network devices |
| **Maintenance** | Easier | Requires planning |
| **Conflicts** | DHCP server prevents them | Must avoid manually |

💡 **Tip:** Servers typically need static IPs so clients can always find them. Workstations usually use DHCP.

---

## 10. Firewall Basics

### What is a Firewall?

A **firewall** controls incoming and outgoing network traffic based on rules. It's a critical security component.

### `iptables` (Traditional)

`iptables` is the traditional Linux firewall:

**View current rules:**
```bash
sudo iptables -L -n -v
```

**Common iptables examples:**
```bash
# Allow incoming SSH
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow incoming HTTP
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT

# Block an IP address
sudo iptables -A INPUT -s 192.168.1.100 -j DROP

# Allow established connections
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

⚠️ **Warning:** `iptables` rules are complex and can lock you out if misconfigured. Be careful, especially with SSH!

### `ufw` - Uncomplicated Firewall

`ufw` is a simpler frontend for `iptables`:

**Install ufw:**
```bash
sudo apt install ufw
```

**Check status:**
```bash
sudo ufw status
sudo ufw status verbose
```

**Enable/disable firewall:**
```bash
sudo ufw enable
sudo ufw disable
```

**Allow/deny ports:**
```bash
sudo ufw allow 22        # Allow SSH
sudo ufw allow 80/tcp    # Allow HTTP
sudo ufw allow 443       # Allow HTTPS
sudo ufw deny 23         # Deny Telnet
```

**Allow by service name:**
```bash
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
```

**Delete a rule:**
```bash
sudo ufw delete allow 80
```

**Reset all rules:**
```bash
sudo ufw reset
```

### Kali Linux Firewall State

💡 **Tip:** By default, Kali Linux doesn't have a restrictive firewall enabled. This is intentional for penetration testing. In production environments, always configure a firewall!

Check Kali's firewall status:
```bash
sudo iptables -L
sudo ufw status
```

---

## 11. Useful Network Tools on Kali

Kali Linux comes with powerful network tools. Here's a brief introduction:

### `nmap` - Network Scanner

`nmap` scans networks and discovers hosts/services:

```bash
# Scan a single host
nmap 192.168.1.1

# Scan a network range
nmap 192.168.1.0/24

# Quick scan (common ports only)
nmap -F 192.168.1.1

# Detect services and versions
nmap -sV 192.168.1.1

# Scan specific ports
nmap -p 22,80,443 192.168.1.1
```

### `netcat` (nc) - Network Swiss Army Knife

`netcat` can read/write network connections:

```bash
# Connect to a port
nc hostname 80

# Listen on a port
nc -l -p 1234

# Simple chat (on two machines)
# Machine 1: nc -l -p 1234
# Machine 2: nc machine1 1234

# Port scanning
nc -zv hostname 20-25
```

### `whois` - Domain Information

```bash
whois google.com
```

Shows domain registration information, owner, registrar, etc.

### `arp` - ARP Table

View the ARP (Address Resolution Protocol) table:

```bash
arp -a
# or
ip neigh
```

This shows the mapping between IP addresses and MAC addresses on your local network.

⚠️ **IMPORTANT WARNING:**

**These tools are for authorized testing only!** Using them against systems without permission is:
- **Illegal** in most jurisdictions
- **Unethical** and harmful
- **Grounds for criminal prosecution**

Only use these tools:
- On your own systems
- On systems you have explicit written permission to test
- In authorized lab environments

---

## 12. Troubleshooting Network Issues

### Systematic Troubleshooting Approach

When network isn't working, follow this checklist:

**Step 1: Check if interface is up**
```bash
ip link show
# Look for "UP" in the output
```

If interface is down:
```bash
sudo ip link set eth0 up
```

**Step 2: Check IP configuration**
```bash
ip addr show
# Do you have an IP address?
```

If no IP via DHCP:
```bash
sudo dhclient eth0
```

**Step 3: Test local connectivity (ping gateway)**
```bash
ip route | grep default
# Note the gateway IP, then:
ping -c 4 192.168.1.1  # your gateway
```

**Step 4: Test internet connectivity**
```bash
ping -c 4 8.8.8.8
```

**Step 5: Test DNS resolution**
```bash
ping -c 4 google.com
```

### Common Problems and Solutions

| Problem | Symptom | Solution |
|---------|---------|----------|
| Interface down | `ip link` shows "DOWN" | `sudo ip link set eth0 up` |
| No IP address | `ip addr` shows no inet | `sudo dhclient eth0` |
| Gateway unreachable | Can't ping gateway | Check cable/WiFi, check gateway IP |
| Internet unreachable | Can ping gateway, not 8.8.8.8 | Check router's internet connection |
| DNS failure | Can ping IP, not domain | Check `/etc/resolv.conf`, try `8.8.8.8` |
| Service unreachable | Can't connect to specific port | Check if service running, check firewall |

### Network Troubleshooting Commands Summary

```bash
# Check interfaces
ip link
ip addr

# Check connectivity
ping gateway
ping 8.8.8.8
ping google.com

# Check routing
ip route
traceroute target

# Check DNS
cat /etc/resolv.conf
dig google.com
host google.com

# Check ports/services
ss -tuln
sudo lsof -i :port

# Check firewall
sudo iptables -L
sudo ufw status
```

💡 **Tip:** The "OSI model" provides a framework for troubleshooting: Physical → Data Link → Network → Transport → Application. Start at the bottom!

---

## Summary: Essential Networking Commands

### Viewing Configuration
| Command | Purpose |
|---------|---------|
| `ip addr` or `ip a` | Show IP addresses |
| `ip link` or `ip l` | Show network interfaces |
| `ip route` or `ip r` | Show routing table |
| `hostname -I` | Quick IP display |

### Testing Connectivity
| Command | Purpose |
|---------|---------|
| `ping host` | Test connectivity |
| `traceroute host` | Trace packet path |
| `tracepath host` | Trace without root |

### DNS
| Command | Purpose |
|---------|---------|
| `host domain` | Simple DNS lookup |
| `dig domain` | Detailed DNS lookup |
| `nslookup domain` | DNS query |

### Connections & Ports
| Command | Purpose |
|---------|---------|
| `ss -tuln` | Show listening ports |
| `netstat -tuln` | Show listening ports (legacy) |
| `sudo lsof -i` | Show network connections |

### Downloading
| Command | Purpose |
|---------|---------|
| `wget URL` | Download files |
| `curl URL` | Transfer data |

### Remote Access
| Command | Purpose |
|---------|---------|
| `ssh user@host` | Remote login |
| `scp file user@host:/path` | Secure copy |

### Firewall
| Command | Purpose |
|---------|---------|
| `sudo ufw status` | Check firewall status |
| `sudo ufw allow 22` | Allow port |
| `sudo ufw enable` | Enable firewall |

---

## ✅ What You Learned

In this module, you learned:

- ✅ Fundamental networking concepts (IP addresses, ports, protocols)
- ✅ How to view and understand network configuration
- ✅ Testing network connectivity with `ping` and `traceroute`
- ✅ How DNS works and DNS lookup commands
- ✅ Understanding routing and the default gateway
- ✅ Viewing network connections and open ports
- ✅ Downloading files with `wget` and `curl`
- ✅ Secure remote access with SSH and `scp`
- ✅ Network configuration files and NetworkManager
- ✅ Basic firewall management with `ufw`
- ✅ Introduction to Kali's networking tools
- ✅ Systematic network troubleshooting

---

## Next Steps

In the next module, you'll learn about **disk management and storage** - how to view disk usage, mount drives, and manage storage in Linux!