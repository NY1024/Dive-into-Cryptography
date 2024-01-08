# Symmetric Cryptosystem

## AES

The Advanced Encryption Standard (AES) algorithm is a block cipher encryption algorithm published by the National Institute of Standards and Technology (NIST) in 2000. Its primary objective is to replace the DES algorithm after vulnerabilities were identified in DES. NIST invited experts from around the world engaged in encryption and data security work to introduce an innovative block cipher algorithm for encrypting and decrypting data with a robust and complex structure.

Many organizations from around the world submitted their algorithms, and NIST accepted five algorithms for evaluation. After assessments based on various standards and security parameters, they chose one of the five encryption algorithms proposed by two Belgian cryptographers, Joan Daeman and Vincent Rijmen. The original name of the AES algorithm is Rijndael. However, this name did not become the widely recognized name for the algorithm; instead, it is globally acknowledged as the Advanced Encryption Standard (AES) algorithm.

Some characteristics of AES include:

1. Symmetric Encryption Algorithm AES is a symmetric encryption algorithm, meaning that the same key is used for both encryption and decryption. This key can be of fixed lengths: 128 bits, 192 bits, or 256 bits, depending on the variant of AES used. As the key is the same, both parties must share the key in a secure communication channel when using AES for communication.
2. Block Cipher AES is a block cipher, meaning that it processes data in blocks. In AES, data is divided into fixed-size blocks, with each block being 128 bits. The encryption and decryption processes operate on each block separately, making the algorithm efficient and suitable for most modern computer systems.
3. Key Lengths AES supports different key lengths, including 128 bits, 192 bits, and 256 bits. Longer key lengths generally provide higher security but also require more computational resources.
   * AES-128 uses a 128-bit key.
   * AES-192 uses a 192-bit key.
   * AES-256 uses a 256-bit key.
4. Round Function The core of AES is the round function, which performs multiple rounds of operations during encryption and decryption. Each round includes four steps: SubBytes, ShiftRows, MixColumns, and AddRoundKey. The combination of these steps allows AES to provide robust encryption protection.
   * SubBytes: Byte substitution using the Substitution Box (S-box).
   * ShiftRows: Circular shifting of data rows.
   * MixColumns: Confusion of columns.
   * AddRoundKey: XOR operation of round key with data.
5. Security AES is widely considered a secure and reliable encryption algorithm. Its security is based on the difficulty of key selection and has withstood various cryptographic analyses in practical applications.
6. Applications AES is extensively used to protect sensitive data in computer systems, network communication, storage media, and various applications. For example, in the TLS/SSL protocol, AES is used to encrypt network connections, ensuring the security of data during transmission over the internet.

AES is an iterative rather than a Feistel cipher. It is based on two common techniques for encrypting and decrypting data, known as the Substitution-Permutation Network (SPN). SPN is a series of mathematical operations performed in block cipher algorithms. AES has the ability to process fixed plaintext block sizes of 128 bits (16 bytes). This 16-byte block is represented as a 4x4 matrix, and AES operates on the byte matrix. Another key feature of AES is the number of rounds, which depends on the key length. The algorithm uses three different key sizes to encrypt and decrypt data: 128, 192, or 256 bits, determining the number of rounds (e.g., 10 rounds for 128-bit key, 12 rounds for 192-bit key, and 14 rounds for 256-bit key). The following diagram illustrates the basic structure of AES.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Let's take a closer look at the encryption process. Here's an example of the encryption process:

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Encryption plays a crucial role in protecting data from unauthorized access. The AES algorithm employs a specific structure to encrypt data, providing optimal security. To achieve this, it relies on multiple rounds of operations, with each round comprising four sub-processes. Each round involves the following four steps for encrypting a 128-bit block, denoted as A.

A. Substitute Bytes Transformation:

* The first stage of each round begins with the SubBytes transformation. This phase relies on a non-linear S-box, used to substitute one byte in the state with another byte. Following the Shannon principles of diffusion and confusion, crucial for cryptographic algorithm design to enhance security. For example, in AES, if the state contains the hexadecimal value 53, it must be replaced with the hexadecimal value ED. ED is created by the intersection of 5 and 3. Similar operations must be performed for the remaining bytes in the state.

The following diagram shows the S-box table:

The following diagram illustrates this step:
