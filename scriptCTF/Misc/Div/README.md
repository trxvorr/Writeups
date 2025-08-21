# Div (720 solves)
## Description:
>Author: NoobMaster

>I love division

## Resources:

[chall.py](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Misc/Div/chall.py)

## Solution:
We need to enter a number where if we divide secret by it, we get 0. Obviously we need to enter a very large number. We tried alot of huge numbers thinking it only accepted int values…thats when we read it again…it took string input and the first thing we thought of was infinity. Started an instance then sent it and got the flag.

```console
nc play.scriptsorcerers.xyz 10231
Enter a number: Infinity
scriptCTF{70_1nf1n17y_4nd_b3y0nd_87463b62ba69}
```



