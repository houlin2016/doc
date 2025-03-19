# GDB

## vector
```bash
(gdb) set $n = vec.size()
(gdb) set $ptr = vec._M_impl._M_start
(gdb) print *($ptr[0])
(gdb) print *($ptr[1])
```

## deque
```bash
deque_._M_impl._M_start._M_cur->get()
(deque_._M_impl._M_start._M_cur+1)->get()
```
