---
title: "THM: LFI Writeup"

Created: March 28, 2022 11:59 PM
Last edited: April 10, 2022 5:50 PM
tags: 
- Cybersecurity
- Inclusion
- LFI
- THM
- Web Exploitation

layout: post
category: blog
headerImage: false
---

## Ayo!

Local File Inclusion is fun, isn’t it 🤫 Join me to do the [LFI room](https://tryhackme.com/room/lfi) on Web Fundamental path. This one is actually very beginner-friendly. The targeted website is so much exposed, and easy to find all valuable data and flags.

If you don’t know what **Local File Inclusion (LFI)** is, let me tell you a bit about it:

<aside>
💡 LFI is a vulnerability that allows you to read files that aren’t supposed to be readable by unauthorized users. How? Mostly, you can put ***a certain file path*** to inputs or as a parameter in the URL. Sometimes, websites might filter out the absolute path, but it doesn’t mean the relative path wouldn’t work. If there is such a vulnerability, outputs from the file will show as expected.
</aside>

Let’s get started! 💪

When you go to the website, try to go to other pages. Notice what parameters are in the URL. You’ll see `page=...` where you can try with a particular path like `/etc/passwd.`

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled.png)

It shows an error. It must filter out an absolute path. Why don’t you try the relative path then? Let’s try `../../../../etc/passwd`

There we go!

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled%201.png)

If you want to understand in-depth what’s in the passwd file, head to [this website.](https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/)

---

## Breakin in 🧨

Now we know what users are on the web server. It’s time to break in! But how? There are two potential ways: 1) see if `Falcon` has his identity for `ssh`; 2) crack Falcon’s password.

Let’s look at each!

### 1) Get an ssh private key

Normally, the ssh's identity or private key is at `~/.ssh/id_rsa`. `id_rsa` is the default file name for the private key. Let’s see if we will find it. Go to `../../../../home/falcon/.ssh/id_rsa`.

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled%202.png)

Luckily, it’s there. You can then copy it to your local machine and remember to change the permission to only the owner can read and write, which means `chmod 600 <path_to_id_rsa_file>`. And you should be able to ssh to the web server as Falcon 👻

### 2) Crack a password 🔑

There are many ways to crack passwords, but I’ll use `hashcat` here.

Before doing anything, let’s get Falcon’s hash from `/etc/shadow` first.

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled%203.png)

If you care to know what’s in the shadow file, let me tell you a bit:

<aside>
💡 The `/etc/shadow` file contains information about a password corresponding to a user.

</aside>

Below is the template, and each piece of information is separated by `:`

```
mark:$6$.n.:17736:0:99999:7:::
[--] [----] [---] - [---] ----
|      |      |   |   |   |||+-----------> 9. Unused
|      |      |   |   |   ||+------------> 8. Expiration date
|      |      |   |   |   |+-------------> 7. Inactivity period
|      |      |   |   |   +--------------> 6. Warning period
|      |      |   |   +------------------> 5. Maximum password age
|      |      |   +----------------------> 4. Minimum password age
|      |      +--------------------------> 3. Last password change
|      +---------------------------------> 2. Encrypted Password
+----------------------------------------> 1. Username
```

The encrypted password piece is where the password can be cracked. The format is `$hashing_algorithm$salt$hash`.

If you want to read more, head to [the linuxize article](https://linuxize.com/post/etc-shadow-file/).

Now, we got Falcon’s encrypted password; we will use `hashcat`. First, copy his encrypted password to a text file. You may notice `$6$....` on Falcon’s encrypted password; that value specifies what method is used, and `$6$` is `SHA-512`. For hashcat command, we have to set **the mode or hashing algorithm** that we want hashcat to perform. In this case, we want `SHA-512`, which is `1800` for hashcat. Below is the command to crack the password: 

```bash
$ hashcat -m 1800 <path_to_password_file> /usr/share/wordlists/rockyou.txt
```

It will take quite a while, and note that you can try other wordlists. Once you get Falcon’s password, ssh to the web server with his credentials.

With both ways, you should now have access to the web server. It’s time for you to get a flag 🚩

---

## Privilege Escalation

Now, you got a shell. BUT as a user. The goal is we need to get it to the superuser. How?

Let’s see what commands or files Falcon can run as a root. Let’s do `sudo -l` that will list Falcon’s privilege. And we can see `/bin/journactl`.

What can we do with that? Heading to [GFTO](https://gtfobins.github.io/gtfobins/journalctl/). 

There we go! We can take advantage of`/bin/journalctl` and once you execute the command, follow with `!/bin/sh`. Root shell is here for you to explore!

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled%204.png)

### What else? 🤔

Hmm, even though we got the root shell, I thought since the `/etc/shadow` is already exposed, why don’t try using `hashcat` and get the root’s password? Shall we do that?

I did hashcat on the root’s encrypted password, but *unfortunately*, I cannot access the credentials. I had to run at the time, so I didn’t explore more on that. 

![Untitled](/assets/posts/2022-04-10-THM-LFI-Writeup/Untitled%205.png)

But if you read to this point and are curious, please play with it, and ***please*** come back and share with me what you’ve got!

Happy hacking 🧙‍♀️