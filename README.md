# Cisco-Device-Functions-Lab
This lab explores the MAC address table on Cisco IOS switches and Routing tables on Cisco IOS routers
--

### 🚀 Open the packet tracer file to load the lab
This preconfigures each router with an IP address in the 10.10.10.0/24 network

---

### Lab Topology
![Lab Topology]()

### Verify the Switch MAC Address Table
1. Log into routers R1 to R4 and verify which interface is configures on the 10.10.10.0/24 network.

The 'show ip interface brief' command in priviledged exec mode (enable) shows which IP addresses are configured 
on which interfaces, and the status of the interfaces.

### On all routers:
![Show IP Interface Brief On All Routers]()

For the 10.10.10.0/24 network, R1, R2, and R4 are using GigabitEthernet 0/0 were as R3 is using GigabitEthernet0/1

---

2. Note down the MAC Addresses of these interfaces which we will use to check on the switches later on in this lab. Why cause we want to understand how packets travel (Life of a packet) and to see the MAC address table of the switches update.

How we are going to achieve this is by using the command in the priviledged exec mode 'show interface gig_/_' as shown in the image below.

### MAC Addresses
![MAC-Addresses]()

R1 address is 0090.2682.ab01
R2 address is 0060.2Fb3.9152
R3 address is 0001.9626.8970
R4 address is 00d0.9701.02a9

**Note: the MAC addresses in your lab may be different.**

---

3. Verify connectivity between the routers by pinging R2, R3, and R4 from R1

'Ping' sends test packets to the destination and waits for a response.
Success indicates the test packet reached the destination and the response to that packet reached this router,
verifying connectivity in both directions. It is not a concern if the first packet fails while the connection is set up, as shown in the output below.

### Verifying Connectivity Ping R2, R3 and R4 from R1
![Verifying-Connectivity1]()

---

### Verifying Connectivity Ping R3 and R4 from R2
![Verifying-Connectivity2]()

---

### Verifying Connectivity Ping R1, R2 and R4 from R3
![Verifying-Connectivity3]()

---

### Verifying Connectivity Ping R1 and R2 from R4
![Verifying-Connectivity4]()

---

4. Remember in section 2 of this lab were I said we will need to noted down the MAC address that will be used in the switches to see how packets travel, well we are going to view the dynamically learned MAC addresses on SW1 and verify that the router's MAC addresses are reachable via the expected ports. 

How we are going to achieve that is using the command 'show mac address-table dynamic' in priviledged exec mode, as shown below. (Ignore any other mac addresses in the table).

### SW1 MAC Address Table
![SW1-MAC-Address-Table]()

---

5. Repeat on SW2

### SW2 MAC Address Table
![SW2-MAC-Address-Table]()

---

6. Clear the dynamic MAC Address Table on SW1. Using 'clear mac address-table dynamic' in priviledged exec mode.

### CLear The Dynamic MAC Address Table
SW1#clear mac address-table dynamic

---

7. Show the dynamic MAC Address Table on SW1. Do you see any MAC addresses? Why or why not?

### SW1 Show Dynamic MAC Address Table
![SW1-Show-Dynamic-MAC-Address-Table]()

Devices in a real world network tend to be chaaty and send traffic frequently, this causes the MAC address table to update (you may see less entries in PAcket Tracer).

The switch will periodically flush old entries.

---

### Examine a Routing Table

8. View the routing table on R1. What routes are present and why?

Well to achieve this action we will use 'show ip route'

### Routing Table
![Routing-Table]()

The router has a connected route for the 10.10.10.0/24 network and a local route for 10.10.10.1/24
These routes were automatically created when the IP address 10.10.10.1/24 was enabled on interface GigabitEthernet0/0

---

9. Configure IP address 10.10.20.1/24 on interface GigabitEthernet0/1

To achieve this we will enter the Global configuration mode then Interface configuration mode to configure the interface GigabitEthernet0/1.

Use:
'interface gig0/1' in the Global configuration mode (config t)
'ip address 10.10.20.1 255.255.255.0' in Interface configuration mode (config-if)

### Configure IP Address
![Configure-IP-Address-10-10-20-1]()

---

10. Verify the status of interfaces GigabitEthernet0/0 and GigabitEthernet0/1.

Pro tip: A router interface status is 'administrtatively down' by default. For a router interface to be used it must be brought online. Remember this or toubleshooting in later labs.

'show' commands are entered in Priviledged Exec mode (Enable)

### Verify The Configured IP Address
![Configure-IP-Address]()

G0/0 was already brought online by an administrator but G0/1 is still shutdown.

---

11. Bring interface GigabitEthernet0/1 online.

To negate a command enter the 'no' form of the command.
Router interfaces are administratively 'shutdown' by default, so enter the 'no shutdown' command to bring them online. 
This is typically done immediately after configuring an IP address on an interface. 
Best practice is to leave interfaces shutdown until they are brought into use.

### Bring Interface Online
![Interface-GigabitEthernet01]()

(Switch interfaces are already online by default. There is no need to enter the 'no shutdown' command on switches unless an administrator has previously explicitly entered the 'shutdown' command on an interface.)

---

12. Verify the status iof interfaces GigabitEthernet0/0 and GigabitEthernet0/1 now.

### Verify Interface Online
![Bring-Interface-Online]()

---

13. Now we check What routes are in the routing table.

Command to use 'show ip route'

### Routing Table
![Routing-Table]()

The router has routes for both interfaces and can route traffic between hosts on the 10.10.10.0/24 and 10.10.20.0/24 networks.

---

14. Configure a static route to 10.10.20.0/24 with a next hop addres of 10.10.10.2 in config t 'ip route _ip_ _subnet mask_ _next hop_'

### Configured A Static Route
![Configured-A-Static-Route0]()

---

15. Now we check the routes that are in the routing table.

'show' commands can be entered outside Priviledged Exec mode by typing 'do' before the command.



### Configured A Static Route
![Configured-A-Static-Route]()

---

### Configured A Static Route
![Configured-A-Static-Route2]()

The router has routes to its locally connected networks, and also to 10.10.30.0/24 which is available via 10.10.10.2

