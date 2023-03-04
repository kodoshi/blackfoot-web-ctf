# SQLi challenges

## Potionseller [https://potionseller.secu-web.blackfoot.dev/]

### Description

SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. SQLi is mostly known as an attack vector for websites but can be used to attack any type of SQL database. It often allows an attacker to view data that they are not normally able to retrieve. It may also allow the attacker to modify arbitrary data, to cause repudiation issues such as voiding transactions or changing balances, to escalate privileges, or to recover the content of a given file present on the database server file system. This is not a modern attack vector, since most applications use prepared statements or abstractions like ORMs to prevent SQL injection, but it is a core attack technique that every web developer must understand.


### Solution
Regarding the Potionseller website, we can see that the website seems vulnerable to SQLi, by inserting code into url queries like `https://potionseller.secu-web.blackfoot.dev/potions/6+or+4`, 
but there seem to be filters in place to prevent us from using the most common payloads. Even though we tried differently crafted payloads, we couldn't get the flag for some reason. More details in the screenshots below.


#### Screenshots
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/sqli_1.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/sqli_2.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/sqli_3.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/sqli_4.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/sqli_5.png?raw=true)

#### Resources
 - https://portswigger.net/web-security/sql-injection/cheat-sheet
 - https://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet