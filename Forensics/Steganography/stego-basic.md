# Basic Steganography

## Challenge Information

- **Platform**: TryHackMe
- **Category**: Forensics - Steganography
- **Difficulty**: Easy
- **Points**: 30

## Description

Find the hidden message in the image.

Provided: `challenge.png`

## Solution

### Initial Analysis

Examined the image visually - looks normal.

Checked basic properties:
```bash
file challenge.png
# PNG image data, 800 x 600, 8-bit/color RGB
```

### Approach

Try common steganography techniques:
1. Metadata examination
2. Strings extraction
3. LSB analysis
4. Tool-based detection

### Step-by-Step

**1. Check Metadata**
```bash
exiftool challenge.png
```
Nothing interesting in EXIF data.

**2. Extract Strings**
```bash
strings challenge.png | grep -i flag
```
No flag found.

**3. Try steghide**
```bash
steghide extract -sf challenge.png
# Enter passphrase: (try empty)
```
❌ Not a steghide file

**4. Use zsteg (PNG/BMP analysis)**
```bash
zsteg challenge.png
```

Output shows:
```
b1,rgb,lsb,xy .. text: "flag{h1dd3n_1n_p1x3ls}"
```

✅ Found flag in LSB (Least Significant Bit)!

**5. Verify with stegsolve**

Opened in stegsolve and viewed red/green/blue plane 0.
Flag visible in binary!

### Flag

```
flag{h1dd3n_1n_p1x3ls}
```

## Lessons Learned

- Always try multiple steganography tools
- LSB is a common hiding technique in images
- zsteg is excellent for PNG/BMP files
- Don't assume password-protection - try without password first

## Tools Used

- `exiftool` - Metadata extraction
- `strings` - Text extraction
- `zsteg` - PNG/BMP steganography detection
- `stegsolve` - Visual analysis
- `steghide` - Steganography tool

## References

- [zsteg](https://github.com/zed-0xff/zsteg)
- [Stegsolve](http://www.caesum.com/handbook/Stegsolve.jar)
- [Steganography Guide](https://0xrick.github.io/lists/stego/)

## Tags

`steganography` `forensics` `lsb` `png` `easy` `tryhackme`
