
### 4.6.1 Performing DoS Attacks with an SYN Flood

In this lab, you will learn to perform DoS attacks with an SYN flood. The DoS (Denial of Service) attack is an attack meant for shutting down the network or a machine and making it inaccessible to its users. An SYN (synchronize) flood is an attack using which the attacker sends a succession of SYN requests to the target's system.  
To perform DoS attacks with an SYN flood, here's what you need to do:

1. From the desktop, open **VirtualBox**
2. In the **Oracle VM VirtualBox Manager** window, close **Can't enumerate USB device...** prompt and click on **Show**to open **windows7**.
3. In the **windows7 Running window, in the bottom left corner, click the **Start**  icon.
4. In the **Search programs and files** field, type _`command`_ and press **Enter**.
5. In the **Command Prompt** window, execute the following command and note the IP address, that is, **192.168.56.102**:
```
ipconfig
```

6. Minimize the **Command Prompt** window
7. On the **windows7 Running desktop, right-click the taskbar and select **Start Task Manager**.
8. Go to the **Performance** tab, and observe the graphs.
9. Minimize the **windows7 Running window and the **VirtualBox** windows.
10. From the **Kali** desktop, open **metasploit framework**
11. In the **Terminal** window, execute the following commands (**Note:** _Execute one command at a time._):

```
use auxiliary/dos/tcp/synflood
show options
set INTERFACE vboxnet0
set RHOST 192.168.56.102
exploit
```

- **`use auxiliary/dos/tcp/synflood`:** It is used to list all the options with the auxiliary.
- **`show options`:** It shows you the available parameters for an exploit if used when the command line is in an exploit context.
- **`set`:** It allows you to configure framework options and parameters for the current module you are working with.
- **`exploit`:** It executes a sequence of commands that target a specific vulnerability found in a system or application to provide the attacker with access to the system.

12. Minimize the **Terminal** window, and restore the **windows7 Running virtual machine.
- Observe the graphs on the **Performance** tabs, you will notice maximum **CPU usage**.
- Minimize the **windows7 Running** window.
- Restore the **Terminal** window, and press **Ctrl+C** (or **Command+C** on Mac) to stop the attack.
- Again, restore the **windows7 Running** window, and you will notice graphs of the **Performance** at normal values.

### 4.6.2 Performing a DHCP Starvation Attack

In this lab, you will learn to perform a DHCP starvation attack. DHCP (Dynamic Host Configuration Protocol) starvation is the process of attacking a DHCP server by sending a lot of requests to it. This process leads to the server's address pool exhausting after which the DHCP server is not able to respond to clients and give them new leases. To perform a DHCP starvation attack, here's what you need to do:

1. From the left sidebar, open **Virtual Machine Manager**
**VMM (Virtual Machine Manager)** is a software program that enables the creation, management, and governance of VMs (virtual machines) and manages the operation of a virtualized environment on top of a physical host machine.

2. In the **Virtual Machine Manager** window, open **metasploitable2-****linux**.
3. If the black screen appears on **metasploitable2-****linux**, then press **Enter**.
4. If required, in the **metasploitable2-****linux** window, login with the following credentials:

- **metasploitable login:** `_msfadmin_`
- **Password:** `_msfadmin_`

5. In the **metasploitable2-****linux** window, execute the following command to open and edit the **/etc/network/interfaces** file:

```
sudo nano /etc/network/interfaces
```

6. When asked for the password, type `_msfadmin_` and press **Enter**.
7. In the **File:** **/etc/network/interfaces** file, replace the line **iface eth0 inet static** with the following line:

```
iface eth0 inet dhcp
```

8. Press **Ctrl+X** (or **Command+X** on Mac), type _`Y`_, and press **Enter** to save the file.
9. In the **metasploitable2-****linux** window, execute the following commands and you will see that DHCP IP will be provided after the DORA process (**Note:** _Execute one command at a time._):

```
sudo /etc/init.d/networking restart
ifconfig
```

10. Minimize the **metasploitable2-****linux** window.
11. Minimize the **Virtual Machine Manager**.
12. On the left sidebar, click the **Terminal** icon to open the **Terminal** window.
13. In the **Terminal** window, execute the following commands (**Note:** _Execute one command at a time._):
```
ifconfig virbr0 192.168.1.10
ifconfig virbr0:1
ifconfig virbr0:1 192.168.1.11
echo 1 > /proc/sys/net/ipv4/ip_forward
route add default gw 192.168.1.1 virbr0:1
route -n
```

14. Minimize the **Terminal** window.
15. From the left sidebar, click the **metasploit framework**  icon to open the **msf** console **T****erminal** window.

16. In the **msf** console **Terminal** window, execute the following commands (**Note:** _Execute one command at a time._):
```
touch /root/msf.txt
spool /root/msf.txt
use auxiliary/server/dhcp
set DHCPIPEND 192.168.1.176
set DHCPIPSTART 192.168.1.170
set dnsserver 8.8.8.8
set netmask 255.255.255.0
set router 192.168.1.11
show options
```

17. Minimize the **msf** console **Terminal** window.
18. On the left sidebar, right-click the **Terminal** icon, select **All Windows**, and open the previous **Terminal** window.
19. In the **Terminal** window, execute the following command to start the DHCP starvation attack:

```
pig.py virbr0:1
```

20. Minimize the **Terminal** window.
21. On the left sidebar, right-click the **Terminal** icon, select **All Windows**, and open the **msf** console **Terminal** window.
22. In the **msf** console **Terminal** window, execute the following commands (**Note:** _Execute one command at a time._):

```
set SRVHOST 192.168.1.1
run
```

23. Minimize the **msf** console **Terminal** window.
24. Resume the **metasploitable2-****linux** window and execute the following commands (**Note:** _Execute one command at a time._):

```
sudo /etc/init.d/networking restart
ifconfig
```

Now, you will see that no IP will be assigned through DHCP.

25. Observe that no IP address will be assigned.
26. Close all windows.
27. Click the **Check** button to check if the DHCP server started successfully.

### 4.6.3 Simulating a DDoS Attack with a SYN Flood

In this lab, you will learn to simulate the DDoS attack with an SYN flood. The DDoS (Distributed Denial of Service) attack is an attempt to make an online service or a website unavailable by overloading it with huge floods of traffic generated from multiple sources To simulate the DDoS attack with an SYN (synchronize) flood, here's what you need to do:

1. From the left sidebar, open **Virtual Machine Manager**
**Virtual Machine Manager (VMM)** is a software program that enables the creation, management, and governance of VMs (virtual machines) and manages the operation of a virtualized environment on top of a physical host machine.

2. In the **Virtual Machine Manager** window, select **metasploitable-2-linux**, and from the **Shut down the virtual machine** drop-down, select **Force Off**.
3. Click **win7**, and at the top, click **Open** to open the **win7** virtual machine.
4. In the **win7 on QEMU/KVM** window, on the taskbar, click the **Start** button.
5. In the **Search program and fiiles** field, type `_command_` and press **Enter**.
6. In the **Administrator: Command Prompt** window, execute the following command and note the IP address, that is, **192.168.122.7**:

```
ipconfig
```

7. Minimize the **Administrator: Command Prompt** window.
8. On the **win7** desktop, right-click the taskbar and select **Start Task Manager**.
9. Observe the graphs on the **Performance** and **Networking** tabs.
10. Now, minimize the **win7 on QEMU/KVM** and **Virtual Machine Manager** windows.
11. On the **Kali** desktop, from the left sidebar, open **metasploit framework**
12. Execute the following commands (**Note:** _Execute one command at a time._):

```
use auxiliary/dos/tcp/synflood
show options
set RPORT 80
set RHOST 192.168.122.7
run
```


- **`auxiliary/dos/tcp/synflood`:** It loads up synflood auxiliary to metasploit.
- **`show options`:** It shows all options required to perform the attack.
- `**set RPORT 80**`**:** It sets the port to 80.
- `**set RHOST 192.168.122.7**`**:** It sets the IP of the target for syn flood auxiliary.

13. Now, restore the **win7** virtual machine and observe graphs on the **Performance** and **Networking** tabs, you can see maximum **CPU usage** and a huge amount of lagging
14. Minimize the **win7 on QEMU/KVM** and **Virtual Machine Manager** windows.
15. In the **Kali** terminal window, press **Ctrl+C** (or **Command+C** on Mac) to stop the attacks.
16. Again, restore the **win7** machine. You will see graphs of the **Performance** and **Networking** tabs at normal values.
17. To verify SYN packets, you need to capture them using Wireshark.
18. From the menu bar, navigate to **Applications** > **09 - Sniffing & Spoofing** and click **wireshark**.
19. Click the **Start capturing packets**  icon to start packet capturing on the **virbr0** interface.
20. Now restore the **metasploit** terminal and execute the following command: `run`
21. Stop the attack after a few seconds by pressing the **Ctrl+C** (or **Command+C** on Mac) key.
22. Restore the **wireshark** window, and click the **Stop capturing packets** icon to stop the packet capturing.
23. From the menu, navigate to **File** > **Save**.
24. In the **Wireshark . Save Capture File as** window, verify that the location is selected as **root**, type **File name** as `_ddos_`, and from the **Save as** drop-down, select **K12 text file**.


### 4.8.1 Protecting Yourself from the DOS Attack

In this lab, you will learn to implement a packet filter. A packet‐filtering firewall inspects the data packets as they attempt to traverse the firewall, and based on the rules that have been defined on the firewall, the firewall allows or denies each packet.  
To implement a packet filter, here's what you need to do:
1. From the bottom-left bar, for the **Base VM** and **Ubuntu** tabs, click On/Connect to power on both machines.
2. Navigate to the **Base VM** tab, on the taskbar, click the **Search Windows** icon, and type `_firewall_` to open **Windows Firewall**.
3. In the left pane, click **Advanced settings**.
4. In the **Windows Firewall with Advanced Security**, in the left pane, click **Inbound Rules** and then in the right pane, click **New Rule**.
5. - In the **New Inbound Rule Wizard** window, perform the following steps:

1. At the **Rule Type** step, select **Custom**, then click **Next**.
2. At the **Program** step, verify that **All programs** is selected, and then click **Next**.
3. At the **Protocol and Ports** step, from the **Protocol type** list, select **ICMPv4**, and then click **Next**.
4. At the **Scope** step, verify **Any IP address** is selected for both, and then click **Next**.
5. At the **Action** step, select **Block the connection**, and then click **Next**.
6. At the **Profile** step, verify all the profiles are selected, and then click **Next**.
7. At the **Name** step, in the **Name** textbox, type `_Deny_Ping_`, and then click **Finish**.

13. Close all windows.
14. Navigate to the **Ubuntu** tab, on the desktop, at the top, click the **Terminal  Emulator**  icon.
15. Execute the following command to verify whether the connection is blocked or not:

```
ping 192.168.1.245
```

16. Close all windows

