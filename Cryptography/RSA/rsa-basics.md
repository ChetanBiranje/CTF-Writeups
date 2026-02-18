# RSA Basics

## Challenge Information

- **Platform**: picoCTF
- **Category**: Cryptography
- **Difficulty**: Easy
- **Points**: 100

## Description

Decrypt the RSA encrypted message. You are given:
- Public key (n, e)
- Ciphertext (c)

Files:
- `public.pem`
- `ciphertext.txt`

## Solution

### Initial Analysis

Examined the public key:
```bash
openssl rsa -pubin -in public.pem -text -noout
```

Found:
- n = 2159...  (small modulus!)
- e = 65537

### Approach

Since n is small, we can factor it and recover the private key.

### Step-by-Step

**1. Extract n and e from public key**
```python
from Crypto.PublicKey import RSA

with open('public.pem', 'r') as f:
    key = RSA.import_key(f.read())
    
n = key.n
e = key.e
print(f"n = {n}")
print(f"e = {e}")
```

**2. Factor n**
```python
import sympy

# For small n, factorization is quick
p = sympy.factorint(n)
print(p)  # {p1: 1, p2: 1}
```

Got factors: p and q

**3. Calculate private key**
```python
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)  # Modular inverse
```

**4. Decrypt ciphertext**
```python
with open('ciphertext.txt', 'r') as f:
    c = int(f.read())

m = pow(c, d, n)  # m = c^d mod n
flag = bytes.fromhex(hex(m)[2:]).decode()
print(flag)
```

### Flag

```
picoCTF{small_n_3asy_f4ct0r}
```

## Lessons Learned

- RSA security depends on large, hard-to-factor moduli
- Small key sizes can be factored quickly
- Python's `pow()` with 3 arguments is efficient for modular exponentiation
- Always check key size before attempting factorization

## Tools Used

- Python
- PyCryptodome
- SymPy (factorization)
- OpenSSL

## References

- [RSA Algorithm](https://en.wikipedia.org/wiki/RSA_(cryptosystem))
- [RSA CTF Tool](https://github.com/Ganapati/RsaCtfTool)

## Tags

`rsa` `cryptography` `factorization` `picoctf` `easy`
