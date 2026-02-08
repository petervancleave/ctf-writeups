
SS1


SS2

SS3

Q: Were the cookies the same on each tab?

A: yes

SS4

SS5

SS6

SS7

SS8

SS9


Q:

The goal of the assignment is to identify which field is susceptible to XSS.

It is always a good practice to validate all input on the server-side. XSS can occur when unvalidated user input gets used in an HTTP response. In a reflected XSS attack, an attacker can craft a URL with the attack script and post it to another website, email it, or otherwise get a victim to click on it.

An easy way to find out if a field is vulnerable to an XSS attack is to use the `alert()` or `console.log()` methods. Use one of them to find out which field is vulnerable.

A: 

enter your credit card number - `4128 3214 0002 1999"><script>alert(1)</script>`
enter your three digit access code -  `111`

SS10

SS11

SS12

SS13

SS14

Q: So, what is the route for the test code that stayed in the app during production? To answer this question, you have to check the JavaScript source.

A: `start.mvc#test/`

SS15

SS16

SS17

Q: Some attacks are "blind". Fortunately, you have the server running here so you will be able to tell if you are successful. Use the route you just found and see if you can use the fact that it reflects a parameter from the route without encoding to execute an internal function in WebGoat. The function you want to execute is …​

**webgoat.customjs.phoneHome()**

Sure, you could just use console/debug to trigger it, but you need to trigger it via a URL in a new tab.

Once you do trigger it, a subsequent response will come to your browser’s console with a random number. Put that random number in below.

A:
-1833978252


SS18

SS19


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

