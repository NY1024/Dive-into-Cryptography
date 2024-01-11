# Symmetric Cryptosystem Ⅰ

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

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

Let's take a closer look at the encryption process. Here's an example of the encryption process:

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Encryption plays a crucial role in protecting data from unauthorized access. The AES algorithm employs a specific structure to encrypt data, providing optimal security. To achieve this, it relies on multiple rounds of operations, with each round comprising four sub-processes. Each round involves the following four steps for encrypting a 128-bit block, denoted as A.

A. Substitute Bytes Transformation:

* The first stage of each round begins with the SubBytes transformation. This phase relies on a non-linear S-box, used to substitute one byte in the state with another byte. Following the Shannon principles of diffusion and confusion, crucial for cryptographic algorithm design to enhance security. For example, in AES, if the state contains the hexadecimal value 53, it must be replaced with the hexadecimal value ED. ED is created by the intersection of 5 and 3. Similar operations must be performed for the remaining bytes in the state.

The following diagram shows the S-box table:

<figure><img src=".gitbook/assets/image (52).png" alt=""><figcaption></figcaption></figure>

The following diagram illustrates this step:

<figure><img src=".gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

B. ShiftRows Transformation: The next step after performing SubByte on the state is the ShiftRows transformation. The primary idea behind this step is to cyclically shift the bytes of the state in each row to the left, except for the bytes in row zero which remain unchanged. In this process, no permutation is done on the bytes in the zeroth row. In the first row, only one byte is cyclically shifted to the left. The second row shifts two bytes to the left, and the last row shifts three bytes to the left. The size of the new state remains unchanged, still maintaining a 16-byte size identical to the original, but the positions of the bytes in the state have changed as depicted in the diagram below.

<figure><img src=".gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

C. MixColumns Transformation: Another crucial step that occurs on the state is the MixColumns transformation. This step involves a matrix transformation on the state, performing multiplication operations on the state. Each byte in a row of the matrix transformation is multiplied with each value (byte) in the state column. In other words, each row of the matrix transformation must be multiplied with each column of the state. The results of these multiplications are combined with XOR operations to produce the new four bytes of the next state. In this step, the size of the state remains unchanged, still maintaining the original 4x4 size, as shown in the diagram below. \[ b1 = (b1 \cdot 2) \oplus (b2 \cdot 3) \oplus (b3 \cdot 1) \oplus (b4 \cdot 1) ] This process continues until all columns of the state have been processed.

<figure><img src=".gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

D. AddRoundKey Transformation: The AddRoundKey stage is the most crucial phase in the AES algorithm. Both the key and input data (also known as the state) are structured as a 4x4 byte matrix. The diagram below illustrates how the 128-bit key and input data are distributed into the byte matrix. During the process of encrypting data, AddRoundKey contributes to enhanced security. This operation is based on the relationship between the key and the ciphertext, which is the result from the preceding stage. The output of AddRoundKey is entirely dependent on the user-specified key. Additionally, in this stage, a subkey is used in conjunction with the state. The main key generates subkeys in each round through the use of Rijndael's key schedule. The size of the subkey matches that of the state. By utilizing bitwise XOR, the subkey is added by combining each byte of the state with the corresponding byte of the subkey.

<figure><img src=".gitbook/assets/image (56).png" alt=""><figcaption></figcaption></figure>

View the code for Byte Substitution in AES.

```
# AES Substitution Box (S-box)
s_box = (
    0x63, 0x7C, 0x77, 0x7B, 0xF2, 0x6B, 0x6F, 0xC5, 0x30, 0x01, 0x67, 0x2B, 0xFE, 0xD7, 0xAB, 0x76,
    0xCA, 0x82, 0xC9, 0x7D, 0xFA, 0x59, 0x47, 0xF0, 0xAD, 0xD4, 0xA2, 0xAF, 0x9C, 0xA4, 0x72, 0xC0,
    0xB7, 0xFD, 0x93, 0x26, 0x36, 0x3F, 0xF7, 0xCC, 0x34, 0xA5, 0xE5, 0xF1, 0x71, 0xD8, 0x31, 0x15,
    0x04, 0xC7, 0x23, 0xC3, 0x18, 0x96, 0x05, 0x9A, 0x07, 0x12, 0x80, 0xE2, 0xEB, 0x27, 0xB2, 0x75,
    0x09, 0x83, 0x2C, 0x1A, 0x1B, 0x6E, 0x5A, 0xA0, 0x52, 0x3B, 0xD6, 0xB3, 0x29, 0xE3, 0x2F, 0x84,
    0x53, 0xD1, 0x00, 0xED, 0x20, 0xFC, 0xB1, 0x5B, 0x6A, 0xCB, 0xBE, 0x39, 0x4A, 0x4C, 0x58, 0xCF,
    0xD0, 0xEF, 0xAA, 0xFB, 0x43, 0x4D, 0x33, 0x85, 0x45, 0xF9, 0x02, 0x7F, 0x50, 0x3C, 0x9F, 0xA8,
    0x51, 0xA3, 0x40, 0x8F, 0x92, 0x9D, 0x38, 0xF5, 0xBC, 0xB6, 0xDA, 0x21, 0x10, 0xFF, 0xF3, 0xD2,
    0xCD, 0x0C, 0x13, 0xEC, 0x5F, 0x97, 0x44, 0x17, 0xC4, 0xA7, 0x7E, 0x3D, 0x64, 0x5D, 0x19, 0x73,
    0x60, 0x81, 0x4F, 0xDC, 0x22, 0x2A, 0x90, 0x88, 0x46, 0xEE, 0xB8, 0x14, 0xDE, 0x5E, 0x0B, 0xDB,
    0xE0, 0x32, 0x3A, 0x0A, 0x49, 0x06, 0x24, 0x5C, 0xC2, 0xD3, 0xAC, 0x62, 0x91, 0x95, 0xE4, 0x79,
    0xE7, 0xC8, 0x37, 0x6D, 0x8D, 0xD5, 0x4E, 0xA9, 0x6C, 0x56, 0xF4, 0xEA, 0x65, 0x7A, 0xAE, 0x08,
    0xBA, 0x78, 0x25, 0x2E, 0x1C, 0xA6, 0xB4, 0xC6, 0xE8, 0xDD, 0x74, 0x1F, 0x4B, 0xBD, 0x8B, 0x8A,
    0x70, 0x3E, 0xB5, 0x66, 0x48, 0x03, 0xF6, 0x0E, 0x61, 0x35, 0x57, 0xB9, 0x86, 0xC1, 0x1D, 0x9E,
    0xE1, 0xF8, 0x98, 0x11, 0x69, 0xD9, 0x8E, 0x94, 0x9B, 0x1E, 0x87, 0xE9, 0xCE, 0x55, 0x28, 0xDF,
    0x8C, 0xA1, 0x89, 0x0D, 0xBF, 0xE6, 0x42, 0x68, 0x41, 0x99, 0x2D, 0x0F, 0xB0, 0x54, 0xBB, 0x16
)

def aes_sub_bytes(state):
    for i in range(4):
        for j in range(4):
            # Get the current byte
            byte = state[i][j]
            # Get the substitution value from the S-box
            substitution = s_box[byte]
            # Replace the byte with the substitution value in the state matrix
            state[i][j] = substitution
    return state

# Example
state = [
    [0x32, 0x88, 0x31, 0xE0],
    [0x43, 0x5A, 0x31, 0x37],
    [0xF6, 0x30, 0x98, 0x07],
    [0xA8, 0x8D, 0xA2, 0x34]
]

print("Original state matrix:")
for row in state:
    print(row)

state = aes_sub_bytes(state)

print("\nState matrix after byte substitution:")
for row in state:
    print(row)

```

This code implements the SubBytes operation in the Advanced Encryption Standard (AES) algorithm, which is a byte substitution operation. SubBytes is a non-linear transformation in AES that utilizes a fixed byte mapping table called the Substitution Box (S-box) to replace each byte in the state matrix.

Firstly, the code defines an S-box named s\_box, containing pre-defined mapping values for 256 bytes. The S-box is a crucial component of the AES algorithm, introducing non-linearity and enhancing the security of the encryption through its utilization.

Next, the code defines a function aes\_sub\_bytes(state), which takes a 4x4 state matrix as input and performs the SubBytes transformation on each byte in the matrix. Inside the function, nested loops iterate through each element of the matrix. It retrieves the value of the current byte, looks up the corresponding substitution value in the S-box, and updates the original state matrix with this substitution value. Finally, the function returns the state matrix after the SubBytes transformation.

Finally, the code provides an example by creating a 4x4 state matrix named state. It then calls the aes\_sub\_bytes function to perform the SubBytes transformation on this state matrix and outputs both the original and transformed state matrices. The example state matrix is a simplified input illustration, and in practical applications, larger state matrices and keys would be used.

This code demonstrates the implementation of a crucial step in the AES algorithm – byte substitution through the use of an S-box. This step is essential for introducing non-linearity and enhancing the security of the encryption process.

Check the row shift in AES.

```
def aes_shift_rows(state):
    for i in range(1, 4):  # Do not shift the first row
        state[i] = state[i][i:] + state[i][:i]
    return state

# Example
state = [
    [0x32, 0x88, 0x31, 0xE0],
    [0x43, 0x5A, 0x31, 0x37],
    [0xF6, 0x30, 0x98, 0x07],
    [0xA8, 0x8D, 0xA2, 0x34]
]

print("Original state matrix:")
for row in state:
    print(row)

state = aes_shift_rows(state)

print("\nState matrix after row shift:")
for row in state:
    print(row)

```

This code implements the ShiftRows operation in the AES algorithm, which is used to perform a circular left shift on each row of the state matrix. ShiftRows is a step in the AES algorithm designed to increase confusion and diffusion effects in the ciphertext.

Specific explanations are as follows:

1. def aes\_shift\_rows(state): defines a function named aes\_shift\_rows that takes a 4x4 state matrix as input and performs the ShiftRows transformation.
2. for i in range(1, 4): iterates through each row of the state matrix starting from the second row (no shift for the first row).
3. state\[i] = state\[i]\[i:] + state\[i]\[:i]: for each row, performs a circular left shift on its elements using slicing. Specifically, moves the elements from the i-th position onwards to the beginning of the row, and then moves the beginning part (the first i elements) to the end of the row.
4. return state: returns the state matrix after the ShiftRows transformation.
5. Example: provides an example by creating a 4x4 state matrix named state. It then calls the aes\_shift\_rows function to perform the ShiftRows transformation on this state matrix and outputs both the original and transformed state matrices.

Through this code, it can be observed that the ShiftRows operation is achieved by a circular left shift of the elements in each row. This helps increase the interdependence between elements in the state matrix, thereby enhancing the security of the AES algorithm. In practical applications, such row-shift transformations are performed multiple times in multiple encryption rounds.

Column Mixing in AES.

```
# Polynomial multiplication in the finite field GF(2^8)
def gf_mult(a, b):
    p = 0
    for _ in range(8):
        if b & 1:
            p ^= a
        # If the highest bit is 1, simulate left shift by one and perform modulo 2 division with the polynomial 0x1b
        high_bit_set = a & 0x80
        a = (a << 1) & 0xFF
        if high_bit_set:
            a ^= 0x1b
        b >>= 1
    return p

def mix_columns(state):
    for i in range(4):
        a = state[0][i]
        b = state[1][i]
        c = state[2][i]
        d = state[3][i]

        # Column Mixing operation
        state[0][i] = gf_mult(a, 0x02) ^ gf_mult(b, 0x03) ^ c ^ d
        state[1][i] = a ^ gf_mult(b, 0x02) ^ gf_mult(c, 0x03) ^ d
        state[2][i] = a ^ b ^ gf_mult(c, 0x02) ^ gf_mult(d, 0x03)
        state[3][i] = gf_mult(a, 0x03) ^ b ^ c ^ gf_mult(d, 0x02)

    return state

# Example
state = [
    [0x32, 0x88, 0x31, 0xE0],
    [0x43, 0x5A, 0x31, 0x37],
    [0xF6, 0x30, 0x98, 0x07],
    [0xA8, 0x8D, 0xA2, 0x34]
]

print("Original state matrix:")
for row in state:
    print(row)

state = mix_columns(state)

print("\nState matrix after column mixing:")
for row in state:
    print(row)

```

This code implements the MixColumns operation in the AES algorithm, which is used to mix the columns of the state matrix. MixColumns is a step in the AES algorithm designed to enhance the diffusion effect of the ciphertext. Below is a detailed explanation of the code:

1. def gf\_mult(a, b): This is a polynomial multiplication function in the finite field GF(2^8). In the AES algorithm, polynomial multiplication in the finite field involves modulo 2 multiplication and modulo 2 division with a fixed polynomial 0x1b. This function takes two polynomial coefficients, a and b, and performs modulo 2 multiplication using bitwise operations, simulating left shifts and modulo 2 division. It finally returns the result.
2. def mix\_columns(state): This is the implementation function for the MixColumns operation. For each column, it uses polynomial multiplication in the finite field to mix each element in the column.
3. for i in range(4): Uses a loop to iterate through each column of the state matrix.
4. Column Mixing operation: For each column, performs the column mixing operation on the four elements (a, b, c, d) in the column using polynomial multiplication in the finite field and XOR operations. The specific rules for the mixing operation are detailed in comments within the code.
5. return state: Returns the state matrix after the mixing operation.
6. Example: Provides an example by creating a 4x4 state matrix named state. It then calls the mix\_columns function to perform the MixColumns mixing on this state matrix and outputs both the original and mixed state matrices. MixColumns operation achieves column mixing through polynomial multiplication in the finite field, enhancing the security of the AES algorithm. This operation is part of the multi-round encryption process in the AES algorithm.

Inspecting Round Key Addition in AES.

```
# Adding Round Key operation in AES
def add_round_key(state, round_key):
    for i in range(4):
        for j in range(4):
            # XOR each element of the state matrix with the corresponding element of the round key
            state[i][j] ^= round_key[i][j]
    return state

# Example
state = [
    [0x32, 0x88, 0x31, 0xE0],
    [0x43, 0x5A, 0x31, 0x37],
    [0xF6, 0x30, 0x98, 0x07],
    [0xA8, 0x8D, 0xA2, 0x34]
]

round_key = [
    [0x2B, 0x28, 0xAB, 0x09],
    [0x7E, 0xAE, 0xF7, 0xCF],
    [0x15, 0xD2, 0x15, 0x4F],
    [0x16, 0xA6, 0x88, 0x3C]
]

print("Original state matrix:")
for row in state:
    print(row)

print("\nRound Key:")
for row in round_key:
    print(row)

state = add_round_key(state, round_key)

print("\nState matrix after Round Key addition:")
for row in state:
    print(row)

```

This code implements the AddRoundKey operation in the AES algorithm, which XORs the current state matrix with the round key. AddRoundKey is a crucial step in the AES algorithm, introducing information from the key to enhance the security of the cryptographic algorithm.

1. def add\_round\_key(state, round\_key): This is the implementation function for the AddRoundKey operation. The function takes two parameters, the current state matrix (state) and the current round key (round\_key). Inside the function, a nested loop is used to iterate through each element of the state matrix, performing XOR operations with the corresponding elements of the round key.
2. for i in range(4): The outer loop iterates through the rows of the state matrix.
3. for j in range(4): The inner loop iterates through the columns of the state matrix.
4. state\[i]\[j] ^= round\_key\[i]\[j]: XOR operation is performed on the corresponding elements.
5. return state: The state matrix after the AddRoundKey operation is returned.
6. Example: An example is provided, creating a 4x4 state matrix (state) and a round key (round\_key). The add\_round\_key function is then called to perform the AddRoundKey operation on the state matrix, and the original state matrix, round key, and the resulting state matrix after encryption are output.

In the encryption process of the AES algorithm, AddRoundKey is a step in each round, XORing the current state matrix with the key for mixing key information into the state matrix. This operation enhances the security of the algorithm, ensuring that the influence of the key propagates into the state matrix in each round.

Thus, we have implemented the four key steps of the AES algorithm from scratch.

## Reference

1\. [https://www.simplilearn.com/tutorials/cryptography-tutorial/aes-encryption](https://www.simplilearn.com/tutorials/cryptography-tutorial/aes-encryption)

2\. [https://www.tutorialspoint.com/cryptography/advanced\_encryption\_standard.htm](https://www.tutorialspoint.com/cryptography/advanced\_encryption\_standard.htm)

3\. [https://www.researchgate.net/figure/Basic-Structure-of-AES\_fig1\_317615794](https://www.researchgate.net/figure/Basic-Structure-of-AES\_fig1\_317615794)

4\. 《Advanced Encryption Standard (AES) Algorithm to Encrypt and Decrypt Data》
