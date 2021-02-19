**Modular Binomials**
```
N = p*q
c1 = (2*p + 3*q)**e1 mod N
c2 = (5*p + 7*q)**e2 mod N
```
```
N = pq \ c1 = (2p)^e1 + (3q)^e1 = (2p + 3q)^e1 (mod N) \ c2 = (5p)^e2 + (7q)^e2 = (5p + 7q)^e2 (mod N)  
x = c1^e2 = (2p + 3q)^(e1 e2) (mod N) \ y = c2^e1 = (5p + 7q)^(e1 e2) (mod N)  
a = 2^(e1 e2) (mod N) \ b = 3^(e1 e2) (mod N) \ c = 5^(e1 e2) (mod N) \ d = 7^(e1 e2) (mod N)  
gcd(cx - ay, N) = p \ gcd(dx - by, N) = q
```

**XorStarter**  
```
s = ''
for i in "label":
    s += chr(ord(i) ^ 13)
print(s)
```

**Base64**
```
import base64
m = input()
b = bytearray.fromhex(m)
result = base64.b64encode(b)
print(result)
```

**Diffie-HellmanStarter1**
```
def modulusInverse(a, m):
    xa = 1
    xm = 0
    while (m != 0):
        q = a // m
        xr = xa - q * xm
        xa = xm
        xm = xr
        r = a % m
        a = m
        m = r
    return xa

print(modulusInverse(209, 991))
```

**BytesAndBigIntergers**
```
from Crypto.Util.number import long_to_bytes

m = input()
result = long_to_bytes(m)
print(result)
```

<!-- **NaiveDecode**
```
import math 

def NaiveDecode(infile):
    M = [] # test design
    f = open(infile, 'r')
    temp = f.readline().split(' ')
    T = int(temp[0]) # number of tests
    n = int(temp[1]) # number of items
    columnIndex = (1 << n) - 1 # columnIndex = 0 if column is removed

    # read test design
    for i in range(0, T):
        temp = f.readline()
        temp = temp.split(' ')
        tempM = 0

        for j in range(0, len(temp) - 1):
            if temp[j] == '1':
                tempM = tempM ^ (1 << (len(temp) - j - 2)) # set ith bit
        M.append(tempM)

        if temp[-1].rstrip() == '0': # if outcome = 0
            for j in range(0, n):
                if M[i] & (1 << j) != 0: # if ith bit = 1
                    if columnIndex & (1 << j) != 0:
                        columnIndex = columnIndex ^ (1 << j)
    # remove column
    columnIndex = bin(columnIndex)
    columnIndex = columnIndex[2:].zfill(n)
    for i in range(0, len(M)):
        for j in range(0, n):
            if columnIndex[j] == '0':
                if M[i] & (1 << (n - j - 1)) != 0:
                    M[i] = M[i] ^ (1 << (n - j - 1))
    return M, n

if __name__ == "__main__":
    X, n = NaiveDecode("input.txt")
    for i in X:
        print(bin(i)[2:].zfill(8))

    # DD
    K = []
    for i in X:
        if i != 0 and math.ceil(math.log(i, 2)) == math.floor(math.log(i, 2)): # check if i is power of 2 or there is only one number 1 in a test
            K.append(n - int(math.log(i, 2)))
    print("DD = ", K)

     # COMP
    K = set()
    for i in X:
        if i != 0:
            for j in range(0, n):
                if i & (1 << j) != 0:
                    K.add(n - int(math.log(1 << j, 2)))
    K = list(K)
    print("COMP = ", K)
    pass
```

**Determinant**
```
def permutation(lst): 
    if len(lst) == 0: 
        return []
    if len(lst) == 1:
        return [lst]

    l = []
    for i in range(len(lst)): 
       m = lst[i] 
       remLst = lst[:i] + lst[i+1:] 
       for p in permutation(remLst): 
           l.append([m] + p) 
    return l 

def sgn(a, b):
    sgn = 1
    for i in range(0, len(a)):
        for j in range(i + 1, len(a)):
            sgn *= (a[i] - a[j])/(b[i] - b[j])
    return sgn

n = int(input("Input n: "))
data = [i for i in range(1, n+1)]
pers = permutation(data)

s = ""
for p in pers:
    r = sgn(data, p)
    if r < 0:
        s += "-"
    else:
        s += "+"
    for i in range(1, n + 1):
        s += "a" + str(i) + str(p[i - 1]) # because start at 0

print(s)
```

**LinearEquations**
```
import numpy as np

def changeRow(A, B, x, y):
    A[[x, y]] = A[[y, x]]
    B[[x, y]] = B[[y, x]]
    return A

def addRow(A, B, n, x, y, k): # Ax+k*Ay
    for i in range(0, n):
        A[x][i] = A[x][i] + k*A[y][i]
    B[x] = B[x] + k*B[y]
    return A

def mulRow(A, B, x, k): # k*Ax
    A[x] = k * A[x]
    B[x] = k * B[x]
    return A


def GaussJordan(A, B, m, n):
    i = 0
    j = 0
    while i < m and j < n:
        if A[i][j] != 0:
            A = mulRow(A, B, i, 1/A[i][j])
            for k in range(0, m):
                if k != i:
                    A = addRow(A, B, n, k, i, -A[k][j])
            i += 1
            j += 1
        else:
            k = m
            for k in range(i + 1, m):
                if A[k][j] != 0:
                    changeRow(A, B, i, k)
                    break
            if k == m:
                j += 1


m = int(input("Input number of equation: "))
n = int(input("Input number of variable: "))

A = []
B = []
inp =  input("Input A: ").split()
for i in inp:
    A.append(int(i))
inp =  input("Input B: ").split()
for i in inp:
    B.append(int(i))
A = np.array(A).reshape(m, n)
B = np.array(B)

GaussJordan(A, B, m, n)
if A[m - 1, n - 1] != 0:
    print("The system of equations has a unique solution: ")
else:
    if B[m - 1] == 0:
        print("The system of equations has countless solutions")
    else:
        print("The system of equations has no solutions")
print("A = ")
print(A)
print("B = ", B)
``` -->