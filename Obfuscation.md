# Obfuscation challenges

## OBF100 [https://obf100.secu-web.blackfoot.dev/]

### Description
Another user input field that needs a valid input from us to return the flag.
By navigate to view-source:https://obf100.secu-web.blackfoot.dev/ we can see the page source code.
The first thing we see is a list of obfuscated strings that logically also hold the flag.

### Solution
By copy pasting the code into a generic deobfuscator, we have human-readable code.
We can see a deobfuscated value of flag now, ggez you might say.

## SCRIPT_KIDDING [https://script-kidding.secu-web.blackfoot.dev/]

### Description
It seems that this page has been hacked and a malicious script has been injected, but even the original exploiter doesn't remember how to operate it.
The goal here was to catch the script then reverse it and use it to get the flag.
The exploiter wants us to see the code. At a quick glance, it seems like a malicious code injection script designed to be executed on a PHP environment, like Wordpress. There are mentions of it over github threads as something to look out for, warning legitimate website admins.
By navigate to this page https://script-kidding.secu-web.blackfoot.dev/?showbackdoor we can get the code executed. 

By adding the code line `echo(gzinflate(base64_decode($a)))`, we get a friendlier output.

The first few lines set some PHP configuration settings, such as disabling error logging and setting the maximum execution time to unlimited.

The script then loops through all the cookies that have been set, decoding them and removing some specific characters using the remove_letter() function.

If the script doesn't find any approvals in the cookies, it loops through all the POST data, decoding it and removing some specific characters using the same remove_letter() function.

The script then checks if a specific parameter 'ak' exists in the approvals array. It also evaluates the value of the 'a' parameter and does either of the following:

If the value of the 'a' parameter is 'i', the script creates an array with some information about the PHP version and outputs the serialized version of this array using the serialize() function.
If the value of the 'a' parameter is 'e', the script evaluates the 'd' parameter as PHP code using the eval() function.

### Solution
Running the code locally for debugging and Along with the public information describing this piece of malware, we know that there are POST or cookie parameters that we can use to get the flag.
Exploiting the cookie path, The name of the cookie to set `4ef63abe-1abd-45a6-913d-6fb99657e24b` and the value is the json with the ak, a and d parameters, along with the custom payload to run.
The payload is a base64 encoded string that is decoded and then executed.
```var_dump(exec('cat * | xargs'));```

```
Cookie name : Cookie payload value
4ef63abe-1abd-45a6-913d-6fb99657e24b : YTozOntzOjI6ImFrIjtzOjE6ImkiO3M6MToiYSI7czoxOiJlIjtzOjE6ImQiO3M6MzI6InZhcl9kdW1wKGV4ZWMoJ2NhdCAqIHwgeGFyZ3MnKSk7Ijt9
```
The flag is now in the response.

#### Screenshots

#### Resources
 - https://security.stackexchange.com/questions/184152/trying-to-understand-what-this-php-malware-does




