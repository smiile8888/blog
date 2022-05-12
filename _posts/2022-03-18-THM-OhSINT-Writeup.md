---
title: "THM: OhSINT Writeup"
emoji: "🧙‍♀️"

date: 2022-03-18 18:41

# Created: March 18, 2022 5:25 PM
# last-updated: March 18, 2022 6:41 PM

layout: post
tags:
- Cybersecurity
- OSINT
- THM
category: blog
headerImage: false
---

<aside>
Have you ever wondered how people get to know you more virtually and legally at their fingertips? 💡
</aside>


OSINT (Open Source INTelligence) is the collection and analysis of data gathered from open sources to produce actionable intelligence. Information can come from government and business websites, blog posts, social media, news, forums, etc. TryHackMe has a brief room, [OhSINT](https://tryhackme.com/room/ohsint), for you to explore OSINT and use it to find out and answer. More about OSINT will be another post. Stay tuned 😊

Let’s go to the room and start the attack 💪

...

There is only one piece of clues given in the room, ***an image.*** What can we do with *an image*?

### 1. What is this user's avatar of?

Hmm, I went blank..., but the hint tells me to use [Exiftool](https://exiftool.org/). Interestingly, we can look into something called the ***“metadata”*** of a file. I quoted from its official website:

> *“ExifTool is a platform-independent [Perl library](https://exiftool.org/ExifTool.html) plus a [command-line application](https://exiftool.org/exiftool_pod.html) for reading, writing and editing meta information in a [wide variety of files](https://exiftool.org/#supported).”*
> 

Go to the website and install if you haven’t already. And let’s see what can be found on the image!

![Untitled]({{ site.baseurl }}/assets/posts/2022-03-18-THM-OhSINT-Writeup/Untitled.png)

One interesting field is ***“Copyright.”*** Looks like someone named ***“OWoodflint”*** owns this image. Let’s google him, shall we? The following attachment shows my search results on Google. Let’s check these top 3 out.

![Untitled]({{ site.baseurl }}/assets/posts/2022-03-18-THM-OhSINT-Writeup/Untitled%201.png)

Above all three, only the account on Twitter has the recognized avatar; so that’d be the answer for Q.1.

### 2. What city is this person in?

If you check out all three sources, you can probably answer this question since he has publicly told the world where he’s from!

But let’s look at the hint. It tells us to dig deep by using ***BSSID*** and *[wigle.net](http://wigle.net).*

What is the BSSID? It’s a ***Basic Service Set Identifier***, which is the *MAC address of an access point.* Can we get more information about location, SSID, and more from this? Of course, that’s what [wigle.net](http://wigle.net) is for. Heading to wigle and search with BSSID that is given on Twitter. Then zoom in, it will show the location on the map.

### 3. What's the SSID of the WAP he connected to?

From Q.2, you have to create an account on [wigle.net](http://wigle.net) to be able to see the information of the BSSID. Then zoom in till you see the SSID.

Note that, when you create an account, wigle mentions that you don’t even have to use the actual information to register; find out more on the website yourself.

![Untitled]({{ site.baseurl }}/assets/posts/2022-03-18-THM-OhSINT-Writeup/Untitled%202.png)

If you want to learn more about wigle, [the article on OSINT Curious](https://osintcurio.us/2019/01/15/tracking-all-the-wifi-things/) wrote in detail.

### 4. What is his personal email address?

### 5. What site did you find his email address on?

### 6. Where has he gone on holiday?

For Qs.4 to 6, let's head to other media to look for more information about OWoodflint. He has publicized his email on it.

### 7. What is this person’s password?

The last source that hasn’t been used is the key for the last two questions! Head to there and see what OWoodflint posted about his holiday location and what he left it to-be-blind on the website, but it’s apparently not if you drag or inspect the website.

### Closing thoughts 💡

This is one of the TryHackMe rooms that I can learn a ton from a brief activity. Social media and other open source intelligence channels can be handy and informative. Of course, it can be served for angel and evil purposes. All methods and sources used here thus far are legal to explore and obtain. As end-users, we should be careful when we share anything, no matter what it is—name, email, address, personal life, etc. Once it’s online, it’s there, and everyone can find out about you. With today's cutting-edge technology, everything is now even more pieced together. As I’m getting into OSINT more and more, it surprises me how much information I can find online.

Happy hacking 🦉