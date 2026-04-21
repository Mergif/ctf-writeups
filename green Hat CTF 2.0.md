### GHCTF 2.0
- Team: RedCall
- Role: Player 

**Description:**
Participated in GHCTF 2.0 as part of team *RedCall*. I contributed by solving multiple challenges, mainly in cryptography and general skills. This experience helped me improve my analytical thinking, speed, and teamwork during real-time competitions.

**What I gained:**
- Real CTF competition experience IRL
- Faster problem-solving under pressure
- Team collaboration skills

## solved challenges:
#### Challenge: The Goat
- Category: Crypto
- Difficulty: Medium

**Description:**
Who is the greatest footballer of all time?

Encrypted Flag:
}WLCMNCUHIWLCMNCUHIWLCMNCUHIWLQK{ybcuu

---

**What I learned:**
- Vigenère cipher decryption
- Identifying layered encryption
- Using hints to find keys

---

** Analysis:**
The hint suggests "Cristiano", which is used as a key.  
The ciphertext structure indicates it is reversed.

Encryption steps:
1. Vigenère cipher
2. Mirror (reverse)

---

**Steps:**
1. Decrypt using Vigenère with key `cristiano`
2. Reverse the result

---

**Solution:**
```python
def vigenere_decrypt(ciphertext, key):
    plaintext = ''
    key = key.lower()
    key_len = len(key)
    key_index = 0

    for char in ciphertext:
        if char.isalpha():
            shift = ord(key[key_index % key_len]) - ord('a')
            if char.islower():
                decrypted = chr((ord(char) - ord('a') - shift) % 26 + ord('a'))
            else:
                decrypted = chr((ord(char) - ord('A') - shift) % 26 + ord('A'))
            plaintext += decrypted
            key_index += 1
        else:
            plaintext += char  

    return plaintext

def mirror_cipher(text):
    return text[::-1]

encrypted = "}WLCMNCUHIWLCMNCUHIWLCMNCUHIWLQK{ybcuu"
key = "cristiano"

decrypted = vigenere_decrypt(encrypted, key)
flag = mirror_cipher(decrypted)

print(flag)
```
------
the Flag:
ghctf{SIUUUUUUUUUUUUUUUUUUUUUUUUUUUUU}
--------
Challenge: LayerCake
- Category: Crypto
- Difficulty: Medium

**Description:**
Many layers, many bases. Only one true flag beneath.

Encoded string:
6cBWBH2vdZsBByKF2y8kXcUyWsbrdxM25Xo4RSSRYYoPXDL8KV1rAhr6aKeQOHChKZq2eyg0jJAPr3uoOQlpi8GqLOYQGdpjHZSoCEkuZN70foxWIwcQPSU0YqmfKguKObocgM9hyGxHDlkjZXAl6EjdEKf6l2euWObDRufYtXVjeM12DdaoLfzmxTK8Vj18vTZq2o5

* What I learned:**
- Identifying and handling multi-layer encoding
- Recognizing uncommon base encodings (Base62, Base58, Base45)
- Using tools to automate cryptanalysis
- Importance of iterative decoding

---

** Analysis:**
The challenge name *LayerCake* suggests multiple layers of encoding.

Using an online analysis tools Decode, the string was identified as likely being encoded in **Base62**. After decoding, each result still appeared encoded, indicating multiple layers.

The strategy was:
- Detect encoding
- Decode
- Repeat until readable output


** Steps:**

1. **Base62 Decode**
2skSSqGTdadZB2etLefaBcTqosASDwcbwgFkZJgmcA95V68P1gkpp5ZymcX86QSmQ4HXrhJMdHQp1G9WV1MSBPHuBQM8kPXu5mT7Y7pxz2ve6oRofiaFCUxanYtegn7qSjNhHrDeH2EPoDvTnzLY
2. **Base58 Decode**
AY96ZA409JS8%6AZ09.A8GIAOS9:G8KB9IB71A07A0C9FH871A769H%6HM8H9IBJB9INAH9-CBBY8UF6HN9.IB4099T9L092G6INATB9
3.**Base45 Decode**
M5UGG5DGPNGTA4RTL5BDI4ZTONPUIMBTONHF65C7JUZTI3S7JUYHEM27KMZWG5LSGF2HS7I=
4. **Base32 Decode (final step)**  
→ Reveals the flag

**Tools used:**
- dCode 
- CyberChef
- Python 

** Flag:**
ghctf{M0r3_B4s3s_D03sN_t_M34n_M0r3_S3cur1ty}

** Key Insight:**
Applying multiple layers of encoding does not increase security if each layer is reversible.  
An attacker can simply decode step by step until reaching the original message.

---
#### Challenge: RSA
- Category: Crypto
- Difficulty: Easy

**Description:**
Got p, q, n, e, c. No excuses.

---

** What I learned:**
- Basics of RSA encryption and decryption
- Computing Euler’s totient φ(n)
- Finding modular inverse for private key
- Converting decrypted integers to readable text

---

**🔍 Analysis:**
In RSA, the public key consists of (n, e), and the private key is derived using the prime factors of n.

Here, both **p** and **q** are given, which makes the challenge straightforward because:
- We can compute φ(n)
- Then compute the private key d
- Then decrypt the ciphertext

---

** Steps:**

1. Compute Euler’s Totient:

φ(n) = (p - 1) × (q - 1)


2. Compute private key:

d ≡ e⁻¹ mod φ(n)


3. Decrypt ciphertext:

m ≡ cᵈ mod n


4. Convert the result to bytes to get the flag

---

**Solution (Python):**
```python
from Crypto.Util.number import long_to_bytes

p = 11752898189721656415758022203738178860337485859841207882239255254813156880083003451361685517755625725180776540539620511563469138074071001826696788332938749
q = 12452672719439878228749697778829318314556739351003812662776493451642917440781597440438345457262305334829767602614898688015947636457572708056834095851209913
n = 146354994661501201090371536506916591767862276963305601521508818383114091853525192182564442406200097774116264176249510795959907920157997479310270600797284279203839071457430892988671766955199730936822182267588819330747434689601943790122273603153224586364552411931862521104589464531576803096838764107362570618837
e = 65537
c = 53251061241972886277843821222876528014839792224815585816122446161552034066430518801757592154438938633412591993722342169573595048472782247571672054554779734936701206472237103411403014872379815434949454103363490522855367360723361647429864800680715939608457650803399026352116414374858006244746941261618144689182

# Step 1: Compute phi(n)
phi = (p - 1) * (q - 1)

# Step 2: Compute private key d
d = pow(e, -1, phi)

# Step 3: Decrypt
m = pow(c, d, n)

# Step 4: Convert to bytes
flag = long_to_bytes(m)

print(flag.decode())
 ```

 Flag:

ghctf{RSA_1s_E4sy_Wh3n_Y0u_H4v3_p_q}

---

** Key Insight:**
RSA is secure only when p and q are secret.  
If the prime factors are known, the private key can be easily computed, making decryption trivial.

---
#### Challenge: chess1
- Category: Misc
- Difficulty: Easy

**Description:**
Identify the fastest checkmate in the given position.  
Provide the full move sequence in chess notation.


---

**What I learned:**
- Analyzing chess positions logically
- Finding forced checkmate sequences
- Translating moves into standard chess notation

---

** Analysis:**
The challenge required finding the **fastest forced checkmate** from a given chess position.

The key idea was:
- Look for immediate checks
- Force the opponent into limited responses
- Continue until checkmate is unavoidable

This type of problem is similar to “mate in X moves” puzzles.

**Steps:**
1. Analyze the board and identify attacking opportunities  
2. Look for forcing moves (checks, captures, threats)  
3. Reduce opponent’s possible responses  
4. Continue until reaching a forced checkmate  

**Solution (Move Sequence):**
Nxe6+, Ke8, Kh5#
** Flag:**
ghctf{Nxe6+,Ke8,Kh5#}


--- 
#### Challenge: Checkmate
- Category: Misc / Forensics
- Difficulty: Easy

**Description:**
Maybe the moves are more than just strategy.

We are given a `.pgn` file containing a chess game with seemingly random moves.

---

**What learned:**
- Data hiding techniques (steganography in unusual formats)
- How to research unfamiliar problems
- Using external tools and GitHub projects in CTFs
- Extracting hidden data from structured files

---

**Analysis:**
The provided PGN file contains chess moves that appear random and do not represent a real game strategy.

This suggests that:
- The moves are not meant to be played normally
- They are likely encoding hidden data

By researching concepts like:
- “data hidden in chess”
- “PGN steganography”

I found tools that allow encoding/decoding files inside chess games.

---

**Steps:**

1. Identify that the PGN file is used to hide data  
2. Search for tools related to chess-based encoding  
3. Find a suitable decoding tool (`chessencryption`)  
4. Use the decoding script on the PGN file  
5. Extract the hidden binary file  
6. Read the content to reveal the flag  

** Solution:**

git clone https://github.com/WintrCat/chessencryption
cd chessencryption
from decode import decode
with open("crazy_game.pgn", "r") as f:
    pgn_data = f.read()
decode(pgn_data, "recovered_secret.bin")
strings recovered_secret.bin
 Flag:
ghctf{ch3ckm4te_g00d_g4m3}
