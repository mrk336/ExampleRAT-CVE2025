# ExampleRAT-CVE2025

This is a modular Remote Access Tool (RAT) written in C# for educational and red team simulation purposes. It demonstrates how encrypted payloads can be delivered and executed in a controlled environment, using techniques inspired by real-world malware.

## Disclaimer

This project is intended strictly for research and educational use. It simulates malware behavior to help security professionals understand threat mechanics. Do not deploy this code in unauthorized systems.

## Why I Built This

I wanted to explore how threat actors design modular malware that can persist, evade detection, and execute payloads dynamically. This project helped me understand key concepts in Windows internals, encryption, and sandbox evasion. It also gave me a chance to practice writing clean, secure code for offensive tooling.

## How It Works

### Key Derivation

The AES-256 encryption key is derived using PBKDF2 with SHA-512 and 10,000 iterations. This adds resistance to brute-force attacks.

Limitation: The salt is currently set to null, which weakens the cryptographic strength. In a production-grade tool, a random salt should be used and stored alongside the ciphertext.

### Payload Decryption

Each payload is stored as a Base64 string and decrypted using AES-CBC. The first 16 bytes of the encrypted data are used as the IV.

Limitation: The code currently slices 15 bytes for the IV, which is incorrect. AES requires a full 16-byte IV. This should be corrected to ensure proper decryption.

Example: You can encrypt any executable payload using OpenSSL or a custom script, convert it to Base64, and embed it in the code.

### Persistence Module

The decrypted persistence payload is written to a temporary file and executed using WMIExec.

Limitation: The code assumes WMIExec.exe is present and accessible. There is no fallback or validation if the executable is missing.

Example: The persistence payload could simulate registry modification or scheduled task creation.

### Sandbox Evasion

A randomized delay between 2 and 8 seconds is introduced to mimic human-like behavior and evade automated sandbox analysis.

Limitation: This is a basic evasion technique. More advanced methods could include checking for virtualization artifacts or user interaction.

### Exfiltration Module

This module follows the same pattern: decrypt, write to temp, execute.

Limitation: The actual exfiltration logic is not implemented. This is a placeholder for demonstration purposes.

Example: A real exfiltration payload might simulate credential dumping or file transfer to a remote server.

### Sandbox Detection Module

The final payload is executed to simulate sandbox detection.

Limitation: No actual detection logic is included. This is a placeholder.

Example: A real module might check for known sandbox DLLs, process names, or restricted system resources.

## Sample Payloads

You can replace the embedded Base64 strings with your own encrypted binaries. Here's a basic example using OpenSSL:

```
openssl enc -aes-256-cbc -in payload.exe -out encrypted.bin -K <key> -iv <iv>
```

Then convert the encrypted binary to Base64 and paste it into the code.

## How to Run

Clone the repository and build the project using .NET:

```
git clone https://github.com/mrk336/ExampleRAT-CVE2025
cd ExampleRAT-CVE2025
dotnet build
```

Note: WMIExec.exe must be available in your system path for execution to work.

## Learn More

You can connect with me on LinkedIn or explore my other security tools:

- LinkedIn: https://www.linkedin.com/in/mark-anthony-mallia-14115561

## Final Thoughts

This project is part of my journey into ethical hacking and threat simulation. It reflects my interest in understanding how attackers think and how defenders can respond. If you're a recruiter or security team looking for someone who can build, break, and secure systems, I’d love to connect.
