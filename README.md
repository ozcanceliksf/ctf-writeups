# CTF Write-ups

Step-by-step solutions for CTF challenges I've solved on picoCTF and other platforms.
Each write-up covers the tools used, my thought process, and what I learned.

---

## picoCTF Challenges

| Challenge | Category | Difficulty | Key Technique |
|-----------|----------|------------|---------------|
| [Riddle Registry](./riddle-registry.md) | Forensics | Easy | PDF metadata · Base64 decoding |
| [Log Hunt](./log-hunt.md) | General Skills | Easy | Log analysis · Regex |
| [Hidden in Plainsight](./hidden-in-plainsight.md) | Forensics | Easy | Steganography · Steghide · Base64 |
| [Corrupted File](./corrupted-file.md) | Forensics | Easy | File magic bytes · Hex repair |
| [Crack the Gate 1](./crack-the-gate-1.md) | Web Exploitation | Easy | ROT13 · HTTP header injection |
| [SSTI Challenge](./ssti-challenge.md) | Web Exploitation | Easy | Jinja2 SSTI · RCE |

---

## Skills Demonstrated

**Forensics**
- File metadata analysis
- File signature / magic bytes repair
- Steganography detection and extraction
- Log file parsing and pattern recognition

**Web Exploitation**
- Source code analysis
- HTTP header manipulation with `curl`
- Server-Side Template Injection (SSTI)
- Remote code execution via Jinja2

**General**
- Base64 encoding/decoding
- ROT13 cipher
- Hex analysis
- Linux command line

---

## Tools Used

- Python 3 (standard library)
- `curl`
- Browser DevTools
- Steghide
- picoCTF Webshell

---

## Profiles

- [picoCTF](https://play.picoctf.org/users/ozcanceliksf)
- [TryHackMe](https://tryhackme.com/p/ozcanceliksf)
- [HackTheBox](https://app.hackthebox.com/profile/ozcanceliksf)
