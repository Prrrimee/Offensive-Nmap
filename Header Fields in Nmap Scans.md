

---

# **Header Fields in Nmap Scans**  

## **IP Header Fields & Their Role in Nmap Scans**  

1. **Version:** Always set to the value **4** in the current version of IP.  
   - Always **4** for IPv4 packets.  
   - Nmap primarily works with IPv4 but also supports IPv6 (using the `-6` option).  
   - Example:  
     ```
     nmap -6 2001:db8:3333:4444:5555:6666:7777:8888
     ```  

2. **IP Header Length (IHL):** Specifies the number of 32-bit words forming the header, usually five.  
   - Default: 5 words = 20 bytes.  
   - Increases if options are used.  
   - Nmap allows modification of headers in raw packet scans (`-sO` for protocol scan).  

3. **Type of Service (ToS) / Differentiated Services Code Point (DSCP):**  
   - Used for **Quality of Service (QoS)** in routing.  
   - Nmap lets you set ToS using `--ip-options`.  
   - Example:  
     ```
     nmap --ip-options 'Ix03' 192.168.1.1
     ```  

4. **Size of Datagram:**  
   - Specifies the total length of the packet (header + data).  
   - Large packets may be fragmented, and Nmap can manipulate fragmentation using `-f`.  
   - Used for firewall evasion.  
   - Example:  
     ```
     nmap -f 192.168.1.1
     ```  

5. **Identification:**  
   - A 16-bit number that, together with the source address, uniquely identifies the packet.  
   - Used during the **reassembly of fragmented datagrams**.  
   - Can be observed using `tcpdump` when analyzing Nmap traffic.  

6. **Flags:**  
   - `DF` (Don't Fragment) → Prevents packet fragmentation.  
   - `MF` (More Fragments) → Indicates that more fragments are coming.  
   - Nmap can manipulate fragmentation to bypass **Intrusion Detection Systems (IDS)**.  
   - Example:  
     ```
     nmap -f 192.168.1.1
     ```  

7. **Fragmentation Offset:**  
   - Indicates where a fragmented packet belongs in the original datagram.  
   - Helps in evading firewalls when used with `-f`.  

8. **Time to Live (TTL):**  
   - Specifies the **number of hops** the packet may be routed over before being discarded.  
   - Decrements at each hop to prevent infinite loops.  
   - Can be set in Nmap to control traceability.  

9. **Protocol:** Identifies the **Transport Layer protocol** used.  
   - Common values:  
     - `1 = ICMP`  
     - `6 = TCP`  
     - `17 = UDP`  
   - Nmap **IP Protocol Scan** (`-sO`) identifies supported protocols.  
   - Example:  
     ```
     nmap -sO 192.168.1.1
     ```  

10. **Header Checksum:**  
   - A **1's complement checksum** inserted by the sender to ensure header integrity.  
   - Updated whenever the packet header is modified by a router.  
   - Automatically calculated—Nmap does not provide direct control over this field.  

11. **Source Address:**  
   - The IP address of the **original sender** of the packet.  
   - Useful for **stealth scans**.  
   - Nmap can **spoof the source IP** using `-S`.  
   - Example:  
     ```
     nmap -S 192.168.1.100 192.168.1.1
     ```  

12. **Destination Address:**  
   - The IP address of the **final destination** of the packet.  
   - Can be set manually or via **CIDR notation** (`/24` for subnet scans).  

13. **Options:**  
   - Rarely used but allows features like **Record Route (RR)**.  
   - Can be set in Nmap using `--ip-options`.  
   - Example:  
     ```
     nmap --ip-options 'Ix07\x27\x08\x08\x08\x08' 192.168.1.1
     ```  

### **How Nmap Uses These Fields in Scans**  

| IP Header Field | Nmap Usage |
|----------------|-------------|
| TTL | Used in OS detection (`-O`), hop counting |
| Flags | Used for fragmentation (`-f`), stealth scans |
| Source Address | IP spoofing (`-S <IP>`) |
| Protocol | Protocol scan (`-sO`) |
| Size of Datagram | Custom payloads |

---

## **TCP Header Fields & Their Role in Nmap Scans**  

The **Transmission Control Protocol (TCP)** header is critical for **reliable communication** and plays a key role in Nmap's scanning techniques.  

1. **Source Port (16 bits):**  
   - The port used by the sender to initiate communication.  
   - Nmap allows setting the **source port** using `--source-port`, which can help evade firewalls.  
   - Example:  
     ```
     nmap --source-port 53 192.168.1.1
     ```  

2. **Destination Port (16 bits):**  
   - Specifies the target port on the **destination machine**.  
   - Nmap scans specific ports or ranges using `-p`.  
   - Example:  
     ```
     nmap -p 22,80,443 192.168.1.1
     ```  

3. **Sequence Number (32 bits):**  
   - Used to order **TCP segments**.  
   - Nmap uses ISNs in **OS fingerprinting (`-O`)** because different OSes generate ISNs differently.  

4. **Acknowledgment Number (32 bits):**  
   - Indicates that the receiver has received data up to this sequence number.  
   - Used in the **TCP three-way handshake**.  

5. **Data Offset (4 bits):**  
   - Defines the **length of the TCP header** (usually **5 words = 20 bytes**).  
   - Increases if **options** are present.  

6. **TCP Flags (9 bits):**  
   - Controls connection establishment, termination, and scanning techniques.  
   - Nmap **stealth scans** use specific flag manipulations:  

| Flag | Description | Nmap Usage |
|------|-------------|-------------|
| **URG (Urgent)** | Marks urgent data | Rarely used in scans |
| **ACK (Acknowledgment)** | Acknowledges received data | Used in ACK scans (`-sA`) |
| **PSH (Push)** | Pushes data to the application | Rarely used in scans |
| **RST (Reset)** | Resets a connection | Used in RST packet responses |
| **SYN (Synchronize)** | Initiates a connection | Used in SYN scans (`-sS`) |
| **FIN (Finish)** | Gracefully closes a connection | Used in FIN scans (`-sF`) |

---

## **UDP Header Fields & Their Role in Nmap Scans**  

UDP is a **connectionless transport layer protocol** used for fast, low-overhead communication. Unlike TCP, UDP **does not guarantee delivery**.  

### **Key UDP Scanning Techniques in Nmap (`-sU`)**  

1. **UDP Scan (`-sU`):**  
   - Sends an **empty UDP packet** to the target port.  
   - Relies on **ICMP Port Unreachable (Type 3, Code 3) responses**.  
   - Example:  
     ```
     nmap -sU -p 53,161 192.168.1.1
     ```  

   **Responses:**  
   - If **ICMP Type 3, Code 3** is received → **Port is closed**.  
   - If **no response** → **Port is open or filtered**.  
   - If **ICMP Type 3, Code 1, 2, 9, 10, or 13** → **Port is filtered (firewall blocking)**.  


---

## **ICMP Header Fields & Their Role in Nmap Scans**  

### **1. Type (8 bits)**  
- Defines the **ICMP message type**.  
- Important for **host discovery (`-sn`)** and **firewall detection**.  

| ICMP Type | Meaning | Nmap Usage |
|-----------|---------|-------------|
| **0** | Echo Reply | Response to ping |
| **3** | Destination Unreachable | Firewall detection |
| **5** | Redirect | Route changes |
| **8** | Echo Request | Sent in pings (`-sn`) |
| **11** | Time Exceeded | Used in traceroute (`--traceroute`) |

Example:  
```
nmap -sn 192.168.1.1
```

### **2. Code (8 bits)**  
- Further categorizes the **ICMP Type**.  
- Example: ICMP Type 3 (Destination Unreachable) has these codes:  

| Code | Meaning |
|------|---------|
| **0** | Network Unreachable |
| **1** | Host Unreachable |
| **2** | Protocol Unreachable |
| **3** | Port Unreachable |

### **3. Checksum (16 bits)**  
- Ensures **integrity of the ICMP message**.  

### **4. Rest of the Header (Variable Length)**  
- Contains additional information depending on **ICMP Type**.  

---

## **How Nmap Uses ICMP in Scanning**  

### **1. ICMP Ping Scan (`-sn`)**  
- Sends an **ICMP Echo Request** to check if a host is up.  
- If a host responds with an **Echo Reply (Type 0)**, it is **alive**.  
- If no response → the host is **down or filtered**.  

Example:  
```
nmap -sn 192.168.1.1
```

### **2. ICMP Timestamp Scan (`--packet-trace`)**  
- Used for **time-based fingerprinting**.  
- Measures the difference between **send time and reply time**.  

### **3. Firewall Detection (ICMP Type 3, Code 3)**  
- If a UDP scan (`-sU`) returns **ICMP Type 3, Code 3**, the port is **closed**.  
- If **no response**, the port is **open or filtered**.  

---
