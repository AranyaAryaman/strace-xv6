# strace-xv6

A minimal implementation of the **`strace` utility** for the **xv6 operating system**, enabling system call tracing for debugging and learning purposes.

This project extends xv6 by introducing a new system call (`strace`) that monitors and logs system calls made by a process or its children, similar to the functionality of `strace` in Linux.

---

## 🚀 Features

* **System Call Tracing**

  * Trace all system calls invoked by a process.
  * Output includes the system call name and its return value.

* **Selective Tracing**

  * Option to trace specific system calls using a bitmask argument.

* **Process Inheritance**

  * Tracing persists across child processes created via `fork`.

* **User-Space Utility**

  * A new `strace` user program demonstrates the feature. Example:

    ```sh
    $ strace echo hello
    syscall write -> 6
    syscall exit  -> 0
    ```

---

## ⚙️ Implementation Details

1. **Kernel Modifications**

   * Added a new system call `sys_strace(int mask)` to enable tracing.
   * Modified `syscall.c` to log system calls if tracing is enabled.
   * Extended `proc.h` with a field to store the tracing mask for each process.

2. **User-Level Program**

   * Implemented a `strace` program in user space.
   * Accepts a mask and a command to execute with tracing enabled.

3. **Inheritance Support**

   * `fork()` propagates the parent’s tracing mask to child processes.

---

## 🖥️ Usage

1. Compile xv6 with the new system call:

   ```bash
   make qemu
   ```

2. Run a program under `strace`:

   ```sh
   $ strace ls
   syscall open  -> 3
   syscall read  -> 128
   syscall write -> 128
   syscall close -> 0
   syscall exit  -> 0
   ```

3. Run with a specific mask:

   ```sh
   $ strace -m 8 echo hello
   syscall write -> 6
   syscall exit  -> 0
   ```

---

## 📖 Example Output

```sh
$ strace grep main README
syscall open   -> 3
syscall read   -> 256
syscall write  -> 128
syscall close  -> 0
syscall exit   -> 0
```

---

## 🔮 Future Improvements

* Add argument printing (like full Linux `strace`).
* Support filtering by syscall name instead of only bitmask.
* Include timestamps for performance debugging.

---

## 📜 License

MIT License

---
