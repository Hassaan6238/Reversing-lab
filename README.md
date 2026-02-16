# Reverse Engineering Practice

This repository contains my reverse engineering challenges and crackme writeups.  
It demonstrates techniques such as:

- Runtime code decryption
- Anti-debugging bypass
- Memory protection analysis (VirtualProtect)
- XOR-based password obfuscation
- Control flow patching

---

## Crackme #1 – Runtime-Decrypted XOR Password Check

### Overview
This crackme dynamically decrypts its password check routine and validates input using an XOR-based algorithm.  
The challenge was to analyze, understand, and patch the binary to bypass the password check.

### Anti-Debugging & Runtime Protections
While debugging in **x64dbg**, the binary used:

- `VirtualProtect` to mark decrypted memory as executable
- `IsDebuggerPresent` and PEB checks to detect debuggers

These are common techniques used in packers and malware.

### Password Validation Routine
After stepping past the decryption:

- Input must be 7 characters long
- `"pass123"` is constructed in memory
- XOR operations using a key (`0xDEAD`) validate the input

Simplified logic:

```text
(pass[i]) == input[i] XOR key[i]
