---
title: "picoCTF2022: Local Authority"

date: 2022-04-08 21:09

layout: post
tags: 
- CTF
- picoCTF
- Web Exploitation
---

## [Local Authority](https://play.picoctf.org/practice/challenge/278?category=1&originalEvent=70&page=1)

> AUTHOR: LT 'SYREAL' JONES
> 
> 
> ### Description
> 
> Can you get the flag?
> Go to this [website](http://saturn.picoctf.net:65317/) and see what you can discover.
> 
> **Point:** 100
> 

There is a login form on the website. Let’s see what’s in the source 🔍

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-08-picoCTF2022-Local-Authority/Untitled.png)

Nothing much on the page really. No `.js` file as well. The form is sending a `POST` request to `/login.php` Let’s try logging in!

<aside>
🤫 Psst remember to open DevTools, so you can look at the `Network` tab and see what’s in a request.

</aside>

You won’t be successfully log in, BUT you see what sources come with the request?

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-08-picoCTF2022-Local-Authority/Untitled%201.png)

Double check with the `login.php` source, the `secure.js` and `style.css` are included. Let’s look at those files, shall we?

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-08-picoCTF2022-Local-Authority/Untitled%202.png)

Nothing in css file. In `secure.js`, turns out, it’s not that secured at all. It’s hard-coded checking credentials. 

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-08-picoCTF2022-Local-Authority/Untitled%203.png)

Once you get those credentials, go back and log in again. The flag shall be revealed 🚩 

## Closing Thoughts 💡

This is not a wise practice to authenticate the website on the client and hard-coded checking reveals credentials to the public. Typically, there would be a database to store users’ credentials. All passwords should be hashed (one-way function, not supposed to be reversible.) One popular algorithm used for this purpose is hashing with salt or so-called `salted-hash.` Salt is made of a unique random string added to an input of hashing function, which is a password in this case. It then results in a unique hash for each input even if inputs are the same.

If you want to go deeper on the salted hash, Auth0 has [an article](https://auth0.com/blog/adding-salt-to-hashing-a-better-way-to-store-passwords/) explaining the salted hash quite well.