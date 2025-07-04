# Nmap Network Scan and Analysis

## Objective

Learn how to discover open ports on devices in your local network using Nmap to understand network exposure and identify potential vulnerabilities.

---

## 1. TCP SYN Scan with Nmap ( I DOWNLOADED IT FROM THE OFFICIAL WEBSITE ) 

### Command:

```bash
nmap -sS <IP Range>
nmap -A 192.168.1.4  ## (May be have use this more aggressive scan)  
```
(-A = aggressive scan: does port scanning + service detection + OS detection + scripting) , I find this more helpful beacuse it even gave me my laptop uptime(impressive) .

### What it does( LEARNED FROM GOOGLE ABOUT 3 WAYS HANDSHAKE AND TCP PROTOCOL):

- Sends a TCP SYN packet to each port (just like starting a connection).
- If the port is **open**, the target replies with **SYN-ACK**.
- If the port is **closed**, the target replies with **RST**.
- If the port is **filtered**, there's **no response**.

This is called a **half-open scan** and is stealthier than a full connection scan.

---

## 2. Understanding Local IP Range ( ADDED A SCREENSHOT OF MY LAPTOP RESULTS )

A **local IP range** refers to the group of IP addresses used within your home  OR PUBLIC network (Wi-Fi or LAN).

### How to find it:

Run `ipconfig` in your Command Prompt. This gives information about your:

- Local IP address
- Subnet mask

### Example:

```
IPv4 Address: 192.168.0.102
Subnet Mask: 255.255.255.0
```

### Interpreting the Range:

- Based on `192.168.0.1` and subnet mask `255.255.255.0`, the range is:

```
192.168.0.0/24  =>  IPs from 192.168.0.1 to 192.168.0.255
```

### Binary Representation ( HELPED GET THE INTUTION WHAT IS EXACTLY IS 255 IT IS JUST BINARY REPRESENATION ):

```
IP Address:    11000000.10101000.00000001.00000100
Subnet Mask:   11111111.11111111.11111111.00000000
```

## 4. Analysis of Nmap Scan Results from the Nmap app

### Scan Summary:

- Scanned: 256 IP addresses (`192.168.1.0/24`)
- Hosts found: 3 active devices

---

### Host 1: 192.168.1.1 

```
Nmap scan report for Unit (192.168.1.1)
Host is up (0.024s latency).
Not shown: 988 filtered tcp ports (no-response), 2 filtered tcp ports (port-unreach)
PORT      STATE  SERVICE
22/tcp    closed ssh
443/tcp   open   https
445/tcp   closed microsoft-ds
631/tcp   closed ipp
49152/tcp closed unknown
MAC Address: 24:DE:8A:6C:BC:E1 (Nokia Solutions and Networks)
```

#### Open Port:

- `443/tcp (https)`: Secure web-based management for router settings

#### Closed Ports:

- `22 (SSH)`, `445 (Microsoft-DS)`: Accessible but no service listening

#### Filtered Ports:

- 990 ports blocked by firewall or router

---

### Host 2: 192.168.1.3

```
Nmap scan report for 192.168.1.3
Host is up (0.051s latency).
All 1000 scanned ports are in ignored states.
Not shown: 1000 closed tcp ports (reset)
MAC Address: 62:96:AF:62:99:B1 (Unknown)
```
### Host 3: 192.168.1.4( THIS IS MY LAPTOP )

```
Nmap scan report for 192.168.1.4
Host is up (0.0010s latency).
Not shown: 995 closed tcp ports (reset)
PORT    STATE SERVICE
135/tcp open  msrpc
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds
902/tcp open  iss-realsecure
912/tcp open  apex-mesh
```

#### Open Ports and Services:

- `135/tcp (msrpc)`: Remote Procedure Calls used for Windows services
- `139/tcp (netbios-ssn)`: NetBIOS Session Service for file sharing (legacy)
- `445/tcp (microsoft-ds)`: SMB protocol for file and printer sharing
- `902/tcp (iss-realsecure)` and `912/tcp (apex-mesh)`: Likely VMware services for VM management

---

## 5. Security Risks Identified

### SMB Exposure (Port 445):

- Can be exploited by worms/ransomware 
- Best practices: strong passwords, updates, firewall rules

### RPC Service (Port 135):

- Can be vulnerable if unpatched

### VMware Ports (902, 912):

- Allow remote management of VMs
- If not secured, can provide attackers access to your virtualization infrastructure

---
### AS I AM IN MY HOME NETWORK IT IS SAFER THAN ON PUBLIC NETWORK 

