---
title: HTB - Knife
author: gpandres
date: 2021-09-08 20:00:00 +0800
categories: [Cybersecurity,HTB,WriteUps]
tags: [HTB,Knife,PHP,GTFOBin,Backdoor]
---



![knifeimg](https://gpandres.github.io/assets/img/posts/knife.jpeg)


<h3 data-toc-skip>Hello, we will start scanning ports with NMAP</h3>
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
