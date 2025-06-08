

# Task 13: Machine Timer Interrupt (MTIP) — RISC-V Interrupt Primer

## 🎯 Objective

Demonstrate how to enable the machine-timer interrupt (MTIP) on a RISC-V processor and write a simple interrupt handler in C. This involves configuring the machine timer registers (`mtime`, `mtimecmp`), enabling interrupts through control and status registers (`mie`, `mstatus`), and handling the interrupt via an interrupt service routine (ISR) with proper CSR management.

---

## 📋 Prerequisites

* Completed Task 12 or equivalent bare-metal programming knowledge
* RISC-V toolchain supporting `rv32imac_zicsr` ISA (with CSR extensions)
* Familiarity with memory-mapped I/O, RISC-V CSRs, and interrupt concepts
* Spike RISC-V ISA simulator installed

---

## 🛠️ Files Overview

| Filename                   | Description                                                                                     |
| -------------------------- | ----------------------------------------------------------------------------------------------- |
| `task13_timer_interrupt.c` | Main C program, contains ISR with `__attribute__((interrupt))` and interrupt enabling functions |
| `interrupt_start.s`        | Assembly startup code; initializes stack, trap vector, and calls `main`                         |
| `interrupt.ld`             | Linker script; maps program sections and sets load/start address                                |
| `build_mtip.sh`            | Bash build script to compile, link, and run the program on Spike simulator                      |

---

## 🔧 Step-by-Step Instructions

### 1. Write the timer interrupt program in C

* Uses memory-mapped addresses for `mtime` and `mtimecmp` registers (example addresses for QEMU virt machine, update if needed)
* Defines an interrupt handler with the attribute `__attribute__((interrupt))`
* Enables machine timer interrupts via CSRs
* Counts the number of interrupts triggered and stops after 5 interrupts

### 2. Create assembly startup file (`interrupt_start.s`)

* Sets up stack pointer
* Sets the trap vector (`mtvec`) to the trap handler
* Calls the `main` function
* Implements a simple trap handler that saves/restores registers and calls the C ISR

### 3. Write a linker script (`interrupt.ld`)

* Defines memory regions (FLASH, SRAM)
* Sets `.text` section start address (e.g., `0x80000000` for typical embedded system memory layout)
* Places `.data`, `.bss`, and stack appropriately

### 4. Build and run

* Use the provided `build_mtip.sh` script to build the program
* Run the program on Spike simulator with sufficient memory size and correct ISA extension

---

## ⚙️ Build Script (`build_mtip.sh`)

```bash
#!/bin/bash
set -e

# Compile assembly startup with Zicsr extension for CSR instructions
riscv32-unknown-elf-gcc -march=rv32imac_zicsr -c interrupt_start.s -o interrupt_start.o

# Compile C source code
riscv32-unknown-elf-gcc -march=rv32imac_zicsr -c task13_timer_interrupt.c -o task13_timer_interrupt.o

# Link objects with the custom linker script
riscv32-unknown-elf-ld -T interrupt.ld interrupt_start.o task13_timer_interrupt.o -o task13_timer_interrupt.elf

echo "Build complete."
```

Make the script executable:

```bash
chmod +x build_mtip.sh
```

---

## 🏃 Running the Program on Spike

```bash
spike --isa=rv32imac -m 256 task13_timer_interrupt.elf
```

* `-m 256` sets simulated memory size to 256 MB
* Make sure your linker script maps `.text` to a valid address (e.g., `0x80000000`) that Spike uses as RAM start

---

## 🔍 Troubleshooting Common Errors

| Error Message                                     | Cause & Solution                                                                                |
| ------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `Access exception occurred while loading payload` | ELF entry address invalid or no memory mapped there. Check linker script `.text` start address. |
| `Error: unrecognized opcode 'csrw mtvec, t0'`     | Missing `zicsr` extension in GCC `-march` flag. Use `-march=rv32imac_zicsr`.                    |
| `No such file or directory` (for ELF file)        | Build failed or file not generated. Rerun build script.                                         |

---

## 📝 Code Explanation Highlights

* **Timer registers:**
  `mtime` and `mtimecmp` are 64-bit memory-mapped registers controlling the timer. `mtimecmp` triggers an interrupt when `mtime` matches or exceeds it.

* **Interrupt Handler:**
  The ISR updates `mtimecmp` to schedule the next timer interrupt and increments an interrupt counter. It is marked with `__attribute__((interrupt))` to ensure proper prologue/epilogue.

* **CSR Configuration:**

  * `mtvec` set to the interrupt handler address for trap vector
  * `mie` register bit 7 (`MTIE`) enabled for timer interrupts
  * `mstatus` bit 3 (`MIE`) enabled for global interrupts

* **Startup:**
  Assembly `_start` sets stack pointer, `mtvec`, and calls main, with an infinite loop afterward. It also contains a trap handler to save/restore context and call the ISR.

---

## References

1. RISC-V Privileged Architectures Specification: [https://riscv.org/specifications/](https://riscv.org/specifications/)
2. RISC-V Interrupt Handling Guide
3. Spike Simulator Documentation: [https://github.com/riscv/riscv-isa-sim](https://github.com/riscv/riscv-isa-sim)




output
![alt text](image-20.png)
![alt text](image-21.png)