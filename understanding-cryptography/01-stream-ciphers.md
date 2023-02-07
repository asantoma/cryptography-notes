---
order: -1
title: Stream Ciphers
---

# Stream Ciphers

!!!ghost Disclaimer
Information and images are taken from [Understanding Cryptography](http://swarm.cs.pub.ro/~mbarbulescu/cripto/Understanding%20Cryptography%20by%20Christof%20Paar%20.pdf). Additional sources are explicitly mentioned.
!!!

## Introduction

!!!
In practice, block ciphers are used more than stream ciphers.
!!!

How do stream ciphers work? It's simple: each plaintext bit $x_{i}$ is encrypted by adding a keystream bit $s_{i}$ modulo 2 (which is the equivalent of XOR).

Encryption: $y_{i} = x_{i} + s_{i} \pmod{2}$

Decryption: $x_{i} = y_{i} + s_{i} \pmod{2}$

Encryption and decryption are actually the same function.

!!!danger
Generation of values $s_{i}$ is the central issue for the security of stream ciphers
!!!

## RNG and One-Time Pad

### True random number generators (TRNG)

* their output cannot be reproduced
* based on physical processes, such as flipping a coin, throwing dice, or, more realistically, semiconductor noise, clock jittering, radioactive decay
* in cryptography, they are needed for generation of session keys 

### Pseudorandom number generators (PRNG)

* they generate sequences starting from an initial value called **seed**
* their output is not random at all, but entirely deterministic
* however, their output should approximate a sequence of true random numbers as well as possible

### Cryptographically secure pseudorandom number generators (CSPRNG)

* they are special PRNGs that have an important property: **their output is not predictable**
* given $n$ bits of a keystream generated by a CSPRNG:
  * no polynomial algorithm can predict the $n+1$ th bit with better than 50% chance of success
  * the previous $n-1$ bits shouldn't be predictable either

### One-time pad

!!!
A cryptosystem is unconditionally or information-theoretically secure if it cannot be broken even with infinite computational resources.
!!!

A One-Time Pad (OTP) is a stream cipher for which:
* the keystream is generated by a TRNG
* the keystream is only known by the communicating parties
* every keystream bit is only used once

The OTP is unconditionally secure. However, its main disadvantage is the fact that the key's length needs to be equal to the message's length because every keystream bit is only used once. To create a real-world implementation of OTP, an idea is to use a CSPRNG with a secret key as its seed in order to generate the keystream.

## Shift Register-Based Stream Ciphers

An elegant way of realizing long pseudorandom sequences is to use linear feedback shift registers (LFSRs). Even though a plain LFSR produces a sequence with good statistical properties, it is cryptographically weak. However, combinations of LFSRs, such as A5/1 or the cipher Trivium, can make secure stream ciphers.

An LFSR consists of clocked storage elements (flip-flops) and a feedback path. An LFSR with $m$ flip-flops is said to be of degree $m$. The leftmost state bit is computed in the
feedback path, which is the XOR sum of some of the flip-flop values in the previous clock period. Since the XOR is a linear operation, such circuits are called linear feedback shift registers.