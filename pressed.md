# PRESSED

### Maquina pressed de HTB hard 

```
Hard
Linux

Skills:
Password Guessing
WordPress Abusing RPC Calls
WordPress XML-RPC Create WebShell
PwnKit Exploit
```

# Reconnaissance

```markdown
 Nmap 7.94SVN scan initiated Fri Jun  7 19:11:26 2024 as: nmap -sCV -oN target 10.10.11.142
Nmap scan report for 10.10.11.142
Host is up 0.17s latency).
Not shown: 999 filtered tcp ports ((no-response)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: UHC Jan Finals &#8211; New Month, New Boxes
|_http-generator: WordPress 5.9
|_http-server-header: Apache/2.4.41 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done at Fri Jun  7 19:11:56 2024 -- 1 IP address (1 host up) scanned in 30.15 seconds


Nmap 7.94SVN scan initiated Sat Jun  8 01:51:48 2024 as: nmap --script http-enum -p80 -oN webscan 10.10.11.142
Nmap scan report for pressed.htb (10.10.11.142)
Host is up (0.18s latency).

PORT   STATE SERVICE
80/tcp open  http
| http-enum:
|   /wp-login.php: Possible admin folder
|   /readme.html: Wordpress version: 2
|   /: WordPress version: 5.9
|   /wp-includes/images/rss.png: Wordpress version 2.2 found.
|   /wp-includes/js/jquery/suggest.js: Wordpress version 2.5 found.
|   /wp-includes/images/blank.gif: Wordpress version 2.6 found.
|   /wp-includes/js/comment-reply.js: Wordpress version 2.7 found.
|   /wp-login.php: Wordpress login page.
|   /wp-admin/upgrade.php: Wordpress login page.
|_  /readme.html: Interesting, a readme.

 Nmap done at Sat Jun  8 01:52:15 2024 -- 1 IP address (1 host up) scanned in 27.06 seconds

```

# Construccion de la pagina web 

### Comando para validar en que esta construida la pagina, nos estamos enfrentando a una pagina construida en wordpress 

`whatweb  http://10.10.11.142`

```
http://10.10.11.142 [200 OK] Apache[2.4.41], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[10.10.11.142], JQuery[3.6.0], MetaGenerator[WordPress 5.9], Script[text/javascript], Title[UHC Jan Finals &#8211; New Month, New Boxes], UncommonHeaders[link], WordPress[5.9]

```
### La pagina nos muestra todas las peticiones http que se estan realizando, entonces con cul podemos tirarle varias peticiones.

`curl -s -X GET 'http://10.10.11.142' -H 'User-Agent: super'`

### Como la pagina esta en wordpress podemos indentificar vulnerabilidades con `wpscan`

``
