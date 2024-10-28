# Use pwntool for server challenge solver

```python
#!/usr/bin/env python3

from pwn import *

r = remote('challenge01.root-me.org', 52022)
io = process(['echo', 'hi'])
io.recvline()

msg = input.split("\'")[1]
print(f"Message to decode => '{msg}'")
result = zlib.decompress(base64.b64decode(msg))
answer = server.send_answer(f"{str(result)}\n")
```