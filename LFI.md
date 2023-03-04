# LFI

## LFI1 [https://noprotection.secu-web.blackfoot.dev]

### Description

This challenge is a simple LFI challenge. The server is vulnerable to LFI, and the flag is in the root directory.

### Solution
It can be accessed by switching the french/english language template parameter to `flag.html`, and the flag is displayed.
`https://noprotection.secu-web.blackfoot.dev/?lang=flag.html`

## LFI2 [https://lfi2.secu-web.blackfoot.dev]

### Description

This challenge is a simple LFI challenge. The server is vulnerable to LFI, and the flag is in the accessible directory.

### Solution

Here we would have to use null byte `%00` to bypass the .php suffix file filter, but still cant access the flag.
#### Screenshots

#### Resources
 - https://blog.clever-age.com/fr/2014/10/21/owasp-local-remote-file-inclusion-lfi-rfi/