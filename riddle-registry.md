# Riddle Registry — picoCTF

**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  

---

## Challenge Description

> You're given a PDF file that looks like it contains nothing but gibberish. But something is hidden inside. Find the flag in the metadata.

---

## Tools Used

- Python 3 (`PyPDF2`)
- Base64 decoding (built-in)

---

## Solution

### Step 1 — Download the file

Downloaded `confidential.pdf` from the challenge page.

### Step 2 — Check the metadata

PDF files store hidden metadata fields like Author, Title, Subject, Keywords.
I read the raw bytes of the PDF to extract the `/Author` field:

```python
with open('confidential.pdf', 'rb') as f:
    content = f.read()

# Search for /Author field
idx = content.find(b'/Author')
print(content[idx:idx+200])
```

**Output:**
```
/Author (cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV80MjQ0MGM3ZH0=)
```

### Step 3 — Decode Base64

The Author field contained a Base64 encoded string:

```python
import base64
encoded = "cGljb0NURntwdXp6bDNkX20zdGFkYXRhX2YwdW5kIV80MjQ0MGM3ZH0="
decoded = base64.b64decode(encoded).decode()
print(decoded)
```

**Output:**
```
picoCTF{puzzl3d_m3tadata_f0und!_42440c7d}
```

---

## Flag

```
picoCTF{puzzl3d_m3tadata_f0und!_42440c7d}
```

---

## What I Learned

- PDF files contain metadata fields (Author, Title, Subject, Keywords) that can store hidden data
- Metadata is not visible when opening the PDF normally
- Data can be further obfuscated using Base64 encoding
- Always check file metadata when analyzing suspicious files in forensics challenges
