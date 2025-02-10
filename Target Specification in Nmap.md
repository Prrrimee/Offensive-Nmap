

## **Target Specification in Nmap**  
1. **Basic Targeting**  
   - You can specify a target using an **IP address** or **hostname**.  
   - Hostnames are resolved via **DNS**, but only the first resolved IP is scanned (unless `--resolve-all` is used).  

2. **CIDR Notation**  
   - Supports CIDR-style addressing (`/<numbits>`), ex.., `192.168.10.0/24` scans `192.168.10.0` to `192.168.10.255`.  
   - `/0` means **the entire Internet**, while `/32` (IPv4) or `/128` (IPv6) scans a **single address**.  

3. **Octet Range Addressing** (More Flexible Than CIDR)  
   - Allows specifying **comma-separated ranges** for each octet.  
   - Example: `192.168.0-255.1-254` avoids `.0` and `.255` addresses.  
   - Example: `192.168.3-5,7.1` scans `192.168.3.1`, `192.168.4.1`, `192.168.5.1`, and `192.168.7.1`.  
   - Can be used for **Internet-wide surveys**, ex., `0-255.0-255.13.37`.  

4. **IPv6 Targeting**  
   - Supports **CIDR notation** but **not octet ranges**.  
   - Link-local addresses need **zone IDs**, ex., `fe80::1%eth0` (Linux) or `fe80::1%1` (Windows).  

5. **Multiple Targets**  
   - You can mix different target types:  
     ```bash
     nmap scanme.nmap.org 192.168.0.0/8 10.0.0,1,3-7.-
     ```

---

## **Additional Target Selection Options**  
1. `-iL <file>` → **Read targets from a file**  
   - Useful for scanning **large lists of IPs**.  
   - Supports **comments (`#`)**.  

2. `-iR <num>` → **Choose random targets**  
   - `-iR 1000` scans **1,000 random IPs** (avoiding private & reserved addresses).  
   - `-iR 0` scans **indefinitely**.  

3. `--exclude <host1,host2,...>` → **Exclude specific hosts**  
   - Useful to skip **mission-critical servers** or subnets.  

4. `--excludefile <file>` → **Exclude targets from a file**  
   - Same as `--exclude`, but loads from a file.  

---

## **DNS & Resolution Options**  
1. `-n` → **Disable DNS resolution** (faster scans).  
2. `-R` → **Force DNS resolution for all targets**.  
3. `--resolve-all` → **Scan all resolved addresses** (not just the first one).  
4. `--unique` → **Avoid scanning the same address multiple times**.  
5. `--system-dns` → **Use system resolver instead of Nmap's parallel DNS resolver**.  
6. `--dns-servers <server1,server2,...>` → **Specify custom DNS servers**.  
   - Useful for scanning private networks with limited rDNS servers.  


---

## **Nmap Target Specification & Scanning Examples**  

### **1. Scanning Individual Hosts**  
- Scan a single IP:  
  ```bash
  nmap 192.168.1.27
  ```
- Scan multiple domain names:  
  ```bash
  nmap armourinfosec.com thehackersworld.com
  ```
- Scan multiple IPs:  
  ```bash
  nmap 192.168.1.27 192.168.1.207
  ```

### **2. Scanning with Verbose Output (-v)** 
         -v → Normal verbose mode (detailed progress updates).
         -vv → Extra verbose mode (even more details, including individual packet responses).
- Verbose scan of multiple hosts:  
  ```bash
  nmap -v 192.168.1.31 192.168.1.1 192.168.1.247
  ```
- Verbose scan of a range:  
  ```bash
  nmap -v 192.168.1.1-100
  ```
- Another verbose scan with a specified list:  
  ```bash
  nmap -v 192.168.1.1 192.168.1.5 192.168.1.27 192.168.1.100 192.168.1.207
  ```

### **3. Scanning Subnets Using CIDR Notation**  
- Scan an entire subnet (`/24` scans 256 addresses):  
  ```bash
  nmap 216.58.196.78/24
  ```
  ```bash
  nmap 192.168.56.1/24
  ```
  ```bash
  nmap 192.168.1.0/24
  ```
- Scan multiple subnets:  
  ```bash
  nmap 192.168.1-3.1/24
  ```
  ```bash
  nmap 192.168.1,2.1/24
  ```

### **4. Scanning Specific IPs & Ranges**  
- Scan specific IPs in a network:  
  ```bash
  nmap 192.168.1.1,5,27,100,207
  ```
- Scan mixed IP ranges and single addresses:  
  ```bash
  nmap 192.168.1.1-10,50-60,100,200,240-254
  ```
- Another example with specific IPs:  
  ```bash
  nmap 192.168.1.1,10,50,60,100,200,244,231,31,61
  ```

### **5. Special Cases**  
- Scan a combination of subnets and ranges:  
  ```bash
  nmap -V 192.168.1,2.1-100
  ```
- Incorrect command due to a typo (`птар` instead of `nmap`):  
  ```bash
  пmар 192.168.1,1/24
  ```

---

These examples showcase how to use **Nmap** to scan **single hosts, multiple hosts, specific IP ranges, entire subnets, and mixed target types**.
   ```
