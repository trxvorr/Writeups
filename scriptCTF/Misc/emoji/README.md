# emoji (513 solves)
## Description:
>Author: noob-abhinav
 
>Emojis everywhere! Is it a joke? Or something is hiding behind it.

Resources:

[out.txt](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Misc/emoji/out.txt)

```
🁳🁣🁲🁩🁰🁴🁃🁔🁆🁻🀳🁭🀰🁪🀱🁟🀳🁮🁣🀰🁤🀱🁮🁧🁟🀱🁳🁟🁷🀳🀱🁲🁤🁟🀴🁮🁤🁟🁦🁵🁮🀡🀱🁥🀴🀶🁤🁽
```
## Solution:
The only byte that changes is the last one, if we print it in this way, we see the flag.
Used this script to print it:

```python
cipher_text = "🁳🁣🁲🁩🁰🁴🁃🁔🁆🁻🀳🁭🀰🁪🀱🁟🀳🁮🁣🀰🁤🀱🁮🁧🁟🀱🁳🁟🁷🀳🀱🁲🁤🁟🀴🁮🁤🁟🁦🁵🁮🀡🀱🁥🀴🀶🁤🁽"
decoded_message = ""

for char in cipher_text:
    decoded_char = chr(ord(char) & 0xFF)
    decoded_message += decoded_char

print(decoded_message)
# Output: scriptCTF{3m0j1_3nc0d1ng_1s_w31rd_4nd_fun!1e46d}
```

