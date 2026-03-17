# Log Hunt — picoCTF

**Category:** General Skills  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  

---

## Challenge Description

> Our server seems to be leaking pieces of a secret flag in its logs. The parts are scattered and sometimes repeated. Can you reconstruct the original flag?

---

## Tools Used

- Python 3 (`re` module)
- Text editor / terminal

---

## Solution

### Step 1 — Download the file

Downloaded `server.log` from the challenge page.

### Step 2 — Search for flag fragments

The log file was large with thousands of normal entries (INFO, WARN, ERROR).
I used regex to find lines containing `FLAGPART`:

```python
import re

with open('server.log', 'r') as f:
    content = f.read()

for line in content.split('\n'):
    if 'FLAGPART' in line or 'picoCTF' in line:
        print(line)
```

**Output (unique fragments):**
```
[1990-08-09 10:00:10] INFO FLAGPART: picoCTF{us3_
[1990-08-09 10:02:55] INFO FLAGPART: y0urlinux_
[1990-08-09 10:05:54] INFO FLAGPART: sk1lls_
[1990-08-09 10:10:54] INFO FLAGPART: cedfa5fb}
```

### Step 3 — Reconstruct the flag

The fragments appeared in order and were repeated multiple times. Combining them:

```
picoCTF{us3_ + y0urlinux_ + sk1lls_ + cedfa5fb}
```

---

## Flag

```
picoCTF{us3_y0urlinux_sk1lls_cedfa5fb}
```

---

## What I Learned

- Log files can contain sensitive data scattered across thousands of lines
- `grep` and `regex` are essential tools for log analysis
- In real-world security incidents, logs are a primary source of evidence
- SIEM tools (like Splunk or ELK) automate this kind of log searching at scale
