# VSD RISC-V SoC Development Guide

![Build Status](https://img.shields.io/badge/build-passing-brightgreen) 
![License](https://img.shields.io/badge/license-MIT-blue) 
![Language](https://img.shields.io/badge/language-C-orange) 
![Platform](https://img.shields.io/badge/platform-RISC--V-lightgrey)

Welcome to the **VSD RISC-V SoC Development Guide**, a comprehensive resource for learning and implementing RISC-V-based SoC development. This repository contains step-by-step tasks, examples, and tools to help you get started with RISC-V architecture, toolchains, and emulators.

---

## 📚 Weekly Tasks

- [Week 1: Toolchain Setup and Basics](./week1/week1.md)
- Week 2: TBD
- Week 3: TBD
- Week 4: TBD
- Week 5: TBD
- Week 6: TBD
- Week 7: TBD

---

## 📚 Table of Contents

1. [🔧 Project Overview](#-project-overview)
2. [⚙️ Setup Instructions](#%EF%B8%8F-setup-instructions)
3. [🚀 Features](#-features)
4. [📂 Directory Structure](#-directory-structure)
5. [🤝 Contributions](#-contributions)
6. [📜 License](#-license)

---

## 🔧 Project Overview

This project is designed to help developers and students explore the RISC-V architecture through practical tasks and examples. It covers topics such as:

- Setting up the RISC-V GCC toolchain.
- Writing and compiling RISC-V programs.
- Running programs on emulators like QEMU and Spike.
- Understanding RISC-V assembly, ABI, and memory-mapped I/O.
- Implementing advanced features like interrupts, atomic operations, and linker scripts.

---

## ⚙️ Setup Instructions

Follow these steps to set up your environment:

1. **Install the RISC-V GCC Toolchain**:
   - Download the prebuilt toolchain for your platform.
   - Extract the archive and add the toolchain to your PATH:
     ```bash
     export PATH=$HOME/riscv-toolchain/bin:$PATH
     ```

2. **Install Emulators**:
   - Install QEMU for RISC-V:
     ```bash
     sudo apt install qemu-system-misc
     ```
   - Clone and build Spike (optional):
     ```bash
     git clone https://github.com/riscv/riscv-isa-sim.git
     cd riscv-isa-sim
     mkdir build && cd build
     ../configure --prefix=/usr/local
     make && sudo make install
     ```

3. **Verify Installation**:
   - Check the GCC version:
     ```bash
     riscv32-unknown-elf-gcc --version
     ```
   - Run QEMU or Spike to confirm they are installed.

---

## 🚀 Features

- **Toolchain Setup**: Step-by-step guide to installing and configuring the RISC-V GCC toolchain.
- **Hello World**: Compile and run a "Hello, RISC-V" program on QEMU and Spike.
- **Assembly Exploration**: Generate and analyze RISC-V assembly code.
- **Memory-Mapped I/O**: Control GPIOs and peripherals using memory-mapped registers.
- **Interrupts**: Implement machine timer interrupts (MTIP) for bare-metal systems.
- **Atomic Operations**: Use RISC-V atomic instructions for synchronization.
- **Linker Scripts**: Create custom linker scripts for memory layout control.

---

## 📂 Directory Structure

```
vsdRiscvSoc/
├── week1/                # Week 1 tasks and examples
│   ├── week1.md          # Toolchain setup and basics
│   ├── ...               # Additional tasks
├── week2/                # Week 2 tasks (TBD)
├── week3/                # Week 3 tasks (TBD)
├── week4/                # Week 4 tasks (TBD)
├── week5/                # Week 5 tasks (TBD)
├── week6/                # Week 6 tasks (TBD)
├── week7/                # Week 7 tasks (TBD)
├── README.md             # Main project README
├── LICENSE               # License file
└── ...                   # Other files and directories
```

---

## 🤝 Contributions

We welcome contributions to improve this project! Here's how you can help:

1. Fork the repository.
2. Create a new branch for your feature or bug fix:
   ```bash
   git checkout -b feature-name
   ```
3. Commit your changes and push to your fork:
   ```bash
   git commit -m "Add feature-name"
   git push origin feature-name
   ```
4. Open a pull request and describe your changes.

---

## 📜 License

This project is licensed under the [MIT License](LICENSE). You are free to use, modify, and distribute this project, provided proper attribution is given.

---

Happy coding! 🚀