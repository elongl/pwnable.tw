# Leads
- Buffer overflow on `scanf`.
- Hack strstr(filename) to return `NULL`.
- Leak virtual address space mapping by reading `/proc/self/maps`.


# Options
- Override `fp`.
- Override `nptr`.
- Set `magicbuf` to arbitrary content by opening `/proc/self/fd/0`.
- Read content of file through `/proc/self/fd/{fd}`.
- Get shell's PID by reading `/proc/self/stat`.


# Inputs
- Choice
- Name
- File name