---
title: "picoCTF2022: Roboto Sans"

date: 2022-03-30 12:34

layout: post
tags: 
- CTF
- picoCTF
- Web Exploitation
---

> ***Roboto Sans***
> 
> 
> *AUTHOR: MUBARAK MIKAIL*
> 
> ***Description***
> 
> *The flag is somewhere on this web application not necessarily on the website. Find it.*
> 
> *Check [this](http://saturn.picoctf.net:55983/) out.*
> 

This challenge got me a while to dig in and find where the flag is. I might be overthinking at time but turned out it’s a classic one. Let’s dive in.

When going to the website, it looks like an under-development website. Looking all source files, not yet found anything. The clue said,

> ***“on this web application, not necessarily on the website”***
> 

Alright, it has to be something behind the scene but part of it. Well, it could be XSS and get something but if looking carefully into `.js` file, the form on the website does basically nothing. Not even sending the form; thus, it’s not connected with the backend at all. 

Alright, next could be some hidden paths and files. How to find it? One potential place to look for that is `/robots.txt.` Shall we go check it out?

![Untitled](/assets/posts/2022-03-29-picoCTF2022%20~%20whoop%2C%20my%20first%20CTF/2022-03-30-picoCTF2022-Roboto-Sans/Untitled.png)

Hah! got you! 

Wait, really? NO, NOT YET. What are those alien languages 🤔

My first thought was *encoding-decoding is everywhere.* With my noob brain, I saw `==` at the end of the second line, which telling me it’s potentially encoded by `base64.` Let’s decode it. Heading to [https://www.base64decode.org/](https://www.base64decode.org/) and paste those text.

The decoded message is 

```jsx
flag1.txtjs/myfijs/myfile.txt,z谖/u%z8
```

Potentially separated by `;`  and new line; thus I broke to new line and decode each line separately, and see what it got.

```jsx
flag1.txt
js/myfi
js/myfile.txt
,z
(hnj^
```

Okay, now we have a lot more and greater idea where the flag can be stored at. Let’s go to each file and see. You shall see where the flag is 😉

And...you shall get 200 points to your scoreboard 🎖️