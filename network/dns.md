# **Introduction to DNS in Linux for Beginners**


### **Understanding DNS and Hostname Resolution**
- Two computers, A and B, are part of the same network with IP addresses 192.168.1.10 and 192.168.1.11.
- System B hosts a database service, and instead of remembering its IP address, we assign it a hostname, "DB."
- By default, System A does not recognize the hostname "DB," requiring manual configuration.

### **Using the Hosts File for Local Name Resolution**
- The `/etc/hosts` file allows assigning a hostname to an IP address.
- By adding an entry (`192.168.1.11 DB`) in the `/etc/hosts` file on System A, it recognizes "DB" as 192.168.1.11.
- The hosts file acts as a local DNS, but it does not verify actual hostnames.
- Misleading entries can be added, such as mapping "google.com" to System B’s IP.

### **Limitations of the Hosts File**
- The hosts file works well for small networks but becomes difficult to manage as the network grows.
- Updating IP addresses requires modifying each system’s hosts file.

### **Introducing a DNS Server**
- Instead of maintaining local `/etc/hosts` files, a centralized DNS server is used.
- Example: DNS server at `192.168.1.100` is specified in `/etc/resolv.conf` (`nameserver 192.168.1.100`).
- All hosts query the DNS server instead of their local files.
- When an IP address changes, only the DNS server needs to be updated.

### **DNS and External Name Resolution**
- If a hostname is not in `/etc/hosts` or the internal DNS server, queries fail.
- Public DNS servers, like Google’s `8.8.8.8`, resolve external domains.
- Internal DNS servers can forward unresolved requests to public DNS servers.

### **Understanding Domain Names and DNS Hierarchy**
- Internet domains follow a structured hierarchy:
  - Top-Level Domains (TLDs): `.com`, `.org`, `.edu`.
  - Subdomains: `mail.google.com`, `drive.google.com`.
- DNS queries resolve names step by step, starting from root DNS servers.
- Organizations can implement internal DNS structures like `mycompany.com` with subdomains for different services (`mail.mycompany.com`, `hr.mycompany.com`).

### **Order of Name Resolution**
- The `/etc/nsswitch.conf` file determines lookup order (e.g., `files` before `dns`).
- If a hostname exists in both `/etc/hosts` and DNS, the local file takes precedence.
- DNS caching improves resolution speed by temporarily storing results.


## What is DNS?
DNS (Domain Name System) is a protocol that translates human-readable domain names (e.g., `example.com`) into IP addresses (e.g., `192.168.1.1`). It acts as the phonebook of the internet, enabling users to access websites using domain names instead of numerical IP addresses.

## How DNS Works
When a user enters a domain name into a browser, the following steps occur:
1. The browser checks the local DNS cache.
2. If not found, the request is sent to a DNS resolver (usually provided by the ISP).
3. The resolver queries the root DNS servers.
4. The root server directs the query to the appropriate TLD (Top-Level Domain) server.
5. The TLD server refers the request to the authoritative DNS server for the domain.
6. The authoritative server provides the IP address, which is then sent back to the browser.

## DNS Configuration in Linux
### 1. `/etc/resolv.conf`
This file contains the DNS server addresses used by the system. A typical example:
```sh
nameserver 8.8.8.8
nameserver 8.8.4.4
```
These entries define Google's public DNS servers.

### 2. `/etc/hosts`
This file maps IP addresses to domain names locally. Example:
```sh
127.0.0.1   localhost
192.168.1.100 myserver.local
```

### 3. Checking DNS Settings
Use the following command to check the system's current DNS settings:
```sh
cat /etc/resolv.conf
```

## Common DNS Commands in Linux
### 1. `nslookup` Name Server Lookup
Performs a DNS query to retrieve IP addresses, Used for querying DNS servers to get domain name or IP address mapping.:
```sh
nslookup example.com
```

### 2. `dig`
Provides detailed DNS information, More powerful and flexible than nslookup.
Provides detailed information, including TTL, authoritative servers, and query time.:
```sh
dig example.com
```
To query specific record types:
```sh
dig example.com A
```
```sh
dig example.com MX
```

### 3. `host`
Used to resolve domain names:
```sh
host example.com
```

### 4. `ping`
Tests connectivity and resolves domain names:
```sh
ping example.com
```

### 5. `traceroute`
Shows the path packets take to a host:
```sh
traceroute example.com
```

### 6. `systemd-resolve`
For checking current DNS resolver status:
```sh
systemd-resolve --status
```

### 7. `resolvectl`
Used in newer Linux distributions to check DNS:
```sh
resolvectl status
```

### 8. Flushing DNS Cache
For systems using `systemd`:
```sh
systemd-resolve --flush-caches
```
For Ubuntu/Debian with `nscd`:
```sh
sudo systemctl restart nscd
```
For macOS:
```sh
sudo killall -HUP mDNSResponder
```

!(https://github.com/user-attachments/assets/8c356541-5227-404e-8175-cb2e60a4a23d)

