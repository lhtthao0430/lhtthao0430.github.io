**[Source of All Evil](http://167.71.246.232/)**  
View soure, we got the flag
```
flag{best_implants_ever}
```
**[Headers for you inspiration]((http://167.71.246.232/)**  
Using burp suite to view header, we got the flag
```
flag{headersftw}
```

**[Stay Away Creepy Crawlers](http://167.71.246.232/)**  
Check http://167.71.246.232:8080/robots.txt and we got the flag
```
flag{mr_roboto}
```
**[Show me what you got](http://167.71.246.232/)**  
View source, it has a img src
```html
<img src='/images/doc_jones.png'>
```
Check images folder, we got the flag in http://167.71.246.232/images/aljdi3sd.txt
```
flag{disable_directory_indexes}
```

**[Can't find it](http://167.71.246.232/)**  
Just go somewhere Page not found and we got
```
flag{404_oh_no}
```

**[Ripper Doc](http://167.71.246.232/)**  
Go to first link in menu http://167.71.246.232/certified_rippers.php  
We have a hint
```
Sorry, gotta be in the club to get this list.
```
Check cookie and change <i>authenticated</i> to True, we got the flag
```
flag{messing_with_cookies}
```

**[Follow The Rabbit Hole](http://167.71.246.232/)**  
Write the code to follow page
```python
import requests

url = 'http://167.71.246.232:8080/rabbit_hole.php?page='
code = 'cE4g5bWZtYCuovEgYSO1'
i = 0;
while i < 1:
    x = requests.get(url+code)
    code = x.text.split(' ')[2]
    print(x.text)
    i = i + 1
```
Notice that i = 1582, this will end, and the first parameter is less than 1582, so assume it is a index, and the second parameter is a value
```python
import requests

url = 'http://167.71.246.232:8080/rabbit_hole.php?page='
code = 'cE4g5bWZtYCuovEgYSO1'
i = 0;
a = ['0' for i in range(1582)]
while i < 15:
    x = requests.get(url+code)
    code = x.text.split(' ')[2]
    para1 = int(x.text.split(', ')[0].split('[')[1])
    para2 = x.text.split(', \'')[1].split('\']')[0]
    a[para1] = para2
    i = i + 1
s = ''
for c in a:
    s = s + c
print(s)
```
It's a png image  
![img](https://lhtthao0430.github.io/web/tenable/FollowTheRabbitHole.png)  
We got
```
flag{automation_is_handy}
```