# TryHackMe: Surfer

**Challenge Name:** Surfer
**Platform:** TryHackMe
**Category:** SSRF
**Difficulty:** Easy
**Date Completed:** 07/02/2025

# Introduction



"Surf some internal webpages to find the flag!"

# Method

When I first visited the IP address, I was met with a login page. I tried `admin` `admin` as the username and password and got an immediate login (this rarely actually happens :D).

SS1

Now on the dashboard, under recent activity we can see a message that reads:
`Internal pages hosted at /internal/admin.php. It contains the system flag`

This is valuable information as we now know where the flag is located.

SS2

If we visit the sub-page, we can see a message reading:
`This page can only be accessed locally`

SS3

At the bottom of the page, there is an "Export to PDF" button. When we try to export to PDF, we can see on the document that the file is generated on localhost.

Because of this, I decided to go for an intercept with **Burp Suite**.
We will intercept the request while requesting the pdf file.

SS4

When we do so, we are given the server-info url. We can modify this url to take us to the admin page that can only be accessed locally.

`url=http://127.0.0.1/internal/admin.php`

SS5

Now we can just forward to send the request and we are met with the flag.

SS6

