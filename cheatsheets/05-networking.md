# Networking Cheatsheet

> Quick reference for network configuration, connectivity, and troubleshooting.  
> *For detailed explanations, see Module 9 of this course.*

---

## Network Information

### ip Command (Modern)

| Command | Description |
|---------|-------------|
| `ip addr` or `ip a` | Show all IP addresses |
| `ip addr show eth0` | Show specific interface |
| `ip link` | Show network interfaces |
| `ip link show` | Show interface status |
| `ip route` or `ip r` | Show routing table |
| `ip neigh` | Show ARP cache |

### ifconfig (Legacy)

| Command | Description |
|---------|-------------|
| `ifconfig` | Show all interfaces |
| `ifconfig eth0` | Show specific interface |
| `ifconfig -a` | Show all (including down) |

### Other Information Commands

| Command | Description |
|---------|-------------|
| `hostname` | Show hostname |
| `hostname -I` | Show IP addresses |
| `cat /etc/resolv.conf` | DNS servers |
| `cat /etc/hosts` | Hosts file |
| `nmcli device status` | NetworkManager status |
| `nmcli connection show` | Show connections |

---

## Connectivity Testing

### ping

| Command | Description |
|---------|-------------|
| `ping host` | Continuous ping |
| `ping -c 4 host` | Send 4 packets |
| `ping -i 2 host` | 2 second interval |
| `ping -s 1000 host` | Custom packet size |
| `ping -W 3 host` | 3 second timeout |

```bash
# Test local connectivity
ping localhost
ping 127.0.0.1

# Test LAN gateway
ping 192.168.1.1

# Test internet
ping 8.8.8.8        # Google DNS
ping 1.1.1.1        # Cloudflare DNS

# Test DNS resolution
ping google.com
```

### traceroute / tracepath

| Command | Description |
|---------|-------------|
| `traceroute host` | Trace route to host |
| `traceroute -n host` | No DNS lookup (faster) |
| `tracepath host` | Similar, no root needed |
| `mtr host` | Continuous traceroute |

### Testing Ports

| Command | Description |
|---------|-------------|
| `nc -zv host port` | Test single port |
| `nc -zv host 20-30` | Test port range |
| `telnet host port` | Connect to port |
| `ss -tuln` | Show listening ports |
| `netstat -tuln` | Show listening ports (legacy) |

```bash
# Check if web server is responding
nc -zv example.com 80
nc -zv example.com 443

# Check SSH port
nc -zv server.local 22

# Check if port is open locally
ss -tuln | grep :80
```

---

## DNS Lookup

### dig

| Command | Description |
|---------|-------------|
| `dig domain` | Query DNS (detailed) |
| `dig domain A` | Query A record |
| `dig domain MX` | Query MX record |
| `dig domain NS` | Query nameservers |
| `dig domain ANY` | Query all records |
| `dig +short domain` | Brief output |
| `dig @8.8.8.8 domain` | Use specific DNS server |
| `dig -x IP` | Reverse lookup |

```bash
# Simple lookup
dig google.com

# Just the IP
dig +short google.com

# All record types
dig google.com ANY

# Reverse lookup
dig -x 8.8.8.8

# Use specific DNS server
dig @1.1.1.1 example.com
```

### host

| Command | Description |
|---------|-------------|
| `host domain` | Simple DNS lookup |
| `host -t MX domain` | Query MX record |
| `host -t NS domain` | Query nameservers |
| `host IP` | Reverse lookup |

### nslookup

| Command | Description |
|---------|-------------|
| `nslookup domain` | DNS lookup |
| `nslookup domain 8.8.8.8` | Use specific DNS |
| `nslookup -type=mx domain` | Query MX record |

---

## Downloading Files

### wget

| Command | Description |
|---------|-------------|
| `wget URL` | Download file |
| `wget -O name URL` | Save as specific name |
| `wget -P dir/ URL` | Save to directory |
| `wget -c URL` | Continue partial download |
| `wget -q URL` | Quiet mode |
| `wget -b URL` | Background download |
| `wget -r URL` | Recursive download |
| `wget --limit-rate=200k URL` | Limit speed |
| `wget -i list.txt` | Download from URL list |

```bash
# Download file
wget https://example.com/file.tar.gz

# Save with different name
wget -O myfile.tar.gz https://example.com/file.tar.gz

# Continue interrupted download
wget -c https://example.com/largefile.iso

# Download in background
wget -b https://example.com/file.tar.gz

# Mirror website (be careful!)
wget -r -l 2 https://example.com
```

### curl

| Command | Description |
|---------|-------------|
| `curl URL` | Fetch URL (stdout) |
| `curl -O URL` | Save with original name |
| `curl -o name URL` | Save as specific name |
| `curl -L URL` | Follow redirects |
| `curl -I URL` | Headers only |
| `curl -s URL` | Silent mode |
| `curl -v URL` | Verbose output |
| `curl -u user:pass URL` | Basic authentication |
| `curl -X POST URL` | POST request |
| `curl -d "data" URL` | Send data |

```bash
# Download file
curl -O https://example.com/file.tar.gz

# Save with specific name
curl -o myfile.tar.gz https://example.com/file.tar.gz

# Follow redirects
curl -L https://example.com/redirect

# View headers
curl -I https://example.com

# POST request
curl -X POST -d "name=value" https://api.example.com

# JSON POST
curl -X POST -H "Content-Type: application/json" \
     -d '{"key":"value"}' https://api.example.com

# Download with progress
curl -# -O https://example.com/file.iso
```

---

## SSH (Secure Shell)

### Basic Connection

| Command | Description |
|---------|-------------|
| `ssh user@host` | Connect to host |
| `ssh host` | Connect (same username) |
| `ssh -p 2222 user@host` | Custom port |
| `ssh -i key.pem user@host` | Use specific key |
| `ssh -v user@host` | Verbose (debug) |

```bash
# Basic connection
ssh john@192.168.1.100
ssh john@server.example.com

# Using different port
ssh -p 2222 john@server.com

# Using SSH key
ssh -i ~/.ssh/my_key user@server
```

### SSH Key Management

```bash
# Generate SSH key pair
ssh-keygen -t ed25519 -C "email@example.com"
ssh-keygen -t rsa -b 4096 -C "email@example.com"

# Copy public key to server
ssh-copy-id user@host
ssh-copy-id -i ~/.ssh/mykey.pub user@host

# Manual key copy
cat ~/.ssh/id_ed25519.pub | ssh user@host "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

### SSH Config File (~/.ssh/config)

```bash
# Example config
Host myserver
    HostName 192.168.1.100
    User john
    Port 22
    IdentityFile ~/.ssh/id_ed25519

Host webserver
    HostName web.example.com
    User admin
    Port 2222

# Then connect with:
ssh myserver
ssh webserver
```

---

## SCP (Secure Copy)

| Command | Description |
|---------|-------------|
| `scp file user@host:path` | Copy to remote |
| `scp user@host:file local` | Copy from remote |
| `scp -r dir user@host:path` | Copy directory |
| `scp -P 2222 file user@host:` | Custom port |

```bash
# Copy file to remote server
scp document.pdf john@server:/home/john/

# Copy file from remote server
scp john@server:/var/log/app.log ./

# Copy entire directory
scp -r ./project john@server:/home/john/

# Copy with custom port
scp -P 2222 file.txt john@server:/home/john/

# Copy between remote servers
scp user1@host1:/file user2@host2:/path/
```

---

## rsync (Advanced Sync)

| Option | Description |
|--------|-------------|
| `-a` | Archive mode (preserves everything) |
| `-v` | Verbose |
| `-z` | Compress during transfer |
| `-P` | Show progress + partial resume |
| `--delete` | Delete extra files at destination |
| `-n` | Dry run (test) |
| `-e ssh` | Use SSH |

```bash
# Basic sync
rsync -av source/ destination/

# Sync to remote server
rsync -avz ./local/ user@server:/remote/

# Sync from remote server
rsync -avz user@server:/remote/ ./local/

# Sync with progress
rsync -avP source/ destination/

# Sync with delete (mirror)
rsync -av --delete source/ destination/

# Dry run (test first!)
rsync -avn --delete source/ destination/

# Sync over SSH with custom port
rsync -avz -e "ssh -p 2222" ./local/ user@server:/remote/

# Exclude files
rsync -av --exclude='*.log' source/ destination/
```

---

## Firewall (ufw)

### Basic Commands

| Command | Description |
|---------|-------------|
| `sudo ufw status` | Show firewall status |
| `sudo ufw status verbose` | Detailed status |
| `sudo ufw status numbered` | Rules with numbers |
| `sudo ufw enable` | Enable firewall |
| `sudo ufw disable` | Disable firewall |
| `sudo ufw reset` | Reset to defaults |

### Allow Rules

```bash
# Allow by port
sudo ufw allow 22        # SSH
sudo ufw allow 80        # HTTP
sudo ufw allow 443       # HTTPS

# Allow by service name
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

# Allow port range
sudo ufw allow 6000:6007/tcp

# Allow from specific IP
sudo ufw allow from 192.168.1.100

# Allow from IP to specific port
sudo ufw allow from 192.168.1.100 to any port 22

# Allow from subnet
sudo ufw allow from 192.168.1.0/24
```

### Deny Rules

```bash
# Deny by port
sudo ufw deny 23

# Deny from specific IP
sudo ufw deny from 192.168.1.50

# Deny to specific port
sudo ufw deny from 192.168.1.50 to any port 22
```

### Delete Rules

```bash
# Delete by rule
sudo ufw delete allow 80

# Delete by number (use status numbered first)
sudo ufw status numbered
sudo ufw delete 3
```

### Common Configurations

```bash
# Basic web server
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw enable

# Allow specific IP range for SSH
sudo ufw allow from 192.168.1.0/24 to any port 22
```

---

## Port and Connection Information

### ss (Socket Statistics)

| Command | Description |
|---------|-------------|
| `ss -tuln` | TCP/UDP listening ports |
| `ss -t` | TCP connections |
| `ss -u` | UDP connections |
| `ss -l` | Listening sockets |
| `ss -p` | Show process info |
| `ss -n` | No DNS resolution |
| `ss -a` | All sockets |

```bash
# Show all listening ports
ss -tuln

# Show with process names (needs sudo)
sudo ss -tulnp

# Show established connections
ss -t state established

# Show connections to specific port
ss -t dst :80
```

### netstat (Legacy)

| Command | Description |
|---------|-------------|
| `netstat -tuln` | TCP/UDP listening ports |
| `netstat -an` | All connections |
| `netstat -r` | Routing table |
| `netstat -i` | Interface statistics |
| `netstat -p` | Show process info |

---

## Network Configuration (Temporary)

```bash
# Set IP address (temporary)
sudo ip addr add 192.168.1.100/24 dev eth0

# Remove IP address
sudo ip addr del 192.168.1.100/24 dev eth0

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down

# Add default route
sudo ip route add default via 192.168.1.1

# Delete route
sudo ip route del default

# Flush IP addresses
sudo ip addr flush dev eth0
```

---

## Quick Reference Card

```
INFORMATION:
  ip addr             # Show IP addresses
  ip route            # Show routes
  hostname -I         # Quick IP lookup

CONNECTIVITY:
  ping -c 4 host      # Test connectivity
  traceroute host     # Trace route
  nc -zv host port    # Test port

DNS:
  dig domain          # DNS lookup
  dig +short domain   # Quick lookup
  host domain         # Simple lookup

DOWNLOAD:
  wget URL            # Download file
  curl -O URL         # Download with curl
  curl -I URL         # Headers only

SSH/SCP:
  ssh user@host       # Connect
  scp file user@host: # Copy to remote
  scp user@host:file . # Copy from remote

FIREWALL (ufw):
  sudo ufw status     # Check status
  sudo ufw allow 22   # Allow SSH
  sudo ufw enable     # Enable firewall

PORTS:
  ss -tuln            # Listening ports
  ss -tulnp           # With process names
```

---

*Reference: Module 9 - Networking Basics*