# Recon
## Nmap
### Find Open Ports
```bash
# Nmap 7.80 scan initiated Mon Apr  5 18:37:11 2021 as: nmap -sC -sV -oA nmap/Delivery 10.129.129.225
Nmap scan report for 10.129.129.225
Host is up (0.025s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
| ssh-hostkey: 
|   2048 9c:40:fa:85:9b:01:ac:ac:0e:bc:0c:19:51:8a:ee:27 (RSA)
|   256 5a:0c:c0:3b:9b:76:55:2e:6e:c4:f4:b9:5d:76:17:09 (ECDSA)
|_  256 b7:9d:f7:48:9d:a2:f2:76:30:fd:42:d3:35:3a:80:8c (ED25519)
80/tcp open  http    nginx 1.14.2
|_http-server-header: nginx/1.14.2
|_http-title: Welcome
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Apr  5 18:37:21 2021 -- 1 IP address (1 host up) scanned in 9.81 seconds
```

## Nikto
### Basic Vuln Scan
```bash
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.129.129.225
+ Target Hostname:    10.129.129.225
+ Target Port:        80
+ Start Time:         2021-04-05 18:37:30 (GMT1)
---------------------------------------------------------------------------
+ Server: nginx/1.14.2
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ 7889 requests: 0 error(s) and 3 item(s) reported on remote host
+ End Time:           2021-04-05 18:40:43 (GMT1) (193 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```