# Random Sequence Ⅲ

## PRNG-Block cipher

Now we are learning to construct a Pseudo-Random Number Generator (PRNG) using block ciphers.

A PRNG based on block ciphers is a mechanism that generates a pseudo-random number sequence by utilizing a block cipher algorithm. A block cipher is a symmetric encryption algorithm that uses the same key for both encrypting and decrypting data.

The basic idea of the PRNG mechanism is to apply a block cipher algorithm to an initial seed (or key) and a series of counter values (or other non-secret values) and generate pseudo-random numbers by encrypting the output. The security of this method relies on the security of the block cipher, as well as the use of appropriate input parameters and key management.

Here is a general workflow for a PRNG based on block ciphers:

1. Choose a block cipher algorithm: Select a secure block cipher algorithm, such as AES (Advanced Encryption Standard).
2. Set the key: Choose a key that is long enough and secure as the input key for the block cipher. The key choice is crucial for the security of the PRNG.
3. Initialization: Combine the initial seed and counter values (or other non-secret parameters) as input data for the block cipher. They can be concatenated or combined using a hash function.
4. Encryption: Use the block cipher algorithm to encrypt the initialization data. The encrypted output serves as the first pseudo-random number.
5. Update the state: Use the generated pseudo-random number to update the internal state. This may involve updating a counter or using some parts of the pseudo-random number as input for the next round.
6. Repeat steps 4 and 5: By repeating the encryption and state update, generate the desired number of pseudo-random numbers.

Since the security of the PRNG depends on the security of the block cipher, careful consideration is needed when selecting the block cipher algorithm and key length. Additionally, handling the combination of the initial seed and counter values, as well as the method for updating the internal state, should be done with caution to prevent potential attacks.

Let's take a look at the CTR mode.

CTR (Counter) mode is a working mode for block ciphers and can also be used to construct a Pseudo-Random Number Generator (PRNG). CTR mode allows the transformation of a block cipher algorithm into a stream cipher algorithm, making it suitable for generating long sequences of pseudo-random numbers.

The working principle of a PRNG using CTR mode is as follows:

1. Choose a block cipher algorithm: Select a block cipher algorithm, such as AES (Advanced Encryption Standard).
2. Set the key and initial counter: Choose a secure key and set an initial counter (usually starting from zero).
3. Encrypt the counter: Use the block cipher algorithm to encrypt the counter, obtaining a pseudo-random number block.
4. Update the counter: Increase the value of the counter to prepare for generating the next pseudo-random number block.
5. Repeat steps 3 and 4: By repeating the encryption and counter update, you can generate the desired number of pseudo-random number blocks.
6. Generate a sequence of pseudo-random numbers: Treat each pseudo-random number block as part of the output sequence. You can generate a long sequence by concatenating these blocks.

The advantage of CTR mode is its parallelizability, allowing efficient generation of a large number of pseudo-random numbers. Additionally, since each counter value is associated with a specific block, there is no need to maintain internal state, making it a stateless PRNG.

The CTR algorithm for PRNG can be represented as follows:

<figure><img src=".gitbook/assets/image (64).png" alt=""><figcaption></figcaption></figure>

The following diagram is an example of a Pseudo-Random Number Generator (PRNG) based on the Counter (CTR) mode.

<figure><img src=".gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes

def generate_aes_key():
    return get_random_bytes(16)  # 128-bit key

def generate_ctr_prng(key, nonce, size):
    cipher = AES.new(key, AES.MODE_CTR, nonce=nonce)
    prng_stream = cipher.encrypt(bytes([0] * size))
    return prng_stream

# Generate AES key and a random nonce
aes_key = generate_aes_key()
nonce = get_random_bytes(8)  # 64-bit nonce

# Specify the size of the pseudo-random number to generate
prng_size = 16

# Generate a pseudo-random number stream in CTR mode
prng_stream = generate_ctr_prng(aes_key, nonce, prng_size)

# Print the result
print("CTR PRNG Stream:", prng_stream.hex())

```

Explanation:

* `generate_aes_key`: Function to generate a random 128-bit AES key.
* `generate_ctr_prng`: Function to generate a pseudo-random number stream using CTR mode with AES encryption.
* `aes_key`: Generated AES key using `generate_aes_key`.
* `nonce`: Random 64-bit nonce generated for CTR mode.
* `prng_size`: Size of the pseudo-random number stream to be generated (16 bytes in this example).
* `prng_stream`: The resulting pseudo-random number stream generated using CTR mode.
* `print("CTR PRNG Stream:", prng_stream.hex())`: Display the hexadecimal representation of the generated pseudo-random number stream.

## PRNG-OFB

Now we are learning about Pseudo-Random Number Generators (PRNG) based on the Output Feedback (OFB) mode.

OFB (Output Feedback) is a working mode commonly used to transform block cipher algorithms into stream cipher algorithms. The OFB mode generates a pseudo-random number stream, which is then XORed with the plaintext to produce the ciphertext. OFB does not require padding and can be used to handle variable-length data.

The general working principle of a Pseudo-Random Number Generator (PRNG) using the OFB mode is as follows:

1. Choose a block cipher algorithm: Select a block cipher algorithm, such as AES (Advanced Encryption Standard).
2. Set the key and initial vector: Choose a secure key and set an Initialization Vector (IV).
3. Encrypt the initial vector: Use the block cipher algorithm to encrypt the initial vector, obtaining a pseudo-random number block.
4. Update the initial vector: Treat the pseudo-random number block as the new initial vector.
5. Repeat steps 3 and 4: By repeatedly encrypting and updating the initial vector, generate the desired number of pseudo-random number blocks.
6. Generate a pseudo-random number sequence: Treat each pseudo-random number block as part of the output sequence. You can create a long sequence by concatenating these blocks.

The characteristic feature of the OFB mode is that it generates one pseudo-random number block each time the initial vector is updated, making OFB mode friendly for parallel computation. Since the encryption involves the initial vector rather than the plaintext, the same process can be used during decryption.

The corresponding algorithm can be represented in pseudocode as follows:

<figure><img src=".gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

The workflow is as follows:

<figure><img src=".gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes

def generate_aes_key():
    return get_random_bytes(16)  # 128-bit key

def generate_ofb_prng(key, iv, size):
    cipher = AES.new(key, AES.MODE_OFB, iv=iv)
    prng_stream = cipher.encrypt(bytes([0] * size))
    return prng_stream

# Generate AES key and a random IV
aes_key = generate_aes_key()
iv = get_random_bytes(16)  # 128-bit IV

# Specify the size of the pseudo-random number to generate
prng_size = 16

# Generate a pseudo-random number stream in OFB mode
prng_stream = generate_ofb_prng(aes_key, iv, prng_size)

# Print the result
print("OFB PRNG Stream:", prng_stream.hex())

```

Explanation:

* `generate_aes_key`: Function to generate a random 128-bit AES key.
* `generate_ofb_prng`: Function to generate a pseudo-random number stream using OFB mode with AES encryption.
* `aes_key`: Generated AES key using `generate_aes_key`.
* `iv`: Random 128-bit Initialization Vector (IV) generated for OFB mode.
* `prng_size`: Size of the pseudo-random number stream to be generated (16 bytes in this example).
* `prng_stream`: The resulting pseudo-random number stream generated using OFB mode.
* `print("OFB PRNG Stream:", prng_stream.hex())`: Display the hexadecimal representation of the generated pseudo-random number stream.

The core of this code is to utilize AES and the OFB mode, combining a randomly generated AES key and IV, to produce a pseudo-random number stream. The randomness of the key and IV is crucial for ensuring the security of the pseudo-random numbers. OFB mode possesses the advantage of parallel computation, and the same process can be applied during decryption.

## ANSI X9.17

ANSI X9.17 is one of the standards published by the American National Standards Institute (ANSI) and defines a Pseudo-Random Number Generator (PRNG) used by financial institutions. The PRNG in ANSI X9.17 is considered to be quite secure and robust in the field of cryptography. This standard is primarily employed in the financial industry, especially for the generation of encryption keys.

Key features and principles of the PRNG in ANSI X9.17:

1. Based on Block Cipher Algorithms: The PRNG in ANSI X9.17 typically relies on block cipher algorithms such as DES (Data Encryption Standard) or 3DES (Triple DES). These algorithms are used to generate pseudo-random number sequences.
2. Seed and Key: The PRNG accepts a seed and a key as input. These two input values collectively influence the generated pseudo-random number sequence.
3. Counter and Feedback: The PRNG employs a counter to keep track of the number of generated pseudo-random numbers. The feedback mechanism uses the output from the previous generation as input for the next round, enhancing randomness.
4. Refresh and Update: ANSI X9.17 specifies when it is necessary to refresh the seed or update the key. This is done to maintain the security and strength of the pseudo-random numbers.
5. Security Strength: The design of ANSI X9.17 aims to provide a high level of cryptographic security, preventing the prediction and analysis of pseudo-random number sequences. This is achieved by using cryptographically secure block cipher algorithms and appropriate parameter settings.
6. Periodicity: The PRNG in ANSI X9.17 should exhibit sufficient periodicity to ensure that the same sequence is not repeated within a predictable timeframe under normal usage.

The following diagram illustrates its operational concept.

<figure><img src=".gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import DES3
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad

class ANSI_X9_17_PRNG:
    def __init__(self, key, seed, counter_size=4):
        if len(seed) != 8:
            raise ValueError("Seed should be 64 bits")
        if counter_size not in {4, 8}:
            raise ValueError("Counter size should be 32 or 64 bits")

        self.key = key
        self.seed = seed
        self.counter_size = counter_size
        self.counter = 0

    def generate_block(self):
        cipher = DES3.new(self.key, DES3.MODE_ECB)
        counter_bytes = self.counter.to_bytes(self.counter_size, 'big')
        plaintext = self.seed + counter_bytes
        ciphertext = cipher.encrypt(pad(plaintext, 8))
        self.counter += 1
        return ciphertext

    def generate_bytes(self, size):
        result = b""
        while size > 0:
            block = self.generate_block()
            result += block[:size]
            size -= len(block)
        return result

# Generate 3DES key and seed
des3_key = get_random_bytes(24)  # 192-bit key for 3DES
seed = get_random_bytes(8)  # 64-bit seed

# Create an ANSI X9.17 PRNG instance
ansi_prng = ANSI_X9_17_PRNG(des3_key, seed)

# Generate a pseudo-random number stream
prng_stream = ansi_prng.generate_bytes(16)

# Print the result
print("ANSI X9.17 PRNG Stream:", prng_stream.hex())

```

Explanation:

* `ANSI_X9_17_PRNG`: Class representing the ANSI X9.17 PRNG with methods for block generation and byte generation.
* `generate_block`: Method to generate a block of pseudo-random numbers using DES3 encryption in ECB mode.
* `generate_bytes`: Method to generate a specified number of pseudo-random bytes by calling `generate_block`.
* `des3_key`: Randomly generated 192-bit key for 3DES.
* `seed`: Randomly generated 64-bit seed.
* `ansi_prng`: Instance of the ANSI X9.17 PRNG class.
* `prng_stream`: Generated pseudo-random number stream using ANSI X9.17 PRNG.
* `print("ANSI X9.17 PRNG Stream:", prng_stream.hex())`: Display the hexadecimal representation of the generated pseudo-random number stream.

Overall, this code implements the Pseudo-Random Number Generator as specified by the ANSI X9.17 standard. It is important to note that the use of Triple DES and a seed with sufficient strength is crucial for ensuring the security of the generated pseudo-random numbers.

## LFSR

Now we are learning about Linear Feedback Shift Registers (LFSR).

<figure><img src=".gitbook/assets/image (69).png" alt=""><figcaption></figcaption></figure>

Linear Feedback Shift Register (LFSR) is a special type of shift register widely used in digital circuits and communication systems. The distinctive feature of LFSR is its linear feedback mechanism, where the value of a new bit is a linear combination of the old bits in the register. LFSRs are commonly used in applications such as pseudo-random number generation, sequence generation, error detection, and correction.

Basic characteristics and principles of LFSR:

1. Register: An LFSR comprises a set of storage units, with each unit storing a binary bit (0 or 1). These bits are arranged in a specific order, forming a register.
2. Feedback Polynomial: The operation of an LFSR is controlled by a special polynomial known as the feedback polynomial. This polynomial determines which bits' values are used to calculate the value of the new bit.
3. Shift: During each clock cycle, all bits in the LFSR shift left by one position, and the value of the leftmost bit is calculated using the feedback polynomial. The new rightmost bit's value is typically an input bit or a fixed value.
4. Feedback Mechanism: The value of the new leftmost bit is a linear combination of selected bits based on the chosen feedback polynomial. Usually, this is achieved by performing XOR operations on specific bits in the feedback polynomial.
5. Clock: The LFSR performs a shift and feedback operation in each clock cycle.

Applications:

1. Pseudo-Random Number Generator: LFSRs can serve as simple yet effective pseudo-random number generators, producing long-period pseudo-random sequences by appropriately selecting the feedback polynomial.
2. Sequence Generation: LFSRs are used to generate specific sequences, such as Gold sequences, for modulation and demodulation applications.
3. CRC Coding: In communication systems, LFSRs are employed to calculate Cyclic Redundancy Check (CRC) codes for detecting and correcting errors during transmission.
4. Cryptanalysis: In cryptography, understanding and analyzing the state and feedback polynomial of LFSRs are crucial for deciphering cryptographic systems.

Example:

A simple three-bit LFSR can be represented as:

<figure><img src=".gitbook/assets/image (70).png" alt=""><figcaption></figcaption></figure>

Here, Q(t) represents the LFSR state at time t, and the symbol ⊕ denotes the XOR operation. In the diagram above, the LFSR state shifts left by one position every clock cycle, and the value of the new bit is obtained by performing an XOR operation on two preceding bits.

LFSR is a fundamental and crucial component in digital circuits. It possesses advantages such as simplicity, efficiency, and the ability to achieve long periods, making it widely applicable in various applications.

```
class LFSR:
    def __init__(self, initial_state, feedback_mask):
        # Initialize the LFSR with the given initial state and feedback mask
        self.state = initial_state
        self.feedback_mask = feedback_mask

    def shift(self):
        # Perform a shift operation on the LFSR state
        feedback_bit = sum((self.state >> i) & 1 for i in range(len(self.feedback_mask)) if self.feedback_mask[i]) % 2
        self.state = (self.state >> 1) | (feedback_bit << (len(self.feedback_mask) - 1))
        return feedback_bit

    def generate_sequence(self, num_bits):
        # Generate a pseudo-random sequence of specified length
        sequence = []
        for _ in range(num_bits):
            sequence.append(self.state & 1)
            self.shift()
        return sequence

# Example: a 3-bit LFSR with initial state 0b101 and feedback mask [True, False, True] (representing x^2 + 1)
initial_state = 0b101
feedback_mask = [True, False, True]
lfsr = LFSR(initial_state, feedback_mask)

# Generate and print 10 pseudo-random bits
random_sequence = lfsr.generate_sequence(10)
print("Generated Sequence:", random_sequence)

```

Explanation:

* `LFSR`: Class representing a Linear Feedback Shift Register with methods for initialization, shifting, and generating pseudo-random sequences.
* `__init__`: Constructor method initializes the LFSR with the provided initial state and feedback mask.
* `shift`: Method performs a shift operation on the LFSR state based on the feedback mask, returning the feedback bit.
* `generate_sequence`: Method generates a pseudo-random sequence of specified length using the LFSR shift operation.
* Example initializes a 3-bit LFSR with an initial state of 0b101 and a feedback mask \[True, False, True] representing the polynomial x^2 + 1.
* The generated sequence of 10 pseudo-random bits is printed..

This implementation is a simple LFSR model designed to illustrate the basic principles of LFSR. In practical applications, LFSRs can be more complex, with more bits and different feedback polynomials to achieve longer periods of pseudo-random sequences. LFSRs find numerous applications in communication, cryptography, and digital systems, such as generating pseudo-random sequences, calculating CRC codes, and more.

## Reference

1\. [https://www.brainkart.com/article/Pseudorandom-Number-Generation-Using-a-Block-Cipher\_8424/](https://www.brainkart.com/article/Pseudorandom-Number-Generation-Using-a-Block-Cipher\_8424/)

2\. [https://www.cryptologie.net/article/152/pseudo-random-number-generators-using-a-block-cipher-in-ctr-mode/](https://www.cryptologie.net/article/152/pseudo-random-number-generators-using-a-block-cipher-in-ctr-mode/)

3\. [http://wiki.kmu.edu.tw/index.php/ANSI\_X9.17\_%E8%99%9B%E6%93%AC%E4%BA%82%E6%95%B8%E7%94%A2%E7%94%9F%E5%99%A8](http://wiki.kmu.edu.tw/index.php/ANSI\_X9.17\_%E8%99%9B%E6%93%AC%E4%BA%82%E6%95%B8%E7%94%A2%E7%94%9F%E5%99%A8)

4\. [https://www.researchgate.net/figure/ANSI-X917-Pseudorandom-Number-Generator\_fig1\_267297736](https://www.researchgate.net/figure/ANSI-X917-Pseudorandom-Number-Generator\_fig1\_267297736)

5\. [https://www.researchgate.net/figure/8-bit-Linear-Feedback-Shift-Register\_fig3\_322515484](https://www.researchgate.net/figure/8-bit-Linear-Feedback-Shift-Register\_fig3\_322515484)

6\. [https://en.wikipedia.org/wiki/Linear-feedback\_shift\_register](https://en.wikipedia.org/wiki/Linear-feedback\_shift\_register)

7\. [https://inst.eecs.berkeley.edu/\~cs150/sp03/handouts/15/LectureA/lec27-2up.pdf](https://inst.eecs.berkeley.edu/\~cs150/sp03/handouts/15/LectureA/lec27-2up.pdf)
