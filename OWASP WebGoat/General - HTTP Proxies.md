
# 1

## What’s a HTTP Proxy

A proxy is some forwarder application that connects your http client to backend resources. HTTP clients can be browsers, or applications like curl, SOAP UI, Postman, etc. Usually these proxies are used for routing and getting access to internet when there is no direct connection to internet from the client itself. HTTP proxies are therefore also ideal when you are testing your application. You can always use the proxy log records to see what was actually sent from client to server. So you can check the request and response headers and the XML, JSON or other payload.

HTTP Proxies receive requests from a client and relay them. They also typically record them. They act as a man-in-the-middle. It even works fine with or without HTTPS as long as your client or browser trusts the certificate of the HTTP Proxy.

### ZAP Proxy Capabilities

With ZAP you can record traffic, inspect traffic, modify requests and response from and to your browser, and get reports on a range of known vulnerabilities that are detected by ZAP through the inspection of the traffic. The passive and active reporting on security issues is usually used in Continuous Delivery pipelines that use a GUI-less ZAP. Here we will use ZAP interactively and mainly to see and modify requests in order to find vulnerabilities and solve assignments. ZAP has a graphical user interface, but now also has a HUD Heads-On-Display which uses a websocket connection between the browser and the ZAP proxy.

### Next pages

You can go through all lesson pages or click on these links to skip some pages.

- [Configuring](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/1) OWASP ZAP and browser
    
- [Filtering](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/5) requests with ZAP
    
- [A proxy assignment](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/6) with ZAP
    
- [Replaying requests](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/7) with ZAP
    
- [Replaying requests](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/8) with Burp

# 2

## HTTP Proxy Setup

Since this is an OWASP project, we’ll be using OWASP ZAP. If you are comfortable using another proxy (e.g. Burp), you can skip this. Otherwise, this will show you how to set up ZAP to act as a proxy on your localhost.

### [Setting up ZAP 2.8.0](http://127.0.0.1:8080/WebGoat/start.mvc#lesson/HttpProxies.lesson/2)

- First download and install ZAP 2.8.0 for your operating system
    
- Start ZAP
    
- Configure the proxy to use a free port, e.g. 8090
    
- Start the browser directly from ZAP


# 3

## Setting up ZAP 2.8.0

1. First download and install ZAP 2.8.0 for your operating system
    
2. Start ZAP
    
3. Configure the proxy to use a free port, e.g. 8090
    

### Start ZAP

### Configure Proxy’s Port

- Select Tools > Options from the menu
    
- Select Local Proxy on the left
    
- Choose an available port …​ Since WebGoat is (or will be) using port 8080, use something different like 8090
    
- Click OK
    

In the options menu, you can also change the language. By default it is set with the language setting of your operating system. The examples are shown in English.

# 4

## Setting up browser

If you use the latest ZAP version (>= 2.8.0) you only need to start ZAP and click the browser button to be able to proxy, see image below:


In the browser type: [http://localhost:8080/WebGoat](http://localhost:8080/WebGoat) you should see WebGoat and the OWASP ZAP Heads On Display (if you use OWASP ZAP as the proxy):

You might notice that this is the dutch login screen. This is determined from the language settings from your browser. For some of the pages there will be some local translations. You can contribute to WebGoat and add more for your preferred language. You can disable the Heads On Display by clicking on the highlighted button. You can learn about the OWASP ZAP HUD on their website. For now it is recommended to disable it as it kind of blocks the menu items.

You should see the following in OWASP ZAP on the history panel:

On the next page we will show how you can filter these requests to see only relevant requests and how to configure the interceptor.

# 5

### Filter requests in history panel

In the main ZAP window click on Filter, see image below

Then in the `URL Inc Regex` box type:

```
.*WebGoat.*
```

And in the `URL Exc Regex` box type:

```
.*lesson.*.mvc
```

Click 'Apply to close the window, ZAP will now no longer show internal WebGoat requests.

# 6

### Configure a breakpoint filter

Before we start diving into intercepting requests with ZAP we need to exclude the internal requests from the WebGoat framework otherwise ZAP will also stop at all the requests which are only necessary for the internal working of WebGoat. Basically a breakpoint is configured that will intercept requests when the request header contains a POST. Which are the most interesting ones. You can add other rules as long as the polling .mvc messages will be excluded. As this would be annoying.

Set the breakpoint as follows:

You can see your active breakpoints here. And if you click on the checkbox you can also temporarily deactivate them and enable them again when you are just about to intercept the request. **DO NOT use the green/red button anymore**

Once you are intercepting requests and a request is made, it should look something like this:

### Intercept and modify a request

Set up the intercept as noted above and then submit the form/request below by clicking the submit button. When you request is intercepted (hits the breakpoint), modify it as follows.

- Change the Method to GET
    
- Add a header 'x-request-intercepted:true'
    
- Remove the request body and instead send 'changeMe' as query string parameter and set the value to 'Requests are tampered easily' (without the single quotes)
    

Then let the request continue through (by hitting the play button).

|   |   |
|---|---|
||The two play buttons behave a little differently, but we’ll let you tinker and figure that out for yourself.|

SS1

<img width="1133" height="784" alt="Screenshot 2026-02-15 170529" src="https://github.com/user-attachments/assets/2bc5760f-28dd-4665-9ab8-a7e09e6f4f9e" />


SS2

<img width="1252" height="823" alt="Screenshot 2026-02-15 170604" src="https://github.com/user-attachments/assets/95425923-7dcb-4438-ae1c-5918fe647afd" />


