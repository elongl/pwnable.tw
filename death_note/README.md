# Notes
- Most likely that the bug is about not checking that the index is bigger than 0.
This basically allows arbitrary write to memory.


---
## Register state at `puts` call.
```
EAX  0xfffffff0
EBX  0x0
ECX  0x0
EDX  0x9b181a0 ◂— 'AAAA'
```

Shellcode length: up to 80 bytes.