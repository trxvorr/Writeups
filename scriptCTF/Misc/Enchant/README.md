# Enchant (410 solves)
## Description:
>Author: NoobMaster

>I was playing minecraft, and found this strange enchantment on the enchantment table. Can you figure out what it is? Wrap the flag in scriptCTF{}

## Resources:
[enc.txt](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Misc/Enchant/enc.txt)

```console
ᒲ╎リᒷᓵ∷ᔑ⎓ℸ ̣ ╎ᓭ⎓⚍リ 
```

## Solution:
This is the Galactic alphabet.

![decoding](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Misc/Enchant/unnamed.png)

We were also told to wrap the flag with scriptctf{}, so after trying it with spaces and with underscores, we found the correct flag:
```console
scriptCTF{minecraftisfun}
```
