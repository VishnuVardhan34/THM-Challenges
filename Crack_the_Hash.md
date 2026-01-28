# TryHackMe - Crack The Hash

## Challenge Information

| Property | Value |
|----------|-------|
| **Difficulty** | Easy |
| **Category** | Hashing |
| **Link** | https://tryhackme.com/room/crackthehash |

## Overview
This challenge focuses on hash identification and cracking techniques, requiring participants to identify hash types and recover plaintext passwords using various tools and wordlists.

## Task 1: Basic Hash Cracking

This introductory task involves identifying and cracking common hash types using online hash cracking tools.

### Challenge 1
- **Hash**: `48bb6e862e54f2a795ffc4e541caed4d`
- **Hash Type**: MD5
- **Plaintext**: `easy`

### Challenge 2
- **Hash**: `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`
- **Hash Type**: SHA-1
- **Plaintext**: `password123`

### Challenge 3
- **Hash**: `1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`
- **Hash Type**: SHA-256
- **Plaintext**: `letmein`

### Challenge 4
- **Hash**: `$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`
- **Hash Type**: bcrypt (blowfish)
- **Plaintext**: `bleh`

### Challenge 5
- **Hash**: `279412f945939ba78ce0758d3fd83daa`
- **Hash Type**: MD5
- **Plaintext**: `Eternity22`


## Task 2: Advanced Hash Cracking with Wordlists

This task increases in difficulty, requiring the use of more sophisticated cracking tools such as Hashcat. All answers are contained within the rockyou.txt password wordlist.

### Challenge 1
- **Hash**: `F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85`
- **Hash Type**: SHA-256
- **Plaintext**: `paule`
- **Wordlist**: rockyou.txt

### Challenge 2
- **Hash**: `1DFECA0C002AE40B8619ECF94819CC1B`
- **Hash Type**: MD5
- **Plaintext**: `n63umy8lkf4i`
- **Wordlist**: rockyou.txt

### Challenge 3
- **Hash**: `$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.`
- **Hash Type**: SHA-512 (Salted - $6$)
- **Salt**: `aReallyHardSalt`
- **Plaintext**: `waka99`
- **Wordlist**: rockyou.txt

### Challenge 4
- **Hash**: `e5d8870e5bdd26602cab8dbe07a942c8669e56d6`
- **Hash Type**: SHA-1 (Salted)
- **Salt**: `tryhackme`
- **Plaintext**: `481616481616`
- **Wordlist**: rockyou.txt


## Tools & References

The following tools and resources were utilized in solving this challenge:

- **Hash Analyzer**: https://www.tunnelsup.com/hash-analyzer/ - For identifying hash types
- **Online Hash Cracking**: https://10015.io/ - For efficient plaintext recovery
- **Hashcat**: Official documentation and examples for advanced hash cracking with wordlists
- **Wordlists**: rockyou.txt (classic password wordlist for dictionary attacks)

## Key Takeaways

1. **Hash Identification**: Recognizing hash types (MD5, SHA-1, SHA-256, bcrypt) by their format and length is critical
2. **Tool Selection**: Online tools are suitable for common hashes in Task 1; Hashcat is necessary for Task 2 complexity
3. **Salted Hashes**: Understanding salt parameters (e.g., `$6$salt$hash`) is essential for proper cracking
4. **Wordlist Attacks**: Dictionary attacks using common wordlists like rockyou.txt are effective for many real-world scenarios