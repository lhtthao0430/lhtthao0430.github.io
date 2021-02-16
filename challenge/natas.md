**Natas0**  
http://natas0.natas.labs.overthewire.org  
```
username: natas0
password: natas0
```

**Natas01**  
http://natas1.natas.labs.overthewire.org  
Ctrl+U to view source code
```
username: natas1
password: gtVrDuiDfck831PqWsLEZy5gyDz1clto 
```

**Natas02**  
http://natas2.natas.labs.overthewire.org  
Ctrl+U to view source code
```
username: natas2
password: ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi 
```

**Natas03**  
http://natas3.natas.labs.overthewire.org  
Ctrl+U to view source code  
In line 15, we have tag img src="files/pixel.png"  
Check http://natas2.natas.labs.overthewire.org/files/ we got users.txt
```
username: natas3
password: sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14 
```

**Natas03**  
http://natas3.natas.labs.overthewire.org  
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

**Natas04**  
http://natas3.natas.labs.overthewire.org  
Ctrl+U to view source code  
We got hint 
```
Access disallowed. You are visiting from "" while authorized users should come only from "http://natas5.natas.labs.overthewire.org/"
```
Using Burp suite, change to http://natas5.natas.labs.overthewire.org/
```
username: natas5
password: Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ 
```
