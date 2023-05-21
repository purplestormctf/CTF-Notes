# CTF-Notes

### From Zero To Hero

#### About this repository

This repository will contain various notes, code snippets, hints and different sources related to all kinds of cybersecurity topics, dropped by our team members.

### Table of Contents

- [Where to start](https://github.com/purplestormctf/CTF-Notes/tree/main#where-to-start)
- [Basic Knowledge](https://github.com/purplestormctf/CTF-Notes/tree/main#basic-knowledge)
- [First Steps](https://github.com/purplestormctf/CTF-Notes/tree/main#first-steps)
- [Checklist](https://github.com/purplestormctf/CTF-Notes/tree/main#checklist)
- [Tool Recommendations](https://github.com/purplestormctf/CTF-Notes/tree/main#tool-recommendations)
- [Trainings](https://github.com/purplestormctf/CTF-Notes/tree/main#trainings)
- [Hacking Resources & Tutorials](https://github.com/purplestormctf/CTF-Notes/tree/main#hacking-resources--tutorials)

### Where to Start

#### Basic Knowledge

- It is recommended to work in a virtual environment like an `Kali Linux` instance running on `VirtualBox` or `VMware Player`.
- Make sure you downloaded the correct `.ovpn` file to connect to the network and access your box.

```c
$ sudo openvpn /PATH/TO/OVPNFILE/<USERNAME>.ovpn
```

- Wordlists are usually located in `/usr/share/wordlists/`. The mostly used ones are:
	- rockyou.txt (sudo gunzip /usr/share/wordlists/rockyou.txt.gz)
	- /usr/share/wordlists/seclists (https://github.com/danielmiessler/SecLists)
- If you are missing some tools, try to install them from the `Kali Linux repository`.
- If a website is not reachable via `IP address` and redirects you, try to add it to the `/etc/hosts` file.
- Always familiarize yourself with the tools you use and checkout their documentation as well as the parameter `-h`.
- Don't run exploits from the internet without understanding what they are doing.

```c
$ sudo apt-get install kali-linux-everything
```

#### First Steps

There are some basic things you should be aware about when you approaching a new box.

- Make sure to take proper `notes`. Probably you want to concider to write them in `Markdown` and `Obsidian` for example. Here are a few alternatives:
	- [cherrytree](https://github.com/giuspen/cherrytree)
	- [Sublime Text](https://www.sublimetext.com/)
	- [Dillinger.io](https://dillinger.io/)
- Always keep some sort of `reconnaissance` running in the background like `directory busting` with `Gobuster`, which can take some time.
- Make sure to `enumerate` every service and every endpoint properly. On a website for example, check for `usernames`, `email address schemes`, check the `source` of the website `click` or `hover over` every `link` you can find to see if they lead to something.
- Check for already known `vulnerabilities` and `exploits`. Therefore you can just use `Google`. Here are a few examples:
	- `<APPLICATION> vulnerability`
	- `<APPLICATION> <VERSION> vulnerability`
	- `<APPLICATION> <VERSION> exploit`
	- `<APPLICATION> <VERSION> poc`
	- `<APPLICATION> <VERSION> github`
	- `<APPLICATION> <VERSION> github poc`
	Alternatively check [Exploit Database](https://www.exploit-db.com/), [Sploitus](https://sploitus.com/) or use `searchsploit` from the command line.

```c
$ searchsploit <APPLICATION>
```

- Try `default credentials` (https://github.com/ihebski/DefaultCreds-cheat-sheet) or `admin:admin`. Especially on `web applications`.
- If you find credentials or just a password, always go for `credential reuse` and try if they work for another user as well.

#### Checklist

Depending on what a box offers to you, you can go through the following checklist.

1. Run nmap!

```c
$ sudo nmap -sC -sV -p- <RHOST>
$ sudo nmap -sC -sV -Pn -p- <RHOST>
$ sudo nmap -sV -sU <RHOST>
```

2. If a webserver is available, check `robots.txt`.

> http://RHOST/robots.txt

3. Also, give `whatweb` a try.

```c
$ whatweb http://<RHOST>
```

4. Ob websites, try `directory busting` with different wordlists.

```c
$ gobuster dir -w /usr/share/wordlists/seclists/Discovery/Web-Content/big.txt
```

5. Checking for `subdomains`. If a box offers you a `vhost` entry like `http://openadmin.htb/` for example, it is always worth it to see if there are more `vhosts` configured.

```c
$ gobuster vhost -u <RHOST> -t 50 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
$ gobuster vhost -u <RHOST> -t 50 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

```c
$ ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.openadmin.htb" -u http://openadmin.htb --mc all --fs <NUMBER>
```
  
6. Intercept `web requests` with `Burp Suite`. Without getting to deep into the usage of `Burp Suite`, here are the steps to configure it in your browser.
  - Start `Burp Suite` and open your browser on `http://burp`.
  - Then download the `CA Certificate`.
  - Depending on your browser, switch to `settings` and then to `certificates`.
  - Import the `certificate`.
  - We recommend to use [FoxyProxy](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-standard/) and configure it there but you can also go with the `proxy settings` of your browser.

| Setting | Value |
| --- | --- |
| Proxy Type | HTTP |
| Proxy IP address or DNS name | 127.0.0.1 |
| Port | 8080 |
  
  - In `Burp Suite` switch to `Target` > `Proxy settings` and select `Use advanced scope control`.
  - Add the `IP address` of the box you are approaching.
  - Switch to the `Proxy` tab, move to `Intercept` and click on `Intercept is off` to enable it.
  - At last switch the proxy in `FoxyProxy` to the `Burp Suite configuration` and access the website. Now you can intercept the web traffic coming from and going to the box and modify as you want.

#### Tool Recommendations

Below you find just a few tools to start with. Of course this is not a complete list and there are always better tools for the job out there. Take small steps and get comfy with tools and techniques to develop and at last improve your unique approach on a system.

###### Information Gathering

- [Nmap](https://github.com/nmap/nmap)
- [masscan](https://github.com/robertdavidgraham/masscan)
- [RustScan](https://github.com/RustScan/RustScan)

###### Vulnerability Analysis

- [nikto](https://github.com/sullo/nikto)

###### Web Application Analysis

- [WhatWeb](https://github.com/urbanadventurer/WhatWeb)
- [Burp Suite](https://portswigger.net/burp/communitydownload)
- [Gobuster](https://github.com/OJ/gobuster)
- [dirsearch](https://github.com/maurosoria/dirsearch)
- [ffuf](https://github.com/ffuf/ffuf)
- [wfuzz](https://github.com/asciimoo/wuzz)
- [WPScan](https://github.com/wpscanteam/wpscan)

###### Database Assessment

- [sqlmap](https://github.com/sqlmapproject/sqlmap)

###### Password Attacks

- [thc-hydra](https://github.com/vanhauser-thc/thc-hydra)
- [CrackMapExec](https://github.com/Porchetta-Industries/CrackMapExec)
- [hashcat](https://hashcat.net/hashcat/)

###### Exploitation / Post Exploitation Tools

- [Metasploit](https://github.com/rapid7/metasploit-framework)
- [BloodHound](https://github.com/BloodHoundAD/BloodHound)
- [Impacket](https://github.com/fortra/impacket)
- [enum4Linux-ng](https://github.com/cddmp/enum4linux-ng)
- [PEASS-ng](https://github.com/carlospolop/PEASS-ng)
- [PSPY](https://github.com/DominicBreuker/pspy)
- [Evil-WinRM](https://github.com/Hackplayers/evil-winrm)

#### Trainings

If you feel you need to learn  fundamentals of a new topic or to improve you knowledge in specific areas, I would recommend checking out [TryHackMe](https://tryhackme.com/) which provides valuable learning  paths to various topics.

Here are a few room recommendations for beginners.

- [Learning Cyber Security](https://tryhackme.com/room/beginnerpathintro)
- [Introductory Networking](https://tryhackme.com/room/introtonetworking)
- [Intro to Offensive Security](https://tryhackme.com/room/introtooffensivesecurity)
- [Linux Fundamentals Part 1](https://tryhackme.com/room/linuxfundamentalspart1)
- [Linux Fundamentals Part 2](https://tryhackme.com/room/linuxfundamentalspart2)
- [Linux Fundamentals Part 3](https://tryhackme.com/room/linuxfundamentalspart3)
- [Windows Fundamentals 1](https://tryhackme.com/room/windowsfundamentals1xbx)
- [Windows Fundamentals 2](https://tryhackme.com/room/windowsfundamentals2x0x)
- [Windows Fundamentals 3](https://tryhackme.com/room/windowsfundamentals3xzx)
- [Pentesting Fundamentals](https://tryhackme.com/room/pentestingfundamentals)
- [Active Reconnaissance](https://tryhackme.com/room/activerecon)
- [Nmap](https://tryhackme.com/room/furthernmap)
- [Burp Suite: The Basics](https://tryhackme.com/room/burpsuitebasics)
- [Web Application Security](https://tryhackme.com/room/introwebapplicationsecurity)
- [OWASP Top 10](https://tryhackme.com/room/owasptop10)
- [SQL Injection](https://tryhackme.com/room/sqlinjectionlm)
- [Hydra](https://tryhackme.com/room/hydra)
- [Metasploit: Introduction](https://tryhackme.com/room/metasploitintro)

#### Hacking Resources & Tutorials

Here are a few resources and knowledgebases to cover various topics. Starting with writeup videos of `Ippsec` is always a good call.

- [ippsec.rocks](https://ippsec.rocks/)
- [Hacking Articles](https://www.hackingarticles.in/)
- [HackTricks](https://book.hacktricks.xyz/welcome/readme)
- [netbiosX/checklists](https://github.com/netbiosX/Checklists)
- [The Penetration Testing Grimoire](https://github.com/RackunSec/Penetration-Testing-Grimoire)
- [pentestmonkey](https://pentestmonkey.net/)
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [GTFOBins](https://gtfobins.github.io/)

Also feel free to get in touch with us on our [Discord](https://discord.gg/CWBmP9h7), we are all willing to help!
