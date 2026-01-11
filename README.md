# TryHackMe GLITCH Machine Writeup

**Machine:** GLITCH  
**Platform:** TryHackMe  
**Difficulty:** Easy  
**Author:** c411ister

## Overview

This repository contains my detailed solution (writeup) for the "GLITCH" room on TryHackMe. The goal of this machine is to find hidden web vulnerabilities and gain root access to the system. It is a great machine to practice API fuzzing, JavaScript analysis, and Linux privilege escalation.

## Steps to Solution

### 1. Reconnaissance
*   I started with an **Nmap scan** to find open ports. Only Port 80 (HTTP) was open.
*   The web page showed a "not allowed" error.
*   I used **Gobuster** to find hidden directories and discovered `/secret`.

### 2. Web Exploitation
*   I analyzed the JavaScript code on the `/secret` page.
*   I found a function called `getAccess()` that generates an access token.
*   I decoded the token (Base64) and hacked the session cookie to bypass the login.
*   I discovered a hidden API endpoint: `/api/items`.

### 3. Remote Code Execution (RCE)
*   The API was vulnerable to Command Injection.
*   Since the Node.js environment was restricted (sandbox), standard commands failed.
*   I used a special payload accessing `global.process.mainModule` to bypass the sandbox.
*   I got a Reverse Shell and gained initial access as the `user`.

### 4. Privilege Escalation
*   **User Flag:** I found the user flag in `/home/user/user.txt`.
*   **Firefox Analysis:** I found a hidden `.firefox` folder. I transferred the profile files (`logins.json` and `key4.db`) to my machine.
*   I decrypted the saved passwords using `firefox_decrypt` and found the password for the user `v0id`.
*   **Root Flag:** I switched to user `v0id`. I checked SUID files and found `doas`. I used `doas /bin/bash` to become root and read the final flag.

## Tools Used
*   Nmap
*   Gobuster
*   Curl
*   Netcat
*   Python (for shell stabilization and decryption)
*   firefox_decrypt

## Disclaimer
This project is for educational purposes only. I performed all actions in a safe and authorized environment provided by TryHackMe.
