# DNS & DHCP on Ubuntu Server

This repository provides a complete and beginner-friendly guide for installing and configuring a **BIND9 DNS server** and a **Kea DHCP4 server** on Ubuntu Server. It includes installation steps, configurations, validation commands, and testing procedures for both Linux and Windows clients.

---

# Table of Contents
- [Network Diagram](#network-diagram)
- [1. Install BIND9 DNS Server](#1-install-bind9-dns-server)
- [2. Configure Static IP for the DNS Server](#2-configure-static-ip-for-the-dns-server)
- [3. Configure Global Options in BIND9](#3-configure-global-options-in-bind9)
- [4. Declare DNS Zones](#4-declare-dns-zones)
- [5. Create Local DNS Database](#5-create-local-dns-database)
- [6. Validate DNS Configuration](#6-validate-dns-configuration)
- [7. Restart BIND9](#7-restart-bind9)
- [8. Test Local DNS Resolution](#8-test-local-dns-resolution)
- [DHCP Configuration (Kea DHCP4 Server)](#dhcp-configuration-kea-dhcp4-server)
- [9. Configure Kea DHCP4 Server](#9-configure-kea-dhcp4-server)
- [10. Validate DHCP Configuration](#10-validate-dhcp-configuration)
- [11. Restart Kea DHCP4 Service](#11-restart-kea-dhcp4-service)
- [Testing](#testing)
  - [Test DNS Assignment](#test-dns-assignment)
  - [Test DHCP Assignment](#test-dhcp-assignment)
  - [DHCP Lease Database](#dhcp-lease-database)
- [Summary](#summary)

---

## Network Diagram

![Network Diagram](images/DNS_DHCP.svg)

---

## 1. Install BIND9 DNS Server

### Update package list
```bash
sudo apt update
```

![Update Our Server](images/update_server.png)

### Install BIND9
```bash
sudo apt install bind9 -y
```

![Install Bind9](images/install_bind9.png)

---

## 2. Configure Static IP for the DNS Server

Edit your Netplan file:
```bash
sudo nano /etc/netplan/50-cloud-init.yaml
```

![Static IP for Server](images/static_IP_server.png)

Apply the configuration:
```bash
sudo netplan apply
```

---

## 3. Configure Global Options in BIND9

Edit:
```bash
sudo nano /etc/bind/named.conf.options
```

![Global Options](images/global_options.png)

---

## 4. Declare DNS Zones

Edit:
```bash
sudo nano /etc/bind/named.conf.local
```

![Declare DNS Zone](images/declare_DNS_zone.png)

---

## 5. Create Local DNS Database

Create zone file:
```bash
sudo nano /etc/bind/db.cyber-with-taptue.local
```

Create reverse zone file:
```bash
sudo nano /etc/bind/db.192.168.1
```

![Local DNS Database](images/local_database_DNS.png)

![Inverse DNS Database](images/database_inverse.png)

---

## 6. Validate DNS Configuration

Check BIND configuration:
```bash
sudo named-checkconf
```

Check zone:
```bash
sudo named-checkzone cyber-with-taptue.local /etc/bind/db.cyber-with-taptue.local
```

![Validate DNS Configuration](images/check_DNS_configuration.png)

---

## 7. Restart BIND9

```bash
sudo systemctl restart bind9
```

---

## 8. Test Local DNS Resolution

Edit resolver settings:
```bash
sudo nano /etc/resolv.conf
```

![Edit Resolver](images/edit_resolver.png)

![Test DNS on server](images/test_DNS_on_server.png)

---

# DHCP Configuration (Kea DHCP4 Server)

### Install Kea DHCP4 Server
```bash
sudo apt install kea-dhcp4-server -y
```

![Install Kea-dhcp4-server](images/install_kea-dhcpv4.png)

---

## 9. Configure Kea DHCP4 Server

Edit main configuration file:
```bash
sudo nano /etc/kea/kea-dhcp4.conf
```

![DHCP Configuration](images/DHCP_configuration.png)

---

## 10. Validate DHCP Configuration

Test configuration syntax:
```bash
sudo kea-dhcp4 -t /etc/kea/kea-dhcp4.conf
```

Start Kea with config:
```bash
sudo kea-dhcp4 -c /etc/kea/kea-dhcp4.conf
```

![Check DHCP configuration](images/check_DHCP_configuration.png)

---

## 11. Restart Kea DHCP4 Service

```bash
sudo systemctl restart kea-dhcp4-server
```

---

# Testing

## Test DNS Assignment

### Windows
```cmd
nslookup srv-dns.cyber-with-taptue.local
```

![DNS test on Windows](images/DNS_test_on_Windows.png)

### Linux
```bash
nslookup srv-dns.cyber-with-taptue.local
```

![DNS test on Linux](images/DNS_test_on_Linux.png)

---

## Test DHCP Assignment

### Windows
```cmd
ipconfig /all
```

![DHCP test on Windows](images/DHCP_test_on_windows.png)

### Linux
```bash
ip a
```

```bash
cat /etc/resolv.conf
```

![DHCP test on Linux](images/DHCP_test_on_Linux.png)

---

## DHCP Lease Database

Kea DHCP lease records are stored in:

```
/var/lib/kea/kea-leases4.csv
```

![DHCP lease database](images/database_lease_DHCP.png)

---

# Summary

This repository includes:

- BIND9 installation and DNS zone setup  
- Kea DHCP4 configuration  
- Validation tools  
- Testing instructions  
- DNS & DHCP database references  