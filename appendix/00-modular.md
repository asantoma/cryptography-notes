---
order: 0
label: Modular Arithmetics
description: Operations with modulus
---

## Basic operations


If $a + b = c$ then $a \pmod{m} + b \pmod{m} \equiv c \pmod{m}$

If $a \cdot b = c$ then $a \pmod{m} \cdot b \pmod{m} \equiv c \pmod{m}$

For modulus of negative numbers with respect to $m$, we must add $m$ to the negative number until we get a number which is between $0$ and $m$. For example, $ -8 \pmod{5} = -8 + 5 \pmod{5} = -3 \pmod{5} = -3 + 5 \pmod{5} = 2 \pmod{5} $.

## Modular multiplicative inverse

A modular multiplicative inverse of $a$ with respect to modulus $m$, denoted $a^{-1}$ exists if $aa^{-1} \equiv 1 \pmod {m}$.

$a^{-1}$ exists if and only if $gcd(a, m) = 1$, that is, $a$ and $m$ are *relatively prime* or *coprime*. Thus:

!!!primary
If $m$ is prime, for each $0 < a < m$, there exists a modular multiplicative inverse $a^{-1}$.
!!!

## Euler's totient function

!!!primary
Two numbers are coprime if they don't share any divisors other than 1. 
!!!

Used to find number of numbers coprime to a given number smaller than that number. In short, the number of numbers comprime to $n$ and smaller than $n$ is:

$$ \varphi(n)= n \cdot (1-\frac{1}{p_{1}}) \cdot (1-\frac{1}{p_{2}}) \cdot ... \cdot (1-\frac{1}{p_{k}}) $$

where $p_1, p_2, ..., p_k$ are the distinct prime factors of $n$ (that is, $n = p_{1}^{e_1} \cdot p_{1}^{e_1} \cdot ... \cdot p_{k}^{e_k}$).

For example, $\varphi(20) = 20\cdot(1-1/2)\cdot(1-1/5) = 20\cdot(1/2)\cdot(4/5) = 8$ because $20 = 2^2\cdot 5$. The numbers coprime to 20 and smaller than 20 are 3, 5, 7, 9, 11, 13, 17, 19.

$\varphi(26) = 26\cdot(1-1/2)\cdot(1-1/13) = 26\cdot(1/2)\cdot(12/13)=12$.
