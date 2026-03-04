# CE Lua Extractor & Decompiler (DecodeCT)

## 📌 Introduction
**DecodeCT** is a reverse-engineering tool designed to extract, decrypt, and decompile Lua scripts embedded and obfuscated within Cheat Engine (CE) Cheat Tables (`.CT`) and text dumps. 

When developers use Cheat Engine's `encodeFunction` to protect their scripts, CE natively compiles the Lua code into pure bytecode, compresses it using Zlib streams (`clmax`), and encodes the payload using a custom Base85 character set. 

This project automatically reverses that entire pipeline: it scans for `decodeFunction(...)` patterns, reverses the custom Base85 encoding, unzips the payload, and dumps the raw **Lua 5.3 64-bit bytecode** (`.luac`). By pairing this tool with a standard Lua decompiler (like `unluac`), users can effortlessly recover the original, human-readable Lua source code.

## ✨ Key Features
* **Automated Extraction**: Scans `.CT` XML files or raw `.txt` dumps to automatically locate `decodeFunction` payloads.
* **Custom Base85 Decoding**: Implements a precise 1:1 Python translation of Cheat Engine's proprietary 85-character set (`0-9`, `A-Z`, `a-z`, `!#$%...`) and bitwise padding logic.
* **Zlib Decompression**: Automatically strips the compression layer to reveal the raw binary.
* **Bytecode Dumping**: Safely exports executable `Lua 5.3` bytecode files ready for standard decompilation.

## 🚀 Workflow
`Obfuscated CE Script` ➔ `Custom Base85 Decode` ➔ `Zlib Decompress` ➔ `Lua Bytecode (.luac)` ➔ `unluac.jar` ➔ **`Original Lua Source Code`**

## 💻 Usage

### 1. Extract and Decrypt `.luac` Bytecode
Run the included python script against a Cheat Table or a text file containing the obfuscated payload:
```bash
python decode_ct.py demo.txt
```
This will extract all hidden payloads and generate `.luac` files (e.g. `decoded_function_1.luac`).

### 2. Decompile the Bytecode
Because Cheat Engine uses Lua 5.3 (64-bit), you will need a Java-based Lua decompiler such as **unluac**.

Download the latest version of `unluac.jar` (e.g., from [SourceForge](https://sourceforge.net/projects/unluac/)), make sure you have Java installed, and run:
```bash
java -jar unluac.jar decoded_function_1.luac > source_1.lua
```

You will now have your recovered Lua script neatly saved inside `source_1.lua`!
