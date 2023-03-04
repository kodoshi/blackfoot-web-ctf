# Auth Challenges

## Auth50 [https://auth50.secu-web.blackfoot.dev/]

### Description

This challenge is a simple authentication challenge. The goal is to login as the admin user.
By inspecting the source code, most of the auth logic is obvious.

```
if (isset($_POST['username']) && isset($_POST['password']))
{
    $username = htmlspecialchars(trim($_POST['username']));
    $password = htmlspecialchars(trim($_POST['password']));
    if (!empty($username) && !empty($password)) {
        if ($username === $g_username) {
            if ($password === $g_password) {
                $message = "Congratz for this great guessing challenge ! $g_flag";
            } else $message = "At least you got the username  ( $g_username )";
        } else $message = 'You are far away from the flag !';
    }
}
```

### Solution

The solution is to simply guess the combo 'admin:password' and get the flag.


## Auth100 [https://auth100.secu-web.blackfoot.dev/]

### Description

Same as the previous challenge, we start by inspecting the source code of the page. it seems that the auth logic is very similar as the previous challenge.

```
if (isset($_POST['username']) && isset($_POST['password']))
{
     $username = htmlspecialchars(trim($_POST['username']));
     $password = htmlspecialchars(trim($_POST['password']));
     if (!empty($username) && !empty($password) && strlen($username) > 5)
     {
         $sum = 0;
         for ($i = 0; $i < strlen($username); ++$i)
             $sum += ord($username[$i]); // 1nTr3sT1nG, 15n't 1t?
         if ($password === strval($sum)) {
             $message = 'Congratulations! Validate with this flag: '.$g_flag;
         } else $message = 'Bad Password! Remember that the password is generated with the username! Like a KeygenMe!';
     } else $message = "Username have to be 6 chars at least";
}
```

### Solution

Each character of the `username` query field in a POST request, gets turned into ascii code and added to a $sum variable.
The password is then compared to the sum of the ascii codes of the username.

We can do our own $sum count through the following code, and found the combo '1nTr3sT1nG' : 837  that took us to the flag.

```
console.log("1nTr3sT1nG".charCodeAt(0) + "1nTr3sT1nG".charCodeAt(1) + "1nTr3sT1nG".charCodeAt(2) + "1nTr3sT1nG".charCodeAt(3) + "1nTr3sT1nG".charCodeAt(4) + "1nTr3sT1nG".charCodeAt(5) + "1nTr3sT1nG".charCodeAt(6) + "1nTr3sT1nG".charCodeAt(7) + "1nTr3sT1nG".charCodeAt(8) + "1nTr3sT1nG".charCodeAt(9) + "1nTr3sT1nG".charCodeAt(10)) = 837
```

## Auth200 [https://auth200.secu-web.blackfoot.dev/]

### Description

By inspecting the source code that is referenced in the Auth200 page, we find an `XOR` string operation on our input and a static key `Th1s_1s_@_x0r_k3y_l0l!`, which then checks if the result is equal to `3c094317451c232d195319026c1e2a09431f7e38075d527e1052`.

```
if (isset($_POST['flag']))
{
    $flag = htmlspecialchars(trim($_POST['flag']));

    $key = "Th1s_1s_@_x0r_k3y_l0l!";
    $encrypted_flag = "";
    if (!empty($flag))
    {
        for ($i = 0; $i < strlen($flag) ; ++$i) {
            $encrypted_flag .= chr(ord($flag[$i]) ^ ord($key[$i % strlen($key)]));
        }
        if (bin2hex($encrypted_flag) === "3c09431700451c00232d19531900026c1e2a09431f7e38075d527e1052")
            $message = "W0w ! Good J0b !! Let's say to SakiiR that u broke this one, show him your keygen ! Oh, I almost forgotten to give u the flag ! ".$g_flag;
        else
            $message = "Arf, It is far from the reality : ".bin2hex($encrypted_flag);
    }
}
```
### Solution

By reversing the XOR operation (`3c09431745...` into `Th1s_1s_@_x0r_k3y_l0l!`), we can get the input that will give us the flag (hard_to_crack_i_guess_lol!!!!).


#### Screenshots

![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/auth_50.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/auth_100.png?raw=true)
![alt text](https://github.com/kodoshi/blackfoot-web-ctf/blob/main/images/auth_200.png?raw=true)


#### Resources
