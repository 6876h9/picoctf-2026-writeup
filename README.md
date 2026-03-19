# picoctf-2026-writeup
PicoCTF 2026 competition write-ups covering web exploitation, binary exploitation, reverse engineering, forensics, and cryptography — 8150 points contributed personally.

# PicoCTF 2026 — Competition Write-Up

**Author:** eightmonths  
**Competition:** PicoCTF 2026  
**Team Score:** 8300 points  
**Personal Contribution:** 8150 points  
**Platform:** [play.picoctf.org](https://play.picoctf.org)

---

## Table of Contents

1. [Overview](#overview)
2. [My Approach and Methodology](#my-approach-and-methodology)
3. [Categories Covered](#categories-covered)
4. [Challenge Highlights](#challenge-highlights)
5. [Tools and Environment](#tools-and-environment)
6. [Key Takeaways](#key-takeaways)
7. [Repository Structure](#repository-structure)
8. [Recommended Reading](#recommended-reading)
9. [Disclaimer](#disclaimer)

---

## Overview

PicoCTF 2026 is a Capture The Flag competition developed and hosted by Carnegie Mellon University, designed to challenge participants across a broad range of cybersecurity disciplines. The competition runs challenges across web exploitation, binary exploitation, reverse engineering, forensics, and cryptography — each requiring a fundamentally different set of skills and mindset to approach effectively.

This repository documents my personal walkthrough of the competition. Out of a combined team score of 8200 points, I solved challenges independently accounting for 8150 of those points. The write-ups here are intended to be educational — explaining not just what the flag was, but the actual thinking process behind reaching it. No flags are published in this repository.

---

## My Approach and Methodology

Every challenge I approached started the same way: read the problem statement twice, identify what category it most likely belongs to, and then resist the urge to immediately run tools. The most common mistake in CTF competitions is throwing tools at a problem before understanding what the problem is actually asking. Enumeration without direction is just noise.

For web challenges, I started with manual inspection — browser developer tools, reading source HTML, checking for exposed comments, odd form fields, or suspicious JavaScript. Only after forming a hypothesis did I bring in tools like Burp Suite or curl to test it.

For binary exploitation, my process was: run the binary once to understand its behavior, then pull it into Ghidra or GDB to understand its structure. I looked for classic patterns — buffer overflows, format string vulnerabilities, use-after-free — but approached each binary without assumptions.

Forensics challenges taught me to slow down. These often reward patience more than technical skill. Steganography, metadata analysis, packet captures — none of these yield to speed. I worked through them methodically, checking file headers manually, using binwalk and Wireshark where appropriate, and always keeping an eye on what the challenge was actually trying to hide.

Cryptography was where I leaned heavily on first principles. I have a solid understanding of RSA, XOR-based encryption, substitution ciphers, and common implementation flaws. Many of the crypto challenges in this year's competition came down to recognizing a known weakness rather than breaking the encryption outright.

---

## Categories Covered

| Category | Challenges Attempted | Points Contributed |
|---|---|---|
| Web Exploitation | Multiple | Significant |
| Binary Exploitation | Multiple | Significant |
| Reverse Engineering | Multiple | Significant |
| Forensics | Multiple | Significant |
| Cryptography | Multiple | Significant |
| General Skills | Multiple | Significant |

---

## Challenge Highlights

### Web Exploitation

Web challenges this year ranged from beginner-accessible SQL injection setups to more nuanced logic flaws that required reading through application behavior carefully. Several challenges involved bypassing client-side validation — a reminder that anything enforced only in the browser is not really enforced at all. Others involved cookie manipulation, insecure direct object references, and a particularly interesting challenge that required chaining two separate vulnerabilities together to reach the flag.

The lesson reinforced repeatedly in this category: never trust what the server sends back without inspecting it. HTTP responses carry a lot of information people forget to look at — headers, redirect chains, and error messages all tell stories.

### Binary Exploitation

Binary exploitation challenges required a combination of static and dynamic analysis. I used Ghidra for decompilation and GDB with the PEDA extension for runtime analysis. Several challenges involved classic stack-based buffer overflows where the goal was to overwrite the return address and redirect execution. Others introduced mitigations like stack canaries or NX bits, requiring more creative approaches using ret2libc or ROP chains.

What made these challenges satisfying was that each binary had a story — a developer who made one small mistake, an assumption about input length that turned out to be wrong, a debug function that never got removed before shipping. Finding that mistake is the job.

### Reverse Engineering

Reverse engineering challenges this year included stripped binaries, obfuscated logic, and one challenge involving a custom bytecode interpreter. The approach for all of them was the same: build a mental model of what the program is supposed to do, then look for the delta between that model and what it actually does. That delta is usually where the flag lives.

Patience matters more than speed here. Renaming variables in Ghidra as you understand them, writing out the logic on paper, tracing execution paths manually — these slow-looking activities actually speed up the overall process.

### Forensics

Forensics challenges included packet capture analysis, image steganography, file carving from raw disk images, and log analysis. The packet capture challenges were solved using Wireshark, filtering by protocol and following TCP streams. Steganography challenges required both automated scanning with tools like zsteg and stegsolve, and manual analysis of pixel data in some cases.

One of the more interesting forensics challenges involved a corrupted file with a wrong magic byte header. Fixing the header manually with a hex editor and then recovering the file was a clean, satisfying solve.

### Cryptography

The cryptography challenges tested knowledge of RSA (including small exponent attacks and common modulus attacks), XOR encryption with key reuse, and classical ciphers. I implemented several attacks from scratch in Python rather than relying on tools, which gave me a clearer understanding of why the vulnerabilities exist.

The most important skill in CTF cryptography is recognizing the category of vulnerability quickly. Once you know whether you are dealing with a padding oracle, a key reuse problem, or a factorization weakness, the path forward becomes much clearer.

---

## Tools and Environment

The following tools were used throughout the competition:

- **Operating System:** Kali Linux (running in a dedicated VM)
- **Disassembly / Decompilation:** Ghidra, objdump
- **Debugging:** GDB with PEDA extension
- **Web Proxying:** Burp Suite Community Edition
- **Network Analysis:** Wireshark, tcpdump
- **Forensics:** binwalk, file, xxd, strings, Autopsy
- **Steganography:** stegsolve, zsteg, exiftool
- **Scripting:** Python 3 (pwntools, pycryptodome, requests)
- **Miscellaneous:** CyberChef, curl, netcat

---

## Key Takeaways

Competing in PicoCTF 2026 reinforced several things I already believed about security work, and taught me a few new ones.

The first is that fundamentals are not optional. Every category — web, binary, forensics, crypto — rewards people who actually understand how the underlying systems work. Tools help, but tools applied without understanding rarely get you to the finish line.

The second is that writing things down matters. Keeping notes on what I tried, what failed, and why it failed dramatically reduced the time I spent going in circles on harder challenges. It also made writing these walkthroughs easier after the fact.

The third is time management under pressure. In a competition format, knowing when to move on from a stuck challenge and come back later with fresh eyes is a real skill. Some of my cleanest solves happened after I stepped away from a problem for an hour.

---

Each challenge folder contains a `writeup.md` explaining the thought process and a `screenshots/` folder with annotated images. Screenshots should capture: the initial challenge view, key moments of discovery during analysis, and the moment of successful exploitation — not the flag itself.

---

## Recommended Reading

These resources were genuinely useful during preparation and during the competition itself:

- **The Web Application Hacker's Handbook** — Stuttard & Pinto. Dense, thorough, and still one of the best references for web exploitation fundamentals.
- **Hacking: The Art of Exploitation** — Jon Erickson. Essential for understanding how memory, assembly, and exploitation actually work at a low level.
- **Practical Binary Analysis** — Dennis Andriesse. Highly recommended if binary exploitation or reverse engineering is your focus.
- **Applied Cryptography** — Bruce Schneier. A strong foundation for anyone working in or around cryptographic challenges.
- **CTF Field Guide** — Trail of Bits. Freely available online. Covers most categories you will encounter in any CTF and explains methodology clearly.
- **picoCTF Official Resources** — [picoctf.org/resources](https://picoctf.org/resources). The official learning materials are underrated.
- **pwn.college** — An outstanding free platform for binary exploitation practice, built by the same people who run ASU's security programs.
- **PortSwigger Web Security Academy** — [portswigger.net/web-security](https://portswigger.net/web-security). Best free resource available for web exploitation. Lab-based and well-structured.

---

## Disclaimer

No flags are published in this repository. The walkthroughs describe methodology, thought process, and tooling. This repository exists for educational purposes — to help others learn how to approach these categories of problems, not to hand out answers. If you are actively competing, work through the challenges yourself first.

All challenge names, descriptions, and point values are the property of Carnegie Mellon University and the PicoCTF team.

---

*Write-up authored by eightmonths — PicoCTF 2026*
