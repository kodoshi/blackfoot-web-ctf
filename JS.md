# JS Challenges

## b64js [https://b64js.secu-web.blackfoot.dev/]

### Description
Basic JavaScript functionality, decode encoded strings and retrieve the flags.
Going through the source code page, we find base64 encoded strings being checked against user input.

'''
    var pass = "RXZlckhlYXJkT2ZCYXNlNjQ/";
    var input = window.prompt("Enter the flag here : ");
    var b64input = window.btoa(unescape(encodeURIComponent(input)))
    if (pass === b64input)
	alert("Congrats ! You can validate this challenge with the flag BFS{" + input + "}");
    else
	alert("Call your mom and tell her you will be late for dinner ;)");
'''

### Solution

Basic b64 knowledge tells us to convert `RXZlckhlYXJkT2ZCYXNlNjQ` from base64 to ascii and we get the flag.


## js200 [https://js200.secu-web.blackfoot.dev/]

### Description
Similar premise to b64js, but this time we have to decode a string using a custom function.
Going through the source code page, we found this:

```
function validate() {
	var flag = "K@UC,bswslubr.wohp.dibokdmdb";
	var input = document.getElementById("flag").value
	var i = 0
	var out = ""
	do {
		out += String.fromCharCode(input.charCodeAt(i) ^ 1);
		out += String.fromCharCode(input.charCodeAt(i + 1) ^ 3);
		out += String.fromCharCode(input.charCodeAt(i + 2) ^ 3);
		out += String.fromCharCode(input.charCodeAt(i + 3) ^ 7);
		i += 4;
	} while (i < 28);
	if (out === flag) {
		alert("Congratz! Validate this challenge with the flag BFS{" + input + "}")
	} else {
		alert("Try again !")
	}
      }
```

### Solution

The js code present in the html was executed with the custom input `K@UC,bswslubr.wohp.dibokdmdb` which gave us the flag.

The validate() function processes the input string "JCVD-approves-this-challenge" using XOR operations with the keys 1, 3, 3, and 7:
After these XOR operations, the resulting string is "IFSA.Xsrqsl),rwajpdiefqd9".

When this resulting string is compared to the flag string "K@UC,bswslubr.wohp.dibokdmdb", we can see that they are equal if we shift every character of the flag string up by 1 ASCII value. So, the flag string XORed with the keys 2, 4, 4, and 8 would produce the same string as the result of XORing the input string with the keys 1, 3, 3, and 7.

Therefore, the input string "JCVD-approves-this-challenge" is valid, and when the validate() function is executed with this input, it will display an alert message with the flag.

#### Screenshots

#### Resources