# Task 5: ABI & Register Cheat-Sheet

## Question
“List all 32 RV32 integer registers with their ABI names and typical calling-convention roles.”

---

## Register Table

| Register | ABI Name | Role (Calling Convention)         |
|----------|----------|-----------------------------------|
| x0       | zero     | Hard-wired zero                   |
| x1       | ra       | Return address                    |
| x2       | sp       | Stack pointer                     |
| x3       | gp       | Global pointer                    |
| x4       | tp       | Thread pointer                    |
| x5       | t0       | Temporary/Caller-saved            |
| x6       | t1       | Temporary/Caller-saved            |
| x7       | t2       | Temporary/Caller-saved            |
| x8       | s0/fp    | Saved register/Frame pointer      |
| x9       | s1       | Saved register/Callee-saved       |
| x10      | a0       | Function argument/return value    |
| x11      | a1       | Function argument/return value    |
| x12      | a2       | Function argument                 |
| x13      | a3       | Function argument                 |
| x14      | a4       | Function argument                 |
| x15      | a5       | Function argument                 |
| x16      | a6       | Function argument                 |
| x17      | a7       | Function argument                 |
| x18      | s2       | Saved register/Callee-saved       |
| x19      | s3       | Saved register/Callee-saved       |
| x20      | s4       | Saved register/Callee-saved       |
| x21      | s5       | Saved register/Callee-saved       |
| x22      | s6       | Saved register/Callee-saved       |
| x23      | s7       | Saved register/Callee-saved       |
| x24      | s8       | Saved register/Callee-saved       |
| x25      | s9       | Saved register/Callee-saved       |
| x26      | s10      | Saved register/Callee-saved       |
| x27      | s11      | Saved register/Callee-saved       |
| x28      | t3       | Temporary/Caller-saved            |
| x29      | t4       | Temporary/Caller-saved            |
| x30      | t5       | Temporary/Caller-saved            |
| x31      | t6       | Temporary/Caller-saved            |

---

## Calling Convention Summary

- **a0–a7**: Function arguments and return values (caller-saved)
- **s0–s11**: Callee-saved (preserved across calls)
- **t0–t6**: Temporaries (caller-saved)
- **ra**: Return address
- **sp**: Stack pointer
- **gp/tp**: Global/thread pointer
- **zero**: Always zero

---
