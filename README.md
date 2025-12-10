# 🖧 DNS & DHCP on Ubuntu Server  
### Full Guide: BIND9 DNS + Kea DHCP4 Configuration

![Ubuntu Badge](https://img.shields.io/badge/Ubuntu-22.04%2B-E95420?style=flat&logo=ubuntu&logoColor=white)
![DNS Badge](https://img.shields.io/badge/BIND9-DNS%20Server-blue?style=flat)
![DHCP Badge](https://img.shields.io/badge/Kea-DHCP4%20Server-green?style=flat)
![Status Badge](https://img.shields.io/badge/Status-Complete-success?style=flat)

This repository provides a complete guide for installing and configuring:

- **BIND9 DNS Server**
- **Kea DHCP4 Server**

You will learn installation, configuration, verification, and testing across both Linux and Windows clients.

---

# 📑 Table of Contents

- [📘 Overview](#-overview)
- [📌 Network Diagram](#-network-diagram)
- [🧩 DNS Configuration](#-dns-configuration)
  - [1. Install BIND9 DNS Server](#1-install-bind9-dns-server)
  - [2. Configure Static IP](#2-configure-static-ip)
  - [3. Configure Global DNS Options](#3-configure-global-dns-options)
  - [4. Declare DNS Zones](#4-declare-dns-zones)
  - [5. Create DNS Zone Files](#5-create-dns-zone-files)
  - [6. Validate DNS Configuration](#6-validate-dns-configuration)
  - [7. Restart BIND9](#7-restart-bind9)
  - [8. Test DNS Resolution](#8-test-dns-resolution)
- [🧩 DHCP Configuration (Kea DHCP4)](#-dhcp-configuration-kea-dhcp4)
  - [9. Install Kea DHCP4 Server](#9-install-kea-dhcp4-server)
  - [10. Configure Kea DHCP4](#10-configure-kea-dhcp4)
  - [11. Validate DHCP Configuration](#11-validate-dhcp-configuration)
  - [12. Restart Kea DHCP Service](#12-restart-kea-dhcp-service)
- [🧪 Testing](#-testing)
  - [DNS Test](#dns-test)
  - [DHCP Test](#dhcp-test)
  - [DHCP Lease Database](#dhcp-lease-database)
- [✔ Summary](#-summary)

---

# 📘 Overview

This guide walks through the configuration of:

- A **local DNS server** using BIND9  
- A **DHCP server** using Kea DHCP4  
- Full validation and troubleshooting  
- Tests for Linux and Windows clients  

Ideal for labs, enterprise networks, and homelab environments.

---

# 📌 Network Diagram

<p align="center">
  <img src="images/DNS_DHCP.svg" width="600" alt="Network Diagram"/>
</p>

---

# 🧩 DNS Configuration

## 1. Install BIND9 DNS Server

### Update package list
```bash
sudo apt update
```

![Update Server](images/update_server.png)

### Install BIND9
```bash
sudo apt install bind9 -y
```

![Install BIND9](images/install_bind9.png)

---

## 2. Configure Static IP

Edit Netplan:
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

Apply:
```bash
sudo netplan apply
```

![Static IP](images/static_IP_server.png)

---

## 3. Configure Global DNS Options

```bash
sudo nano /etc/bind/named.conf.options
```

![Global Options](images/global_options.png)

---

## 4. Declare DNS Zones

```bash
sudo nano /etc/bind/named.conf.local
```

![Declare DNS Zone](images/declare_DNS_zone.png)

---

## 5. Create DNS Zone Files

### Forward Zone
```bash
sudo nano /etc/bind/db.cyber-with-taptue.local
```

### Reverse Zone
```bash
sudo nano /etc/bind/db.192.168.1
```

![DNS Database](images/local_database_DNS.png)
![Reverse DNS](images/database_inverse.png)

---

## 6. Validate DNS Configuration

Check global configuration:
```bash
sudo named-checkconf
```

Check zone:
```bash
sudo named-checkzone cyber-with-taptue.local /etc/bind/db.cyber-with-taptue.local
```

![DNS Validation](images/check_DNS_configuration.png)

---

## 7. Restart BIND9

```bash
sudo systemctl restart bind9
```

---

## 8. Test DNS Resolution

```bash
sudo nano /etc/resolv.conf
```

![Resolver](images/edit_resolver.png)
![DNS Test](images/test_DNS_on_server.png)

---

# 🧩 DHCP Configuration (Kea DHCP4)

## 9. Install Kea DHCP4 Server

```bash
sudo apt install kea-dhcp4-server -y
```

![Install Kea](images/install_kea-dhcpv4.png)

---

## 10. Configure Kea DHCP4

```bash
sudo nano /etc/kea/kea-dhcp4.conf
```

Configure:

- Subnet
- IP Pool
- Default Gateway
- DNS Servers
- Lease Database

![DHCP Configuration](images/DHCP_configuration.png)

---

## 11. Validate DHCP Configuration

```bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

```bash
sudo kea-dhcp4 -c /etc/kea/kea-dhcp4.conf
```

![DHCP Validation](images/check_DHCP_configuration.png)

---

## 12. Restart Kea DHCP Service

```bash
sudo systemctl restart kea-dhcp4-server
```

---

# 🧪 Testing

## DNS Test

### On Windows
```cmd
nslookup srv-dns.cyber-with-taptue.local
```

![Windows DNS Test](images/DNS_test_on_Windows.png)

### On Linux
```bash
nslookup srv-dns.cyber-with-taptue.local
```

![Linux DNS Test](images/DNS_test_on_Linux.png)

---

## DHCP Test

### On Windows
```cmd
ipconfig /all
```

![Windows DHCP Test](images/DHCP_test_on_windows.png)

### On Linux
```bash
ip a
cat /etc/resolv.conf
```

![Linux DHCP Test](images/DHCP_test_on_Linux.png)

---

## DHCP Lease Database

Kea stores DHCP leases in:

```
/var/lib/kea/kea-leases4.csv
```

![DHCP Lease File](images/database_lease_DHCP.png)

---

# ✔ Summary

This guide includes:

- Full BIND9 DNS configuration  
- Kea DHCP4 setup  
- Validation commands  
- Complete test procedures  
- DNS & DHCP zone file references  

---