
# Task 17: Endianness & Struct Packing on RV32

## Question

**Is RV32 little-endian by default? Show how to verify byte ordering with a union trick in C.**

---

## Background

* RISC-V RV32 (32-bit) architecture **is little-endian by default**.
* Little-endian means the least significant byte of a multi-byte word is stored at the lowest memory address.
* We can verify this by writing a known 32-bit pattern and inspecting its individual bytes in memory.

---

## Method

Use a C union consisting of:

* a 32-bit integer (`uint32_t`) member
* a byte array (`uint8_t[4]`) member

Store `0x01020304` in the `uint32_t`, then print the bytes in the array to observe byte order.

---

## Example Code: `check_endianness.c`

```c
#include <stdio.h>
#include <stdint.h>

int main() {
    union {
        uint32_t val;
        uint8_t bytes[4];
    } test;

    test.val = 0x01020304;

    printf("Stored 32-bit value: 0x%08X\n", test.val);
    printf("Bytes in memory: %02X %02X %02X %02X\n",
           test.bytes[0], test.bytes[1], test.bytes[2], test.bytes[3]);

    if (test.bytes[0] == 0x04 &&
        test.bytes[1] == 0x03 &&
        test.bytes[2] == 0x02 &&
        test.bytes[3] == 0x01) {
        printf("System is Little-endian.\n");
    } else if (test.bytes[0] == 0x01 &&
               test.bytes[1] == 0x02 &&
               test.bytes[2] == 0x03 &&
               test.bytes[3] == 0x04) {
        printf("System is Big-endian.\n");
    } else {
        printf("Unknown endianness.\n");
    }

    return 0;
}
```

---

## Build and Run

```bash
riscv32-unknown-elf-gcc -march=rv32imac -o check_endianness check_endianness.c
spike --isa=rv32imac -m 16M:0x80000000 check_endianness
```

---

## Expected Output (on RV32 Little-endian)

```
Stored 32-bit value: 0x01020304
Bytes in memory: 04 03 02 01
System is Little-endian.
```

---

## Explanation

* The least significant byte `0x04` is stored at the lowest address (`bytes[0]`).
* This confirms RISC-V’s default little-endian byte ordering.
* If it were big-endian, bytes would appear as `01 02 03 04`.

---

