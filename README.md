# 🔐 Cybersecurity Lab Environment Setup

**Building an isolated virtual lab for penetration testing and ethical hacking practice**

![Skill](https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000)
![VirtualBox](https://img.shields.io/badge/Ver-Virtualbox%207.x-0070C0?style=flat-square&labelColor=000000)
![Kali](https://img.shields.io/badge/Kali%20Linux-Latest-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000)
![Pentest](https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white)
![NetworkWalks](https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000)

---

## 📌 Project Overview

This project focuses on setting up a **virtual cybersecurity and penetration-testing laboratory** using VirtualBox and Kali Linux.

The purpose of the lab is to create a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

The lab is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

---

## 🎯 Objectives

- Install and configure VirtualBox.
- Install/import Kali Linux as a virtual machine.
- Create a private **NAT Network** for the cybersecurity lab.
- Configure network connectivity for Kali Linux.
- Assign a consistent IP address to the Kali VM.
- Verify network connectivity and DNS resolution.
- Take a clean VM snapshot for recovery.
- Document the complete setup process.
- Prepare the environment for future cybersecurity projects.

---

## 🛡️ Purpose of the Lab

The lab provides an isolated and controlled environment for cybersecurity learning and authorized security testing.

It can be used for activities such as:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

⚠️ **Important:** This laboratory must only be used for systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

---

---

## ⚙️ Lab Configuration

| 🧩 Component | ⚙️ Configuration |
|---|---|
| 🖥️ Host OS | Windows |
| 🧰 Hypervisor | Oracle VirtualBox |
| 🐉 Security OS | Kali Linux |
| 🧠 Kali RAM | 2048 MB |
| 🌐 Virtual Network | NAT Network (NatNetwork) |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |
| 🔮 Future VM Range | 10.0.0.3 – 10.0.0.99 |

---

# 🪜 Lab Setup Procedure

## Step 1. Install 7-Zip

7-Zip was installed to extract the Kali Linux virtual-machine package, which is distributed as a `.7z` archive.

**Tool:** 7-Zip

---

## Step 2. Install VirtualBox

Oracle VirtualBox was installed as the hypervisor.

---

## Step 3. Create the NAT Network

A dedicated NAT Network was created in VirtualBox (Tools → Network → NAT Networks → Create).

Configuration:

```
Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled
```

![VirtualBox NAT Network settings](1-screenshot-network-settings-1.png)

A **NAT Network** was selected because multiple virtual machines connected to the same NAT Network can communicate with one another while also having outbound network connectivity.

This will allow future attacker and target VMs to communicate within the lab.

---

## Step 4. Import Kali Linux

The Kali Linux virtual machine was downloaded from the official Kali Linux website, extracted with 7-Zip, and imported into VirtualBox.

The VM network adapter was configured as follows:

```
Adapter 1
Attached to:  NAT Network
Network:      NatNetwork
```

The VM was allocated:

```
RAM: 2048 MB
```

![Kali Linux desktop](2-screenshot-kali-linux.png)

---

## Step 5. Configure the Kali Linux Network

In Kali Linux, the wired connection was edited (Network icon → Edit Connections → Wired connection 1 → IPv4 Settings) and set to **Manual** with a consistent IPv4 address:

```
Method:      Manual
IP Address:  10.0.0.2
Netmask:     24 (255.255.255.0)
Gateway:     10.0.0.1
DNS:         8.8.8.8
```

A consistent IP address makes it easier to document the lab and reference the Kali machine in future exercises.

![Kali network settings](3-screenshot-kali-network-settings.png)

---

## Step 6. Create a Clean VM Snapshot

After completing the initial configuration, a VirtualBox snapshot was created (Machine → Take Snapshot).

Snapshot name:

```
Clean Kali - Network Setup
```

The snapshot represents the clean baseline of the laboratory. If a future exercise changes or damages the VM configuration, the machine can be restored to this baseline.

---

# 🔎 Lab Verification

| ✅ Test | 🧾 Command | 🎯 Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` | Correct Kali IP displayed |
| 📡 Test gateway | `ping -c 4 10.0.0.1` | Successful replies |
| 🌍 Test Internet connectivity | `ping -c 4 8.8.8.8` | Successful replies |
| 🔎 Test DNS resolution | `nslookup google.com` | Domain resolves |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot and run `ip a` | Baseline configuration restored |

### Example Results

```
IP Address: 10.0.0.2/24
Gateway:    10.0.0.1
DNS:        8.8.8.8
```

---

# 🐞 Problems Encountered & Solutions

## Problem 1. Internet Connectivity After Static IP Configuration

After manually configuring the IPv4 settings, Internet connectivity may fail depending on the Kali/NetworkManager configuration.

One workaround is:

```
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

The network connection was then restarted/rebooted and connectivity was tested again.

> **Note:** Connection names may differ between systems. Check yours first with `nmcli connection show`.

---

## Problem 2. VirtualBox VT-x / Virtualization Error

The VM can fail to start if hardware virtualization is disabled in the BIOS/UEFI. This is resolved by:

1. Restarting the computer.
2. Entering BIOS/UEFI settings.
3. Enabling Intel VT-x / AMD-V (hardware virtualization).
4. Saving the configuration and restarting.
5. Starting the Kali VM again.

---

# 💡 What I Learned

### 1. NAT vs NAT Network
A NAT Network lets multiple VMs on the same virtual network communicate with one another while still having outbound Internet access, which makes it ideal for a multi-machine cybersecurity lab.

### 2. Virtual Machine Networking
I learned how VirtualBox virtual network adapters connect virtual machines to different types of networks and how network configuration affects communication between machines.

### 3. Static IP Configuration
I learned how to configure and verify IPv4 addressing, subnet masks, gateways, and DNS settings in Kali Linux.

### 4. VM Snapshots
A clean snapshot should be created **before** performing risky or experimental activities. It provides a known-good recovery point for future exercises.

### 5. Documentation
Documenting commands, configuration, screenshots, problems, and solutions is an important part of a professional cybersecurity project.

---

# 🔐 Security & Ethical Use

This laboratory is intended strictly for educational purposes only. Only test systems that you own or have explicit written permission to test.

---

# 🔗 Tools & Resources

- **7-Zip:** https://7-zip.org/download.html
- **VirtualBox:** https://virtualbox.org/wiki/Downloads
- **Kali Linux:** https://kali.org/get-kali

---

# 👤 Author

Shahd Wael
Cybersecurity Student B083


---

## 📌 Project Information

**Program Name:** Cybersecurity at Networkwalks | **Week:** 01 | **Project:** Cybersecurity & Pentesting Lab Setup | **Repository:** GitHub
