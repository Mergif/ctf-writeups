# ctf-writeups
hello its Meriem Derbil AkA Watson, im  a computer science student learning cybersecurity and participating in CTF competitions
##  Skills
- Web Exploitation 
- Cryptography
- misk
- osint
- Linux basics
## Platforms
- picoCTF
---
## Writeups

### picoCTF2025
Challenge: [HASHCRACK]
- Category: Crypto 
- Difficulty: Easy

**What I learned:**
dentify hash types using length (MD5, SHA-1, SHA-256)
Understand that hashes are one-way functions 
Use scripting to brute-force hashes with wordlists (ROCKYOU)

**Steps:**
-setps:
dentified hash type based on length:
32 chars means MD5
40 chars means SHA-1
64 chars means SHA-256
Wrote a Python script using hashlib to hash candidate passwords
Compared generated hashes with the target hash
 use common passwords to find matches
-tools used :Python  lib :(hashlib)
-Wordlists (rockyou.txt)
the flag:
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_6965e43b}
