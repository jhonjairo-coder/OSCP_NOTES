
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

