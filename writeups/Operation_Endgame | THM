Operation Endgame
![Operation_Endgame](../images/Operation_Endgame/Operation_Endgame.webp)

STEP I: Start the machine and using nmap scan the all ports

└─$ nmap -sC -sV -A -p- 10.49.130.64
```bash
Starting Nmap 7.98 ( https://nmap.org ) at 2026-03-07 12:38 -0500
Nmap scan report for 10.49.130.64
Host is up (0.065s latency).
Not shown: 65505 closed tcp ports (reset)
PORT      STATE SERVICE           VERSION
53/tcp    open  domain            Simple DNS Plus
80/tcp    open  http              Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec      Microsoft Windows Kerberos (server time: 2026-03-07 17:40:06Z)
135/tcp   open  msrpc             Microsoft Windows RPC
139/tcp   open  netbios-ssn       Microsoft Windows netbios-ssn
389/tcp   open  ldap              Microsoft Windows Active Directory LDAP (Domain: thm.local, Site: Default-First-Site-Name)
443/tcp   open  ssl/https?
| tls-alpn: 
|   h2
|_  http/1.1
| ssl-cert: Subject: commonName=thm-LABYRINTH-CA
| Not valid before: 2023-05-12T07:26:00
|_Not valid after:  2028-05-12T07:35:59
|_ssl-date: 2026-03-07T17:42:24+00:00; 0s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ldapssl?
3268/tcp  open  ldap              Microsoft Windows Active Directory LDAP (Domain: thm.local, Site: Default-First-Site-Name)
3269/tcp  open  globalcatLDAPssl?
3389/tcp  open  ms-wbt-server     Microsoft Terminal Services
| ssl-cert: Subject: commonName=ad.thm.local
| Not valid before: 2026-03-06T17:34:40
|_Not valid after:  2026-09-05T17:34:40
|_ssl-date: 2026-03-07T17:42:24+00:00; 0s from scanner time.
7680/tcp  open  pando-pub?
9389/tcp  open  mc-nmf            .NET Message Framing
47001/tcp open  http              Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc             Microsoft Windows RPC
49665/tcp open  msrpc             Microsoft Windows RPC
49666/tcp open  msrpc             Microsoft Windows RPC
49667/tcp open  msrpc             Microsoft Windows RPC
49669/tcp open  msrpc             Microsoft Windows RPC
49670/tcp open  ncacn_http        Microsoft Windows RPC over HTTP 1.0
49671/tcp open  msrpc             Microsoft Windows RPC
49675/tcp open  msrpc             Microsoft Windows RPC
49676/tcp open  msrpc             Microsoft Windows RPC
49681/tcp open  msrpc             Microsoft Windows RPC
49685/tcp open  msrpc             Microsoft Windows RPC
49712/tcp open  msrpc             Microsoft Windows RPC
49718/tcp open  msrpc             Microsoft Windows RPC
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ )
```
