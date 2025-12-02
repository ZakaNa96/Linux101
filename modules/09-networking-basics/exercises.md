# Module 9: Exercises - Basic Networking

Practice these exercises to reinforce your understanding of Linux networking commands and concepts.

---

## Exercise 1: Explore Your Network

**Objective:** Understand your system's network configuration.

### Tasks

1. **Find your IP address(es):**
   ```bash
   ip addr
   ```
   
   Write down:
   - Your IP address: _______________
   - Your subnet mask (in CIDR notation, e.g., /24): _______________

2. **Identify all network interfaces:**
   ```bash
   ip link
   ```
   
   List all interfaces you see:
   - _______________________________
   - _______________________________
   - _______________________________

3. **Find your default gateway:**
   ```bash
   ip route | grep default
   ```
   
   Your gateway IP: _______________

4. **Display your DNS servers:**
   ```bash
   cat /etc/resolv.conf
   ```
   
   DNS servers: _______________

5. **Check your hostname:**
   ```bash
   hostname
   hostname -I
   ```

### Expected Output

Your `ip addr` output should look similar to:
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
    inet 127.0.0.1/8 scope host lo
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
```

<details>
<summary>💡 Hints</summary>

- The `lo` interface is the loopback (always 127.0.0.1)
- Look for `inet` followed by an IP address
- Your main interface is probably `eth0` or starts with `enp`
- In VirtualBox, you might see `10.0.2.15` if using NAT mode

</details>

<details>
<summary>✅ Solution</summary>

```bash
# Complete exploration script
echo "=== IP Addresses ==="
ip addr show

echo -e "\n=== Network Interfaces ==="
ip link show

echo -e "\n=== Default Gateway ==="
ip route | grep default

echo -e "\n=== DNS Servers ==="
cat /etc/resolv.conf

echo -e "\n=== Hostname ==="
hostname
echo "IP: $(hostname -I)"
```

</details>

---

## Exercise 2: Test Connectivity

**Objective:** Verify network connectivity at different levels.

### Tasks

1. **Ping localhost (test your network stack):**
   ```bash
   ping -c 4 localhost
   ```
   
   Did all 4 packets return? _______________

2. **Ping your default gateway:**
   ```bash
   # Replace with YOUR gateway IP from Exercise 1
   ping -c 4 192.168.1.1
   ```
   
   Average response time: _______________ ms

3. **Ping an external IP (Google's DNS):**
   ```bash
   ping -c 4 8.8.8.8
   ```
   
   Packet loss: _______________%

4. **Ping a domain name:**
   ```bash
   ping -c 4 google.com
   ```
   
   What IP address did it resolve to? _______________

5. **Trace the route to a website:**
   ```bash
   traceroute -m 15 google.com
   # or if traceroute isn't installed:
   tracepath google.com
   ```
   
   How many hops to reach Google? _______________

### Expected Output

Successful ping output:
```
PING google.com (142.250.185.78) 56(84) bytes of data.
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=1 ttl=117 time=12.3 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=2 ttl=117 time=11.8 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=3 ttl=117 time=12.1 ms
64 bytes from fra16s51-in-f14.1e100.net: icmp_seq=4 ttl=117 time=11.9 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3005ms
```

<details>
<summary>💡 Hints</summary>

- If ping to `8.8.8.8` works but `google.com` fails, you have a DNS problem
- If ping to gateway fails, check your cable/WiFi or VirtualBox network settings
- Some networks block ICMP (ping), so failure doesn't always mean no connectivity
- `traceroute` might need installation: `sudo apt install traceroute`

</details>

<details>
<summary>✅ Solution</summary>

```bash
#!/bin/bash
# Connectivity test script

echo "=== Testing localhost ==="
ping -c 2 localhost

echo -e "\n=== Testing gateway ==="
GATEWAY=$(ip route | grep default | awk '{print $3}')
echo "Gateway: $GATEWAY"
ping -c 2 $GATEWAY

echo -e "\n=== Testing internet (IP) ==="
ping -c 2 8.8.8.8

echo -e "\n=== Testing DNS resolution ==="
ping -c 2 google.com

echo -e "\n=== Traceroute ==="
traceroute -m 10 google.com 2>/dev/null || tracepath -m 10 google.com
```

</details>

---

## Exercise 3: DNS Exploration

**Objective:** Understand DNS resolution and manipulation.

### Tasks

1. **Look up an IP address for a domain:**
   ```bash
   host google.com
   ```
   
   IP address(es): _______________

2. **Use dig for detailed DNS information:**
   ```bash
   dig google.com
   ```
   
   Find and note:
   - Query time: _______________ ms
   - DNS server used: _______________
   - TTL (Time To Live): _______________

3. **Look up specific record types:**
   ```bash
   dig google.com MX      # Mail servers
   dig google.com NS      # Name servers
   dig +short google.com  # Brief output
   ```

4. **Add a custom entry to /etc/hosts:**
   ```bash
   # First, backup the file
   sudo cp /etc/hosts /etc/hosts.backup
   
   # Add a custom entry
   echo "127.0.0.1 mytest.local" | sudo tee -a /etc/hosts
   
   # Verify it was added
   cat /etc/hosts
   ```

5. **Test the custom entry:**
   ```bash
   ping -c 2 mytest.local
   ```
   
   What IP does it resolve to? _______________

6. **Clean up - remove the custom entry:**
   ```bash
   sudo cp /etc/hosts.backup /etc/hosts
   ```

### Expected Output

`dig google.com` partial output:
```
;; ANSWER SECTION:
google.com.		276	IN	A	142.250.185.78

;; Query time: 23 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
```

<details>
<summary>💡 Hints</summary>

- `host` gives simple output, `dig` gives detailed output
- The TTL tells you how long the DNS record is cached
- `/etc/hosts` is checked before DNS servers
- Always backup before editing system files!

</details>

<details>
<summary>✅ Solution</summary>

```bash
#!/bin/bash
# DNS exploration script

echo "=== Simple DNS lookup ==="
host google.com

echo -e "\n=== Detailed DNS lookup ==="
dig google.com

echo -e "\n=== Mail servers ==="
dig +short google.com MX

echo -e "\n=== Name servers ==="
dig +short google.com NS

echo -e "\n=== Testing /etc/hosts ==="
# Backup
sudo cp /etc/hosts /etc/hosts.backup

# Add entry
echo "127.0.0.1 mytest.local" | sudo tee -a /etc/hosts

# Test
echo "Testing mytest.local:"
ping -c 1 mytest.local

# Restore
sudo cp /etc/hosts.backup /etc/hosts
echo "Restored original /etc/hosts"
```

</details>

---

## Exercise 4: Check Network Connections

**Objective:** View active network connections and listening services.

### Tasks

1. **List all listening TCP and UDP ports:**
   ```bash
   ss -tuln
   ```
   
   List 3 ports that are listening:
   - Port: _____ Protocol: _____
   - Port: _____ Protocol: _____
   - Port: _____ Protocol: _____

2. **Find what process is using port 22 (if SSH is running):**
   ```bash
   sudo ss -tulnp | grep :22
   ```
   
   Process name: _______________

3. **View all active connections:**
   ```bash
   ss -tun
   ```

4. **Use lsof to see network connections:**
   ```bash
   sudo lsof -i -P -n | head -20
   ```

5. **Check if a specific port is in use:**
   ```bash
   sudo lsof -i :80
   # or
   ss -tuln | grep :80
   ```

### Expected Output

`ss -tuln` output example:
```
Netid  State   Recv-Q  Send-Q  Local Address:Port   Peer Address:Port
udp    UNCONN  0       0       0.0.0.0:68           0.0.0.0:*
tcp    LISTEN  0       128     0.0.0.0:22           0.0.0.0:*
tcp    LISTEN  0       511     0.0.0.0:80           0.0.0.0:*
```

<details>
<summary>💡 Hints</summary>

- `0.0.0.0` means listening on all interfaces
- `127.0.0.1` means listening only on localhost
- `LISTEN` state means waiting for connections
- You need `sudo` to see process names with `-p`

</details>

<details>
<summary>✅ Solution</summary>

```bash
#!/bin/bash
# Network connections exploration

echo "=== Listening ports (ss) ==="
ss -tuln

echo -e "\n=== Listening ports with processes ==="
sudo ss -tulnp

echo -e "\n=== Active connections ==="
ss -tun

echo -e "\n=== Network connections (lsof) ==="
sudo lsof -i -P -n 2>/dev/null | head -20

echo -e "\n=== Check specific ports ==="
for port in 22 80 443; do
    echo -n "Port $port: "
    if ss -tuln | grep -q ":$port "; then
        echo "IN USE"
    else
        echo "not in use"
    fi
done
```

</details>

---

## Exercise 5: Download Files

**Objective:** Practice downloading files with wget and curl.

### Tasks

1. **Download a file with wget:**
   ```bash
   cd /tmp
   wget https://www.google.com/robots.txt
   ls -la robots.txt
   cat robots.txt
   ```

2. **Download and rename with wget:**
   ```bash
   wget -O google-robots.txt https://www.google.com/robots.txt
   ls -la google-robots.txt
   ```

3. **Use curl to view a webpage:**
   ```bash
   curl https://example.com
   ```
   
   What does it show? _______________

4. **Download a file with curl:**
   ```bash
   curl -O https://www.google.com/robots.txt
   # or with custom name:
   curl -o curl-robots.txt https://www.google.com/robots.txt
   ```

5. **View only HTTP headers:**
   ```bash
   curl -I https://google.com
   ```
   
   HTTP status code: _______________

6. **Compare the files:**
   ```bash
   diff google-robots.txt curl-robots.txt
   ```
   
   Are they identical? _______________

7. **Clean up:**
   ```bash
   rm -f robots.txt google-robots.txt curl-robots.txt
   ```

### Expected Output

`curl -I https://google.com`:
```
HTTP/2 301
location: https://www.google.com/
content-type: text/html; charset=UTF-8
```

<details>
<summary>💡 Hints</summary>

- `wget` saves to a file by default
- `curl` outputs to screen by default (use `-O` or `-o` to save)
- `-I` with curl shows only headers (good for checking if URL exists)
- Status 200 = OK, 301/302 = redirect, 404 = not found

</details>

<details>
<summary>✅ Solution</summary>

```bash
#!/bin/bash
# Download practice script

cd /tmp

echo "=== Download with wget ==="
wget -q https://www.google.com/robots.txt
echo "Downloaded: $(ls -la robots.txt)"

echo -e "\n=== Download with wget (renamed) ==="
wget -q -O google-robots.txt https://www.google.com/robots.txt
echo "Downloaded: $(ls -la google-robots.txt)"

echo -e "\n=== View webpage with curl ==="
curl -s https://example.com | head -10

echo -e "\n=== Download with curl ==="
curl -s -o curl-robots.txt https://www.google.com/robots.txt
echo "Downloaded: $(ls -la curl-robots.txt)"

echo -e "\n=== HTTP headers ==="
curl -I -s https://google.com | head -5

echo -e "\n=== Compare files ==="
if diff -q google-robots.txt curl-robots.txt > /dev/null; then
    echo "Files are identical"
else
    echo "Files differ"
fi

echo -e "\n=== Cleanup ==="
rm -f robots.txt google-robots.txt curl-robots.txt
echo "Cleaned up temporary files"
```

</details>

---

## Exercise 6: SSH Practice

**Objective:** Practice SSH connections and secure file copying.

⚠️ **Note:** These exercises work best if you have SSH service running. On Kali, SSH might not be running by default.

### Tasks

1. **Check if SSH service is running:**
   ```bash
   systemctl status ssh
   ```
   
   Is it running? _______________
   
   If not running, start it:
   ```bash
   sudo systemctl start ssh
   ```

2. **Connect to localhost via SSH:**
   ```bash
   ssh $(whoami)@localhost
   ```
   
   - Accept the host key if prompted (type `yes`)
   - Enter your password
   - You're now in a new SSH session!
   - Type `exit` to disconnect

3. **Create a test file and copy it with scp:**
   ```bash
   # Create a test file
   echo "Hello from scp test!" > /tmp/scp-test.txt
   
   # Copy it to your home directory via SCP
   scp /tmp/scp-test.txt $(whoami)@localhost:~/scp-received.txt
   
   # Verify it was copied
   cat ~/scp-received.txt
   ```

4. **View SSH host keys:**
   ```bash
   cat ~/.ssh/known_hosts
   ```
   
   What type of key is stored? _______________

5. **Clean up:**
   ```bash
   rm -f /tmp/scp-test.txt ~/scp-received.txt
   ```

### Expected Output

SSH connection:
```
$ ssh user@localhost
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ED25519 key fingerprint is SHA256:xxxxxxxxxxxxxxxxx.
Are you sure you want to continue connecting (yes/no)? yes
user@localhost's password: 
Welcome to Kali Linux!
user@kali:~$ exit
logout
Connection to localhost closed.
```

<details>
<summary>💡 Hints</summary>

- Kali's SSH service is `ssh`, not `sshd` in systemctl
- The first connection always asks about the fingerprint
- `known_hosts` stores fingerprints of servers you've connected to
- If SSH fails, make sure the service is started

</details>

<details>
<summary>✅ Solution</summary>

```bash
#!/bin/bash
# SSH practice script

echo "=== SSH Service Status ==="
systemctl is-active ssh && echo "SSH is running" || echo "SSH is not running"

echo -e "\n=== Starting SSH if needed ==="
sudo systemctl start ssh
systemctl is-active ssh

echo -e "\n=== Testing SCP locally ==="
# Create test file
echo "Test file created at $(date)" > /tmp/scp-test.txt
echo "Created: /tmp/scp-test.txt"

# Note: This will prompt for password
echo "To test SCP, run:"
echo "scp /tmp/scp-test.txt $(whoami)@localhost:~/scp-received.txt"

echo -e "\n=== SSH Known Hosts ==="
if [ -f ~/.ssh/known_hosts ]; then
    echo "Known hosts file exists"
    wc -l ~/.ssh/known_hosts
else
    echo "No known_hosts file yet (connect somewhere first)"
fi
```

To fully test SSH interactively:
```bash
# Start SSH
sudo systemctl start ssh

# Connect to self
ssh $(whoami)@localhost

# Inside the SSH session
hostname
whoami
exit

# Test SCP
echo "Hello SCP" > /tmp/testfile.txt
scp /tmp/testfile.txt $(whoami)@localhost:~/
cat ~/testfile.txt
rm ~/testfile.txt /tmp/testfile.txt
```

</details>

---

## Challenge Exercise: Network Diagnostics

**Objective:** Combine everything you've learned to create a comprehensive network diagnostic report and troubleshooting script.

### Part 1: Document Your Network Setup

Create a file called `network-report.txt` that contains:

1. Your hostname
2. All IP addresses
3. All network interfaces and their status
4. Default gateway
5. DNS servers
6. Routing table
7. Listening ports
8. Current connections

### Part 2: Create a Network Troubleshooting Checklist

Create a markdown file `network-checklist.md` with a systematic troubleshooting checklist.

### Part 3: Test All Layers of Connectivity

Write a script that tests:
1. Loopback (127.0.0.1)
2. Local interface (your IP)
3. Gateway
4. External IP (8.8.8.8)
5. DNS resolution (google.com)

### Part 4: Write a Network Status Script

Create a script called `netstat-check.sh` that:
- Shows a summary of network configuration
- Tests connectivity
- Lists listening services
- Shows active connections
- Displays any errors or warnings

<details>
<summary>💡 Hints</summary>

- Use functions to organize your script
- Use color codes for better readability (green for OK, red for errors)
- Check if commands exist before running them
- Add timestamps to your report

</details>

<details>
<summary>✅ Complete Solution</summary>

**network-report.txt generator:**
```bash
#!/bin/bash
# Generate network report

REPORT="network-report.txt"

{
    echo "=========================================="
    echo "       NETWORK CONFIGURATION REPORT"
    echo "=========================================="
    echo "Generated: $(date)"
    echo ""
    
    echo "=== HOSTNAME ==="
    hostname
    echo ""
    
    echo "=== IP ADDRESSES ==="
    ip -4 addr show | grep inet
    echo ""
    
    echo "=== NETWORK INTERFACES ==="
    ip link show
    echo ""
    
    echo "=== DEFAULT GATEWAY ==="
    ip route | grep default
    echo ""
    
    echo "=== DNS SERVERS ==="
    grep nameserver /etc/resolv.conf
    echo ""
    
    echo "=== ROUTING TABLE ==="
    ip route
    echo ""
    
    echo "=== LISTENING PORTS ==="
    ss -tuln
    echo ""
    
    echo "=== ACTIVE CONNECTIONS ==="
    ss -tun
    echo ""
    
} > "$REPORT"

echo "Report saved to $REPORT"
cat "$REPORT"
```

**network-checklist.md:**
```markdown
# Network Troubleshooting Checklist

## 1. Physical Layer
- [ ] Is the network cable connected?
- [ ] Is WiFi enabled and connected?
- [ ] Are link lights on the network adapter?

## 2. Data Link Layer
- [ ] Is the interface UP? (`ip link show`)
- [ ] Is there a MAC address assigned?

## 3. Network Layer
- [ ] Is an IP address assigned? (`ip addr`)
- [ ] Is the subnet mask correct?
- [ ] Can you ping the gateway?
- [ ] Can you ping external IPs (8.8.8.8)?

## 4. Transport Layer
- [ ] Are required services listening? (`ss -tuln`)
- [ ] Is the firewall blocking traffic? (`sudo ufw status`)

## 5. Application Layer
- [ ] Does DNS work? (`ping google.com`)
- [ ] Can you reach the specific service?

## Quick Commands
| Check | Command |
|-------|---------|
| Interface status | `ip link show` |
| IP address | `ip addr show` |
| Gateway | `ip route \| grep default` |
| DNS config | `cat /etc/resolv.conf` |
| Ping gateway | `ping -c 2 GATEWAY_IP` |
| Ping internet | `ping -c 2 8.8.8.8` |
| Ping DNS test | `ping -c 2 google.com` |
| Listening ports | `ss -tuln` |
| Firewall | `sudo ufw status` |
```

**netstat-check.sh:**
```bash
#!/bin/bash
# Network Status Check Script
# Usage: ./netstat-check.sh

# Colors
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Functions
print_header() {
    echo -e "\n${BLUE}=== $1 ===${NC}"
}

print_ok() {
    echo -e "${GREEN}[OK]${NC} $1"
}

print_fail() {
    echo -e "${RED}[FAIL]${NC} $1"
}

print_warn() {
    echo -e "${YELLOW}[WARN]${NC} $1"
}

test_ping() {
    if ping -c 1 -W 2 "$1" > /dev/null 2>&1; then
        print_ok "Ping to $1 successful"
        return 0
    else
        print_fail "Ping to $1 failed"
        return 1
    fi
}

# Main script
echo "========================================"
echo "       NETWORK STATUS CHECK"
echo "========================================"
echo "Date: $(date)"
echo "Hostname: $(hostname)"

# Interface Status
print_header "INTERFACE STATUS"
ip -br link show | while read iface state rest; do
    if [[ "$state" == "UP" ]]; then
        print_ok "$iface is $state"
    elif [[ "$iface" == "lo" ]]; then
        print_ok "$iface (loopback) is $state"
    else
        print_warn "$iface is $state"
    fi
done

# IP Configuration
print_header "IP CONFIGURATION"
IP_ADDR=$(hostname -I | awk '{print $1}')
if [[ -n "$IP_ADDR" ]]; then
    print_ok "IP Address: $IP_ADDR"
else
    print_fail "No IP address assigned"
fi

GATEWAY=$(ip route | grep default | awk '{print $3}')
if [[ -n "$GATEWAY" ]]; then
    print_ok "Default Gateway: $GATEWAY"
else
    print_fail "No default gateway"
fi

# DNS Configuration
print_header "DNS CONFIGURATION"
DNS_SERVERS=$(grep nameserver /etc/resolv.conf | awk '{print $2}')
if [[ -n "$DNS_SERVERS" ]]; then
    for dns in $DNS_SERVERS; do
        print_ok "DNS Server: $dns"
    done
else
    print_fail "No DNS servers configured"
fi

# Connectivity Tests
print_header "CONNECTIVITY TESTS"

echo -n "1. Loopback (127.0.0.1): "
test_ping 127.0.0.1

if [[ -n "$GATEWAY" ]]; then
    echo -n "2. Gateway ($GATEWAY): "
    test_ping "$GATEWAY"
fi

echo -n "3. Internet (8.8.8.8): "
test_ping 8.8.8.8

echo -n "4. DNS Test (google.com): "
test_ping google.com

# Listening Services
print_header "LISTENING SERVICES"
LISTENING=$(ss -tuln | grep LISTEN | wc -l)
echo "Total listening ports: $LISTENING"
ss -tuln | grep LISTEN | head -10
if [[ $LISTENING -gt 10 ]]; then
    echo "... and more (showing first 10)"
fi

# Active Connections
print_header "ACTIVE CONNECTIONS"
ACTIVE=$(ss -tun | grep -v State | wc -l)
if [[ $ACTIVE -gt 0 ]]; then
    print_ok "$ACTIVE active connection(s)"
    ss -tun | grep -v State | head -5
else
    print_warn "No active connections"
fi

# Firewall Status
print_header "FIREWALL STATUS"
if command -v ufw > /dev/null 2>&1; then
    UFW_STATUS=$(sudo ufw status 2>/dev/null | head -1)
    if [[ "$UFW_STATUS" == *"active"* ]]; then
        print_ok "UFW is active"
    else
        print_warn "UFW is inactive"
    fi
else
    print_warn "UFW not installed"
fi

# Summary
print_header "SUMMARY"
echo "Network status check complete."
echo "If you see failures above, investigate those areas."
echo ""
```

Make the script executable and run it:
```bash
chmod +x netstat-check.sh
./netstat-check.sh
```

</details>

---

## Quick Reference Card

### IP Information
```bash
ip addr                    # Show IP addresses
ip link                    # Show interfaces
ip route                   # Show routing table
hostname -I                # Quick IP display
```

### Connectivity Testing
```bash
ping -c 4 host            # Ping with count
traceroute host           # Trace path
tracepath host            # Trace (no root)
```

### DNS
```bash
host domain               # Simple lookup
dig domain                # Detailed lookup
dig +short domain         # Brief output
nslookup domain           # DNS query
```

### Connections & Ports
```bash
ss -tuln                  # Listening ports
ss -tulnp                 # With process (sudo)
netstat -tuln             # Legacy listening
lsof -i :port             # What's using port
```

### Downloading
```bash
wget URL                  # Download file
wget -O name URL          # Download with name
curl URL                  # View content
curl -O URL               # Download file
curl -I URL               # Headers only
```

### SSH
```bash
ssh user@host             # Connect
ssh -p port user@host     # Custom port
scp file user@host:/path  # Copy to remote
scp user@host:/file ./    # Copy from remote
```

### Firewall
```bash
sudo ufw status           # Check firewall
sudo ufw allow 22         # Allow port
sudo ufw enable           # Enable firewall
```

---

## Next Steps

After completing these exercises, you should be comfortable with:

- ✅ Viewing and understanding network configuration
- ✅ Testing network connectivity at different layers
- ✅ Understanding and querying DNS
- ✅ Monitoring network connections and ports
- ✅ Downloading files from the internet
- ✅ Using SSH for remote access
- ✅ Basic firewall management
- ✅ Systematic network troubleshooting

Proceed to the next module to learn about **disk management and storage**!