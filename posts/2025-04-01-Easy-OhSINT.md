
# # TryHackMe: OhSINT

**Challenge Name:** OhSINT
**Platform:** TryHackMe
**Category:** OSINT
**Difficulty:** Easy
**Date Completed:** 04/01/2025

## 1. Overview

The OhSINT challenge asks us to download a jpg file to start. It is a beginner friendly challenge that tests basic OSINT skills. The goal of the challenge is to extract information from the file and use that as a foundation to answer questions about the owner.
## 2. Tools and Techniques

- ExifTool
- A Web Browser (in my case Firefox)
## 3. Solution

We begin by downloading the file. We are given a jpg file that looks like this when opened:

![](attachment/6a2d98e0dc28bfd16931ceb906935fd8.png)

This does not tell us much. It appears to be the classic Window XP desktop background. 

We can use ExifTool in the command line to extract metadata about this image:

```bash
exiftool WindowsXP_1551719014755.jpg
```

After doing so, we are given a list of useful information about the image:

![](attachment/9ad4771b4e77a8be568692ccfdad7f09.png)

The first task of the challenge is to find the "user's avatar." Knowing this, I thought the name attributed to the image Copyright might lead to something useful.

![](attachment/4ae8cff725de9454cfa27204b6fc5967.png)

The name is "OWoodflint."

It sounds like a username, so I decided to enter it into Firefox to see what would pop up.

![](attachment/7533d9024e5045c3a2908ad9e95d2eae.png)

The first three hits are links to a github, twitter, and wordpress blog. They all seem to be associated with the OWoodflint username.

The github profile picture was just a default pfp, so I checked the twitter account.

![](attachment/90fc228fc37ccb7e01fd964ffe21bca5.png)

Given this information we can now answer the first task, and it works.

In the user's twitter posts, we can see a post about a Bssid:

```pug
Bssid:B4:5D:50:AA:86:41
```

![](attachment/bb3ac386d76761543fcd01292eb9e2e3.png)

The next task asks us which city this person is in. We can use this BSSID to help us locate the user.

We can go to Wigle and use a basic search of the BSSID to help us locate the user:

![](attachment/57096fae626ad937c5173c154bc64ab0.png)

![](attachment/0afec66cc4c7a8c422c525bfa0e07274.png)

This puts us in London.

If we zoom in, we can see the SSID associated:

![](attachment/b9aa7eb70cf940abaceb8532867deb43.png)

Now we have found the location and the SSID of the WAP the user is connected to. We now have answers to the first three tasks. 

Lets move on to the fourth task where we are asked about the user's personal email address.

If I remember correctly, I saw an email address pop up when I initially searched for the user in Firefox. I will go back and check.

![](attachment/49f07e53259483c2d593a21ec712a3e4.png)

I was able to locate the email under the GitHub profile.

This information helps us answer the fourth and fifth task of the email address and the site where it was found.

The next task is to find out where the user has recently gone on vacation.

I decided to go to the GitHub profile to see if there was anything informative regarding this. I could not find anything, but it did link to the personal blog site we saw before, so I visited the site.
![](attachment/e56e13c4f1df96a8b9e2197bb5919a38.png)

![](attachment/85c2dc592438247aa21aeee580e9f4ca.png)
On the blog site, we can see where the user is currently on vacation.

---

Now, we can move on to the final task, which is to find the user's password. At first I was a little confused by this because this room is about OSINT and did not include any exploitation or vulnerability finding up until this point. With that knowledge, I concluded the password must be hidden within the three sites associated with the user - the most likely being the wordpress blog site as twitter and github would not have revealing source code.

I inspected the source code of the blog site and dug around for a bit until I stumbled upon something interesting - a strange string in plaintext under the entry content. This was the answer to the final task.

![](attachment/ffff8e15e273689e3b9e028c166a3733.png)






