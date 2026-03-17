# Hidden in Plainsight — picoCTF

**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  

---

## Challenge Description

> You're given a JPG image. Something is tucked away out of sight inside the file. Your task is to discover the hidden payload and extract the flag.

---

## Tools Used

- Python 3 (raw byte analysis)
- Base64 decoding
- Steghide (via futureboy.us online decoder)

---

## Solution

### Step 1 — Download the file

Downloaded `img.jpg` from the challenge page.

### Step 2 — Analyze the JPEG structure

Instead of just opening the image, I read the raw bytes to look for hidden data in JPEG segments:

```python
with open('img.jpg', 'rb') as f:
    content = f.read()

# Look for JPEG comment segment (FFFE marker)
# Found at offset 20, length 30
# Content: c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9
```

The JPEG comment segment (`FFFE`) contained a Base64 string.

### Step 3 — Decode the comment

```python
import base64

encoded = "c3RlZ2hpZGU6Y0VGNmVuZHZjbVE9"
decoded = base64.b64decode(encoded).decode()
print(decoded)
# Output: steghide:cEF6endvcmQ=
```

This revealed two pieces of information:
- **Tool:** `steghide`
- **Password (Base64):** `cEF6endvcmQ=`

### Step 4 — Decode the password

```python
password = base64.b64decode("cEF6endvcmQ=").decode()
print(password)
# Output: pAzzword
```

### Step 5 — Extract hidden data with Steghide

Used the online Steghide decoder at `futureboy.us/stegano/decinput.html`:
- Uploaded `img.jpg`
- Passphrase: `pAzzword`
- Extracted the hidden file containing the flag

---

## Flag

```
picoCTF{forensics_analysis_is_amazing_2561a194}
```

---

## What I Learned

- Steganography hides data inside normal-looking files
- JPEG files have multiple segments — comment segments (`FFFE`) can store hidden data
- Hidden data can be double-encoded (Base64 → tool name + password)
- Steghide is a common steganography tool used in CTF forensics challenges
