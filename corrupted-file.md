# Corrupted File — picoCTF

**Category:** Forensics  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  

---

## Challenge Description

> This file seems broken... or is it? Maybe a couple of bytes could make all the difference. Can you figure out how to bring it back to life?

---

## Tools Used

- Python 3 (raw byte manipulation)
- Knowledge of file magic bytes / file signatures

---

## Background — File Magic Bytes

Every file type has a specific "magic number" — the first few bytes that identify the file format:

| Format | Magic Bytes (Hex) |
|--------|-------------------|
| JPEG   | `FF D8 FF`        |
| PNG    | `89 50 4E 47`     |
| PDF    | `25 50 44 46`     |
| ZIP    | `50 4B 03 04`     |

---

## Solution

### Step 1 — Download the file

Downloaded the file (no extension) from the challenge page.

### Step 2 — Check the magic bytes

```python
with open('file', 'rb') as f:
    content = f.read()

print(f"First 8 bytes: {content[:8].hex()}")
# Output: 5c78ffe000104a46
```

A valid JPEG should start with `FF D8 FF E0`.
This file started with `5C 78 FF E0` — the first two bytes were corrupted.

### Step 3 — Fix the magic bytes

```python
# Replace first 2 bytes (5C 78) with correct JPEG magic (FF D8)
fixed = b'\xff\xd8' + content[2:]

with open('fixed_image.jpg', 'wb') as f:
    f.write(fixed)
```

### Step 4 — Open the fixed image

Opened `fixed_image.jpg` — it displayed an image with the flag written in red text at the bottom center.

---

## Flag

```
picoCTF{r3st0r1ng_th3_by73s_2326ca93}
```

---

## What I Learned

- Files are identified by their magic bytes, not their extension
- Corrupting or changing the first few bytes of a file will break it
- Forensics often involves repairing files by restoring correct headers
- This technique is used in real-world digital forensics to recover damaged files
