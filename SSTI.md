# SSTI challenges

## SSTI1 [https://ssti1.secu-web.blackfoot.dev/]

### Description

Server-side template injection (SSTI) is a web security vulnerability that occurs when user input is embedded in a server-side template. The attacker can use this vulnerability to execute arbitrary code on the server.

### Solution

By using a tool like tplmap that does most of the legwork for us, we can find the vulnerability and exploit it to get a remote shell.

```console
python3 tplmap.py -u 'https://ssti1.secu-web.blackfoot.dev/?username=myg' --os-shell
```

Different commands can be inputted to the shell, such as ```cat app.py``` to get this specific flag.


## SSTI2 [https://ssti2.secu-web.blackfoot.dev/]

### Solution
Same solution as SSTI1, but with a different flag location, ```cat config.py```.


## SSTI3 [https://ssti3.secu-web.blackfoot.dev/]

### Solution
Same solution as SSTI1 and SSTI2, but with a different flag location, ```./getFlag.py```.

## SSTI4 [https://ssti4.secu-web.blackfoot.dev/]

### Description

This challenge is a bit more complex than the previous ones. We can see that the website is using a template engine called Jinja2, which is vulnerable to SSTI.
Following advanced exploitation techniques, we tried to exploit jinja2 templates to upload a full payload that would allow revshell and rce.
Multiple payloads were tried, but none of them worked. I even provisioned some AWS resources for a public-facing Kali VM, since so far I tried to port-forward myself with ngrok, but it didn't work either.

#### Screenshots
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_1.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_2.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_3.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_4.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_5.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/ssti_6.png?raw=true)

#### Resources
 - https://github.com/epinna/tplmap
 - https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection
 - https://www.onsecurity.io/blog/server-side-template-injection-with-jinja2/