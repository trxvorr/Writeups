# MOD
>Just a simple modulo challenge

## Solution:
We can use this propery of mod to solve the challenge: -1 % x = x-1. 

```python
from pwn import *

HOST, PORT = "play.scriptsorcerers.xyz", 10192

def solve():
    io = remote(HOST, PORT)

    io.recvuntil(b"Provide a number: ")
    io.sendline(b"-1")

    remainder = int(io.recvline().strip())
    secret = remainder + 1

    io.recvuntil(b"Guess: ")
    io.sendline(str(secret).encode())

    print(io.recvall(timeout=2).decode().strip())
    io.close()

if __name__ == "__main__":
    solve()
```
