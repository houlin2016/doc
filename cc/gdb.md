# GDB

## vector
```bash
(gdb) set $n = vec.size()
(gdb) set $ptr = vec._M_impl._M_start
(gdb) print *($ptr[0])
(gdb) print *($ptr[1])
```