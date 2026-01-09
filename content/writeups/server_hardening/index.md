---
title : 'Server Hardening'
description : "My experience hardening my linux home server"
category : ['writeups']
tags : ['Server,Ubuntu']
date : 2025-10-22
draft : True
showAuthorBottom: true
---

## Intro

Hello friends, this will be me kinda taking notes on how I set up my server for future reference and for funsies. I actually got an insane deal on the server I got, its a 10 year old gaming PC however for the low low price of 70 dollaridoos so pretty happy with that. It was on windows of course so I first downloaded a Ubuntu server ISO(basically a file for putting that OS on your computer) and set that all up. I then exchanged SSH keys between this server and my computer so I could work on it from my main computer(I used the ssh-copy-id command). Ill include stuff I looked at below.


## Hardening SSH
Im gonna do this section in a kinda step format because im not so sure how to format it. The config file is in /etc/ssh/sshd_config btw.

1. Change my port for SSH away from the default of 22
2. Set these configs to no 
```
PasswordAuthentication no
PermitRootLogin no

```
3. Run this command to only allow SSH connections from your IP, also make sure you install iptables-persistent for the file to save to.
```
#Basically what its doing is making a rule for incoming connections(-A INPUT) for TCP(-p TCP) at the port of your SSH port(--dport <your ssh port>) -s from your address (-s <your ip address>)
allow them into the SSH(-j ACCEPT) if that rule doesn't happen then the same thing but don't let them into the SSH(-j DROP) also save it using that last line.

 sudo iptables -A INPUT -p tcp --dport <your ssh port> -s <your ip address> -j ACCEPT

sudo iptables -A INPUT -p tcp --dport <your ssh port> -j DROP

sudo iptables-save | sudo tee /etc/iptables/rules.v4 

```
4. 





## Stuff I looked at
- [Harden your Linux server: Best practices for securing SSH,User Privileges, firewall configurations](https://medium.com/@habibullah.127.0.0.1/harden-your-linux-server-best-practices-for-securing-ssh-user-privileges-firewall-configurations-b3c7f1007543)