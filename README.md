# 🔐 Cybersecurity Lab Environment Setup

### **Building an isolated virtual lab for penetration testing and ethical hacking practice**

![Cybersecurity](https://shields.io) ![Virtualbox](https://shields.io) ![Kali Linux](https://shields.io) ![Linux](https://shields.io) ![Network](https://shields.io) ![Penetration Testing](https://shields.io)
![Virtualization](https://shields.io) ![GitHub](https://shields.io) ![Kali Linux](https://shields.io) ![NetworkWalks](https://shields.io) ![Ethical Hacking](https://shields.io) ![Waqas Karim CCIE](https://shields.io)

---

## 🔗 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

## 🎯 Objectives

The main objectives of this project are to:
* Install and configure VirtualBox.
* Install/import Kali Linux as a virtual machine.
* Create a private NAT Network for the cybersecurity lab.
* Configure network connectivity for Kali Linux.
* Assign a consistent IP address to the Kali VM.
* Verify network connectivity and DNS resolution.
* Take a clean VM snapshot for recovery.
* Document the complete setup process.
* Prepare the environment for future cybersecurity projects.

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing. It can be used for activities such as:
* Network reconnaissance
* Port scanning
* Vulnerability assessment
* Packet analysis
* Web security testing
* Exploitation practice
* Security-tool experimentation

⚠️ **Important:** *This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.*

## 📐 Lab Architecture 
<img width="1346" height="616" alt="Lab network topology map including host OS, virtual networks, and guest nodes" src="https://github.com/user-attachments/assets/121d8cd6-6ae5-4154-8c5a-72e8e4320119" />

## ⚙️ Lab Configuration

| Component | Value / Specification |
| :--- | :--- |
| **🖥️ Host OS** | Windows 10 |
| **🧠 Host RAM** | 8 GB |
| **⚡ Processor** | Intel Core i5 |
| **🧰 Hypervisor** | VirtualBox 7.2 |
| **🐉 Security OS** | Kali Linux 2026.2 |
| **🧠 Kali RAM** | 4096 MB |
| **🌐 Virtual Network** | NAT Network |
| **📡 Network Address** | 10.0.0.0/24 |
| **🐧 Kali IP Address** | 10.0.0.2/24 |
| **🚪 Default Gateway** | 10.0.0.1 |
| **🌍 DNS Server** | 8.8.8.8 |

---

## 🛠️ Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip was installed to extract the Kali Linux virtual-machine package, which may be distributed as a `.7z` archive.

### Step 2. Install VirtualBox
VirtualBox was installed on the host system to function as the primary Type-2 hypervisor.

### Step 3. Create the NAT Network
A dedicated NAT Network was created in VirtualBox to maintain an internal zone while allowing external traffic routing.

* **Configuration:** 
 * Network Name: `NatNetwork`
 * IPv4 Prefix: `10.0.0.0/24`
 * DHCP: Enabled
 * IPv6: Disabled

<img width="761" height="720" alt="VirtualBox NAT network creation parameters window screenshot" src="https://github.com/user-attachments/assets/6d80c472-e241-4285-8531-9353ff0bce3b" />

A NAT Network was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity. This will allow future attacker and target VMs to communicate within the lab environment.

### Step 4. Import Kali Linux
The Kali Linux virtual machine was downloaded from the official website and imported into VirtualBox.

The VM network adapter was configured as follows:
* **Adapter 1:**
 * Attached to: `NAT Network`
 * Network: `NatNetwork`
 * Adapter Type: `Intel PRO/1000 MT Desktop`

The VM resource allocation:
* **RAM:** 4096 MB

<img width="1366" height="749" alt="VirtualBox manager instance system settings display" src="https://github.com/user-attachments/assets/15774aa5-61b1-487f-8c9a-b2e67ea27fbd" />

A permanent, auto-mounted shared folder was also configured under the `vboxsf` mount group to streamline transferring tools and operational source files between the host operating system and the guest Kali VM.

### Step 5. Configure the Kali Linux Network
The Kali Linux interface configuration settings were modified to enforce a consistent static IPv4 footprint:
* **IP Address:** `10.0.0.2`
* **Subnet Mask:** `255.255.255.0`
* **Gateway:** `10.0.0.1`
* **DNS:** `8.8.8.8`

A consistent IP profile reduces administrative friction across subsequent projects and scanning exercises.

<img width="1364" height="768" alt="Kali Linux desktop interface showing terminal network configurations" src="https://github.com/user-attachments/assets/f235bae8-635b-4aac-b6d6-08a8b848af0a" />

### Step 6. Create a Clean VM Snapshot
After completing the initial network confirmation steps, a system baseline snapshot was taken:
* **Snapshot Name:** `Clean Kali - Network Setup`

The snapshot represents the clean baseline of the laboratory. If a future security tool testing exercise changes or corrupts the configuration state, the machine can be instantaneously restored back to this point.

---

## 🚦 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
| :--- | :--- | :--- |
| **🌐 Check IP address** | `ip a` | Correct Kali IP address (`10.0.0.2`) displayed |
| **📡 Test gateway** | `ping -c 4 10.0.0.1` | Successful ICMP echoes/replies from default gateway |
| **🌍 Test Internet connectivity** | `ping -c 4 8.8.8.8` | Successful outbound ICMP echoes/replies |
| **🔎 Test DNS resolution** | `nslookup networkwalks.com` | Target domain name successfully maps to valid IP |
| **🧰 Verify Nmap** | `nmap --version` | Nmap engine builds and version stats display output |
| **🔄 Verify snapshot** | Restore snapshot and run `ip a` | Baseline clean configuration safely verified and restored |

### Example Diagnostic Outputs
* **IP Address:** `10.0.0.2/24`
* **Gateway:** `10.0.0.1`
* **DNS:** `8.8.8.8`

---

## 🪲 Problems Encountered & Solutions

### Problem 1. VirtualBox Guest Additions & Package Manager Lock Error
When attempting to install the VirtualBox Guest Additions to enable shared folders, the installation may fail due to package management typos or interrupted background processes locking the database.

One workaround used during this lab was:
```bash
sudo dpkg --configure -a && sudo apt update && sudo apt install -y virtualbox-guest-x11
```
The system was then rebooted to initialize the kernel drivers and finalize the shared folder mount points.

> ⚠️ **Important:** If the terminal returns a `"Resource temporarily unavailable"` lock error, users must ensure no other instance of `apt` or `apt-get` is running in the background before repairing the configuration database.

### Problem 2. Internet Connectivity After Static IP Configuration
After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround used during this lab was:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```
The network connection was then restarted/rebooted and connectivity was tested again.

> ⚠️ **Important:** Network interface and connection names may differ between systems. Students should first identify their actual connection name before running an nmcli command.

---

## 🧠 What I Learned

This project provided hands-on experience with virtualization, Linux network management, and system troubleshooting. The key takeaways include:

### 1. NAT vs NAT Network
A standard NAT configuration and a NAT Network serve different purposes. A NAT Network allows multiple VMs connected to the same virtual network to communicate with one another while providing network address translation for external connectivity. This makes it useful for building a multi-machine cybersecurity laboratory.

### 2. Virtual Machine Networking
I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.

### 3. Static IP Configuration
I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux. This included utilizing utilities like `nmcli` to resolve network stability and duplicate address detection (`dad-timeout`) issues.

### 4. VM Snapshots
I learned that a clean snapshot should be created before performing risky or experimental activities. This provides a known-good recovery point for future cybersecurity exercises.

### 5. Shared Folder & Driver Integration
Understood the process of fixing broken package manager states, resolving database locks (`dpkg`), and loading guest addition kernel modules (`virtualbox-guest-x11`) to securely transfer files between the host system and the guest VM.

### 6. Documentation

I learned that documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

## Security & Ethical Use

This laboratory is intended strictly for education purposes only.

## Tools & Resources

 7-Zip: https://7-zip.org/download.html
 
 VirtualBox: https://virtualbox.org/wiki/Downloads
 
 Kali Linux: https://kali.org/get-kali
 
## Author

maloni zulu
Linkedin: https://www.linkedin.com/in/maloni-zulu-407b7b446/

Cybersecurity Professional B084

## Project Information

Program Name: Cybersecurity at Networkwalks | Week: 01 | Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub
