
SS1
<img width="1494" height="477" alt="Screenshot 2026-02-07 183813" src="https://github.com/user-attachments/assets/dcd77020-3780-4193-8e9c-cfd70bea4c2a" />


SS2
<img width="1474" height="749" alt="Screenshot 2026-02-07 184038" src="https://github.com/user-attachments/assets/3b12b002-0db5-40b4-9255-422e36003311" />

SS3
<img width="1809" height="831" alt="Screenshot 2026-02-07 184341" src="https://github.com/user-attachments/assets/79a96dbb-8494-42d4-b37e-a9b967849195" />

Q: Were the cookies the same on each tab?

A: yes

SS4
<img width="1523" height="758" alt="Screenshot 2026-02-07 184412" src="https://github.com/user-attachments/assets/08a78cc8-0bbd-44eb-a91d-1e238e48a0a0" />

SS5
<img width="1534" height="427" alt="Screenshot 2026-02-07 184508" src="https://github.com/user-attachments/assets/afb3ff49-0380-4ff9-8753-5cbff2377010" />

SS6
<img width="1536" height="600" alt="Screenshot 2026-02-07 184518" src="https://github.com/user-attachments/assets/92207c9d-97dd-4d2d-bfe2-6e07fdb93e2c" />

SS7
<img width="1528" height="600" alt="Screenshot 2026-02-07 184536" src="https://github.com/user-attachments/assets/8258f1a0-fd1e-4da8-a969-c943a92ae0ca" />

SS8
<img width="1534" height="691" alt="Screenshot 2026-02-07 184544" src="https://github.com/user-attachments/assets/49cb81dd-9452-4aa9-88b3-3af7391889ff" />

SS9
<img width="1527" height="650" alt="Screenshot 2026-02-07 184606" src="https://github.com/user-attachments/assets/a1805102-80eb-4276-af23-a6f184c42b04" />


Q:

The goal of the assignment is to identify which field is susceptible to XSS.

It is always a good practice to validate all input on the server-side. XSS can occur when unvalidated user input gets used in an HTTP response. In a reflected XSS attack, an attacker can craft a URL with the attack script and post it to another website, email it, or otherwise get a victim to click on it.

An easy way to find out if a field is vulnerable to an XSS attack is to use the `alert()` or `console.log()` methods. Use one of them to find out which field is vulnerable.

A: 

enter your credit card number - `4128 3214 0002 1999"><script>alert(1)</script>`
enter your three digit access code -  `111`

SS10
<img width="1613" height="830" alt="Screenshot 2026-02-07 185216" src="https://github.com/user-attachments/assets/2d8a81c8-9f42-4ef7-a852-221fd64acf14" />

SS11
<img width="1534" height="744" alt="Screenshot 2026-02-07 185236" src="https://github.com/user-attachments/assets/90d0df41-e050-46b3-9d79-fe5d21c6b547" />

SS12
<img width="1555" height="340" alt="Screenshot 2026-02-07 185331" src="https://github.com/user-attachments/assets/a8576655-2e72-411e-8041-d3fe4175d750" />

SS13
<img width="1531" height="446" alt="Screenshot 2026-02-07 185338" src="https://github.com/user-attachments/assets/88e77c45-4945-409a-b4cf-4c65e086ed38" />

SS14
<img width="1552" height="370" alt="Screenshot 2026-02-07 185347" src="https://github.com/user-attachments/assets/1bb5bc80-baf0-42da-ae89-d0e2cbb98132" />

Q: So, what is the route for the test code that stayed in the app during production? To answer this question, you have to check the JavaScript source.

A: `start.mvc#test/`

SS15
<img width="1835" height="897" alt="Screenshot 2026-02-07 185748" src="https://github.com/user-attachments/assets/2fd887f1-6064-426b-ba0a-dc80e884f8a9" />

SS16
<img width="1567" height="567" alt="Screenshot 2026-02-07 190202" src="https://github.com/user-attachments/assets/dfa6920d-77ae-4186-a6df-4fc3e04a5538" />

SS17
<img width="1539" height="373" alt="Screenshot 2026-02-07 190252" src="https://github.com/user-attachments/assets/76b39824-703e-49c0-922f-8211a99408f0" />

Q: Some attacks are "blind". Fortunately, you have the server running here so you will be able to tell if you are successful. Use the route you just found and see if you can use the fact that it reflects a parameter from the route without encoding to execute an internal function in WebGoat. The function you want to execute is …​

**webgoat.customjs.phoneHome()**

Sure, you could just use console/debug to trigger it, but you need to trigger it via a URL in a new tab.

Once you do trigger it, a subsequent response will come to your browser’s console with a random number. Put that random number in below.

A:
-1833978252


SS18
<img width="1173" height="563" alt="Screenshot 2026-02-07 190403" src="https://github.com/user-attachments/assets/5c9f7832-9960-40f6-8beb-91ee70b6014a" />

SS19
<img width="1845" height="950" alt="Screenshot 2026-02-07 194407" src="https://github.com/user-attachments/assets/969f4590-0374-4355-afde-86aff065b718" />

<img width="1529" height="588" alt="Screenshot 2026-02-07 194449" src="https://github.com/user-attachments/assets/b00d6aea-1adc-456d-a5a8-bed2210a8910" />



### Quiz:

1. Are trusted websites immune to XSS attacks?

Solution 1: Yes they are safe because the browser checks the code before executing.  
Solution 2: Yes because Google has got an algorithm that blocks malicious code.  
Solution 3: No because the script that is executed will break through the defense algorithm of the browser.  
**Solution 4: No because the browser trusts the website if it is acknowledged trusted, then the browser does not know that the script is malicious.**  

2. When do XSS attacks occur?

Solution 1: Data enters a web application through a trusted source.  
Solution 2: Data enters a browser application through the website.  
**Solution 3: The data is included in dynamic content that is sent to a web user without being validated for malicious content.**  
Solution 4: The data is excluded in static content that way it is sent without being validated.  

3. What are Stored XSS attacks?

**Solution 1: The script is permanently stored on the server and the victim gets the malicious script when requesting information from the server.**  
Solution 2: The script stores itself on the computer of the victim and executes locally the malicious code.  
Solution 3: The script stores a virus on the computer of the victim. The attacker can perform various actions now.  
Solution 4: The script is stored in the browser and sends information to the attacker.  

4. What are Reflected XSS attacks?

Solution 1: Reflected attacks reflect malicious code from the database to the web server and then reflect it back to the user.  
**Solution 2: They reflect the injected script off the web server. That occurs when input sent to the web server is part of the request.**  
Solution 3: Reflected attacks reflect from the firewall off to the database where the user requests information from.  
Solution 4: Reflected XSS is an attack where the injected script is reflected off the database and web server to the user.  

5. Is JavaScript the only way to perform XSS attacks?

Solution 1: Yes you can only make use of tags through JavaScript.  
Solution 2: Yes otherwise you cannot steal cookies.  
Solution 3: No there is ECMAScript too.  
**Solution 4: No there are many other ways. Like HTML, Flash or any other type of code that the browser executes.**

SS20
<img width="1489" height="808" alt="Screenshot 2026-02-07 194647" src="https://github.com/user-attachments/assets/13e3b269-f735-4339-a672-467491b9e800" />

