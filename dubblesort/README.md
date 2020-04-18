# Notes
- Try to somehow leak the canary and keep it in `nums` so that it is kept on `main`'s stack check.
- If `scanf` gets a value that doesn't match its directive, it doesn't change the memory.



# One Gadget
```c
0x3a819 execve("/bin/sh", esp+0x34, environ)
constraints:
  esi is the GOT address of libc
  [esp+0x34] == NULL

0x5f065 execl("/bin/sh", eax)
constraints:
  esi is the GOT address of libc
  eax == NULL

0x5f066 execl("/bin/sh", [esp])
constraints:
  esi is the GOT address of libc
  [esp] == NULL
```