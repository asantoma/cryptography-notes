---
order: -3
title: More About Block Ciphers
---

# More About Block Ciphers

!!!ghost Disclaimer
Information and images are taken from [Understanding Cryptography](http://swarm.cs.pub.ro/~mbarbulescu/cripto/Understanding%20Cryptography%20by%20Christof%20Paar%20.pdf). Additional sources are explicitly mentioned.
!!!

A block cipher is much more than just an encryption algorithm.  For instance, we can use them for building different types of blockbased encryption schemes, and we can even use block ciphers for realizing stream ciphers. The different ways of encryption are called modes of operation and are discussed in this chapter.

Most popular modes of operation are:
* Electronic Code Book mode (ECB)
* Cipher Block Chaining mode (CBC)
* Output Feedback mode (OFB)
* Cipher Feedback mode (CFB)
* Counter mode (CTR)

!!!danger
Initialization Vectors (mentioned below) should be *nonces*, that is random numbers used only once per communication session.
!!!


## Electronic Code Book Mode

The ECB mode implies blockwise encryption of messages split in chunks of the block size. If the length of the message is not a multiple of the block size, padding must be added. Each block is encrypted individually.

The ECB mode has advantages. Block synchronization between the encryption and decryption parties Alice and Bob is not necessary, i.e., if the receiver does not receive all encrypted blocks due to transmission problems, it is still possible to decrypt the received blocks. Also, block ciphers operating in ECB mode can be parallelized.

However, as is often the case in cryptography, there are some unexpected weaknesses associated with the ECB mode which we will discuss in the following. The main problem of the ECB mode is that it encrypts highly deterministically. This means that identical plaintext blocks result in identical ciphertext blocks, as long as the key does not change. The ECB mode can be viewed as a gigantic code book — hence the mode’s name — which maps every input to a certain output. Of course, if the key is changed the entire code book changes, but as long as the key is static the book is fixed. This has several undesirable consequences. First, an attacker recognizes if the same message has been sent twice simply by looking at the ciphertext.

## Cipher Block Chaining Mode

There are two main ideas behind the CBC mode. First, the encryption of all blocks are “chained together” such that ciphertext $y_i$ depends not only on block $x_i$ but on all previous plaintext blocks as well. Second, the encryption is randomized by using an initialization vector (IV).

The ciphertext $y_i$, which is the result of the encryption of plaintext block $x_i$, is fed back to the cipher input and XORed with the succeeding plaintext block $x_{i+1}$. This XOR sum is then encrypted, yielding the next ciphertext $y_{i+1}$, which can then be used for encrypting $x_{i+2}$, and so on. For the first plaintext block $x_{1}$ there is no previous ciphertext. For this an IV is added to the first plaintext, which also allows us to make each CBC encryption nondeterministic.

## Output Feedback Mode

In OFB mode a block cipher is used to build a stream cipher encryption scheme. The key stream is not generated bitwise but instead in a blockwise fashion. The output
of the cipher gives us $b$ key stream bits, where $b$ is the width of the block cipher used, with which we can encrypt $b$ plaintext bits using the XOR operation.

The idea behind the OFB mode is quite simple. We start with encrypting an IV with a block cipher. The cipher output gives us the first set of $b$ key stream bits. The next block of key stream bits is computed by feeding the previous key stream bits back into the block cipher and encrypting it.

So, basically, we are encrypting the IV over and over again. We could get into a cycle by doing this, althought it would be quite unlikely. One advantage of OFB mode is the fact that the keystream can be pregenerated before encryption, as it only depends on the IV.

## Cipher Feedback Mode

Very similar to OFB mode. The difference is that, instead of encrypthing the IV over and over again, in CFB we encrypt ciphertext blocks over and over. We start with encrypting an IV with a block cipher, thus obtaining $b$ keystream bits, then XORing the result with first $b$ bits from the message. The resulted ciperthext is fed into the next cipher.

Unlike OFB, the keystream can't be pregenerated because it depends on the message to be encrypted as well.

## Counter Mode

CTR mode is also used to build a stream cipher encryption scheme that works on blocks. In CTR mode, some bits of the randomly chosen IV are used to represent a counter. For example, if we choose a 128-bit long IV we can split it by considering the most significant 96 bits to be the fixed IV and the remaining 32 bits to act as a counter. In this scenario, we can encrypt $2^32$ blocks before needing to change the IV. The "counter" can be a simple integer counter or a more complex function such as a maximum-length LFSR.

Unlike OFB and CBC mode, CTR mode can be parallelized.

!!!
CBC and CTR are the two block cipher modes recommended by Niels Ferguson and Bruce Schneier.
!!!

