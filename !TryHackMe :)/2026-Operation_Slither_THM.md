# Operation Slither 

<img width="900" height="900" alt="image" src="https://github.com/user-attachments/assets/a556decb-a817-4e7c-8acc-170476c2b9c6" />


[room here](https://tryhackme.com/room/operationslitherIU)

---

### Task 1 - The Leader

<img width="1256" height="502" alt="image" src="https://github.com/user-attachments/assets/faf67fa3-8cb2-42de-9431-eca6bae1ada5" />

The first question to answer is: 
Aside from Twitter / X, what other platform is used by v3n0mbyt3_? Answer in lowercase.

After a simple online search, we see the username v3n0mbyt3_ is active on twitter and threads.

<img width="633" height="816" alt="image" src="https://github.com/user-attachments/assets/8c1b59c4-894b-4638-ac46-c5cd878c6f9c" />

A: **Threads**

The next question to answer is: What is the value of the flag?

To solve this, we should look around within the threads profile. No threads revealed anything, but looking in the replies we see a Base64 encoded string

<img width="626" height="64" alt="image" src="https://github.com/user-attachments/assets/3330f22e-1056-4b4a-9da9-03fcbb59b136" />

<img width="522" height="145" alt="image" src="https://github.com/user-attachments/assets/9bd9ec7f-987c-49bb-8af5-e21249a43c7d" />

The string is: VEhNe3NsMXRoM3J5X3R3MzN0el80bmRfbDM0a3lfcjNwbDEzcyF9

We can feed this to CyberChef for the output of the string

<img width="1285" height="602" alt="image" src="https://github.com/user-attachments/assets/6919826f-fcde-47b7-baa0-7a5beb376e09" />

---

### Task 2 - The Sidekick

<img width="1258" height="526" alt="image" src="https://github.com/user-attachments/assets/d37cc5f3-6925-4509-932b-bd3bf277b8dd" />

The first question of the task is: What is the username of the second operator talking to v3n0mbyt3 from the previous platform?

We already know this from the previous step, the person replying was ` _myst1cv1x3n_ `

Now we need another flag value, and this time it will be related to `_myst1cv1x3n_`

If we go to their instagram, we see 5 posts, look around in these...

<img width="1573" height="766" alt="image" src="https://github.com/user-attachments/assets/ba6d5ccc-37c2-4ec3-b40a-a3249b8b98ca" />

In the video post, we see a link to a soundcloud page:

<img width="980" height="855" alt="image" src="https://github.com/user-attachments/assets/e89323ef-ffc3-4501-8c76-63aa464921d3" />

It takes use to a song "Prototype1" which doesnt help us, but looking around on the profile, there is another Base64 encoded string in the description of the song "Prototype2"

<img width="877" height="678" alt="image" src="https://github.com/user-attachments/assets/d27cdb07-8de1-4f26-825d-54e33dc564ce" />

The string is: VEhNe3MwY20xbnRfMDBwczNjX2Yxbmczcl9tMXNjbDFja30=

Again, give it to cyberchef, and it reveals the flag

<img width="1282" height="640" alt="image" src="https://github.com/user-attachments/assets/9025e754-bf2a-4cf2-9ab5-ac62449d7cdb" />

---

### Task 3 - The Last Operator

Finding a lead on this one was a bit trickier, the result is that the third person of interest is found in the likes of the song we were just looking at. The name is "sh4d0wF4NG" 

I figured this was worth looking into because of the similarly AI generated profile photo along with the similar wolf inspired type of name the others had.

<img width="1363" height="773" alt="image" src="https://github.com/user-attachments/assets/84b0244b-46e7-473f-a883-8bf363eb13cd" />

I put the name into **idcrawl.com**   <------- great site!

There were profiles for soundcloud, roblox, and Github.

So now we have the answers to the first two questions of this task -

Q: What is the handle of the third operator?

A: **sh4d0wF4NG**

Q: What other platform does the third operator use? Answer in lowercase.

A: **github**


Now, we just have to find a flag value related to this person, and the obvious place to look is on Github as this is where many flags are found.

This is the profile:

<img width="1433" height="701" alt="image" src="https://github.com/user-attachments/assets/5818af72-973e-4277-8dcf-bb3857c7e03b" />

The repo of interest is red-team-infra since it is not forked.

<img width="1395" height="595" alt="image" src="https://github.com/user-attachments/assets/e9d85ee1-7e83-46eb-8ead-af2a988c1d2d" />

I looked through all of the plain files but nothing stuck out, I assume we are looking for more Base64.

Its always good to look through commit history on github for these types of challenges, the idea is that credentials or flags can be left behind in commit history for people to see.

A convenient way to look through everything is to take the link of the repo: https://github.com/sh4d0wF4NG/red-team-infra

and append a /commits to it:
https://github.com/sh4d0wF4NG/red-team-infra/commits

<img width="1715" height="898" alt="image" src="https://github.com/user-attachments/assets/eca36092-53aa-40c2-a2dd-2d1ce253fa7c" />

So now we can see all of the commit history in order.

Scrolling down and looking through the commits, I found Base64 in terraform.tfstate

<img width="1343" height="581" alt="image" src="https://github.com/user-attachments/assets/5979e5dc-78b2-46f9-816e-bf24d40a142e" />

The Base64 is under the value for shadow-password: VEhNe3NoNHJwX2Y0bmd6X2wzNGszZF9ibDAwZHlfcHd9

Again, give it to cyberchef

<img width="1188" height="656" alt="image" src="https://github.com/user-attachments/assets/15e32bba-83e5-495d-8dab-14de735265c7" />

Now we have the final flag value!
