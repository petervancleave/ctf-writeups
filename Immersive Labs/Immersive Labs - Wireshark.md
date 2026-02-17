
![](attachment/b549e5dddd532fb61fc664d02ca11e1f.png)

# <font color="#ff0000">Introduction to Wireshark</font>

---

1. Familiarise yourself with the Wireshark application and its uses by reading the ‘briefing’ section.
### Wireshark Overview

- **What it is:** A free, open-source packet analyzer (GUI-based). The terminal version is called **TShark**.
- **Primary Use:** Network troubleshooting, protocol development, analysis, and education.

### Key Features

- **Capture Sources:** Can capture live traffic from networks (Ethernet, Wi-Fi) or read from saved packet files (PCAP).
- **Filtering:** Uses a powerful display filter language to refine which packets are shown.
- **Protocol Analysis:** Plugins allow for dissecting new protocols. Can detect and even play VoIP calls.
- **Scope:** Can capture raw USB traffic and wireless traffic visible to the monitor.
- **Name Resolution:** Can convert numerical addresses/ports into human-readable names (e.g., `80` → `HTTP`).

### Using the Interface

- **Main Panes:** The default view shows a packet list, packet details (tree view), and packet bytes (raw data).
- **Inspection:** Click a packet to expand its details in the tree view to see protocol information.
- **Filter Creation:** Right-click a detail in the packet view and select **"Apply as Filter"** to quickly create filters.

### Filtering Basics

- **Purpose:** To search for specific packets (e.g., by protocol, IP, port).
- **Syntax:** Uses fields and operators (e.g., `==`, `&&`, `||`).
    **Example:** `tcp.port eq 161 or icmp` (Shows SNMP or ICMP traffic).
- **Built-in Options:** Can access default filters via **Analyze > Display Filters**.

Investigation Goal: To spot patterns in network traffic and determine if they correspond to legitimate activity or potential abuse.

---

2. Open the pcap file found in the `labfiles` directory.

![](attachment/3e8e4a3ea7113be89fff1b0d1c3945e7.png)

![](attachment/36db9ad9c57bdaabecb9db529e4a0115.png)

---

3. What is the difference between resolved and unresolved ports on the Wireshark display setup?

A. Resolved ports display all information about the port (including destination and header data), whereas unresolved sources only show the raw data.

B. Resolved ports display the name of the well-known service that runs on that port, whereas unresolved ports just display the number.

Answer: B

---

4. What is the correct syntax to use on Wireshark for showing only SMTP and ICMP traffic?

A. tcpdump.list 25 7

B. tcp.show smtp & icmp

C. show 25 & icmp

D. tcp.port eq 25 or icmp

Answer: D

---

5. Using wireshark_setup.pcapng, filter the packets to view only HTTP requests. What is the source IP address shown on the last packet?

![](attachment/e88c9d8f50ad95dc566ee8516bab9c75.png)

Answer: 172.21.2.217

---

6. Within that same packet, what is the time shown? Your answer must be in YYYY-MM-DD HH:MM:SS format adjusted for UTC.

![](attachment/1482c996b02a7a5fcbf968a8946ad96d.png)

Answer: 2017-12-12 13:04:10

---

7. What is the destination IP address of the last packet?

Answer: 34.232.90.203

---
# <font color="#ff0000">Display Filters - Introduction to Filters</font>

---

1. Understand what Wireshark display filters are and how they are used.

### Purpose of Display Filters

- **Why use them:** Network captures (PCAPs) contain大量 noise, even over short periods.
- **Goal:** To filter out irrelevant traffic and focus on data relevant to an investigation, such as **Indicators of Compromise (IOCs)** .
- **Benefit:** Helps identify malicious traffic patterns between systems without manually scrolling through thousands of packets.

### Basic Syntax

The fundamental structure for a filter is:

`<field>.<subfield> <comparison_operator> <value>`

- **Field:** The protocol or field you want to inspect (e.g., `tcp`, `ip`, `dns`).
- **Comparison Operators:** Used to define the relationship.
    - `eq` or `==` (Equal to)
    - `ne` or `!=` (Not equal to)
- **Logical Operators (for combining):**
    - `and` or `&&`
    - `or` or `||`

### Common Examples

- **Filter by Protocol:** Simply type the protocol name.
    - `tcp` (Shows all TCP packets)
    - `dns` (Shows all DNS packets)
- **Filter by Specific Value:** Target a specific port or address.
    - `tcp.port == 80` (Shows TCP traffic on port 80/HTTP)
- **Finding Supported Protocols:** View the full list via **View → Internals → Supported Protocols**.

---

2. Open the PCAP file located in the `labfiles` directory.

![](attachment/28dcc46db0a47dc975bedbc28bfd5777.png)

![](attachment/4c4515d17156a37e8673e6fd96638556.png)

---

3. Analyze and identify the information needed to complete the lab exercise.

---

4. From the PCAP provided, apply a filter to display all TCP packets. What percentage of results are then displayed in the current capture?

![](attachment/605a80fede88e09c373b208309d9d9b2.png)

Answer: 46.8%

---

5. From the PCAP provided, apply a filter to display all packets from the source IP 10.0.50.227. What percentage of results are then displayed in the current capture?

ip.src == 10.0.50.227

![](attachment/e7df44a238bc49a2b0fce98532e5f900.png)

Answer: 38.2%

---

6. From the PCAP provided, apply a filter to display all packets sent to the IP 31.13.90.36. What is the source IP of each of these?

ip.dst == 31.13.90.36

![](attachment/f11f9adf4e9cc5ccab50e3d03a7a61cc.png)

Answer: 10.0.50.227

---

7. From the PCAP provided, apply a filter to display all TCP packets on port 80. How many packets are displayed?

tcp.port == 80

![](attachment/dcc1d675e1dbf6583f7d5d1fcac7378d.png)

Answer: 15

---

8. ip.dst == 10.10.10.10. What does this line of code do?

A. Filter capture to show all packets sent to the IP address 10.10.10.10

B. Filter capture to show everything but the packets sent to 10.10.10.10

C. Filter capture to hide all packets sent to the IP address 10.10.10.10

Answer: A

---
# <font color="#ff0000">Display Filters - Diving In</font>

---

1. Read the **Briefing** panel to understand some more complex filters/operators.

### Visual Feedback

- **Red Bar:** The filter expression is invalid/unaccepted.
- **Green Bar:** The expression is valid and will work as expected.
- **Yellow Bar:** The expression is accepted but may not work as intended.

### Advanced Operators

#### 1. Comparing Values

- **Basic Comparisons:** Use operators like `eq`, `ne`, `gt`, `lt` to compare field values.
- **`contains`** : Searches for a **sequence of characters** (string) anywhere within a field.
    - _Example:_ `http contains "https://malicious.com"`
- **`matches` (or `~`)** : Uses **Perl-compatible regular expressions (PCRE)** for pattern matching. Allows for case-sensitive matching.
    - _Example:_ `tcp.flags.str matches "(?-i)ACK"`

#### 2. The Slice Operator (`[]`)

- **Purpose:** Filters based on a **specific portion (range)** of a field (e.g., first 3 bytes of a MAC address).
- **Syntax:** `field[start:length]`
    - _Example:_ `eth.src[0:3] == 32.65.76` (Filters for Ethernet addresses starting with that specific byte sequence).
- **Negative Offsets:** Count from the end of the field.
    - _Example:_ `eth.src[-3:3]` looks at the last three bytes.
- **Concatenation:** Combine multiple slices using a comma (`,`).

#### 3. The Membership Operator (`in`)

- **Purpose:** Tests if a field matches **any value** within a specified set.
- **Syntax:** `field in {value1 value2 value3}`
    - _Example:_ `tcp.srcport in {80 8080 808080}` (Shows traffic from any of these source ports).
- **Ranges:** Can include ranges within the set using `..`.
    - _Example:_ `tcp.srcport in {80 8080 80..8080}`

---

2. Navigate to your **Desktop** and open **file.pcap** in Wireshark.

![](attachment/0533982f766b6b66fc1eb9f9f50a75db.png)

---

3. Apply a filter that displays all SMTP traffic containing the text “Subject: ”

smtp contains "Subject:"

![](attachment/d4bff4db5152c939b8937aae573ba978.png)

---

4. What is the first name of the recipient of that email?

![](attachment/0b4ab77413890c3cfe8d52451dda9be9.png)

Answer: Sarah

---

5. Change the filter so it now displays all SMTP response traffic matching the text ".co.uk".

smtp contains ".co.uk"

![](attachment/c2d8e4a41e86972718789a06667f9fd8.png)

---

6. What is the frame number of this packet?

![](attachment/08e6d16d14fc5fc66513acbcb0c70bed.png)

Answer: 9932

---

7. Remove the existing filter. Now, apply a filter that displays all packets from UDP source ports 53, 59015, and 63518.

udp.srcport == 53 || udp.srcport == 59015 || udp.srcport == 63518

![](attachment/eacbb8d3587a30b2439c3c8185c298a1.png)

---

8. How many packets are then displayed?

![](attachment/0038263806a554b63bfddf1e734d8a55.png)

Answer: 60

---

9. Take the following slice expression `(frame[-4:4] == 0.1.2.3).`

`frame[-4:4] == 00:01:02:03`

---

10. At which offset does the slice begin?

Answer: -4

---

11. Take the following slice expression `(frame[:4] == 0.1.2.3).`

`frame[:4] == 00:01:02:03`

---

12. At which offset does the slice begin?

Answer: 0

---
# <font color="#ff0000">Display Filters - Combining Filters</font>

---

### Purpose of Combining Filters

- **Why:** In large packet captures, single filters are often not enough. Combining filters allows you to narrow down traffic using multiple conditions in one expression.
- **Goal:** To create precise searches for specific IOCs or traffic patterns (e.g., a specific IP communicating on a specific port).

### Logical Operators

These are used to join multiple filter expressions:

|Operator|Meaning|Example|
|---|---|---|
|`and` / `&&`|Both conditions must be true|`ip.src == 192.168.0.1 && tcp.port == 80`|
|`or` / `\|`|Either condition can be true|`ip.src == 192.168.0.1 \| tcp.port == 80`|
|`not` / `!`|Excludes packets matching the condition|`!(tcp.port == 80)`|

### Example Breakdown

**Filter:** `ip.src == 192.168.0.1 && tcp.port == 80`

- **`ip.src == 192.168.0.1`** : Looks for packets where the source IP address is `192.168.0.1`.
- **`&&`** : The logical AND operator, meaning **both** conditions must be met.
- **`tcp.port == 80`** : Looks for packets with TCP port 80 (HTTP).

**Result:** Shows only packets coming from IP `192.168.0.1` **AND** using TCP port 80.

### Exclusion/ Negation

To filter **for** an IP but **exclude** a specific port:

- **Method 1:** Use the `!=` operator.
    - `ip.src == 192.168.0.1 && tcp.port != 80`
- **Method 2 (More Reliable):** Use brackets `()` with the `!` operator.
    - `ip.src == 192.168.0.1 && !(tcp.port == 80)`

---

1. Open the PCAP located on your desktop in Wireshark.

![](attachment/5ea40972c21474ffe9625bde00d0ce21.png)

---

2. From the PCAP provided, apply a filter to display all packets where the destination UDP port is **59485** and the destination IP address is `10.6.5.102`.

udp.dstport == 59485 && ip.dst == 10.6.5.102

![](attachment/8d09d646611efe2ab51bb2fbaed22cbf.png)

---

3. What is the transaction ID of this packet response?

![](attachment/03cc3bd606c2a341c139f5ee6fba8091.png)

Answer: 0xd27b

---

4. From the PCAP provided, apply the following filter to display all web traffic:
`http.request or ssl.handshake.type == 1`

![](attachment/e6b4d27742f3e341f0b04ad21efca946.png)

---

5. What percentage of results are then displayed in the capture?

Answer: 0.3%

---

6. Take the filter used in the previous question, and add an OR expression that includes packets where `tcp.flags` is equal to 0x0002. Add another expression to the filter which excludes packets going to/from TCP port 25.

http.request or ssl.handshake.type == 1 or tcp.flags == 0x0002 and !(tcp.port == 25)

![](attachment/3ebacf110f2ac120e302441569889826.png)

---

7. What percentage of results are then displayed in the capture?

Answer: 0.8%

---

8. Using the same filter, expand the OR section to also include DNS packets.

(http.request or ssl.handshake.type == 1 or tcp.flags == 0x0002 or dns) and !(tcp.port == 25)

![](attachment/5a5831ffeb28e8a01accefe82d20ae83.png)

---

9. What percentage of results are now displayed in the capture?

Answer: 1.9%

---
# <font color="#ff0000">Metrics and Statistics</font>

---

### Purpose of Statistics

- **Why:** Manually filtering for IPs assumes you already know what to look for. Statistics provide a **bird's-eye view** of the capture to identify anomalies, top talkers, and patterns quickly.
- **Goal:** To surface key investigative leads (like large data transfers or suspicious endpoints) without prior knowledge of the traffic.

### Key Statistics in Wireshark

Found under the **Statistics** menu at the top of the screen.

#### 1. Conversations

- **Definition:** Traffic between **two specific endpoints** (e.g., all traffic between IP A and IP B).
- **Use Case:** Identify which pairs of devices are communicating the most. Sorting by "Bytes" can reveal potential data exfiltration or command & control traffic.
- **Organization:** Grouped by protocol (IPv4, TCP, UDP, etc.). The number in the tab indicates how many conversations exist for that protocol.

#### 2. Endpoints

- **Definition:** A single logical device on the network (e.g., one specific IP address).
- **Difference from Conversations:** Shows traffic **to and from** a single machine, rather than a pair.
- **Use Case:** Identify which specific device is the "noisiest" or is sending/receiving the most data.

#### 3. Resolved Addresses

- **Definition:** A list of all IP addresses and their corresponding **hostnames** (resolved from DNS traffic in the capture).
- **Use Case:** Quickly get a human-readable summary of all external resources the infected host contacted (e.g., `malware.com` instead of `192.0.2.1`). Useful for reports.

#### 4. IO Graphs

- **Definition:** A **visual, graphical representation** of traffic over time.
- **Use Case:** Spot patterns visually (e.g., periodic beaconing to a C2 server). You can plot up to five different filters on the same graph to compare protocols or traffic types.
- **Export:** Can be saved as an image or copied as CSV for reports.

### Summary Diagram 

- **Conversations:** Show traffic _between_ pairs.
- **Endpoints:** Show traffic _to/from_ a single machine.
- Both display **byte counts** to help spot unusual transfers.

---

1. Open the PCAP file located in the `labfiles` directory and use Wireshark statistics to analyze the packet capture.

![](attachment/234d17af7da28d14447dcf11eb069bda.png)

---

2. What percentage of packets in the capture file are TCP? (rounded to one decimal place)

![](attachment/93783464235240da179a2db5169155bd.png)

Answer: 97.6%

---

3. How many IPv4 'Endpoints' are in the capture file?

- Navigate to the **Statistics** menu at the top of the window.
- Select **Endpoints** from the dropdown list.
- In the window that pops up, click on the **IPv4** tab.

![](attachment/032571c99fecd1f02a533f56f1ba63c2.png)

or

![](attachment/da8c6c08c8e81f830c1c6668ffd16346.png)

Answer: 6

---

4. How many IPv4 'Conversations' are in the capture file?

**Statistics** -> **Conversations** -> **IPv4**

![](attachment/c8fa89f845cf682471e86c2bdbecc91c.png)

Answer: 5

---

5. How many packets are sent between the IP addresses 10.1.10.101 and 185.236.202.244?

![](attachment/b7baf8d0de03826b897c584c8d40e84f.png)

Answer: 3127

---

6. How many packets are sent AND received from 185.236.202.244 port 443?

Apply the filter:

ip.addr == 185.236.202.244 and tcp.port == 443

![](attachment/c7fe423c8172ccbe43a026ffe9f498db.png)

Answer: 2377

---

7. What is the resolved host of the IP address 185.236.202.244?

- Go to **Statistics** -> **Resolved Addresses**.
- In the dialog box, click the **Hosts** tab.
- Use the search/filter bar at the bottom to type `185.236.202.244`.
- The hostname will appear in the column next to the IP.

![](attachment/5452df05f1428b934b8da025176353df.png)

Answer: 1shining.xyz

---
# <font color="#ff0000">Stream/Object Extraction</font>

---

### Purpose

- **Why:** To move beyond raw packets and view or recover the actual **data being transmitted** (e.g., files, images, text) as seen by the application layer.
- **Goal:** To extract artifacts (objects) or reconstruct conversations for evidence and deeper inspection.

### Following Streams

- **What it does:** Reassembles the conversation between two endpoints into a single, readable view (ASCII or hex).
- **How to do it:** Right-click a packet → **Follow** → **Follow `<protocol>` Stream** (TCP, UDP, HTTP, TLS).
- **Benefit:** Simplifies analysis by showing the raw data exchange (e.g., the contents of a chat message or an FTP command) without packet overhead. You can also save the raw stream to disk.

### Exporting Objects

- **What it does:** Extracts complete files that were transferred over the network.
- **Supported protocols:** HTTP, SMB, etc.
- **How to do it:** **File** → **Export Objects** → Select protocol type (e.g., HTTP) → Choose file(s) to save.
- **Use Case:** Recovering malware, images, documents, or web pages transferred during the capture for offline analysis.

### Exporting Packet Bytes

- **What it does:** Exports the raw bytes from a **single specific packet** (or a selection).
- **When to use it:** When data is fragmented across multiple packets (e.g., MIME multipart data).
- **Process:**
    1. Export the data section from each relevant packet as individual files (e.g., `file0`, `file1`).
    2. Reassemble them using a command line tool.
- **Reassembly Example:** `cat file* > complete_file` (Concatenates all parts back into the original file).

### FTP

- When files are transferred via FTP, look for **FTP-DATA** packets. Following this stream or extracting the object will recover the transferred file.

---

1. Open the PCAP file located in the `labfiles` directory and analyze the traffic in this file.

2. What is the stream index number of the first FTP `PASS` command?

Answer: 190

3. What source port was used to transfer the FTP text file?

Answer: 49233

4. Open the **export** **HTTP objects** window.

5. What is the size of the first image/gif (in bytes) in the HTTP objects list?

Answer: 43

6. Export the file `show_ads.js` . Once saved, run `md5sum` on this file.

7. What are the last five characters of this hash?

Answer: c6414

8. Open the export SMB objects window and export the `.docx` file. Once saved, use `unzip` to extract the contents of the file.

9. Inside one of the XML files is a flag. What is the flag?

Answer: 79e0f325804dafbdaef73b3b17c0fd8d

---
# <font color="#ff0000">TLS Traffic</font>

---

### Purpose

- **Why:** Modern network traffic (especially HTTPS) is often encrypted with TLS, making packet contents unreadable.
- **Goal:** To decrypt TLS traffic in Wireshark using session keys to analyze hidden malicious activity.

### TLS Overview

- **TLS (Transport Layer Security):** A cryptographic protocol that encrypts communication between two hosts (successor to SSL).
- **Dual Purpose:**
    1. **Encryption:** Protects data in transit.
    2. **Identity Assurance:** Provides trust via certificates.
- **Attacker Use:** Cybercriminals also use TLS to hide malicious traffic and make phishing sites appear "secure."

### Decrypting TLS in Wireshark

Wireshark can decrypt TLS if provided with the appropriate secrets. Two main methods:

1. **Key Log File (Pre-Master Secret) - Focus of this lab:** Uses per-session keys logged by the browser or client.
2. **RSA Private Key:** Uses the server's private key (less common).

### Method: Using a Key Log File

- **What it is:** A text file containing session secrets (keys) that allow Wireshark to decrypt the traffic.
- **Source:** Can be generated by setting an environment variable in the browser before capture, or extracted post-capture using third-party tools.
- **How to configure in Wireshark:**
    1. Go to **Edit** → **Preferences**.
    2. Navigate to **Protocols** → **SSL** (or **TLS** if available).
    3. In the "(Pre)-Master-Secret log filename" field, browse and select the key log file.

---

1. What does TLS stand for?

Answer: Transport Layer Security

---

2. Open the pcap file located in the `labfiles` directory.

![](attachment/b81f7b9b0c1cd023622616b86bd26c6d.png)

---

3. Decrypt SSL traffic in the Wireshark application.

- **Open Wireshark** and load your lab PCAP file.
- Go to **Edit** > **Preferences** (on a Mac: **Wireshark** > **Settings**).
- On the left-hand menu, expand **Protocols**.
- Scroll down to **TLS** (in older versions of Wireshark, this is labeled **SSL**).
- Find the field labeled **(Pre)-Master Secret log filename**.
- Click **Browse...** and select your `sslkeylog.log` file.
- Click **OK**.

![](attachment/c06e3001507a157635a40803bccb1368.png)

---

4. Identify the online service that was used to exfiltrate stolen data.

http.request.method == "POST" || http.request.method == "PUT"

![](attachment/48c8a0a61595a6b179993d32bef120ab.png)

---

5. What domain was used to exfiltrate the data?

![](attachment/f945e3157ada0f9d5a43c3e5af442cd7.png)

Answer: ghostbin.com

---

6. What is the TCP stream index number of the GET request in which the stolen data was submitted?

http.request.method == "GET"

![](attachment/0c0a421adf5f61fad4d68a53ab3c26b8.png)

Answer: 48

---

7. What is the filename of the submitted html file?

Answer: mst49

---

8. Identify the flag in the POSTed data.

http.request.method == "POST" || http.request.method == "PUT"

![](attachment/f29ef481ea21b90d89bc26c99d82c27b.png)

---

9. What is the hidden flag posted in place of the stolen data?

Answer: H3r3_iS_Y0u3r_F14g!

---
# <font color="#ff0000">Demonstrate Your Skills</font>

---

1. Analyze the PCAP file located in the `labfiles` directory.

![](attachment/d2206a33a147c2a0258d0372fc63cd94.png)

---

2. What is the IP address of the server the attacker was attempting to exfiltrate information to?

http.request.method == "POST" || http.request.method == "PUT"

![](attachment/029fb145cf256c81183429f736dac5f4.png)

Answer: 34.242.181.224

---

3. Identify the network packets where the attacker was attempting to steal information.

---

4. Identify how many files the attacker was attempting to exfiltrate.

---

5. How many files did the attacker attempt to send to the remote server?

http.content_type contains "multipart"

![](attachment/4804ebb66426fbfd50ef25de72396223.png)

Answer: 10

---

6. Based on the time between each file transfer, how many seconds did the attacker wait before attempting to send each file to the remote server?

Calculate the time by subtracting the time stamps of the packets.

They are generally between 90-95 seconds.

Example:

![](attachment/c6ff155c3228124ac663a598c388e01d.png)

![](attachment/26ae1645829233421a08668c48dcb9bc.png)

![](attachment/a726aff2b8b8fc918686f5b0af009abc.png)

Answer: 90

---

7. In bytes, what size limit did the attacker apply to the files they attempted to exfiltrate?

![](attachment/65f17f47f97036af851cefed01a89acc.png)

Answer: 2097152

---

8. What is the boundary for the data in the first HTTP packet sent to the remote server?

![](attachment/7ae7dfbf95f30805f7b34307469415bd.png)

Answer: 29c9b0d697280692

---

9. Using the Linux command line or another appropriate tool available on the desktop, rebuild the original file and identify what information the attacker was attempting to steal.

![](attachment/c1e8fac42017f8dd97537ac005751076.png)

---

10. Rebuild the file using the segments found in the PCAP. What is the MD5 hash of this file?

File > Export Packet Bytes.. > Save all the packet bytes

```
cat 1 2 3 4 5 6 7 8 9 10 > rebuilt_file
```

```
md5sum rebuilt_file
```


Answer: 8cbf90aeab2c93b2819fcfd6262b2cdb

---

11. What file type did the attacker attempt to exfiltrate?

Answer: pdf

---

12. What is the title of the document targeted in this attack?

Answer: **Final Flight Plan**

---





