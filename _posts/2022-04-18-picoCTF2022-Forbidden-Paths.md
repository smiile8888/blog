---
title: "picoCTF2022: Forbidden Paths"

date: 2022-04-18 20:19

layout: post
tags: 
- CTF
- picoCTF
- Web Exploitation

---

## [Forbidden Paths](https://play.picoctf.org/practice/challenge/270?category=1&originalEvent=70&page=1)

> AUTHOR: LT 'SYREAL' JONES
> 
> 
> ### Description
> 
> Can you get the flag? Here's the [website](http://saturn.picoctf.net:50561/).
> We know that the website files live in `/usr/share/nginx/html/` and the flag is at `/flag.txt` but the website is filtering absolute file paths. Can you get past the filter to read the flag?
> 
> **Points:** 200
> 

This is a classic problem of file inclusion. If you want to learn more ***what the file inclusion is?*** 🤔 Check out [my local file inclusion writeup.](/THM-File-Inclusion) I explain it in details and link with informative resources for you to dive deeper.

Come back to the problem. First thing first, let’s go the website.

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-18-picoCTF2022-Forbidden-Paths/Untitled.png)

There is nothing really, excepts the input box that we can do something with. From the description, it says *“the website is filtering absolute file paths”.* Should we try if that is true?

Yepper, it show back `Not Authorized`.

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-18-picoCTF2022-Forbidden-Paths/Untitled%201.png)

So what else can we do to access files? A relative path it is.

**What is a relative path?**

<aside>
💡 A relative path refers to a location that is relative to a current working directory. It uses `.`, a dot, to refer a current working directory, and `..`, or a double dot, to refer a parent directory.

</aside>

How can we get the relative path of `/flag.txt`?

Instead of entering `/flag.txt`, we can enter `../../../../flag.txt`.

And there we go 🚩

### ✍️ Note:

when it comes to a relative path, you can do `../` as many as levels of directory or more. In this case, you can do `../../../../../../../flag.txt`; that will still bring you to `/`.