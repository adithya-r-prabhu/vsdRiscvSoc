
---

# Task 4: Hex Dump & Disassembly

## Objective

* Convert the compiled ELF file (`hello.elf`) into a raw hex file (`hello.hex`).
* Disassemble the ELF to see the assembly instructions.
* Understand the meaning of each column in the disassembly output.

---

## Steps

### 1. Disassemble ELF to readable assembly

Use `objdump` to disassemble the ELF binary into assembly code with addresses:

```bash
riscv32-unknown-elf-objdump -d hello.elf > hello.dump
```

* This creates `hello.dump` containing disassembled instructions.

### 2. Convert ELF to raw Intel HEX format

Use `objcopy` to generate a hex file from the ELF:

```bash
riscv32-unknown-elf-objcopy -O ihex hello.elf hello.hex
```

* This produces `hello.hex`, a hex representation of the binary suitable for flashing or inspection.

---

## Understanding `objdump` Output

Example snippet from `hello.dump`:

```
00000000 <main>:
   0:   13 01 01 fd     addi    sp,sp,-16
   4:   ef 00 00 00     call    puts
   ...
```

### Columns explained:

| Column           | Meaning                                                        |
| ---------------- | -------------------------------------------------------------- |
| `00000000`       | **Address**: Memory address of the instruction                 |
| `0:`             | **Offset** within function or section                          |
| `13 01 01 fd`    | **Opcode**: The machine code bytes (hex)                       |
| `addi sp,sp,-16` | **Mnemonic & Operands**: Assembly instruction and its operands |

* **Address**: Where this instruction is located in memory.
* **Opcode**: Raw bytes of the instruction; what the CPU actually executes.
* **Mnemonic**: Human-readable instruction name (e.g., `addi`, `call`).
* **Operands**: Registers, immediates, or labels the instruction uses.

---

## Summary

* Use `riscv32-unknown-elf-objdump -d` to disassemble ELF to assembly with addresses.
* Use `riscv32-unknown-elf-objcopy -O ihex` to convert ELF to Intel HEX file.
* The disassembly columns provide a full view from raw bytes to human-readable instructions with memory context.
* Understanding these helps debugging and firmware development.

---

If you want, I can prepare detailed notes on interpreting hex files or further ELF tools!



output
![alt text](image-5.png)