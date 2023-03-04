# JWT challenges

## Mythique1 [https://mythique1.secu-web.blackfoot.dev/]

### Description

This challenge is a simple JWT exploitation CTF. The goal is to login as the admin user.

### Solution

We can easily extract the JWT from the requests being sent back and forth between the server and we as a client.
By inspecting the JWT, we can see that the isAdmin field is set to false, and we need to set it to true to get the flag.
Replaying an old request with this modified JWT will give us the flag, through Chrome DevTools or BurpSuite.

## Mythique2 [https://mythique2.secu-web.blackfoot.dev/]

### Description
Another JWT challenge, same goal as the previous one.
It was obvious that the token was signed with a private key and that the public key was given on the page, as a gesture by the admin to 'verify' their identity.
Somehow we have to leverage the public key to sign our own custom token, even though assymetric encryption doesn't work that way.

### Solution
On a previous CTF I encountered buggy Node.js code that signed tokens assymetrically with RS256, but then proceeeded to verify them using both RS256 and HS256. Since HS256 is symmetric, we can do our own jwt.sign() and use the public key as the secret to sign our own token, and change the isAdmin field to true.
With this modified token, we can replay an old request and get the flag.

#### Screenshots

#### Resources
