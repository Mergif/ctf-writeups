### GHCTF 2.0
- Team: RedCall
- Role: Player (cryptography,misc,osint,forensics)
- solved: +10 challenges

**Description:**
Participated in GHCTF 2.0 as part of team *RedCall*. I contributed by solving multiple challenges, mainly in cryptography,misc,osint,forensics . This experience helped me improve my analytical thinking, speed, and teamwork during real-time competitions.

**What I gained:**
- Real CTF competition experience IRL
- Faster problem-solving under pressure
- Team collaboration skills

## solved challenges:

#### Challenge 1: The Goat
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
### Challenge 2 : LayerCake
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
The challenge  *LayerCake* suggests multiple layers of encoding.

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
#### Challenge 3: RSA
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

** Analysis:**
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
#### Challenge 4: chess1
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
#### Challenge 5: Checkmate
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

**Solution:**

git clone https://github.com/WintrCat/chessencryption
cd chessencryption
from decode import decode
with open("crazy_game.pgn", "r") as f:
    pgn_data = f.read()
decode(pgn_data, "recovered_secret.bin")
strings recovered_secret.bin

 Flag:
ghctf{ch3ckm4te_g00d_g4m3}
----
#### Challenge 6: Que miras bobo 1
- Category: Misc / Steganography
- Difficulty: Easy

**Description:**
“Qué mirás, bobo? Qué mirás, bobo? Andá p'allá, bobo.”

A meme image hides a secret flag inside it.

** What I learned:**
- Basics of image steganography
- Using brute-force tools to crack hidden data
- Importance of common password wordlists (rockyou.txt)
- Real-world usage of `stegseek`

---

** Analysis:**
The challenge provides an image file that likely contains hidden data.

Since it is a steganography challenge, the flag is probably:
- embedded inside the image
- protected with a password

The mention of a meme strongly suggests weak or common passwords → hinting at using a wordlist attack.


**Steps:**

1. Identify the file as a steganography challenge  
2. Use `stegseek` to attempt extraction  
3. Perform brute-force using `rockyou.txt`  
4. Extract hidden `flag.txt` file  

** Flag:**
ghctf{r0ck1ng_1t_w17h_st3gs33kkkkk}

---

#### Challenge 7: Saddam Hussein
- Category: OSINT 
- Difficulty: Medium

**Description:**
Can you find the exact coordinates of this place?

** What I learned:**
- Reverse image search techniques
- Using contextual clues for geolocation
- OSINT reasoning and elimination process
- Geoguessing strategies

---

**🔍 Analysis:**
The challenge provides an image of a structure (a tobacco barn).
A reverse image search reveals that the structure is a **tobacco barn commonly found in Kentucky, USA**.
However, there are many similar barns in the region, so additional hints are needed.

---

The challenge name **"Saddam Hussein"** is an important clue:
- Saddam Hussein → Iraq → Baghdad
- This suggests a naming pattern or indirect hint
In the United States, there is a place named **Bagdad, Kentucky**, which helps narrow down the search area significantly.

---

** Steps:**

1. Perform reverse image search on the provided image  
2. Identify the structure as a tobacco barn in Kentucky  
3. Use the challenge title as a geographical hint  
4. Focus search on Bagdad, Kentucky  
5. Use Google Maps to pinpoint exact barn location  
6. Extract coordinates  

---

**Flag:**
ghctf{38.266_-85.056}

---

#### Challenge 8: down there
- Category: Reverse / Forensics
- Difficulty: Easy

**Description:**
An image was used to exfiltrate hidden data. We managed to recover the binary used for that process.



**What I learned:**
- Basic reverse engineering of ELF binaries
- Understanding data exfiltration techniques
- File structure analysis (image + appended data)
- Using Linux tools for binary inspection


**Analysis:**
The provided ELF binary is responsible for hiding/exfiltrating data inside an image file.

After analyzing the binary, it becomes clear that:
- The image file contains appended hidden data
- The flag is directly embedded at the end of the file
- The program logic confirms a fixed-size data append operation


The key insight is that the flag was appended as raw bytes to the image.

Since the flag length is known (26 bytes), it can be extracted directly.

---

**Steps:**

1. Analyze the ELF binary to understand behavior  
2. Identify that data is appended to the image file  
3. Extract last 26 bytes from the image  
4. Inspect extracted output  
5. Confirm and reconstruct the flag  


**Solution:**

Extract last bytes of the image:
```bash id="revbash1"```
tail -c 26 kol3otla.png

Alternative method:

strings kol3otla.png
Flag:
ghctf{k0l_30tl4f1h4_kh1r3}

---
#### Challenge 9: BabyGo
- Category: Reverse Engineering
- Difficulty: Easy

**Description:**
Are you ready to go?



** What I learned:**
- Identifying Go (Golang) binaries in reverse engineering
- Using static analysis tools (IDA) on Go compiled executables
- Understanding simple XOR-based obfuscation
- Extracting and reversing verification logic


** Analysis:**
After inspecting the binary using `file` and `strings`, it becomes clear that this is a **Go (Golang) compiled binary**, which is known to behave differently from standard C binaries.

The main function reads user input and passes it to a validation function called `main_checkInput()`.

---

Inside `main_checkInput()`:
- A static array `r[]` is defined
- Each input character is processed
- A XOR operation is applied with a constant value (`0xCA`)
- The result is compared against the stored array

This confirms that the flag is hidden using a **simple XOR cipher**.

---

** Steps:**

1. Identify binary as Go executable  
2. Analyze `main_checkInput()` logic  
3. Observe comparison with XOR-transformed array  
4. Extract encoded values  
5. Reverse XOR operation with `0xCA` to recover flag  

---

**Solution Idea:**
Each character of the flag was encoded as:

encoded_char = original_char XOR 0xCA

So to retrieve the flag:

original_char = encoded_char XOR 0xCA


---

** Method:**
Using Python to reverse the array:

```python
id="babygo_solve"
encoded = [
    173,162,169,190,172,177,147,249,255,149,
    159,184,149,152,249,254,174,179,149,158,
    250,149,141,250,149,172,169,175,254,255,
    174,183
]

flag = "".join(chr(c ^ 0xCA) for c in encoded)
print(flag)
```
Flag:

ghctf{Y35_Ur_R34dy_T0_G0_fce45d}
