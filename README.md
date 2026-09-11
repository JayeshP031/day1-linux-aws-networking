# Day 1 — Linux Networking & AWS EC2 Practical Tasks

**Organization:** Fortune Cloud Technologies  
**Topic:** Linux Networking, IPv4 Analysis, Dynamic IP Investigation, AWS EC2 and Network Troubleshooting  
**Environment:** Amazon Linux 2023 / AWS EC2  
**Date:** 11 September 2026

## 📌 Overview

This repository contains my Day 1 practical work for Linux and Cloud networking.

The tasks focus on:

1. Linux IP investigation
2. IPv4 address analysis
3. Dynamic IP investigation
4. AWS EC2 Linux server and IP configuration
5. Cloud network troubleshooting

The practical work was performed using Linux networking commands and an AWS EC2 Linux server.

> **Security note:** Public repositories should not contain private keys, passwords, access tokens, AWS credentials, or unnecessary public IP/account information. Any screenshots published publicly should be reviewed and redacted where appropriate.

---

# 🧪 Task 1 — Linux IP Investigation

## Objective

Identify the Linux system's:

- IPv4 address
- Network interface
- Default gateway
- DNS information
- Complete network configuration

## Commands Used

```bash
ip a
```

```bash
ip link
```

```bash
ip route
```

```bash
cat /etc/resolv.conf
```

```bash
hostname -I
```

## Observed Configuration

| Item | Result |
|---|---|
| IPv4 Address | `172.31.16.110` |
| Network Interface | `ens5` |
| Network | `172.31.16.0/20` |
| Default Gateway | `172.31.16.1` |
| DNS Server | `172.31.0.2` |

### Key Observation

The active network interface is `ens5`, and the Linux server has a private IPv4 address in the `172.31.x.x` range.

---

# 🌐 Task 2 — IPv4 Address Analysis

## IPv4 Address

```text
172.31.16.110
```

## Four Octets

| Octet | Value |
|---|---:|
| 1 | 172 |
| 2 | 31 |
| 3 | 16 |
| 4 | 110 |

## Private or Public?

The address is a **private IPv4 address**.

The address belongs to the private `172.16.0.0 – 172.31.255.255` range.

## Connectivity Testing

The following commands were used:

```bash
ping -c 4 8.8.8.8
```

```bash
ping -c 4 google.com
```

Both tests returned successful replies in the submitted terminal evidence.

### Key Observation

The server's private IPv4 address is different from its public IPv4 address. The private address is used within the AWS VPC, while the public address is used for internet-facing access.

---

# 🔄 Task 3 — Dynamic IP Investigation

## Objective

Investigate dynamic IP behavior by recording the address before and after a safe network disconnect/reconnect.

## Evidence Collected

The Linux interface output shows that the IPv4 address on `ens5` is marked as:

```text
dynamic
```

Current observed private IP:

```text
172.31.16.110
```

## Required Commands

```bash
hostname -I
```

```bash
nmcli general status
```

```bash
nmcli device status
```

After a safe disconnect/reconnect:

```bash
hostname -I
```

## Evidence Status

The submitted evidence shows a dynamically assigned address, but the final report still requires a complete:

```text
Before disconnect → IP Address
After reconnect   → IP Address
```

record.

This should be completed before treating Task 3 as fully evidenced.

---

# ☁️ Task 4 — AWS EC2 Linux Server & IP

## Objective

Create/access an AWS EC2 Linux server, connect using SSH, identify private/public IP information, and compare AWS Console information with Linux.

## SSH

Example command:

```bash
ssh -i linux.pem ec2-user@<PUBLIC-IP>
```

## Commands Used Inside EC2

```bash
hostname -I
```

```bash
ip -4 addr
```

```bash
ip route
```

```bash
curl -4 ifconfig.me
```

## Observed IP Information

The submitted AWS Console and Linux terminal evidence show:

| Source | Private IPv4 | Public IPv4 |
|---|---|---|
| AWS Console | `172.31.16.110` | `<REDACTED_PUBLIC_IP>` |
| Linux | `172.31.16.110` | `<REDACTED_PUBLIC_IP>` |

### Key Observation

The private IPv4 address displayed in the AWS Console matches the private IPv4 address reported from inside Linux.

The public IPv4 address is used to connect to the EC2 instance from outside the private VPC.

---

# 🔧 Task 5 — Cloud Network Troubleshooting

## Problem

A cloud Linux server needs systematic network/service troubleshooting to determine why a service cannot be accessed.

## Troubleshooting Sequence

### 1. Check IP Address

```bash
ip -4 addr
```

Result:

```text
ens5 is UP
Private IPv4: 172.31.16.110
```

### 2. Check Network Interface

```bash
ip link
```

Result:

```text
ens5 → UP
```

### 3. Check Default Route

```bash
ip route
```

Observed default route:

```text
default via 172.31.16.1 dev ens5
```

### 4. Check Network Connectivity

```bash
ping -c 4 8.8.8.8
```

The submitted evidence shows:

```text
4 packets transmitted
4 received
0% packet loss
```

DNS/connectivity was also tested:

```bash
ping -c 4 google.com
```

### 5. Check Nginx Service

```bash
sudo systemctl status nginx
```

Observed result:

```text
active (running)
```

### 6. Check Listening Ports

```bash
sudo ss -tulpn
```

The submitted evidence shows Nginx listening on TCP port:

```text
80
```

### 7. Test the Service Locally

```bash
curl http://localhost
```

The command returned the Nginx welcome page.

## Troubleshooting Conclusion

The collected evidence confirms:

```text
IP Address       → Available
Network Interface → UP
Default Route    → Present
Internet Access  → Working
Nginx            → Running
Port 80          → Listening
Local HTTP       → Working
```

The submitted AWS security-group screenshot already shows TCP port 80 allowed from `0.0.0.0/0`.

Therefore, the current evidence does **not** support claiming that a Security Group rule was the original cause of failure or that changing the rule was the fix.

A complete troubleshooting demonstration should capture a genuine failed external access test, identify its actual cause, apply the fix, and capture the successful final result.

---

# 📝 Commands Reference

## Network Information

```bash
ip a
```

```bash
ip -4 addr
```

```bash
ip link
```

```bash
hostname -I
```

## Routing

```bash
ip route
```

```bash
ip -4 route
```

## DNS

```bash
cat /etc/resolv.conf
```

## Connectivity

```bash
ping -c 4 8.8.8.8
```

```bash
ping -c 4 google.com
```

## Public IP

```bash
curl -4 ifconfig.me
```

## Services

```bash
sudo systemctl status nginx
```

```bash
sudo systemctl is-active nginx
```

## Listening Ports

```bash
sudo ss -tulpn
```

## Local Web Test

```bash
curl http://localhost
```

---

# 📸 Evidence

The repository includes the consolidated practical report:

```text
Day1_Tasks_Submission_Jayesh_Patil.pdf
```

The PDF contains selected screenshots of the actual terminal and AWS Console work, organized by task.

---

# 🎯 Learning Outcomes

Through this practical I worked with:

- Linux IP addressing
- IPv4 octets
- Private vs public IP addresses
- Linux network interfaces
- Routing tables
- Default gateways
- DNS configuration
- Network connectivity testing
- Dynamic IP concepts
- AWS EC2
- SSH
- AWS private/public IP addressing
- Nginx service management
- TCP listening ports
- AWS Security Groups
- Basic cloud network troubleshooting

---

# ⚠️ Security & GitHub Publishing

Before making this repository **public**, check every screenshot and file for:

- `.pem` / `.ppk` private keys
- AWS access keys
- Secret keys
- Passwords
- API tokens
- Session tokens
- Personal information
- AWS account IDs
- Public IP addresses you do not want exposed

Never upload a private key such as:

```text
linux.pem
```

Use `.gitignore` to prevent accidental uploads.

Example:

```gitignore
*.pem
*.ppk
.env
.env.*
credentials
.aws/
```

---

# 👨‍💻 Author

**Jayesh Patil**

Day 1 practical submission for Linux and Cloud/DevOps training.
