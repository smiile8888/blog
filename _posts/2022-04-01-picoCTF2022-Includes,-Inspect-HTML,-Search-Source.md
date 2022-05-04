---
title: "picoCTF2022: Includes, Inspect HTML, Search Source"

date: 2022-04-01 15:41

layout: post
tags: 
- CTF
- picoCTF
- Web Exploitation
---

I sum three questions, *Includes, Inspect HTML, and Search Source,* in this writeup since they both have the similar vulnerability—**leave the important information in the source code.** Let’s dig in.

## [1. Includes](https://play.picoctf.org/practice/challenge/274?category=1&originalEvent=70&page=1)

> AUTHOR: LT 'SYREAL' JONES

**Description**
Can you get the flag?Go to this [website](http://saturn.picoctf.net:61570/) and see what you can discover.

**Point:** 100
> 

On the website, there is only one button; when clicking it, the alert says “Hello” pops up.

Let’s inspect the source code.

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF%20%F0%9F%9A%80/2022-04-01-picoCTF2022-Includes%2C-Inspect-HTML%2C-Search-Source/Untitled.png)

There are two files included in the html. Let’s look at those!

To inspect, if you are using Chrome, go to `Sources` tab in DevTools. If you are using Firefox, go to `Debugger` tab in the DevTools. There should be `style.css` and `script.js` showing. Let’s check at each file. And there we go! **The flag is commented, put in separate files.**

## [2. Inspect HTML](https://play.picoctf.org/practice/challenge/275?category=1&originalEvent=70&page=1)

> AUTHOR: LT 'SYREAL' JONES
> 
> 
> ### **Description**
> 
> Can you get the flag?
> Go to this [website](http://saturn.picoctf.net:49511/) and see what you can discover.
> 
> **Point:** 100
> 

This website is only text context, nothing interesting or seems like exploitable. Well, how about anything hidden or commented? Let’s inspect! Similar to the previous one, when checking the source, turns out, the flag hanging as a comment line 🪂

## [3. Search Source](https://play.picoctf.org/practice/challenge/295?category=1&originalEvent=70&page=1)

> AUTHOR: MUBARAK MIKAIL
> 
> 
> ### Description
> 
> The developer of this website mistakenly left an important artifact in the website source, can you find it?
> 
> The website is [here](http://saturn.picoctf.net:58519/)
> 
> **Point:** 100
> 

It looks like the website is under developed. Inspecting around html, doesn’t seem to see any flag. The form doesn’t seem to work or send any request to the backend; so no XSS. 

🤔 Hmm, let’s check each source file. Boom! one of the source files includes **a comment with the flag.**

## Closing Thoughts 💡

Remember to **remove** unused comments or any variable, especially secrets/credentials before deploying to production. Or even just commit to version control. Don’t let the crowd gets your clue.