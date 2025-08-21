---

# xv6-strace

A custom implementation of `strace` for the **xv6 teaching operating system**.
This project allows you to trace system calls made by processes inside xv6, providing insights into program behavior, debugging, and even memory leak detection.

---

## 📌 Features

* Turn tracing **on/off globally** or for specific commands.
* Trace **system calls** with arguments and return values.
* Maintain a **circular buffer** for the last 8 trace events.
* Support for **tracing child processes**.
* User-friendly command interface with multiple options:

  * `-e <syscall>` → trace only specific syscalls
  * `-s` → show all syscalls (including failed ones)
  * `-f` → show only failed syscalls
* Demonstration of **memory leak detection** using a test program.

---

## 🚀 How to Run

1. **Compile and boot xv6**:

   ```bash
   make clean
   make qemu
   # or for non-graphical version
   make qemu-nox
   ```

2. **Inside xv6 shell**, use:

   ```bash
   # Enable global tracing
   strace on

   # Disable tracing
   strace off

   # Run a single command with tracing
   strace run <command>

   # Dump last 8 trace events
   strace dump

   # Demonstrate tracing child processes
   childFork

   # Test for memory leaks
   leak
   ```

3. **With options**:

   ```bash
   strace -e <syscall>
   strace -s
   strace -f
   strace -s -e <syscall>
   strace -f -e <syscall>
   ```

---

## 🛠 Implementation Details

### Modified Files

* `proc.h` → Added strace-related fields to `proc` structure
* `proc.c` → Process management with strace functionality
* `syscall.c` → Support for syscall tracing
* `syscall.h` → New syscall numbers for strace
* `usys.S` → User-space syscall stubs
* `user.h` → User-space function declarations
* `strace.c` → Implementation of the strace command
* `sh.c` → Shell integration for strace commands
* `childFork.c` → Test program for child process tracing
* `leak.c` (user/) → xv6 memory leak test
* `leak.c` (external/) → Linux-compatible memory leak test

### Kernel vs User Space

* **Kernel Space**: process struct changes, tracing logic, buffer storage
* **User Space**: command parsing, output formatting

---

## 📊 Example Outputs

### Tracing `childFork`

```
TRACE: pid = 8 | command = childFork | syscall = fork | return = 9
TRACE: pid = 8 | command = childFork | syscall = wait | return = 9
TRACE: pid = 8 | command = childFork | syscall = trace | return = 0
TRACE: pid = 8 | command = childFork | syscall = fork | return = 10
```

Shows parent process (PID 8) spawning two child processes (PIDs 9, 10).

---

### Memory Leak Detection (`leak`)

```
TRACE: pid = 20 | command = leak | syscall = sbrk | return = 12288
TRACE: pid = 20 | command = leak | syscall = sbrk | return = 40012296
TRACE: pid = 20 | command = leak | syscall = sbrk | return = -1
```

Demonstrates repeated allocations until memory exhaustion → leak detected.

---

## ⚠️ Known Limitations

* `strace dump` should not be used while `strace on` is active.
* `-o <file>` option for writing output to files is **not yet implemented**.

---

## 📚 Authors

* **Aranya Aryaman**

---
