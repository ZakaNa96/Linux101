# Module 9: Resources - Basic Networking

A curated list of resources to deepen your understanding of Linux networking.

---

## 📚 Networking Fundamentals

### IP Addressing & Subnetting
- [IP Addressing Basics - Cisco](https://www.cisco.com/c/en/us/support/docs/ip/routing-information-protocol-rip/13788-3.html) - Comprehensive guide to IP addressing
- [Subnet Calculator](https://www.subnet-calculator.com/) - Online tool for subnet calculations
- [CIDR Notation Explained](https://www.digitalocean.com/community/tutorials/understanding-ip-addresses-subnets-and-cidr-notation-for-networking) - DigitalOcean tutorial on CIDR
- [Private IP Address Ranges](https://www.arin.net/reference/research/statistics/address_filters/) - Official private address ranges

### TCP/IP & Protocols
- [TCP/IP Guide](http://www.tcpipguide.com/free/index.htm) - Free comprehensive TCP/IP reference
- [Common Port Numbers](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) - IANA official port registry
- [OSI Model Explained](https://www.cloudflare.com/learning/ddos/glossary/open-systems-interconnection-model-osi/) - Cloudflare's OSI model guide

---

## 🔧 Linux Networking Commands

### The `ip` Command
- [ip Command Cheat Sheet](https://access.redhat.com/sites/default/files/attachments/rh_ip_command_cheatsheet_1214_jcs_print.pdf) - Red Hat's official ip command reference (PDF)
- [Linux ip Command Examples](https://www.cyberciti.biz/faq/linux-ip-command-examples-usage-syntax/) - Practical ip command examples
- [iproute2 Documentation](https://wiki.linuxfoundation.org/networking/iproute2) - Linux Foundation wiki

### Network Troubleshooting
- [Linux Network Troubleshooting Guide](https://www.redhat.com/sysadmin/beginners-guide-network-troubleshooting-linux) - Red Hat's troubleshooting guide
- [Ping Command Tutorial](https://www.howtogeek.com/190148/how-to-use-the-ping-command-to-test-your-network/) - How-To Geek's ping guide
- [Traceroute Explained](https://www.cloudflare.com/learning/network-layer/what-is-traceroute/) - How traceroute works

---

## 🌐 DNS (Domain Name System)

### Understanding DNS
- [How DNS Works](https://howdns.works/) - Visual and fun explanation of DNS
- [DNS Explained](https://www.cloudflare.com/learning/dns/what-is-dns/) - Cloudflare's DNS learning center
- [DNS Record Types](https://www.cloudflare.com/learning/dns/dns-records/) - Explanation of A, AAAA, MX, CNAME, etc.

### DNS Tools
- [dig Command Tutorial](https://www.hostinger.com/tutorials/how-to-use-the-dig-command-in-linux/) - Complete dig command guide
- [Online DNS Lookup](https://toolbox.googleapps.com/apps/dig/) - Google's online dig tool
- [DNS Propagation Checker](https://www.whatsmydns.net/) - Check DNS propagation globally

---

## 🔐 SSH (Secure Shell)

### SSH Basics
- [OpenSSH Manual](https://www.openssh.com/manual.html) - Official OpenSSH documentation
- [SSH Essentials](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys) - DigitalOcean's SSH guide
- [SSH Tutorial](https://www.ssh.com/academy/ssh) - SSH Academy comprehensive tutorials

### SSH Key Management
- [SSH Key Generation](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) - GitHub's SSH key guide
- [SSH Config File](https://linuxize.com/post/using-the-ssh-config-file/) - How to use ~/.ssh/config
- [SSH Key Best Practices](https://www.ssh.com/academy/ssh/keygen) - Security best practices

### SCP & SFTP
- [SCP Command Examples](https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/) - Comprehensive SCP guide
- [SFTP Tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-sftp-to-securely-transfer-files-with-a-remote-server) - SFTP usage guide

---

## 📥 File Transfer (wget & curl)

### wget
- [GNU Wget Manual](https://www.gnu.org/software/wget/manual/wget.html) - Official wget documentation
- [wget Tutorial](https://www.hostinger.com/tutorials/wget-command-examples/) - Practical wget examples
- [Advanced wget Usage](https://www.cyberciti.biz/tips/linux-wget-your-ultimate-command-line-downloader.html) - Advanced techniques

### curl
- [curl Documentation](https://curl.se/docs/) - Official curl documentation
- [Everything curl](https://everything.curl.dev/) - Comprehensive curl book (free online)
- [curl Cookbook](https://catonmat.net/cookbooks/curl) - Common curl recipes
- [curl vs wget](https://www.baeldung.com/linux/curl-wget) - Comparison and use cases

---

## 🔥 Firewalls

### UFW (Uncomplicated Firewall)
- [UFW Essentials](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands) - Common UFW rules
- [UFW Documentation](https://help.ubuntu.com/community/UFW) - Ubuntu community documentation
- [UFW Tutorial](https://linuxize.com/post/how-to-setup-a-firewall-with-ufw-on-ubuntu-20-04/) - Step-by-step UFW guide

### iptables
- [iptables Tutorial](https://www.howtogeek.com/177621/the-beginners-guide-to-iptables-the-linux-firewall/) - Beginner's guide
- [iptables Essentials](https://www.digitalocean.com/community/tutorials/iptables-essentials-common-firewall-rules-and-commands) - Common rules and commands
- [iptables Deep Dive](https://www.booleanworld.com/depth-guide-iptables-linux-firewall/) - In-depth guide

---

## 🛠️ Network Analysis Tools

### Nmap
- [Nmap Official Site](https://nmap.org/) - Download and documentation
- [Nmap Tutorial](https://hackertarget.com/nmap-tutorial/) - Practical scanning guide
- [Nmap Cheat Sheet](https://www.stationx.net/nmap-cheat-sheet/) - Quick reference

### Netcat
- [Netcat Tutorial](https://www.sans.org/security-resources/sec560/netcat_cheat_sheet_v1.pdf) - SANS netcat cheat sheet (PDF)
- [Netcat Examples](https://www.varonis.com/blog/netcat-commands) - Common netcat uses

### Wireshark
- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/) - Official documentation
- [Wireshark Tutorial](https://www.comparitech.com/net-admin/wireshark-cheat-sheet/) - Getting started guide

⚠️ **Warning:** Only use these tools on networks and systems you have permission to test!

---

## 📖 Man Pages & Documentation

Access these directly from your terminal:

```bash
man ip              # IP routing and devices
man ss              # Socket statistics
man ping            # ICMP echo
man traceroute      # Trace packet route
man dig             # DNS lookup
man ssh             # Secure shell
man scp             # Secure copy
man wget            # Download utility
man curl            # Transfer data
man ufw             # Uncomplicated firewall
man iptables        # Packet filtering
man nmap            # Network scanner
man nc              # Netcat
```

---

## 🎓 Online Courses & Tutorials

### Free Resources
- [Linux Journey - Networking](https://linuxjourney.com/lesson/network-basics) - Free Linux networking lessons
- [Cybrary - Networking Fundamentals](https://www.cybrary.it/course/network-fundamentals/) - Free networking course
- [Professor Messer - Network+](https://www.professormesser.com/network-plus/n10-008/n10-008-video/n10-008-training-course/) - Free video training

### Practice Environments
- [TryHackMe - Networking](https://tryhackme.com/room/introtolan) - Interactive networking labs
- [HackTheBox - Starting Point](https://www.hackthebox.com/starting-point) - Guided networking challenges
- [OverTheWire - Bandit](https://overthewire.org/wargames/bandit/) - Command-line challenges

---

## 🔒 Security Considerations

### Network Security Best Practices
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) - Security guidelines
- [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks/) - Security configuration guides
- [Linux Security Guide](https://www.redhat.com/en/topics/security) - Red Hat security resources

### Ethical Guidelines
- [Responsible Disclosure](https://www.hackerone.com/vulnerability-management/responsible-disclosure) - How to report vulnerabilities
- [Computer Fraud Laws](https://www.law.cornell.edu/uscode/text/18/1030) - US CFAA (understand the law)
- [PTES Standard](http://www.pentest-standard.org/) - Penetration Testing Execution Standard

---

## 📱 Useful Online Tools

### Network Utilities
- [What Is My IP](https://whatismyipaddress.com/) - Find your public IP
- [DNS Leak Test](https://dnsleaktest.com/) - Test for DNS leaks
- [Speed Test](https://www.speedtest.net/) - Test internet speed
- [Ping Test](https://ping.eu/) - Online ping tool
- [Port Checker](https://www.yougetsignal.com/tools/open-ports/) - Check if ports are open

### IP & Domain Information
- [Whois Lookup](https://www.whois.com/whois/) - Domain registration info
- [IP Geolocation](https://www.iplocation.net/) - Find IP location
- [BGP Looking Glass](https://www.bgp4.as/looking-glasses) - BGP route information

---

## 📚 Books (Recommended)

### Networking
- **"TCP/IP Illustrated" by W. Richard Stevens** - The classic TCP/IP reference
- **"Computer Networking: A Top-Down Approach" by Kurose & Ross** - Academic networking textbook
- **"Network Warrior" by Gary A. Donahue** - Practical networking guide

### Linux System Administration
- **"The Linux Command Line" by William Shotts** - Command-line mastery
- **"UNIX and Linux System Administration Handbook"** - Comprehensive sysadmin guide
- **"Linux Networking Cookbook" by Carla Schroder** - Practical recipes

### Security
- **"Practical Packet Analysis" by Chris Sanders** - Network traffic analysis
- **"Nmap Network Scanning" by Gordon Lyon** - Official Nmap book (free online)

---

## 🔗 Quick Reference Sites

- [ExplainShell](https://explainshell.com/) - Paste any command and get explanation
- [tldr pages](https://tldr.sh/) - Simplified man pages
- [cheat.sh](https://cheat.sh/) - Unified cheat sheets (try `curl cheat.sh/ip`)
- [Linux Command Library](https://linuxcommandlibrary.com/) - Searchable command reference

---

## 💡 Tips for Learning

1. **Practice in a safe environment** - Use VirtualBox VMs for experiments
2. **Read man pages** - They're comprehensive and always available
3. **Break things intentionally** - Learn by fixing what you break (in a VM!)
4. **Document what you learn** - Keep notes and create your own cheat sheets
5. **Join communities** - r/linux, r/networking, Stack Overflow, Linux forums
6. **Set up a home lab** - Raspberry Pi, old computers, or cloud VMs

---

*Remember: The best way to learn networking is hands-on practice. Use these resources alongside the practical exercises!*