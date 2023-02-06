---
order: -2
label: Hill Cipher
---

# Hill Cipher

!!!ghost Disclaimer
Information and images are taken from [Crypto Corner](https://crypto.interactive-maths.com/). Additional sources are explicitly mentioned.
!!!

The Hill cipher is a polygraphic (that acts on groups of letters, instead of one letter) substitution cipher that is based on matrix operations. This page is focused on $2\times2$ (digraphic) and $3\times3$ (trigraphic) variants of the Hill cipher, but it can be extended to more dimensions. The cipher is more easily explained with an example.

## Encryption

In this scenario, we will use with the English alphabet again. Each letter is represented by its index in the alphabet, starting from 0. We are working with modular operations again $\pmod{26}$. For example, in a $2\times2$ Hill cipher scenario, let's consider the following $2\times2$ key:

$$ K = \begin{pmatrix} H && I \\ L && L \end{pmatrix} = \begin{pmatrix} 7 && 8 \\ 11 && 11 \end{pmatrix} $$

The message "short example" will be encrypted. Spaces will be taken out and letters will be written into digraphs represented by 2-dimensional vectors, as such:

$$ M = \begin{pmatrix} S \\ H \end{pmatrix} \begin{pmatrix} O \\ R \end{pmatrix} \begin{pmatrix} T \\ E \end{pmatrix} \begin{pmatrix} X \\ A \end{pmatrix} \begin{pmatrix} M \\ P \end{pmatrix} \begin{pmatrix} L \\ E \end{pmatrix} = \begin{pmatrix} 18 \\ 7 \end{pmatrix} \begin{pmatrix} 14 \\ 17 \end{pmatrix} \begin{pmatrix} 19 \\ 4 \end{pmatrix} \begin{pmatrix} 23 \\ 0 \end{pmatrix} \begin{pmatrix} 12 \\ 15 \end{pmatrix} \begin{pmatrix} 11 \\ 4 \end{pmatrix} $$ 

To encrypt each digraph, we perform a matrix multiplication. 2-dimensional matrix multiplication is performed in the following way:

$$ \begin{pmatrix} a && b \\ c && d \end{pmatrix} \begin{pmatrix} x && y \end{pmatrix} = \begin{pmatrix} ax + by \\ cx + dy\end{pmatrix} $$

In general, when we multiply two matrices, there are two things to keep in mind:
* the first matrix's column number must be equal to the second matrix's row number
* if the first matrix is of dimension $(M, N)$ and the second matrix is of dimension $(N, P)$, their product is of dimension $(M, P)$
  
The first encrypted digraph is computed as follows:

$$ \begin{pmatrix} 7 && 8 \\ 11 && 11 \end{pmatrix} \begin{pmatrix} 18 \\ 7 \end{pmatrix} = \begin{pmatrix} 182 \\ 275 \end{pmatrix} \pmod{26} = \begin{pmatrix} 0 \\ 15 \end{pmatrix} = \begin{pmatrix} A \\ P \end{pmatrix} $$

Remember we are in the the English alphabet, so we must apply modulus. Each digraph is encrypted like the above operation. After encryption, we can switch back from letter indexes to letters.

## Decryption

For decrpytion, we must compute the inverse of the key matrix.

$$ K^{-1} = d^{-1} \times adj(K) $$, where $d^{-1}$ is the multiplicative modular inverse of $K$'s determinant, and $adj(K)$ is $K$'s adjugate matrix.

$$ d = \begin{vmatrix} a && b \\ c && d \end{vmatrix} = ad - bc $$

To be invertible, a matrix must have a non-zero determinant and, **since we are working in the ring of integers modulo 26**, the determinant must be coprime with 26.

!!!primary
In general, the determinant must be coprime with the alphabet's length.
!!!

$$ adj\begin{pmatrix} a && b \\ c && d \end{pmatrix} = \begin{pmatrix} d && -b \\ -c && a \end{pmatrix} $$

After computing $K^{-1}$, we multiply it with each encrypted digraph to get back to the plaintext. For the example above:

$$ d(K) = \begin{vmatrix} 7 && 8 \\ 11 && 11 \end{vmatrix} = 7 \cdot 11 - 8 \cdot 11 = -11 \pmod {26} = -11 + 26 \pmod{26} = 15 \pmod {26} $$

$$ d^{-1} = 15^{-1} \pmod {26} = 7 \pmod {26} $$, which is a non-zero number coprime with 26.

$$ adj(K) = \begin{pmatrix} 11 && -8 \\ -11 && 7 \end{pmatrix} \pmod{26} = \begin{pmatrix} 11 && 18 \\ 15 && 17 \end{pmatrix} \pmod{26} $$

$$ K^{-1} = 7\begin{pmatrix} 11 && 18 \\ 15 && 17 \end{pmatrix} = \begin{pmatrix} 77 && 126 \\ 165 && 49 \end{pmatrix} \pmod{26} = \begin{pmatrix} 25 && 22 \\ 1 && 23 \end{pmatrix} \pmod{26} $$

Let's try decrypting the first encrypted digraph, $ \begin{pmatrix} A \\ P \end{pmatrix} = \begin{pmatrix} 0 \\ 15 \end{pmatrix} $

$$ K^{-1}\begin{pmatrix} 0 \\ 15 \end{pmatrix} = \begin{pmatrix} 25 && 22 \\ 1 && 23 \end{pmatrix} \begin{pmatrix} 0 \\ 15 \end{pmatrix} = \begin{pmatrix} 25 \cdot 0 + 22 \cdot 15 \\ 1 \cdot 0 + 23 \cdot 15 \end{pmatrix} = \begin{pmatrix} 330 \\ 345 \end{pmatrix} \pmod{26} = \begin{pmatrix} 18 \\ 7 \end{pmatrix} \pmod{26} = \begin{pmatrix} S \\ H \end{pmatrix} $$



