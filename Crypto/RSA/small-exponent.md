# RSA - Small Exponent Attack

## Challenge Information

- **Platform**: CTFtime
- **Category**: Crypto
- **Difficulty**: Medium
- **Points**: 200
- **Author**: CryptoMaster
- **Date Solved**: 2024-02-10

## Description

```
We intercepted an RSA encrypted message. 
The public key has a small exponent e=3.
Can you decrypt it?

n = 1234567890123456789...
e = 3
c = 9876543210987654321...
```

## Tools Used

- Python
- gmpy2 library
- RsaCtfTool

## Solution

### Step 1: Understand the Vulnerability

When RSA uses a small exponent (e=3) and the message m is small such that m³ < n, 
we can simply take the cube root of the ciphertext to get the plaintext.

### Step 2: Check if m³ < n

Since e=3, check if the message is small enough:
```python
import gmpy2

# If c = m^3 and m^3 < n
# Then we can just take cube root
```

### Step 3: Decrypt

```python
import gmpy2

n = 1234567890123456789  # Given
e = 3
c = 9876543210987654321  # Given

# Take cube root of ciphertext
m, exact = gmpy2.iroot(c, 3)

if exact:
    print(f"Message: {m}")
    # Convert to bytes
    flag = bytes.fromhex(hex(m)[2:])
    print(f"Flag: {flag.decode()}")
else:
    print("Not exact cube root, try other methods")
```

### Step 4: Get Flag

```python
Flag: CTF{small_exponent_weak_rsa}
```

## Alternative Solutions

**Method 2: If m³ > n**

Use Håstad's Broadcast Attack if you have multiple ciphertexts encrypted 
with the same small exponent.

## Key Learnings

- Small RSA exponents (e=3) are vulnerable if message is small
- Always use proper padding (OAEP)
- Large exponents (e=65537) are recommended
- Never encrypt the same message with different moduli and same e

## Common Mistakes

- Forgetting to check if m³ < n
- Not converting integer to bytes properly
- Using wrong encoding (hex vs utf-8)

## References

- [RSA Small Exponent Attack](https://crypto.stackexchange.com/questions/6770/small-rsa-public-exponent)
- [RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)

## Tags

`rsa` `small-exponent` `crypto` `medium`

---

**Solved by**: Chetan Biranje  
**Date**: 2024-02-10  
**Time Taken**: 1.5 hours
