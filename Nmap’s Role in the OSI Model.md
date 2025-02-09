    
Nmap’s Role in the OSI Model
	
	Nmap primarily operates at the Network Layer (Layer 3) and Transport Layer (Layer 4) of the OSI model, allowing it to craft and manipulate packets for network
	scanning.

	Key OSI Layers for Nmap
		
		• Network Layer (Layer 3): Handles IP headers, source/destination addresses, TTL, fragmentation, etc.

		• Transport Layer (Layer 4): Manages TCP/UDP headers, ports, and flags for communication and scanning.

		• Data Link Layer (Layer 2): Used for ARP scans and MAC address spoofing.


	Network Layer in Nmap (Layer 3)
		
		The Network Layer is responsible for routing packets across networks. Nmap utilizes this layer for network discovery and scanning.

		How Nmap Uses the Network Layer

		1. IP Packet Handling
		
			• Sends raw IP packets to scan hosts and detect live systems.

			• Uses ICMP Echo Requests (ping sweeps) for host discovery.

			• Can send fragmented IP packets to evade detection.


		2. Routing & Reachability
			
			• Determines the best route to a target.

			• Allows users to specify a network gateway using --source-ip or --route-dst.


		3. Custom IP Packet Manipulation
			
			• Crafts and sends raw IP packets using scan types like -sS, -sU, -sA, etc.

			• Controls packet expiration using TTL adjustments (--ttl).

			• Uses fragmented packets (-f) to bypass firewalls.


		4. Firewall & IDS Evasion
			
			• Decoy scanning (-D) helps mask the real source.

			• Custom MTU size (--mtu) can be set to evade detection.


		5. IP Protocol Scan (-sO)
		
			• Lists supported network layer protocols on a target.


	Transport Layer in Nmap (Layer 4)
		
		The Transport Layer ensures end-to-end communication, reliability, and flow control. Nmap interacts with this layer for port scanning, service detection, and
		security analysis.

	How Nmap Uses the Transport Layer

		1. Port Scanning (TCP & UDP)
		
			• Most common Nmap function at this layer.

			• Checks for open TCP and UDP ports on a target system.


		2. TCP Scans (-sS, -sT, -sN)
			
			• SYN Scan (-sS) – Performs a half-open scan, stealthier than a full TCP handshake.

			• Connect Scan (-sT) – Completes a full TCP handshake, more detectable.

			• Null Scan (-sN) – Sends TCP packets with no flags for firewall evasion.


		3. UDP Scans (-sU)
			
			• Detects open UDP ports (e.g., DNS, SNMP, TFTP).

			• Since UDP is connectionless, scanning is slower and may require packet retransmissions.


		4. Service Version Detection (-sV)
		
			• Identifies the exact version of a service running on an open port.


		5. TCP Flags & Firewall Evasion

			• FIN Scan (-sF) – Sends TCP packets with only the FIN flag, often bypassing some firewalls.


		6. Determining Firewall Rules
			
			• Adjusts scan speed using --scan-delay and --max-retries to bypass IDS/IPS detection.
