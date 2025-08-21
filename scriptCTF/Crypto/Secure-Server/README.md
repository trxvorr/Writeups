# Secure-Server
>John Doe uses this secure server where plaintext is never shared. Our Forensics Analyst was able to capture this traffic and the source code for the server. Can you recover John Doe's secrets?

## Resources:

[files.zip](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Crypto/Secure-Server/files.zip)  
├── [capture.pcap](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Crypto/Secure-Server/capture.pcap)  
└── [server.py](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Crypto/Secure-Server/server.py)  


## Solution:
The provided files are a network capture(pcap) and a Python file. 
```bash
❯ unzip -l files.zip
Archive:  files.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
     2580  2025-08-04 12:47   capture.pcap
      412  2025-07-17 14:22   server.py
---------                     -------
     2992                     2 files
```


The contents of the Python file are

```python
#server.py
import os
from pwn import xor
print("With the Secure Server, sharing secrets is safer than ever!")
enc = bytes.fromhex(input("Enter the secret, XORed by your key (in hex): ").strip())
key = os.urandom(32)
enc2 = xor(enc,key).hex()
print(f"Double encrypted secret (in hex): {enc2}")
dec = bytes.fromhex(input("XOR the above with your key again (in hex): ").strip())
secret = xor(dec,key)
print("Secret received!")
```
Opening the pcap file in Wireshark, we get some TCP streams that we follow and see.
I'll show the strings below
```bash
strings capture.pcap

Intel(R) Core(TM) i9-14900HX (with SSE4.2)
Linux 6.12.32-amd64
Dumpcap (Wireshark) 4.0.17 (Git v4.0.17 packaged as 4.0.17-0+deb12u1)
enp0s3
host 108.59.80.160
Linux 6.12.32-amd64
>WRU
tEl;P
>WRU
With the Secure Server, sharing secrets is safer than ever!
Enter the secret, XORed by your key (in hex): 
151e71ce4addf692d5bac83bb87911a20c39b71da3fa5e7ff05a2b2b0a83ba03
>WRU
tGl;P
>WRU
Double encrypted secret (in hex): e1930164280e44386b389f7e3bc02b707188ea70d9617e3ced989f15d8a10d70
XOR the above with your key again (in hex): 
87ee02c312a7f1fef8f92f75f1e60ba122df321925e8132068b0871ff303960e
>WRU
tEl;P
>WRU
t3l;P
Secret received!
>WRU
tCl;P
>WRU
tBl;P
>WRU
tAl;P
>WRU
t@l;P
~6zU
Counters provided by dumpcap
`6zU
```


To decrypt it
```python
from binascii import unhexlify

def xor_bytes(a, b):  
    return bytes(x ^ y for x, y in zip(a, b))

# Variables from the captured session  
enc = bytes.fromhex("151e71ce4addf692d5bac83bb87911a20c39b71da3fa5e7ff05a2b2b0a83ba03")  
enc2 = bytes.fromhex("e1930164280e44386b389f7e3bc02b707188ea70d9617e3ced989f15d8a10d70")  
dec = bytes.fromhex("87ee02c312a7f1fef8f92f75f1e60ba122df321925e8132068b0871ff303960e")

# Recover the key  
key = xor_bytes(enc, enc2)

# Recover the secret  
secret = xor_bytes(dec, key)

print("Recovered key:", key.hex())  
print("Recovered secret:", secret.decode())
```
Running this, we get the flag in plaintext.
```bash
trevo@kali:~/ctf/scriptctf2025/crypto/Secure-Server$ python3 solve.py    
Recovered key: f48d70aa62d3b2aabe82574583b93ad27db15d6d7a9b20431dc2b43ed222b773  
Recovered secret: scriptCTF{x0r_1s_not_s3cur3!!!!}  
```
The server prints enc2 (which leaks enc XOR key) while also sending/receiving the original enc and the follow-up dec. With both enc and enc2, an eavesdropper can derive the session key (enc XOR enc2) and then decrypt whatever the client sends later (dec XOR key) — so the server ends up leaking secrets it was supposed to protect.


