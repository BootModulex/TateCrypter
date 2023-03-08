# TateCrypter

## Crypter Link

http://tatecrypter.ddns.net/

## Crypter

TateCrypter crypts executables, which are decrypted at runtime and executed in-memory.

## Before

![](https://media.discordapp.net/attachments/1077814222949470285/1077814315140268094/before.png?width=1074&height=817)

## After

![](https://cdn.discordapp.com/attachments/1077814222949470285/1077814324728434810/after.png)

## Key feature overview

* Emulator detection
* Low-entropy packing scheme
* Code obfuscation
* File compression
* In-memory invocation of .NET executables
* EOF support

## Implementation & execution flow

Obfuscation and evasive features are fundamental to the design of PEunion and do not need further configuration. The exact implementation is fine tuned to decrease detection and is subject to change in future releases.

This graph illustrates the execution flow of the native stub decrypting and executing a PE file. The .NET stub works similarly.

![](https://bytecode77.com/images/pages/pe-union/execution-flow-light.png)

The **fundamental concept** is that the stub **only** contains code to detect emulators and to decrypt and pass execution to the next layer. The second stage is position independent shellcode that retrieves function pointers from the PEB and handles the payload. To mitigate AV detections, only the stub requires adjustments. Stage 2 contains all the "suspicious" code that is not readable at scantime and not decrypted, if an emulator is detected.

The shellcode is encrypted using a proprietary 4-byte XOR stream cipher. To decrease entropy, the encrypted shellcode is intermingled with null-bytes at randomized offsets. Because the resulting data has no repeating patterns, it is impossible to identify this particular encoding and infer YARA rules from it. Hence, AV detection is limited to the stub itself.

## Obfuscation

Assembly code is obfuscated by nop-like instructions intermingled with the actual code, such as an increment followed by a decrement. Strings are not stored in the data section, but instead constructed on the stack using mov-opcodes.

The C# obfuscator replaces symbol names with barely distinguishable Unicode characters. Both string and integer literals are decrypted at runtime.

[![](https://bytecode77.com/images/pages/pe-union/obfuscation.png)](https://bytecode77.com/images/pages/pe-union/obfuscation.png)
[![](https://bytecode77.com/images/pages/pe-union/obfuscation-dotnet.thumb.jpg)](https://bytecode77.com/images/pages/pe-union/obfuscation-dotnet.png)

## Audience

In order to use this program, you should:

* be familiar with crypters and the basic concept of what a crypter does
* have a basic understanding of in-memory execution and evasion techniques
* acknowledge that uploading the stub to VirusTotal will decrease the time that the stub remains FUD

I do not take any responsibility for anybody who uses PEunion in illegal malware campaigns. This is an educational project.

## FUD

This project is FUD on the day of release (February 21st 2023). A crypter that is free and publicly available, will not remain undetected for a long time. Adjusting the stub so it does not get detected is a daunting task and all efforts are in vain several days later. Therefore, there will be **no updates** to fix detection issues.

## Crypter Link

http://tatecrypter.ddns.net/
