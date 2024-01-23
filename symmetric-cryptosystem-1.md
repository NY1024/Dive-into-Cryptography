# Symmetric Cryptosystem Ⅱ

## Implementaion of AES

Now we are learning to implement AES applications using existing libraries.

```
# Import necessary libraries
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Function to generate a random key for AES encryption
def generate_key():
    return get_random_bytes(16)  # 16 bytes for AES-128, 24 bytes for AES-192, 32 bytes for AES-256

# Function to encrypt plaintext using AES in CBC mode
def encrypt(plaintext, key):
    cipher = AES.new(key, AES.MODE_CBC)
    ciphertext = cipher.encrypt(pad(plaintext.encode('utf-8'), AES.block_size))
    return ciphertext, cipher.iv

# Function to decrypt ciphertext using AES in CBC mode
def decrypt(ciphertext, key, iv):
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = unpad(cipher.decrypt(ciphertext), AES.block_size)
    return plaintext.decode('utf-8')

# Example
key = generate_key()
plaintext = "Hello, AES!"
ciphertext, iv = encrypt(plaintext, key)

# Display results
print("Plaintext:", plaintext)
print("Ciphertext:", ciphertext)
print("IV:", iv)

# Decrypt and display the original text
decrypted_text = decrypt(ciphertext, key, iv)
print("Decrypted Text:", decrypted_text)

```

This code utilizes functionalities provided by the Crypto library to implement the encryption and decryption processes of the Advanced Encryption Standard (AES) algorithm. Here's an explanation of the code:

1. `from Crypto.Cipher import AES`: Imports the AES class, the primary component in the Crypto library for implementing AES encryption.
2. `from Crypto.Random import get_random_bytes`: Imports the `get_random_bytes` function, used to generate random byte sequences, i.e., the encryption key.
3. `from Crypto.Util.Padding import pad, unpad`: Imports the `pad` and `unpad` functions for data padding and unpadding, ensuring that the data block to be encrypted meets AES requirements.
4. `def generate_key():`: Defines a function to generate a key. In this example, it uses `get_random_bytes` to generate a random 16-byte sequence as the AES-128 key.
5. `def encrypt(plaintext, key):`: Defines an encryption function that takes plaintext and a key as inputs. Inside the function:
   * `AES.new(key, AES.MODE_CBC)` creates an AES encryption object using the specified key and encryption mode (CBC mode in this case).
   * `pad(plaintext.encode('utf-8'), AES.block_size)` pads the plaintext to ensure its length is a multiple of the AES block size.
   * `cipher.encrypt()` encrypts the padded plaintext, producing ciphertext.
   * Returns the ciphertext and the initialization vector (IV) generated during encryption.
6. `def decrypt(ciphertext, key, iv):`: Defines a decryption function that takes ciphertext, a key, and an IV as inputs. Inside the function:
   * `AES.new(key, AES.MODE_CBC, iv)` creates an AES decryption object using the specified key, encryption mode, and IV.
   * `cipher.decrypt(ciphertext)` decrypts the ciphertext.
   * `unpad()` removes the padding from the decrypted data.
   * Returns the decrypted plaintext.
7. Example: At the end of the code, a random key is generated, and the plaintext "Hello, AES!" is encrypted. The plaintext, ciphertext, and the generated IV are printed. Then, using the same key and IV, the ciphertext is decrypted, and the decrypted plaintext is printed.

This example demonstrates how to use the Crypto library to implement AES encryption and decryption, showcasing the entire process of key generation, padding, encryption, and decryption. In real-world applications, special attention is needed for key protection and management, secure IV generation, etc., to ensure system security.

## Key expansion

Now let's learn about AES key expansion.

The AES algorithm relies on key expansion for encrypting and decrypting data. This is another crucial step in the AES structure where a new key is generated for each round. This section focuses on the technology of AES key expansion. The key expansion routine generates round keys, with each key being a four-byte array. It creates 4x(Nr+1) words, where Nr is the number of rounds. The process is as follows:

The cipher key (initial key) is used to create the first four words. The key size is 16 bytes (k0 to k15), represented in an array as shown below. The first four bytes (k0 to k3) are denoted as w0, the next four bytes (k4 to k7) in the first column are denoted as w1, and so forth. Specific equations can be used to easily calculate and find the key for each round, as shown below:

<figure><img src=".gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

This equation is used to find a key for each round, except for w0. For w0, we must use a specific equation different from the one mentioned above.

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The following diagram illustrates the AES key expansion:

<figure><img src=".gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The pseudocode for the entire process is as follows:

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

AES key expansion algorithm takes a four-word (16-byte) key as input and generates a linear array containing 44 words (176 bytes). This is sufficient to provide a four-word round key for the initial AddRoundKey stage and each round of the cipher, totaling 10 rounds. The pseudocode above describes the key expansion process.

The key is copied into the first four words of the expanded key. The remaining expanded key is filled four words at a time. Each added word, w\[i], depends on the previous word, w\[i-1], and the word four positions back, w\[i-4]. In three-quarters of the cases, a simple XOR operation is used. For words at positions that are multiples of 4 in the w array, a more complex function is employed.

1. RotWord (Word Rotation): Performs a one-byte circular left shift on a word. This means an input word \[B0, B1, B2, B3] is transformed into \[B1, B2, B3, B0].
2. SubWord (Byte Substitution): Applies byte substitution using an S-box to each byte of the input word, using the S-box (Table 5.2a).
3.  The results of steps 1 and 2 are XORed with a round constant Rcon\[j].

    The round constant is a word, where the rightmost three bytes are always 0. Therefore, the effect of XORing a word with Rcon is only performed on the leftmost byte of the word. The round constant is different for each round and is defined as Rcon\[j] = (RC\[j], 0, 0, 0), where RC\[1] = 1, RC\[j] = 2 \* RC\[j - 1], and multiplication is defined in the finite field GF(2^8). In hexadecimal, the values of RC\[j] are as follows:

<figure><img src=".gitbook/assets/image (4) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

For example, suppose the round key for the 8th round is: EA D2 73 21 B5 8D BA D2 31 2B F5 60 7F 8D 29 2F Then, the computation for the first 4 bytes (first column) of the round key for the 9th round is as follows:

<figure><img src=".gitbook/assets/image (5) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The developers behind Rijndael aimed to make the key expansion algorithm resistant to known cryptanalysis attacks. Introducing a round constant dependent on the round number eliminates symmetry or similarity between the ways round keys are generated in different rounds. Specific criteria included in the design are:

* Limited knowledge of the cipher key or a portion of the round key should not allow the calculation of many other round key bits.
* Reversible transformations (i.e., knowledge of any Nk consecutive words of the expanded key should not enable the regeneration of the entire expanded key, where Nk is the key size in words).
* Efficient operation on a wide range of processors.
* The use of round constants to eliminate symmetry.
* Diffusion of changes in the cipher key to the round key; meaning, each key bit should affect many round key bits.
* Sufficient non-linearity to prevent complete determination of round key differences solely from cipher key differences.
* Simplicity in description.

Now, let's take a look at the implementation in Python code.

```
def aes_key_expansion(key):
    # Number of initial round keys
    num_round_keys = 10
    # Number of columns
    nb = 4

    # Initial key with 4 columns
    key_schedule = [key[i:i+nb] for i in range(0, len(key), nb)]

    # AES S-box
    s_box = [
        # ... (s_box values)
    ]

    # Round constants
    rcon = [
        0x01, 0x02, 0x04, 0x08, 0x10, 0x20, 0x40, 0x80, 0x1B, 0x36,
    ]

    # Key expansion
    for i in range(nb, nb * (num_round_keys + 1)):
        temp = key_schedule[i-1]

        if i % nb == 0:
            # Perform circular left shift on each byte, then substitute bytes
            temp = [temp[1], temp[2], temp[3], temp[0]]
            for j in range(4):
                temp[j] = s_box[temp[j]]

            # Check the length of the rcon list to avoid going out of range
            if i // nb < len(rcon):
                # XOR with round constant
                temp[0] ^= rcon[i // nb]

        # XOR with the previous round key
        key_schedule.append([temp[j] ^ key_schedule[i-nb][j] for j in range(4)])

    return key_schedule

# Example
key = [0x2B, 0x7E, 0x15, 0x16, 0x28, 0xAE, 0xD2, 0xA6, 0xAB, 0xF7, 0x97, 0xC9, 0x15, 0xD2, 0x15, 0x4F]

round_keys = aes_key_expansion(key)

print("Initial Key:")
print(key)

print("\nExpanded Round Keys:")
for i, round_key in enumerate(round_keys):
    print(f"Round Key {i}:", round_key)
```

This code implements the AES key expansion algorithm, which is used to generate round keys in AES encryption and decryption.

1. The `aes_key_expansion` function takes an initial key (`key`) and generates a series of round keys. In the AES algorithm, there are a total of 10 rounds, and the length of each key is 128 bits (16 bytes). The function returns a list (`key_schedule`) containing all the round keys.
2. The variable `nb` represents the number of columns, which is 4 for AES.
3. The initial key (`key`) is divided into blocks with a number of columns equal to 4, forming the initial value of `key_schedule`. These four bytes form the initial four round keys.
4. `s_box` is the AES S-box used for byte substitution operations. The S-box is a fixed byte substitution table, and by looking up values in this table, byte substitutions can be performed on input bytes.
5. `rcon` is the round constant, which is a constant array used in the AES key expansion. It enhances the complexity of the key.
6. The subsequent loop, through the `for` loop from `nb` to `nb * (num_round_keys + 1)`, calculates the remaining round keys.
7. For each round key, the following operations are performed:
   * Take the previous round key `temp`.
   * If the position of the round key is a multiple of `nb`, perform circular left shift (and byte substitution) and XOR with the round constant `rcon`.
   * Otherwise, XOR with the previous round key.
8. Finally, the generated round key is added to the `key_schedule` list.
9. The example demonstrates an initial key and the generated expanded round keys.

Overall, this code implements the algorithm for AES key expansion, a critical step in the AES encryption and decryption process, ensuring that each round uses a unique and complex key.

## Avalanche effect

Now let's learn about the Avalanche Effect.

In cryptography, the Avalanche Effect refers to the property where a tiny change in input data should result in a drastic change in the output. This is a crucial characteristic, particularly in symmetric cryptography, and is highly significant in cryptographic hash functions and symmetric encryption algorithms.

<figure><img src=".gitbook/assets/image (7) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

In symmetric encryption algorithms, the Avalanche Effect signifies that even a slight change in the plaintext results in a significant change in the ciphertext. This is a key property for the security of symmetric cryptographic algorithms.

* **Key Sensitivity:** Even with a minor alteration in the key, the Avalanche Effect ensures that every bit of the ciphertext undergoes a substantial change.
* **Security:** The Avalanche Effect prevents attackers from deducing information about the key and algorithm by observing large sets of similar plaintexts and their corresponding ciphertexts. This is because even small differences lead to significant disparities in the ciphertext, making it challenging for attackers to infer information about the encryption key and algorithm.

The Avalanche Effect is a crucial feature for ensuring the security of cryptographic algorithms, as it increases the difficulty for attackers attempting to crack the cryptographic system. Therefore, when designing cryptographic algorithms, a strong Avalanche Effect is an important design objective.

Now, let's take a look at the relevant code.

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
import binascii
from Crypto.Util.Padding import pad, unpad

def aes_encrypt(plaintext, key):
    # Create AES cipher object in ECB mode
    cipher = AES.new(key, AES.MODE_ECB)
    # Encrypt the plaintext with padding
    ciphertext = cipher.encrypt(pad(plaintext, AES.block_size))
    return ciphertext

def display_hex_difference(hex_str1, hex_str2):
    # Calculate the number of differing bits and percentage difference
    diff_count = sum(x != y for x, y in zip(hex_str1, hex_str2))
    total_bits = len(hex_str1) * 4  # Each hex digit is 4 bits
    percentage_diff = (diff_count / total_bits) * 100
    print(f"Difference: {diff_count} bits out of {total_bits} bits ({percentage_diff:.2f}%)")

def main():
    # Generate a random 16-byte key
    key = get_random_bytes(16)

    # Initial plaintext
    plaintext = b'This is a secret!'

    # Encrypt the original plaintext
    original_ciphertext = aes_encrypt(plaintext, key)

    # Change one bit in the plaintext
    modified_plaintext = bytearray(plaintext)
    modified_plaintext[0] ^= 0b00000001  # Flip the least significant bit

    # Encrypt the modified plaintext
    modified_ciphertext = aes_encrypt(modified_plaintext, key)

    # Display the hexadecimal representation of ciphertexts and their differences
    print(f"Original Ciphertext: {binascii.hexlify(original_ciphertext)}")
    print(f"Modified Ciphertext: {binascii.hexlify(modified_ciphertext)}")

    # Display the differences in the Avalanche Effect
    display_hex_difference(
        binascii.hexlify(original_ciphertext).decode(),
        binascii.hexlify(modified_ciphertext).decode()
    )

if __name__ == "__main__":
    main()

```

The purpose of this code is to demonstrate the Avalanche Effect in AES encryption. The Avalanche Effect refers to the property where a small change in input data results in unpredictable and extensive changes in the output. This is a crucial property in cryptography, helping prevent attackers from gaining information about the plaintext or key by analyzing patterns in the ciphertext.

The main steps are as follows:

1. **Generate a random key:** Generate a random 16-byte (128-bit) key using `get_random_bytes(16)`.
2. **Encrypt the original plaintext:** Use the AES algorithm in ECB mode to encrypt the initial plaintext, obtaining the original ciphertext.
3. **Modify one bit in the plaintext:** Simulate a small change in input data by altering one bit in the plaintext, i.e., `modified_plaintext[0] ^= 0b00000001`.
4. **Encrypt the modified plaintext:** Encrypt the modified plaintext using the same key, resulting in the corresponding modified ciphertext.
5. **Display the hexadecimal representation of ciphertexts and differences:** Print the hexadecimal representation of both the original and modified ciphertexts and calculate the bit differences between them.
6. **Display the differences in the Avalanche Effect:** Use the `display_hex_difference` function to compute and display the bit differences in the hexadecimal representations of the original and modified ciphertexts. This function compares two hexadecimal strings, calculating the number and percentage of differing bits.

By modifying one bit in the plaintext, significant changes in the output ciphertext can be observed, demonstrating the essence of the Avalanche Effect. Even with a very small input change, the output should exhibit a high degree of unpredictability.

## Double DES

In response to the vulnerability of DES to attacks, initially, there was a suggestion to use Double DES.

<figure><img src=".gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>

Double DES (Double DES) is a method of encrypting data using two consecutive DES encryption processes. The basic idea is to first encrypt the plaintext with one DES operation and then encrypt the resulting ciphertext with another DES operation, using two different keys. This encryption process can be represented in the following form:

<figure><img src=".gitbook/assets/image (10) (1).png" alt=""><figcaption></figcaption></figure>

Where:

* P represents the plaintext.
* E\_{k\_1} and E\_{k\_2} are two DES encryption functions corresponding to different keys k\_1 and k\_2.
* C represents the final ciphertext.

While Double DES may seem to provide stronger security as it requires two different keys for decryption, in reality, it does not offer more security than single DES.

The primary issue lies in the size of the key space. Since the DES key length is fixed at 56 bits, exhaustive enumeration of all possible keys results in only 2^{56} possibilities. Although Double DES uses two keys, the effective key strength is still only 2^{57} due to the quick feasibility of exhaustive attacks within the key space, significantly lower than the theoretically stronger 2^{112}. This is because in Double DES, if an attacker can obtain an intermediate pair of plaintext and ciphertext, they can use a known-plaintext attack to find both keys, thus compromising the entire system.

Due to this issue with the effective key space, the use of Double DES is not recommended in practice. Instead, more modern symmetric encryption algorithms, such as AES (Advanced Encryption Standard), provide larger key spaces and higher security, making them more preferable. Now, let's take a look at the code for Double DES.

```
from Crypto.Cipher import DES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def double_des_encrypt(message, key1, key2):
    # First round of encryption
    des_cipher1 = DES.new(key1, DES.MODE_ECB)
    encrypted_message = des_cipher1.encrypt(pad(message, DES.block_size))

    # Second round of encryption
    des_cipher2 = DES.new(key2, DES.MODE_ECB)
    final_encrypted_message = des_cipher2.encrypt(encrypted_message)

    return final_encrypted_message

def double_des_decrypt(ciphertext, key1, key2):
    # Second round of decryption
    des_cipher2 = DES.new(key2, DES.MODE_ECB)
    decrypted_message = des_cipher2.decrypt(ciphertext)

    # First round of decryption
    des_cipher1 = DES.new(key1, DES.MODE_ECB)
    final_decrypted_message = unpad(des_cipher1.decrypt(decrypted_message), DES.block_size)

    return final_decrypted_message

# Example usage
message = b"Hello, Double DES Encryption!"
key1 = get_random_bytes(8)  # Generate the first key
key2 = get_random_bytes(8)  # Generate the second key

# Encryption
encrypted_message = double_des_encrypt(message, key1, key2)
print("Encrypted Message:", encrypted_message)

# Decryption
decrypted_message = double_des_decrypt(encrypted_message, key1, key2)
print("Decrypted Message:", decrypted_message.decode())

```

Double DES Encryption Function: This function performs Double DES encryption on plaintext using two DES keys.

1. First Encryption Round (Encryption with Key1):
   * Create a DES encryption object (DES.new(key1, DES.MODE\_ECB)) using the first key (key1) and Electronic CodeBook (ECB) mode.
   * Pad the plaintext using PKCS7 padding (pad(message, DES.block\_size)).
   * Encrypt the padded plaintext using the first key.
2. Second Encryption Round (Encryption with Key2):
   * Create another DES encryption object (DES.new(key2, DES.MODE\_ECB)) using the second key (key2) and ECB mode.
   * Encrypt the result of the first encryption using the second key.
3. Return Result:
   * Return the final encrypted result.

Double DES Decryption Function double\_des\_decrypt:

1. First Decryption Round (Decryption with Key2):
   * Create a DES decryption object using the second key (key2) and ECB mode.
   * Decrypt the ciphertext using the second key.
2. Second Decryption Round (Decryption with Key1):
   * Create another DES decryption object using the first key (key1) and ECB mode.
   * Decrypt the result of the first decryption using the first key.
3. Return Result:
   * Return the final decrypted result.

Example Usage:

* Generate two random 8-byte keys (key1 and key2).
* Encrypt the plaintext.
* Decrypt the ciphertext and output the decrypted result.

## MITM attack

Now, let's study the Meet-in-the-Middle Attack against DES.

The Meet-in-the-Middle Attack is a cryptographic analysis attack targeting bidirectional cipher systems, such as DES. It leverages the symmetry between encryption and decryption operations.

Principle of the Meet-in-the-Middle Attack:

1. Prerequisites:
   * Known pairs of plaintext and corresponding ciphertext (plaintext-ciphertext pairs).
   * The attacker can select a portion of the key space.
2. Attack Steps:
   * Encryption Phase:
     * Utilize keys from a portion of the key space for forward encryption (generate ciphertext from plaintext).
     * Store intermediate values (the state at some round in the Feistel network) and the corresponding ciphertext for each key.
   * Decryption Phase:
     * Use keys from a portion of the key space for reverse decryption (generate intermediate values from ciphertext).
     * Store intermediate values and the decrypted plaintext for each key.
   * Matching Intermediate Values:
     * Search for key pairs with identical intermediate values in both phases.
     * When identical intermediate values are found, the corresponding key pair may be the correct key.
3. Advantages of the Meet-in-the-Middle Attack:
   * In the Meet-in-the-Middle Attack, the attacker can search for keys in two directions through precomputation. This significantly reduces the time complexity of the attack, as it only needs to use a portion of the key space in both directions.

It is important to note that while the Meet-in-the-Middle Attack is a powerful technique, substantial computational and storage resources are still required in practical applications. Therefore, for modern cryptographic algorithms that use an adequate key length, the Meet-in-the-Middle Attack becomes considerably challenging.

Let's take a look at the code.

```
from Crypto.Cipher import DES3
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad
from itertools import product

def generate_keyspace():
    # Generate all possible 8-byte (64-bit) key space
    keyspace = [bytes(k) for k in product(range(256), repeat=8)]
    return keyspace

def double_des_encrypt(message, key1, key2):
    # Create DES3 object, specifying the use of two keys
    des3_cipher = DES3.new(key1 + key2, DES3.MODE_ECB)

    # Encrypt with the first key, simultaneously perform PKCS7 padding
    encrypted_message = des3_cipher.encrypt(pad(message, DES3.block_size))

    # Encrypt with the second key, simultaneously perform PKCS7 padding
    des3_cipher = DES3.new(key2 + key1, DES3.MODE_ECB)
    final_encrypted_message = des3_cipher.encrypt(pad(encrypted_message, DES3.block_size))

    return final_encrypted_message

def meet_in_the_middle_attack(ciphertext, keyspace):
    # Try all possible key combinations
    for key1 in keyspace:
        for key2 in keyspace:
            # Try decryption
            candidate_message = double_des_encrypt(ciphertext, key1, key2)

            # If the decryption result matches the original message, the correct key is found
            if candidate_message == original_message:
                return key1, key2

    return None, None

# Example usage
original_message = b"Hello, Meet-in-the-Middle Attack!"
key1 = get_random_bytes(8)  # Generate the first key
key2 = get_random_bytes(8)  # Generate the second key

# Encrypt the original message
ciphertext = double_des_encrypt(original_message, key1, key2)

# Generate all possible key space
keyspace = generate_keyspace()

# Meet-in-the-Middle Attack
found_key1, found_key2 = meet_in_the_middle_attack(ciphertext, keyspace)

# Output results
print("Original Message:", original_message)
print("Recovered Message:", triple_des_decrypt(ciphertext, found_key1, found_key2).decode())
print("Key1:", found_key1)
print("Key2:", found_key2)

```

This code demonstrates an example of a Meet-in-the-Middle Attack against DES (Data Encryption Standard):

1. **Generate Key Space:**
   * The function generates all possible 8-byte (64-bit) key space. It utilizes `itertools.product` to iterate over each byte's possible values (0 to 255) and combines them into keys.
2. **Double DES Encryption:**
   * The `double_des_encrypt` function performs double DES encryption using two keys. It first encrypts with `key1` and `key2` using DES3, and then encrypts the result again using `key2` and `key1`. This double encryption process simulates the Meet-in-the-Middle Attack.
3. **Meet-in-the-Middle Attack Function:**
   * The `meet_in_the_middle_attack` function attempts a Meet-in-the-Middle Attack by trying all possible key combinations. It iterates over all key pairs in the `keyspace`, calling the `double_des_encrypt` function with these key pairs. If the decryption result matches the original message, it is considered a correct key.
4. **Example Usage:**
   * Random keys `key1` and `key2` are generated using `get_random_bytes`, and the original message is double DES encrypted to obtain `ciphertext`.
   * The entire key space `keyspace` is generated.
   * The Meet-in-the-Middle Attack is executed, attempting to find key pairs that match the original message.
   * The original message, the message recovered through the attack, and the found key pairs are output.

**After execution, it gets killed.**

The term "Killed" typically appears when a program is forcefully terminated by the operating system or other external factors. This can happen for various reasons:

1. **Resource Exceedance:**
   * The program may have consumed excessive resources (such as memory or CPU), prompting the operating system to terminate it to prevent system instability.
2. **Infinite Loop or Recursion:**
   * If the program contains an infinite loop or excessive recursion, it may never complete, leading the operating system to terminate it.
3. **Memory Exhaustion:**
   * If the program's memory usage surpasses the available or allowed limits, it may be terminated.
4. **External Signals:**
   * The program might receive signals from the operating system or other processes, indicating termination. This could result from various reasons, including manual intervention or system constraints.
5. **Program Errors:**
   * There might be errors or vulnerabilities in the code causing unexpected behavior, leading to the program's termination.

In our example, this could be due to resource limitations, resulting in the program being killed.

## Reference

1\. 《Advanced Encryption Standard (AES) Algorithm to Encrypt and Decrypt Data》

2\. [https://www.brainkart.com/article/AES-Key-Expansion\_8410/](https://www.brainkart.com/article/AES-Key-Expansion\_8410/)

3\. [https://en.wikipedia.org/wiki/Avalanche\_effect](https://en.wikipedia.org/wiki/Avalanche\_effect)

4\. [https://www.semanticscholar.org/paper/EFFECTIVE-IMPLEMENTATION-AND-AVALANCHE-EFFECT-OF-Kumar-Tiwari/e15e76133e5eba55fbb2c7817ae9859d6491c98c](https://www.semanticscholar.org/paper/EFFECTIVE-IMPLEMENTATION-AND-AVALANCHE-EFFECT-OF-Kumar-Tiwari/e15e76133e5eba55fbb2c7817ae9859d6491c98c)

5\. [https://www.geeksforgeeks.org/double-des-and-triple-des/](https://www.geeksforgeeks.org/double-des-and-triple-des/)

6\. [https://www.tutorialspoint.com/what-is-double-des](https://www.tutorialspoint.com/what-is-double-des)
