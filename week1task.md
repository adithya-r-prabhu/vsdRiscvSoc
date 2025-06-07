# Week 1 Task: Install & Sanity-Check the Toolchain

## 🎯 Objective
Set up the RISC-V toolchain, add it to the system PATH, and verify the functionality of the `gcc`, `objdump`, and `gdb` binaries.

---

## 🔧 Steps Performed

1. **Download the Toolchain**  
   Used `wget` to download the RISC-V toolchain archive:
   ```bash
   wget https://vsd-labs.sgp1.cdn.digitaloceanspaces.com/vsd-labs/riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
   ```

2. **Extract the Archive**  
   Extracted the downloaded `.tar.gz` file:
   ```bash
   tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
   ```

3. **Add Toolchain to PATH**  
   Updated the PATH environment variable to include the toolchain binaries:
   ```bash
   export PATH=$HOME/Desktop/opt/riscv/bin:$PATH
   echo 'export PATH=$HOME/Desktop/opt/riscv/bin:$PATH' >> ~/.bashrc
   source ~/.bashrc
   ```

4. **Verify Installation**  
   Checked the versions of the installed binaries:
   ```bash
   riscv32-unknown-elf-gcc --version
   riscv32-unknown-elf-objdump --version
   riscv32-unknown-elf-gdb --version
   ```

---

## 🖥️ Terminal Output

Below is the terminal output during the setup process:

```plaintext
adi@asus:~/Desktop$ wget https://vsd-labs.sgp1.cdn.digitaloceanspaces.com/vsd-labs/riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
--2025-06-07 09:05:28--  https://vsd-labs.sgp1.cdn.digitaloceanspaces.com/vsd-labs/riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
Resolving vsd-labs.sgp1.cdn.digitaloceanspaces.com (vsd-labs.sgp1.cdn.digitaloceanspaces.com)... 104.18.42.227, 172.64.145.29
Connecting to vsd-labs.sgp1.cdn.digitaloceanspaces.com (vsd-labs.sgp1.cdn.digitaloceanspaces.com)|104.18.42.227|:443... connected.
HTTP request sent, awaiting response... 200 OK
Saving to: ‘riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz’

riscv-toolchain-rv32imac-x86_6 100%[====================================================>]   1.85G  39.6MB/s    in 57s

2025-06-07 09:06:24 (33.4 MB/s) - ‘riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz’ saved [1988893108/1988893108]

adi@asus:~/Desktop$ tar -xzf riscv-toolchain-rv32imac-x86_64-ubuntu.tar.gz
adi@asus:~/Desktop$ export PATH=$HOME/Desktop/opt/riscv/bin:$PATH
adi@asus:~/Desktop$ echo 'export PATH=$HOME/Desktop/opt/riscv/bin:$PATH' >> ~/.bashrc
adi@asus:~/Desktop$ source ~/.bashrc
adi@asus:~/Desktop$ riscv32-unknown-elf-gcc --version
riscv32-unknown-elf-gcc (g04696df096) 14.2.0
...
```

---

## 🖼️ Screenshot

![Ubuntu Terminal View](w1t1p1.png)  
*Screenshot of the terminal showing the commands and outputs.*

---

## 🛠️ Issues Encountered

1. **`riscv32-unknown-elf-gdb` Error**  
   Encountered the following error while running `riscv32-unknown-elf-gdb`:
   ```plaintext
   riscv32-unknown-elf-gdb: error while loading shared libraries: libpython3.10.so.1.0: cannot open shared object file: No such file or directory
   ```
   **Resolution**: Checked for the required library using:
   ```bash
   ldconfig -p | grep libpython3.10
   ```
   Found that `libpython3.10.so.1.0` was missing but `libpython3.12.so.1.0` was available. Created a symbolic link to resolve the issue:
   ```bash
   sudo ln -s /usr/lib/x86_64-linux-gnu/libpython3.12.so.1.0 /usr/lib/x86_64-linux-gnu/libpython3.10.so.1.0
   ```
   Verified the fix by running:
   ```bash
   riscv32-unknown-elf-gdb --version
   ```
   Output:
   ```plaintext
   riscv32-unknown-elf-gdb: Symbol `PyBool_Type' has different size in shared object, consider re-linking
   riscv32-unknown-elf-gdb: Symbol `PySlice_Type' has different size in shared object, consider re-linking
   riscv32-unknown-elf-gdb: Symbol `PyFloat_Type' has different size in shared object, consider re-linking
   GNU gdb (GDB) 15.2
   Copyright (C) 2024 Free Software Foundation, Inc.
   License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
   This is free software: you are free to change and redistribute it.
   There is NO WARRANTY, to the extent permitted by law.
   ```

---

## Task 2: Compile "Hello, RISC-V"

### 🎯 Objective
Write and compile a minimal C program for the RISC-V architecture (RV32IMC) and verify the generated ELF file.

---

### 🔧 Steps Performed

1. **Write a Simple C Program**  
   Created a minimal "Hello, RISC-V" program:
   ```c
   #include <stdio.h>
   int main() {
       printf("Hello, RISC-V!\n");
       return 0;
   }
   ```

2. **Compile the Program**  
   Used the RISC-V GCC toolchain to compile the program:
   ```bash
   riscv32-unknown-elf-gcc -march=rv32imc -mabi=ilp32 -o hello.elf hello.c
   ```

3. **Verify the ELF File**  
   Checked the generated ELF file to confirm it is for the RISC-V architecture:
   ```bash
   file hello.elf
   ```

---

## 🖼️ Screenshot

![Hello, RISC-V Compilation Output](w1t2p2.png)  
*Screenshot of the terminal showing the compilation and ELF verification output.*

---

### 🛠️ Issues Encountered

1. **Multilib Error During Compilation**  
   Encountered the following error while compiling the "Hello, RISC-V" program:
   ```plaintext
   riscv32-unknown-elf-gcc: fatal error: Cannot find suitable multilib set for '-march=rv32imc'/'-mabi=ilp32'
   compilation terminated.
   ```
   **Resolution**: Checked the available multilibs using:
   ```bash
   riscv32-unknown-elf-gcc --print-multi-lib
   ```
   Output:
   ```plaintext
   .;
   ```
   This indicated that the toolchain was built with a single multilib configuration. Recompiled the program without specifying `-march` and `-mabi` flags:
   ```bash
   riscv32-unknown-elf-gcc -o hello.elf hello.c
   ```

2. **Verification of ELF File**  
   Verified the generated ELF file:
   ```bash
   file hello.elf
   ```
   Output:
   ```plaintext
   hello.elf: ELF 32-bit LSB executable, UCB RISC-V, RVC, soft-float ABI, version 1 (SYSV), statically linked, not stripped
   ```

3. **Disassembly of ELF File**  
   Disassembled the ELF file to inspect the generated assembly code:
   ```bash
   riscv32-unknown-elf-objdump -d hello.elf
   ```
   Example output:
   ```plaintext
   Disassembly of section .text:

   000100b4 <exit>:
      100b4:       1141                    addi    sp,sp,-16
      100b6:       4581                    li      a1,0
      100b8:       c422                    sw      s0,8(sp)
      100ba:       c606                    sw      ra,12(sp)
      100bc:       842a                    mv      s0,a0
      100be:       7d0000ef                jal     1088e <__call_exitprocs>


      .....
      ....
      ...
      ..
      .
   ```

4. **Run the Compiled ELF File Using QEMU**  
   Executed the compiled ELF file using QEMU to emulate the RISC-V environment:
   ```bash
   qemu-riscv32 hello.elf
   ```
   Terminal Output:
   ```bash
   adi@asus:~/Desktop$ qemu-riscv32 ./hello.elf
   Hello, RISC-V world!
   ```

---

## 🖼️ Screenshot

![Running the Compiled ELF Using QEMU](w1t2p3.png)  
*Screenshot of the terminal showing the execution of the compiled ELF file using QEMU.*

---

## ✅ **Task 3: From C to Assembly**

> **Objective:** Generate RISC-V assembly (`.s`) from your C source and understand the function prologue and epilogue.

---

### 🧾 Step 1: Generate `.s` file from `hello.c`

Use this command:

```bash
riscv32-unknown-elf-gcc -S -O0 hello.c
```

This generates a file `hello.s` which contains **unoptimized** assembly for the `hello.c` code.

---

### 👨‍💻 Step 2: Open and read the generated `hello.s`

You’ll see something like this in the `main` function:

```asm
main:
    addi    sp,sp,-16         # Function prologue (make stack frame)
    sw      ra,12(sp)         # Save return address
    sw      s0,8(sp)          # Save frame pointer
    addi    s0,sp,16          # Set new frame pointer
    ...
    lw      ra,12(sp)         # Function epilogue (restore return address)
    lw      s0,8(sp)          # Restore frame pointer
    addi    sp,sp,16          # Pop stack frame
    ret                       # Return to caller
```

---

### 🔍 Explanation: Prologue & Epilogue

#### ✅ **Function Prologue** – sets up the stack frame:

```asm
addi sp, sp, -16        # Reserve 16 bytes on the stack
sw   ra, 12(sp)         # Save return address
sw   s0, 8(sp)          # Save previous frame pointer
addi s0, sp, 16         # Set s0 as current frame pointer (convention)
```

* The stack pointer (`sp`) is moved down to make space.
* `ra` is saved to be restored later.
* `s0` is used as the frame pointer (standard calling convention in RISC-V).

---

#### ✅ **Function Epilogue** – tears down the stack frame:

```asm
lw   ra, 12(sp)         # Restore return address
lw   s0, 8(sp)          # Restore old frame pointer
addi sp, sp, 16         # Restore old stack pointer
ret                     # Jump back to caller
```

* All saved registers are restored.
* Stack is cleaned up before returning.
* `ret` uses the restored `ra`.

---

### 🧠 Why this matters:

* This stack frame setup is part of the **RISC-V calling convention**.
* Even in simple functions like `main()`, the compiler ensures that the function can **safely return** and **preserve state**.

---

### ✅ Task 3 is complete when:

* You have successfully run `riscv32-unknown-elf-gcc -S -O0 hello.c`
* You understand the **prologue and epilogue** structure inside the generated `.s` file.