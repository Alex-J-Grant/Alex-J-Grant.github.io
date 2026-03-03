---
title : 'Server Hardening'
description : "My experience hardening my linux home server"
category : ['writeups']
tags : ['Server,Ubuntu']
date : 2025-10-22
draft : False
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
4. Next I saw this cool video by [John Hammond on Youtube](https://youtu.be/NWytrZVM6WM?si=uu4InYAzT5qKglgC) on his honey pot and I was like wow that's cool so next was setting up a honey pot on my port 22 since i already moved my ssh port. I chose to follow what he did and went with "cowrie".

First I made a docker compose file to get the cowrie image and its files
```
#docker-compose.yml

# https://github.com/cowrie/cowrie

version: '3'
services:
  cowrie:
    image: cowrie/cowrie
    volumes:
      - ./session-logs:/cowrie/cowrie-git/var/lib/cowrie/tty/
      - ./user-files:/cowrie/cowrie-git/var/lib/cowrie/downloads/
      #where the logs of what people do inside are stored
      - ./cowrie/cowrie.json:/cowrie/cowrie-git/var/log/cowrie/cowrie.json
      - ./cowrie/cowrie.log:/cowrie/cowrie-git/var/log/cowrie/cowrie.log
      #message of the day when they come in
      - ./motd:/cowrie/cowrie-git/honeyfs/etc/motd
      #store userpasswords
      - ./cowrie/userdb.txt:/cowrie/cowrie-git/etc/userdb.txt
    #map outside port 22 to internal 2222
    ports:
      - 22:2222
```

Make those respective files
```
mkdir cowrie user-files session-logs
touch cowrie/cowrie.json
touch cowrie/cowrie.log
# enter the fake accounts you want e.g. user:x:password
nano cowrie/userdb.txt
#Make sure to make the motd serious and cool so hackers think they got you
nano motd
```

Then I experienced some permission errors so makes the files executable

```
# -R means recursive
chmod -R 777 cowrie/session-logs/ user-files/ session-logs/
```

Make sure your port is changed in your /etc/ssh/sshd_config
```
# look for the line #Port 22 and change to whatever port
nano /etc/ssh/sshd_config
```

Okay now ya can run it!
```
#to start the contianer
sudo docker-compose up -d

#to stop the container
sudo docker-compose down
```

When attackers get it by say ssh root@` <your ip> ` they can do whatever but it wont be saved upon exiting and you can view the activity
in cowrie/cowrie.json

The file in user-files/ is for exporting to other software for analysis





## Stuff I looked at
- [Harden your Linux server: Best practices for securing SSH,User Privileges, firewall configurations](https://medium.com/@habibullah.127.0.0.1/harden-your-linux-server-best-practices-for-securing-ssh-user-privileges-firewall-configurations-b3c7f1007543)

- [Cowrie honeypot setup guide](https://youtu.be/WdvBrfLn2G8?si=hGDZbdb5FXhx29fZ)