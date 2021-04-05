# User Flag
## Checking Web Page
Going to the webpage we see some links the first one takes you to helpdesk.delivery.htb, So lets add it to our hosts file and see what we get...
So we get a support webpage lets keep it in the background and see what else we can find...
![[Pasted image 20210405135404.png]]
If you open the contact card you can see that they have a MatterMost Server lets go to there, from there they redirect us to delivery.htb:8065 and ask for a login, and we can also create an account...
![[Pasted image 20210405135601.png]]
On Creating an account we are asked for email verification, the problem is that it has no contact to other servers like gmail or any other general mail server what we can do is we can create a smtp server on our host machine and then have the email sent there... lets keep that at the back of our heads for now...
![[Pasted image 20210405142634.png]]

## Exploiting mail service for support
so we find out that we can make a ticket on our support site... so lets take a look and create a support ticket to see what happens, also put dummy info just to see what happens.
![[Pasted image 20210405143032.png]]
So we get a ticket id and a ticket **EMAIL** so lets go and see the staus of our ticket... `ALSO REMEMBER TO SAVE YOUR TICKET ID AND THE EMIAL YOU USE`
![[Pasted image 20210405143801.png]]
SO now that we have a viable email server... thinking that the box can send emails to it self then we can get the link that is to verify the email address and then we can be off to continue pwning this box... `So LETS GET TO IT`
## Creating Account
Now Create your account using the email that was provided for me its `6441381@delivery.htb` and then put in the info needed, use whatever username you like and password that fills the criteria. `After That Check back on your support ticket and FIND A LINK TO OPEN YOUR ACCOUNT! YES!`
now copy this link and put it into your browser and and going into the account...
![[Pasted image 20210405144621.png]]
fill out your password and join the Internal Team... after this skip the tutorial and then we find creds and some very useful information
![[Pasted image 20210405144856.png]]
### NOW SSH IN AND GET USER FLAG!!!
```bash
┌─[user@parrot-virtual]─[~]
└──╼ $ssh maildeliverer@10.129.129.225
The authenticity of host '10.129.129.225 (10.129.129.225)' can't be established.
ECDSA key fingerprint is SHA256:LKngIDlEjP2k8M7IAUkAoFgY/MbVVbMqvrFA6CUrHoM.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.129.129.225' (ECDSA) to the list of known hosts.
maildeliverer@10.129.129.225's password: 
Linux Delivery 4.19.0-13-amd64 #1 SMP Debian 4.19.160-2 (2020-11-28) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Tue Jan  5 06:09:50 2021 from 10.10.14.5
maildeliverer@Delivery:~$ ls
user.txt
maildeliverer@Delivery:~$ 

```
![[Pasted image 20210405145230.png]]
