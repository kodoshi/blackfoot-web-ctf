# OpenField challenge

## Wordstress [https://wordstress.secu-web.blackfoot.dev/]

### Description
An outdated Wordpress 4.2 installation that is vulnerable to multiple XSS and user enumeration attacks. The challenge is to exploit as many vulnerabilities as possible.

### Solutions
 - CVE 2015-3440, being an admin or contributor/author will let you exploit through stored xss, by adding comments with valid html tags (bypassing html sanitizers), but longer than 64kb so that it gets truncated by mysql, therefore it becomes malformed hmtl, leading to the exploit by commenting something like ``` <a title='x onmouseover=alert(unescape(/You%20are%20vulnerable/.source)) AA....[more chars to cross 64kb threshhold]```. The code embedded inside onmousover flag will keep running every time it is hovered over.

 - CVE-2009-2335, doing user enumeration through recognizing that earlier version of wordpress login return different errors if an account exists and just the password is wrong, or if the username doesnt exist at all.
 
 - User enumeration can be also done by iterating through the ids of the 'author' flag in the link `https://wordstress.secu-web.blackfoot.dev/?author=<author id>`, Wordpress has a generic system of redirecting to the 'alias' of this id, thus giving us direct access to the name of the author(their auth username) in plain view.

 - CWE-548, Delete Plugin Path Traversal/Arbitrary File Deletion: As Admin, by going to the Plugin management system, we have options to delete entries, which takes us to a screen with a special button that lets us visualize which physical files will be deleted from the hard drive as a result of removing the plugin. This mechanism can be exploited by understanding that the file path is given in the url of the page, thus changed easily from something like plugin_file.php to `./../../../../`. The full example would be `/wp-admin/plugins.php?action=delete-selected&checked%5B0%5D=<WHATEVER_FILES_WE_WANT_HERE>&plugin_status=all&paged=1&s&_wpnonce=25c604f1f7`

#### Screenshots
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_1.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_2.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_3.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_4.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_5.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_6.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_7.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/openfield_8.png?raw=true)

#### Resources
 - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-3440
 - https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2009-2335
 - https://cwe.mitre.org/data/definitions/548.html
 - wp-scan
