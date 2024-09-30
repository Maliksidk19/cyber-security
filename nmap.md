### **Nmap (Network Mapper)**

Nmap is a powerful and widely-used **network discovery** and **security auditing** tool. It's an open-source utility that is commonly used by system administrators, security professionals, and hackers for **network scanning**, **host discovery**, and **port scanning**.

Let’s explore Nmap in **extreme detail**, including its core features, scanning techniques, options, and usage.

---

### **What is Nmap?**

- **Nmap** is a command-line tool designed to scan networks, discover hosts, services, open ports, and detect security vulnerabilities.
- It works by sending packets and analyzing responses, helping users understand network structures and potential weaknesses.
- Nmap can detect **open ports**, **operating systems**, **service versions**, and much more.

---

### **Key Features of Nmap**

1. **Host Discovery**:
   - Identifying live hosts in a network.
   - Common methods include ICMP echo requests (ping), TCP SYN requests, or ARP requests.
2. **Port Scanning**:
   - Discovering **open, closed, filtered**, and **unfiltered ports** on a target machine.
   - Different port scanning techniques can determine the state of ports.
3. **Service and Version Detection**:
   - Identifying running services on open ports (e.g., HTTP, SSH) and determining their versions.
4. **OS Fingerprinting**:
   - Determining the operating system of the target based on unique characteristics of the response packets.
5. **Scripting Engine (NSE)**:
   - Nmap's **Scripting Engine** can automate various network tasks like vulnerability detection, malware detection, and service discovery.
6. **Vulnerability Scanning**:
   - Nmap can scan for specific vulnerabilities (e.g., Heartbleed) using NSE scripts.

---

### **Nmap Scanning Techniques**

Nmap supports various **scanning techniques**, each useful for different purposes. These techniques are typically specified with the `-s` flag.

#### 1. **TCP Connect Scan (`-sT`)**:

- The **default scan** when run without privileges.
- Nmap uses the **complete TCP three-way handshake** to establish connections with open ports.
- **Drawback**: It’s noisy and easily detectable since full connections are established.

#### 2. **TCP SYN Scan (`-sS`)**:

- Also known as **half-open scan**.
- Nmap sends **SYN packets** and waits for a **SYN-ACK** response. If received, Nmap knows the port is open but terminates the connection before the handshake is complete.
- **Advantage**: Faster and stealthier since no full connections are made.

#### 3. **UDP Scan (`-sU`)**:

- Used for scanning **UDP ports**.
- Nmap sends **UDP packets** to the target and analyzes the response.
- **Note**: UDP scanning is slower than TCP due to the nature of the protocol (no handshake).

#### 4. **TCP ACK Scan (`-sA`)**:

- Used to **map out firewall rulesets**.
- Sends ACK packets and determines whether ports are filtered or unfiltered based on the response.

#### 5. **TCP Window Scan (`-sW`)**:

- Exploits TCP window size variations to differentiate between open and closed ports.

#### 6. **FIN, Xmas, and NULL Scans**:

- **FIN Scan (`-sF`)**: Sends a **FIN packet** (no SYN or ACK).
- **Xmas Scan (`-sX`)**: Sends a packet with **FIN, PSH, and URG** flags set.
- **NULL Scan (`-sN`)**: Sends a packet with **no flags**.
- These scans exploit systems that adhere to RFC 793 (the TCP specification) and don’t reply when the port is closed, but respond when the port is open.
- **Advantage**: Stealthier than SYN and connect scans.

#### 7. **Idle Scan (`-sI`)**:

- A **completely stealthy** scan by bouncing packets off an **idle host** (zombie) to avoid detection.
- Attacker uses a third-party machine to scan the target, making the attack hard to trace.

#### 8. **IP Protocol Scan (`-sO`)**:

- Determines which **IP protocols** (ICMP, TCP, UDP, etc.) are supported by the target.

#### 9. **SCTP INIT Scan (`-sY`)**:

- For scanning **SCTP (Stream Control Transmission Protocol)**, which is used for telecommunication applications.
- Like a SYN scan but for SCTP.

#### 10. **SCTP COOKIE-ECHO Scan (`-sZ`)**:

- Similar to the SCTP INIT scan but more focused on how cookies are exchanged in the handshake.

---

### **Nmap Output Formats**

Nmap allows you to output scan results in different formats, useful for automated processing or reporting.

- **Normal Output (`-oN`)**: Default human-readable output.
- **XML Output (`-oX`)**: Used for integration with other tools.
- **Grepable Output (`-oG`)**: Easy to parse using `grep` or other text-processing tools.
- **JSON Output (`-oJ`)**: JSON format for structured output.

---

### **Important Nmap Options and Flags**

#### 1. **Host Discovery**:

- **Ping Scan (`-sn`)**: Simply determines which hosts are up without doing a port scan.
- **ARP Scan (`-PR`)**: Detects hosts on the same LAN via ARP requests.

#### 2. **Port Scanning**:

- **`-p <port>`**: Scans specific ports (e.g., `-p 80`, `-p 1-1000`).
- **`-F`**: Performs a **fast scan**, only scanning the top 100 most common ports.
- **`--top-ports <n>`**: Scans the top `n` most commonly used ports.

#### 3. **Service and Version Detection**:

- **`-sV`**: Detects the version of the services running on open ports.

#### 4. **Operating System Detection**:

- **`-O`**: Enables **OS fingerprinting** to determine the target’s operating system.
- **`--osscan-guess`**: Aggressively guesses the OS if standard detection fails.

#### 5. **Timing and Performance**:

- **`-T<0-5>`**: Sets timing templates from **paranoid** (`T0`) to **aggressive** (`T5`).
  - **`-T4`** is recommended for faster scans with minimal noise.
- **`--min-rate <num>`** and **`--max-rate <num>`**: Set minimum and maximum packet rates.

#### 6. **Verbose and Debugging Output**:

- **`-v`**: Increases verbosity to show more detailed output.
- **`-vv`**: Maximum verbosity.
- **`-d`**: Enables debugging information.

#### 7. **Stealth Techniques**:

- **`-D <decoy1,decoy2,...>`**: Decoy scanning to hide the origin of the scan by spoofing other addresses.
- **`-S <source_ip>`**: Spoofs the source IP address.
- **`--data-length <num>`**: Adds random data to the probe, increasing packet size for evading IDS/IPS.

---

### **Nmap Scripting Engine (NSE)**

The **Nmap Scripting Engine (NSE)** allows users to write or use custom **Lua scripts** to automate network tasks and vulnerability detection. NSE scripts can perform a wide variety of tasks, from simple ping sweeps to complex exploitation.

#### **Types of NSE Scripts**:

- **Discovery Scripts**: Host discovery, OS detection, service version detection.
- **Vulnerability Detection Scripts**: Check for specific vulnerabilities like **Heartbleed**, **MS17-010 (EternalBlue)**.
- **Exploitation Scripts**: Perform specific attacks or vulnerabilities exploitation.
- **Brute Force Scripts**: Crack passwords for services like SSH, FTP, etc.

#### **Running NSE Scripts**:

- `nmap --script <script-name>`: Runs a specific script.
- `nmap --script <category>`: Runs all scripts within a category.

Examples:

- `nmap --script vuln <target>`: Runs all vulnerability-related scripts.
- `nmap --script http-enum <target>`: Enumerates directories on a web server.

---

### **Sample Nmap Commands**

1. **Simple Host Discovery**:

   ```bash
   nmap -sn 192.168.1.0/24
   ```

   - This command will discover live hosts in the 192.168.1.0/24 subnet without scanning ports.

2. **TCP SYN Scan with Service Version Detection**:

   ```bash
   nmap -sS -sV 192.168.1.10
   ```

   - Performs a SYN scan on the target `192.168.1.10` and attempts to detect the version of services running on open ports.

3. **OS Detection and Version Scan**:
   ```bash
   nmap -O -sV 192.168.1.10
   ```
   - Detects the operating system and service versions on the### **Nmap (Network Mapper)**

Nmap is a powerful and widely-used **network discovery** and **security auditing** tool. It's an open-source utility that is commonly used by system administrators, security professionals, and hackers for **network scanning**, **host discovery**, and **port scanning**.

Let’s explore Nmap in **extreme detail**, including its core features, scanning techniques, options, and usage.

---

### **What is Nmap?**

- **Nmap** is a command-line tool designed to scan networks, discover hosts, services, open ports, and detect security vulnerabilities.
- It works by sending packets and analyzing responses, helping users understand network structures and potential weaknesses.
- Nmap can detect **open ports**, **operating systems**, **service versions**, and much more.

---

### **Key Features of Nmap**

1. **Host Discovery**:
   - Identifying live hosts in a network.
   - Common methods include ICMP echo requests (ping), TCP SYN requests, or ARP requests.
2. **Port Scanning**:
   - Discovering **open, closed, filtered**, and **unfiltered ports** on a target machine.
   - Different port scanning techniques can determine the state of ports.
3. **Service and Version Detection**:
   - Identifying running services on open ports (e.g., HTTP, SSH) and determining their versions.
4. **OS Fingerprinting**:
   - Determining the operating system of the target based on unique characteristics of the response packets.
5. **Scripting Engine (NSE)**:
   - Nmap's **Scripting Engine** can automate various network tasks like vulnerability detection, malware detection, and service discovery.
6. **Vulnerability Scanning**:
   - Nmap can scan for specific vulnerabilities (e.g., Heartbleed) using NSE scripts.

---

### **Nmap Scanning Techniques**

Nmap supports various **scanning techniques**, each useful for different purposes. These techniques are typically specified with the `-s` flag.

#### 1. **TCP Connect Scan (`-sT`)**:

- The **default scan** when run without privileges.
- Nmap uses the **complete TCP three-way handshake** to establish connections with open ports.
- **Drawback**: It’s noisy and easily detectable since full connections are established.

#### 2. **TCP SYN Scan (`-sS`)**:

- Also known as **half-open scan**.
- Nmap sends **SYN packets** and waits for a **SYN-ACK** response. If received, Nmap knows the port is open but terminates the connection before the handshake is complete.
- **Advantage**: Faster and stealthier since no full connections are made.

#### 3. **UDP Scan (`-sU`)**:

- Used for scanning **UDP ports**.
- Nmap sends **UDP packets** to the target and analyzes the response.
- **Note**: UDP scanning is slower than TCP due to the nature of the protocol (no handshake).

#### 4. **TCP ACK Scan (`-sA`)**:

- Used to **map out firewall rulesets**.
- Sends ACK packets and determines whether ports are filtered or unfiltered based on the response.

#### 5. **TCP Window Scan (`-sW`)**:

- Exploits TCP window size variations to differentiate between open and closed ports.

#### 6. **FIN, Xmas, and NULL Scans**:

- **FIN Scan (`-sF`)**: Sends a **FIN packet** (no SYN or ACK).
- **Xmas Scan (`-sX`)**: Sends a packet with **FIN, PSH, and URG** flags set.
- **NULL Scan (`-sN`)**: Sends a packet with **no flags**.
- These scans exploit systems that adhere to RFC 793 (the TCP specification) and don’t reply when the port is closed, but respond when the port is open.
- **Advantage**: Stealthier than SYN and connect scans.

#### 7. **Idle Scan (`-sI`)**:

- A **completely stealthy** scan by bouncing packets off an **idle host** (zombie) to avoid detection.
- Attacker uses a third-party machine to scan the target, making the attack hard to trace.

#### 8. **IP Protocol Scan (`-sO`)**:

- Determines which **IP protocols** (ICMP, TCP, UDP, etc.) are supported by the target.

#### 9. **SCTP INIT Scan (`-sY`)**:

- For scanning **SCTP (Stream Control Transmission Protocol)**, which is used for telecommunication applications.
- Like a SYN scan but for SCTP.

#### 10. **SCTP COOKIE-ECHO Scan (`-sZ`)**:

- Similar to the SCTP INIT scan but more focused on how cookies are exchanged in the handshake.

---

### **Nmap Output Formats**

Nmap allows you to output scan results in different formats, useful for automated processing or reporting.

- **Normal Output (`-oN`)**: Default human-readable output.
- **XML Output (`-oX`)**: Used for integration with other tools.
- **Grepable Output (`-oG`)**: Easy to parse using `grep` or other text-processing tools.
- **JSON Output (`-oJ`)**: JSON format for structured output.

---

### **Important Nmap Options and Flags**

#### 1. **Host Discovery**:

- **Ping Scan (`-sn`)**: Simply determines which hosts are up without doing a port scan.
- **ARP Scan (`-PR`)**: Detects hosts on the same LAN via ARP requests.

#### 2. **Port Scanning**:

- **`-p <port>`**: Scans specific ports (e.g., `-p 80`, `-p 1-1000`).
- **`-F`**: Performs a **fast scan**, only scanning the top 100 most common ports.
- **`--top-ports <n>`**: Scans the top `n` most commonly used ports.

#### 3. **Service and Version Detection**:

- **`-sV`**: Detects the version of the services running on open ports.

#### 4. **Operating System Detection**:

- **`-O`**: Enables **OS fingerprinting** to determine the target’s operating system.
- **`--osscan-guess`**: Aggressively guesses the OS if standard detection fails.

#### 5. **Timing and Performance**:

- **`-T<0-5>`**: Sets timing templates from **paranoid** (`T0`) to **aggressive** (`T5`).
  - **`-T4`** is recommended for faster scans with minimal noise.
- **`--min-rate <num>`** and **`--max-rate <num>`**: Set minimum and maximum packet rates.

#### 6. **Verbose and Debugging Output**:

- **`-v`**: Increases verbosity to show more detailed output.
- **`-vv`**: Maximum verbosity.
- **`-d`**: Enables debugging information.

#### 7. **Stealth Techniques**:

- **`-D <decoy1,decoy2,...>`**: Decoy scanning to hide the origin of the scan by spoofing other addresses.
- **`-S <source_ip>`**: Spoofs the source IP address.
- **`--data-length <num>`**: Adds random data to the probe, increasing packet size for evading IDS/IPS.

---

### **Nmap Scripting Engine (NSE)**

The **Nmap Scripting Engine (NSE)** allows users to write or use custom **Lua scripts** to automate network tasks and vulnerability detection. NSE scripts can perform a wide variety of tasks, from simple ping sweeps to complex exploitation.

#### **Types of NSE Scripts**:

- **Discovery Scripts**: Host discovery, OS detection, service version detection.
- **Vulnerability Detection Scripts**: Check for specific vulnerabilities like **Heartbleed**, **MS17-010 (EternalBlue)**.
- **Exploitation Scripts**: Perform specific attacks or vulnerabilities exploitation.
- **Brute Force Scripts**: Crack passwords for services like SSH, FTP, etc.

#### **Running NSE Scripts**:

- `nmap --script <script-name>`: Runs a specific script.
- `nmap --script <category>`: Runs all scripts within a category.

Examples:

- `nmap --script vuln <target>`: Runs all vulnerability-related scripts.
- `nmap --script http-enum <target>`: Enumerates directories on a web server.

---

### **Sample Nmap Commands**

1. **Simple Host Discovery**:

   ```bash
   nmap -sn 192.168.1.0/24
   ```

   - This command will discover live hosts in the 192.168.1.0/24 subnet without scanning ports.

2. **TCP SYN Scan with Service Version Detection**:

   ```bash
   nmap -sS -sV 192.168.1.10
   ```

   - Performs a SYN scan on the target `192.168.1.10` and attempts to detect the version of services running on open ports.

3. **OS Detection and Version Scan**:
   ```bash
   nmap -O -sV 192.168.1.10
   ```
   - Detects the operating system and service versions on the
