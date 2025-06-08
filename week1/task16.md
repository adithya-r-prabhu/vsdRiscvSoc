# task 16
# Bare-metal Newlib printf Retargeting with UART on RISC-V

## Overview

This example demonstrates how to use **newlib's `printf` function on a bare-metal RISC-V system without an OS** by retargeting the low-level `_write` syscall to send characters to a UART device via memory-mapped I/O.

Since newlib expects OS syscalls like `_write` to be defined, on bare-metal systems these need to be implemented by the user, often to send characters to a UART for console output.

---

## Key Concepts

* `printf` ultimately calls `_write(int fd, char* buf, int len)` syscall.
* Without an OS, `_write` is undefined and must be provided.
* Implement `_write` to send bytes one by one to UART's data register.
* The UART is typically memory-mapped; you write bytes to a specific address.
* Link with `-nostartfiles` and provide a minimal `syscalls.c` with `_write`.

---

## Files

| File         | Description                            |
| ------------ | -------------------------------------- |
| `main.c`     | Example program calling `printf`       |
| `syscalls.c` | Implements `_write` syscall to UART    |
| `link.ld`    | Linker script placing program and data |
| `build.sh`   | Build script for compiling and linking |

---

## Example Code

### 1. `syscalls.c` — Retarget `_write` to UART

```c
#include <unistd.h>
#include <sys/types.h>

// Memory-mapped UART base address (adjust per your hardware)
#define UART0_BASE 0x10000000
#define UART0_TX   (*(volatile unsigned char *)(UART0_BASE + 0x00))

// Retarget _write syscall for newlib printf
ssize_t _write(int fd, const void *buf, size_t count) {
    const char *ptr = (const char *)buf;

    for (size_t i = 0; i < count; i++) {
        // Wait for UART ready if needed (poll status register)
        // For simplicity, assume always ready

        UART0_TX = ptr[i];
    }
    return count;
}
```

### 2. `main.c` — Simple test for printf

```c
#include <stdio.h>

int main(void) {
    printf("Hello from bare-metal RISC-V UART!\n");
    while (1) { /* Loop forever */ }
    return 0;
}
```

### 3. `link.ld` — Simple linker script (example)

```ld
ENTRY(_start)

MEMORY
{
  RAM (rwx) : ORIGIN = 0x80000000, LENGTH = 16M
}

SECTIONS
{
  .text : { *(.text*) } > RAM
  .rodata : { *(.rodata*) } > RAM
  .data : { *(.data*) } > RAM
  .bss : { *(.bss*) *(COMMON) } > RAM
}
```

### 4. `_start` entry (optional)

You can write a minimal startup file `start.s` that calls `main`.

---

## Build and Run

Create a build script (`build.sh`):

```bash
#!/bin/bash
set -e

riscv32-unknown-elf-gcc -march=rv32imac -nostartfiles -T link.ld \
  -o uart_printf.elf syscalls.c main.c

spike --isa=rv32imac -m 16M:0x80000000 uart_printf.elf
```

Make the script executable:

```bash
chmod +x build.sh
```

Run:

```bash
./build.sh
```

---

## Notes

* Adjust `UART0_BASE` to match your hardware or simulator memory map.
* Real UARTs have status registers; you might need to poll before writing.
* You can add other syscalls (\_read, \_sbrk, etc.) if you want more libc features.
* This approach enables using `printf` for debugging over UART without OS support.

---

## References

* [newlib libc syscalls](https://sourceware.org/newlib/libc.html#Syscalls)
* [RISC-V privileged spec](https://riscv.org/specifications/privileged-isa/)
* [Spike Simulator](https://github.com/riscv/riscv-isa-sim)

---
