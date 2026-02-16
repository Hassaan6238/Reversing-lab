# Crackme #1 – Runtime-Decrypted XOR Password Check

## Overview
This crackme dynamically decrypts its password validation routine at runtime and checks user input using an XOR-based algorithm.  
The challenge was to analyze the binary, understand the password verification logic, and patch it to bypass the check.

---

## Anti-Debugging & Runtime Protections
While debugging with **x64dbg**, the binary used:

- `VirtualProtect` to mark memory as executable after runtime decryption
- `IsDebuggerPresent` and PEB checks to detect the presence of a debugger

These techniques are commonly used in packers, malware, and obfuscated binaries to protect code from static analysis.

---

## Runtime Decrypted Function
After stepping through the decryption routine, the real password validation function appeared in dynamically allocated memory.  

The function:

- Requires exactly 7 characters of input
- Builds the string `"pass123"` on the stack
- Applies XOR-based transformations with a key (`0xDEAD`)
- Compares each byte of input to the transformed bytes

Simplified pseudo-code:

```c
for (int i = 0; i < 7; i++) {
    transformed = input[i] XOR key[i];
    if (transformed != pass[i]) {
        return false;
    }
}
return true;
