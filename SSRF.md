# SSRF challenges

## SSRF1 [https://ssrf1.secu-web.blackfoot.dev/]

### Description
Server-side request forgery (also known as SSRF) is a web security vulnerability that allows an attacker to induce the server-side application to make requests to an unintended location. This can allow the attacker to interact with any internal system that the server can access, including systems on the internal network that are not otherwise accessible to the attacker.

By lurking around the website, we can see a `GOSESSION` property always attached to the request body.

The page invites us to go to the secret page, but we need to have a specific token to get the flag, otherwise we get messages like `{"ok":false,"message":"You have to come from 127.0.0.1 not 172.21.0.11 :)","flag":""}` or `{"ok":false,"message":"Missing GOSESSION ... You are not connected... get away !","flag":""}`.

### Solution

Through some OSINT, some basic knowledge of Golang, and ssrf tutorials, I found similar CTF challenges that attack this known vulnerability [https://github.com/golang/go/issues/30794].

By adding `"%20HTTP/1.1%0ACookie:GOSESSION="` and going to the secret page, we get a JWT token and by simply decoding it, we get the needed flag.`https://ssrf1.secu-web.blackfoot.dev/host?host=localhost/secret?xx=xx%20HTTP/1.1%0ACookie:GOSESSION=`


#### Screenshots

#### Resources
 - https://www.synacktiv.com/en/publications/fic2020-prequals-ctf-write-up.html
 - https://github.com/golang/go/issues/30794
