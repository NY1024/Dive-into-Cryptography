# Symmetric Cryptosystem Ⅲ

## Triple DES

Now we are studying Triple DES (3DES).

Triple DES (Triple Data Encryption Standard) is a variant of the Data Encryption Standard (DES), also known as DES-EDE (Encrypt-Decrypt-Encrypt). Due to the short key length of DES (56 bits), making it susceptible to brute-force attacks, Triple DES was designed to enhance security.

1. Key Length:
   * Single Key Length:
     * DES uses a 56-bit key, but due to parity bits at positions 8, 16, 24, 32, 40, 48, and 56, the actual effective key length is 56 bits.
   * Triple DES Key Length:
     * Triple DES uses two independent 56-bit keys (usually referred to as Key1 and Key2), or three identical 56-bit keys (Key1, Key2, and Key3), depending on the encryption mode.
2.  Encryption Process:

    1. Encryption: Use Key1 to encrypt the data.
    2. Decryption: Use Key2 to decrypt the result of the previous step.
    3. Encryption: Use Key3 again to encrypt the result of the previous decryption.

    This process is commonly referred to as the EDE mode, where E stands for encryption and D stands for decryption.
3. Key Options:
   * Two-Key Mode:
     * Key1 and Key2 are equal, known as two-key mode.
   * Three-Key Mode:
     * Key1, Key2, and Key3 are all different, known as three-key mode.
4. Encryption Modes:
   * ECB Mode (Electronic Codebook):
     * Each data block is independently encrypted, suitable for independent blocks.
   * CBC Mode (Cipher Block Chaining):
     * Each data block is XORed with the previous block, increasing data correlation.
   * Other Modes:
     * Triple DES can be used in various encryption modes, such as CFB (Cipher Feedback), OFB (Output Feedback), etc.
5. Security:
   * Strong Key Space:
     * The key space of Triple DES is much larger than DES, enhancing resistance to brute-force attacks.
   * Adaptability:
     * With proper key management, one can choose between two-key and three-key modes as needed.
   * Relatively Slow:
     * Due to the multiple rounds of encryption and decryption, Triple DES is relatively slower compared to single DES.
6. Use Cases:
   * Transitional Use:
     * Triple DES is often used for transitional applications to migrate existing DES systems to more secure algorithms like AES.
   * High Security Requirements:
     * In scenarios with high security requirements, Triple DES can still provide sufficient protection.

Overall, while Triple DES addresses some issues of DES in its design, more advanced encryption algorithms like AES have gradually replaced Triple DES with the development of computer technology.

The workflow of 3DES in two-key mode can be represented as follows:

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```

from Crypto.Cipher import DES3
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def triple_des_encrypt(key1, key2, plaintext):
    # Create a Triple DES cipher object with key1 in ECB mode
    cipher = DES3.new(key1, DES3.MODE_ECB)
    # Encrypt the plaintext, ensuring it is padded to the block size
    ciphertext = cipher.encrypt(pad(plaintext, DES3.block_size))
    
    # Create another Triple DES cipher object with key2 in ECB mode
    cipher = DES3.new(key2, DES3.MODE_ECB)
    # Decrypt the previous ciphertext
    decrypted_text = cipher.decrypt(ciphertext)
    
    # Create a third Triple DES cipher object with key1 in ECB mode
    cipher = DES3.new(key1, DES3.MODE_ECB)
    # Encrypt the decrypted text, ensuring it is padded to the block size
    ciphertext = cipher.encrypt(pad(decrypted_text, DES3.block_size))
    
    return ciphertext

def triple_des_decrypt(key1, key2, ciphertext):
    # Create a Triple DES cipher object with key1 in ECB mode
    cipher = DES3.new(key1, DES3.MODE_ECB)
    # Decrypt the ciphertext
    decrypted_text = cipher.decrypt(ciphertext)
    
    # Create another Triple DES cipher object with key2 in ECB mode
    cipher = DES3.new(key2, DES3.MODE_ECB)
    # Encrypt the previously decrypted text
    plaintext = cipher.encrypt(decrypted_text)
    
    # Create a third Triple DES cipher object with key1 in ECB mode
    cipher = DES3.new(key1, DES3.MODE_ECB)
    # Decrypt the encrypted text and unpad the result
    decrypted_text = cipher.decrypt(plaintext)
    
    return unpad(decrypted_text, DES3.block_size)

# Generate two keys (each key is 8 bytes)
key1 = get_random_bytes(8)
key2 = get_random_bytes(8)

# Concatenate the two keys to create a 16-byte key (used for Triple DES)
key = key1 + key2

# Plaintext to be encrypted
plaintext = b'Triple DES encryption and decryption example'

# Encryption
ciphertext = triple_des_encrypt(key, key, plaintext)
print(f'Encrypted result: {ciphertext.hex()}')

# Decryption
decrypted_text = triple_des_decrypt(key, key, ciphertext)
print(f'Decrypted result: {decrypted_text.decode("utf-8")}')

```

This code implements encryption and decryption using the Triple DES (3DES) algorithm. Below is a detailed analysis and explanation of the code:

Encryption function `triple_des_encrypt`:

1. Parameters:
   * `key1` and `key2` are two 8-byte DES keys.
   * `plaintext` is the plaintext to be encrypted.
2. Process:
   * Perform the first round of DES encryption using `key1`.
   * Perform the first round of DES decryption using `key2`.
   * Perform the second round of DES encryption using `key1`.
   * Return the final ciphertext.

Decryption function `triple_des_decrypt`:

1. Parameters:
   * `key1` and `key2` are two 8-byte DES keys.
   * `ciphertext` is the ciphertext to be decrypted.
2. Process:
   * Perform the first round of DES decryption using `key1`.
   * Perform the first round of DES encryption using `key2`.
   * Perform the second round of DES decryption using `key1`.
   * Return the final plaintext.

Main program:

1. Generate keys:
   * Use the `get_random_bytes` function to generate two 8-byte random keys, `key1` and `key2`.
2. Concatenate keys:
   * Concatenate `key1` and `key2` to create a 16-byte key (`key`) for Triple DES.
3. Plaintext to be encrypted:
   * Define a plaintext to be encrypted (`plaintext`).
4. Encryption:
   * Call the `triple_des_encrypt` function for encryption and output the encrypted result.
5. Decryption:
   * Call the `triple_des_decrypt` function for decryption and output the decrypted result.

Note:

* The code uses the ECB (Electronic Codebook) mode, which is a basic block cipher mode. In practical applications, consideration may be given to using more secure block cipher modes, such as CBC (Cipher Block Chaining) mode.
* Using randomly generated keys is a good practice, avoiding the use of fixed keys to enhance system security.

Now, let's learn about the three-key mode of Triple DES (3DES). Its definition is as follows:

<figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Triple DES (3DES) is a variant of the DES algorithm that utilizes three independent keys for data encryption. This mode is commonly referred to as the three-key mode (Triple-Key Triple-DES), which involves the following steps:

1. Key Generation:
   * Generate three independent 8-byte (64-bit) keys, referred to as key1, key2, and key3.
2. Encryption Process:
   * Perform the first encryption round using key1.
   * Perform the first decryption round using key2.
   * Perform the second encryption round using key3.
3. Decryption Process:
   * Perform the first decryption round using key3.
   * Perform the first encryption round using key2.
   * Perform the second decryption round using key1.

The encryption and decryption process for this mode can be succinctly represented as: Ciphertext = E(key3, D(key2, E(key1, Plaintext))). Here, E denotes encryption, and D denotes decryption.

Features and Advantages:

1. Enhanced Security:
   * 3DES provides higher security compared to single DES. While DES is considered insecure, 3DES partially addresses this vulnerability.
2. Backward Compatibility:
   * 3DES maintains backward compatibility with the DES algorithm, allowing systems using 3DES to communicate with systems still employing DES.
3. Adaptability:
   * With the use of three independent keys, 3DES offers a larger key space, improving resistance against cryptographic analysis.

Notes:

1. Performance:
   * 3DES is relatively slower compared to some modern symmetric encryption algorithms because it involves multiple rounds of encryption and decryption operations.
2. Key Management:
   * Key management for 3DES can be relatively complex due to the need to maintain three keys.
3. Security Recommendations:
   * In some new applications, it is recommended to use more modern encryption algorithms like AES for better performance and security.

In summary, 3DES was widely used in the past as an encryption algorithm. However, due to changes in modern encryption requirements, some newer algorithms are now more recommended.

```


from Crypto.Cipher import DES3
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def triple_des_encrypt(key1, key2, key3, plaintext):
    # Concatenate the three keys to create a 24-byte key for Triple DES
    key = key1 + key2 + key3
    # Create a Triple DES cipher object in ECB mode
    cipher = DES3.new(key, DES3.MODE_ECB)
    # Encrypt the plaintext, ensuring it is padded to the block size
    ciphertext = cipher.encrypt(pad(plaintext, DES3.block_size))
    return ciphertext

def triple_des_decrypt(key1, key2, key3, ciphertext):
    # Concatenate the three keys to create a 24-byte key for Triple DES
    key = key1 + key2 + key3
    # Create a Triple DES cipher object in ECB mode
    cipher = DES3.new(key, DES3.MODE_ECB)
    # Decrypt the ciphertext and unpad the result
    decrypted_text = cipher.decrypt(ciphertext)
    return unpad(decrypted_text, DES3.block_size)

# Generate three keys (each key is 8 bytes)
key1 = get_random_bytes(8)
key2 = get_random_bytes(8)
key3 = get_random_bytes(8)

# Plaintext to be encrypted
plaintext = b'Triple DES encryption and decryption example'

# Encryption
ciphertext = triple_des_encrypt(key1, key2, key3, plaintext)
print(f'Encrypted result: {ciphertext.hex()}')

# Decryption
decrypted_text = triple_des_decrypt(key1, key2, key3, ciphertext)
print(f'Decrypted result: {decrypted_text.decode("utf-8")}')

```

This code performs encryption and decryption using three independent keys for Triple DES (3DES) operations.

1. Key Generation:
   * Three independent 8-byte keys, namely key1, key2, and key3, are generated using `get_random_bytes(8)`.
2. Encryption function `triple_des_encrypt`:
   * Takes three keys and the plaintext to be encrypted as parameters.
   * Concatenates the three keys to create a 16-byte key.
   * Creates a Triple DES encryption object using `DES3.new`, specifying the concatenated key and ECB mode.
   * Encrypts the plaintext using the `encrypt` method, applying PKCS7 padding.
   * Returns the encrypted ciphertext.
3. Decryption function `triple_des_decrypt`:
   * Takes three keys and the ciphertext to be decrypted as parameters.
   * Concatenates the three keys to create a 16-byte key.
   * Creates a Triple DES decryption object using `DES3.new`, specifying the concatenated key and ECB mode.
   * Decrypts the ciphertext using the `decrypt` method, reversing the PKCS7 padding.
   * Returns the decrypted original plaintext.
4. Usage Example:
   * Generates three random keys: key1, key2, and key3.
   * Defines the plaintext to be encrypted.
   * Calls the `triple_des_encrypt` function for encryption, obtaining the ciphertext.
   * Prints the encrypted result.
   * Calls the `triple_des_decrypt` function for decryption, obtaining the decrypted original text.
   * Prints the decrypted result.

In summary, this code serves as a complete example of Triple DES encryption and decryption. It is important to note that Triple DES is a symmetric key encryption algorithm, and due to its cumbersome key management, modern applications often prefer more secure and efficient symmetric key encryption algorithms, such as AES.

## Encryption mode

Now, let's use AES as an example to learn about different modes of operation, starting with ECB, Electronic Codebook mode.

ECB (Electronic Codebook) mode is a working mode in symmetric encryption algorithms, used to apply block cipher algorithms (such as AES) to long messages. Here is a detailed explanation of ECB mode:

Working Principle:

1. Block Encryption:
   * ECB mode divides the entire message to be encrypted into multiple equally-sized blocks. The size of each block is typically the block size of the block cipher algorithm (such as AES), for example, AES has a block size of 128 bits (16 bytes).
2. Independent Encryption:
   * Each block undergoes an independent encryption process using the same key. This means that identical plaintext blocks will always be encrypted into identical ciphertext blocks.
3. Block-by-Block Processing:
   * The message is processed block by block, and each block is processed using the same encryption algorithm and key. Both encryption and decryption are performed independently for each block.

Advantages:

1. Simple and Intuitive:
   * The implementation of ECB mode is relatively simple, making it easy to understand and implement.
2. Parallel Processing:
   * Due to the independent encryption of each block, it is possible to parallelize the processing of each block, improving the speed of both encryption and decryption.

Disadvantages:

1. Identical Block Issue:
   * A major drawback of ECB mode is that for identical input blocks, the output blocks are always the same. This may lead to security issues, especially when dealing with data containing a significant number of identical blocks.
2. Not Suitable for Large Identical Blocks of Data:
   * When there is a large amount of data with identical blocks in the plaintext, ECB mode may leak information, as an attacker can infer plaintext information by observing repeated occurrences of identical blocks.

Usage Considerations:

1. Not Suitable for Streaming Data:
   * ECB mode is not an ideal choice for streaming data. In such cases, better alternatives might include other more secure modes such as CBC, CTR, etc.
2. Not Suitable for High-Security Requirements:
   * Due to the identical block issue, ECB mode is generally not recommended for use in scenarios with high security requirements.

In general, ECB mode might still find utility in specific applications, but in most cases, more secure alternatives (such as CBC, CTR, GCM, etc.) are usually more recommended.

The following diagram illustrates the operation of ECB:

<figure><img src=".gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```

from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def aes_encrypt_ecb(key, plaintext):
    # Create an AES cipher object in ECB mode
    cipher = AES.new(key, AES.MODE_ECB)
    # Pad the plaintext and convert it to bytes
    padded_plaintext = pad(plaintext.encode('utf-8'), AES.block_size)
    # Encrypt the padded plaintext
    ciphertext = cipher.encrypt(padded_plaintext)
    return ciphertext

def aes_decrypt_ecb(key, ciphertext):
    # Create an AES cipher object in ECB mode
    cipher = AES.new(key, AES.MODE_ECB)
    # Decrypt the ciphertext
    decrypted_data = cipher.decrypt(ciphertext)
    # Unpad the decrypted data and convert it to plaintext
    plaintext = unpad(decrypted_data, AES.block_size)
    return plaintext.decode('utf-8')

# Generate a 128-bit key (16 bytes)
key = get_random_bytes(16)

# Input plaintext
plaintext = "Hello, AES ECB mode!"

# Encryption
encrypted_data = aes_encrypt_ecb(key, plaintext)
print("Encrypted Data:", encrypted_data)

# Decryption
decrypted_text = aes_decrypt_ecb(key, encrypted_data)
print("Decrypted Text:", decrypted_text)

```

This code utilizes the AES algorithm in Electronic Codebook (ECB) mode for data encryption and decryption.

1. Key Generation:
   * A random 16-byte (128-bit) key is generated using `get_random_bytes(16)`.
2. Encryption Function (`aes_encrypt_ecb`):
   * Creates an AES encryptor object using the provided key and ECB mode.
   * Encodes the plaintext in UTF-8, then performs PKCS7 padding (`pad`) to fit the AES block size.
   * Uses the AES encryptor to encrypt the padded plaintext, producing the ciphertext.
3. Decryption Function (`aes_decrypt_ecb`):
   * Creates an AES decryptor object using the provided key and ECB mode.
   * Decrypts the ciphertext using the AES decryptor to obtain the decrypted data.
   * Performs PKCS7 reverse padding (`unpad`) on the decrypted data to restore the original plaintext.
4. Example Usage:
   * Generates a random key.
   * Encrypts the plaintext using the key and prints the ciphertext.
   * Decrypts the ciphertext using the same key and prints the decrypted plaintext.

ECB mode is a fundamental mode for AES, where the entire plaintext is divided into blocks, and each block is independently encrypted. This results in the same plaintext blocks producing identical ciphertext blocks, posing security issues, especially for identical plaintext blocks. Therefore, in practical applications, more secure modes such as Cipher Block Chaining (CBC) are often preferred.



Learning CBC Cipher Block Chaining Mode

CBC mode (Cipher Block Chaining) is a symmetric encryption cipher block chaining mode used in block ciphers (such as AES). In CBC mode, each plaintext block is XORed with the previous ciphertext block before encryption. The result of this operation is then encrypted. This chaining mechanism ensures that identical plaintext blocks appear differently in the ciphertext, enhancing security.

Steps of CBC mode:

1. Initialization Vector (IV):
   * CBC mode requires an Initialization Vector (IV), which is a random, public, and key-independent input. The IV is XORed with the plaintext in the first block.
2. Block Operations:
   * Divide the plaintext into blocks, each with a size matching the encryption algorithm's block size.
   * XOR the first block with the IV and then encrypt it.
   * XOR each subsequent block with the previous ciphertext block before encryption.
3. Encryption:
   * The encrypted ciphertext is a series of blocks XORed with the IV and the previous ciphertext block.
4. Decryption:
   * Decrypt the ciphertext blocks to obtain a series of intermediate values.
   * XOR each intermediate value with the previous ciphertext block to obtain the plaintext block.

Characteristics and Considerations:

* Chaining Effect:
  * CBC mode introduces information from the previous block through the chaining effect, enhancing security.
* Initialization Vector (IV):
  * The choice of IV is crucial for security, preferably a random and unpredictable value.
* Parallel Operation:
  * Each block in CBC mode depends on the encryption result of the previous block, making parallel encryption and decryption challenging and a performance disadvantage.
* Padding:
  * Padding, usually using PKCS#7, is required when the last block is less than a complete block size.
* Error Propagation:
  * An error in one bit of a ciphertext block affects the decryption of subsequent blocks, as each block's decryption depends on the result of the previous block's decryption.

In summary, CBC mode is a relatively secure mode, but attention should be paid to the reasonable selection of the IV and ensuring the implementation of other security measures. Below is the workflow of CBC mode.

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Function to encrypt using AES in CBC mode
def aes_encrypt_cbc(key, plaintext):
    # Generate a random Initialization Vector (IV)
    iv = get_random_bytes(AES.block_size)
    
    # Create an AES cipher object in CBC mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_CBC, iv)
    
    # Pad the plaintext and encrypt it
    padded_plaintext = pad(plaintext.encode('utf-8'), AES.block_size)
    ciphertext = cipher.encrypt(padded_plaintext)
    
    # Concatenate the IV and ciphertext and return the result
    return iv + ciphertext

# Function to decrypt using AES in CBC mode
def aes_decrypt_cbc(key, ciphertext):
    # Extract the IV from the ciphertext
    iv = ciphertext[:AES.block_size]
    
    # Create an AES cipher object in CBC mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_CBC, iv)
    
    # Decrypt the ciphertext and unpad the result
    decrypted_data = cipher.decrypt(ciphertext[AES.block_size:])
    plaintext = unpad(decrypted_data, AES.block_size)
    
    # Decode the plaintext and return it
    return plaintext.decode('utf-8')

# Generate a random 128-bit key (16 bytes)
key = get_random_bytes(16)

# Input plaintext
plaintext = "Hello, AES CBC mode!"

# Encrypt the plaintext
encrypted_data = aes_encrypt_cbc(key, plaintext)
print("Encrypted Data:", encrypted_data)

# Decrypt the ciphertext
decrypted_text = aes_decrypt_cbc(key, encrypted_data)
print("Decrypted Text:", decrypted_text)

```

This code implements encryption and decryption using the AES algorithm in CBC mode:

1. Key Generation:
   * `key = get_random_bytes(16)`: Generates a 128-bit (16-byte) random key.
2. Encryption Function `aes_encrypt_cbc`:
   * `iv = get_random_bytes(AES.block_size)`: Generates a random Initialization Vector (IV) with a size matching the AES block size.
   * `cipher = AES.new(key, AES.MODE_CBC, iv)`: Creates an AES encryptor using CBC mode and the generated IV.
   * `padded_plaintext = pad(plaintext.encode('utf-8'), AES.block_size)`: Pads the input plaintext using PKCS#7 to make its length a multiple of the AES block size.
   * `ciphertext = cipher.encrypt(padded_plaintext)`: Encrypts the padded plaintext using the AES encryptor.
   * `return iv + ciphertext`: Returns a byte string containing the IV and ciphertext.
3. Decryption Function `aes_decrypt_cbc`:
   * `iv = ciphertext[:AES.block_size]`: Extracts the first AES block-sized bytes from the ciphertext as the IV.
   * `cipher = AES.new(key, AES.MODE_CBC, iv)`: Creates an AES decryptor using CBC mode and the extracted IV.
   * `decrypted_data = cipher.decrypt(ciphertext[AES.block_size:])`: Decrypts all ciphertext blocks following the first block using the AES decryptor.
   * `plaintext = unpad(decrypted_data, AES.block_size)`: Reverses the PKCS#7 padding on the decrypted data to obtain the final plaintext.
   * `return plaintext.decode('utf-8')`: Converts the decrypted plaintext to a UTF-8 encoded string and returns it.
4. Example Usage:
   * Generates a 128-bit random key.
   * Inputs the plaintext string "Hello, AES CBC mode!".
   * Encrypts using the AES algorithm in CBC mode, returning a byte string containing the IV and ciphertext.
   * Prints the encrypted result.
   * Decrypts using the same key and ciphertext, obtaining the original plaintext.
   * Prints the decrypted result.

This code demonstrates the basic usage of the AES algorithm in CBC mode, including encryption and decryption processes. CBC mode enhances security by utilizing an Initialization Vector (IV) and the chaining effect. In practical applications, it is crucial to make informed choices regarding key management, storage, and ensuring the security of the IV.

Learning Cipher Feedback (CFB) Mode

CFB (Cipher Feedback) mode is a block cipher chaining mode that allows converting block ciphers into self-synchronizing stream ciphers. In CFB mode, the encryption algorithm operates on the previous ciphertext block instead of the plaintext block.

Basic Principles:

1. Initialization: CFB mode is initialized using an Initialization Vector (IV). The size of the IV is typically equal to the block size.
2. Encryption Process:
   * Initial Step: Input the IV into the encryption algorithm to obtain the first ciphertext block.
   * Subsequent Steps: Input the previous ciphertext block into the encryption algorithm to obtain the next ciphertext block.
3. Decryption Process:
   * Decryption is the same as encryption, using the IV or the previous ciphertext block as input to generate the decrypted plaintext block.
4. Feedback Mechanism: CFB mode achieves feedback by using the previous ciphertext block as the input for the next block. This feedback mechanism creates a stream cipher, where the encryption of each block depends on the ciphertext of the preceding block.
5. Stream Cipher: CFB mode can transform block ciphers into stream ciphers. A stream cipher generates one bit or a stream of bits at a time. In CFB mode, the rate at which ciphertext is produced is the same as the rate of input plaintext.

Advantages and Considerations:

* Real-time Encryption: CFB mode allows real-time encryption and decryption without waiting for the entire message to be cached.
* Self-Synchronization: As each block's encryption depends only on the previous block's ciphertext, CFB mode is self-synchronizing. This means that if some data is lost during transmission, it only affects the corresponding block without impacting the decryption of other blocks.
* Considerations: CFB mode requires selecting an appropriate IV. Reusing the IV across multiple messages may lead to security issues. Key security is also critical.

Example Use Cases:

* Real-time Communication: CFB mode is suitable for communications requiring real-time encryption, such as voice or video streams.
* Serial Transmission: CFB mode can be used to encrypt long messages in chunks without waiting for the entire message to be available.

In summary, CFB mode is a practical block cipher chaining mode that can provide real-time and self-synchronizing capabilities based on the application's requirements. The workflow of CFB mode is illustrated below.

<figure><img src=".gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

# Function to encrypt using AES in CFB mode
def aes_encrypt_cfb(key, plaintext):
    # Generate a random Initialization Vector (IV)
    iv = get_random_bytes(AES.block_size)
    
    # Create an AES cipher object in CFB mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_CFB, iv)
    
    # Encrypt the padded plaintext
    ciphertext = cipher.encrypt(pad(plaintext.encode('utf-8'), AES.block_size))
    
    # Concatenate the IV and ciphertext and return the result
    return iv + ciphertext

# Function to decrypt using AES in CFB mode
def aes_decrypt_cfb(key, ciphertext):
    # Extract the IV from the ciphertext
    iv = ciphertext[:AES.block_size]
    
    # Create an AES cipher object in CFB mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_CFB, iv)
    
    # Decrypt the ciphertext and unpad the result
    decrypted_data = cipher.decrypt(ciphertext[AES.block_size:])
    plaintext = unpad(decrypted_data, AES.block_size)
    
    # Decode the plaintext and return it
    return plaintext.decode('utf-8')

# Generate a random 128-bit key (16 bytes)
key = get_random_bytes(16)

# Input plaintext
plaintext = "Hello, AES CFB mode!"

# Encrypt the plaintext
encrypted_data = aes_encrypt_cfb(key, plaintext)
print("Encrypted Data:", encrypted_data)

# Decrypt the ciphertext
decrypted_text = aes_decrypt_cfb(key, encrypted_data)
print("Decrypted Text:", decrypted_text)

```

This code utilizes the Cipher Feedback (CFB) mode of the AES algorithm for encrypting and decrypting data. Below is a detailed analysis of the code:

1. Importing Modules:
   * Crypto.Cipher: Provides encryption and decryption functionality for cryptographic algorithms.
   * Crypto.Random: Used for generating random byte sequences, typically for initializing Initialization Vectors (IV).
   * Crypto.Util.Padding: Used to add and remove padding, ensuring that data block sizes comply with the requirements of cryptographic algorithms.
2. Encryption Function `aes_encrypt_cfb`:
   * Generates a random Initialization Vector (IV).
   * Creates an AES cipher object using AES.new, selecting CFB mode, and initializing it with the provided key and IV.
   * Pads the plaintext to ensure its size is a multiple of the AES block size.
   * Encrypts the padded plaintext using the cipher object.
   * Returns a byte sequence containing the IV and ciphertext.
3. Decryption Function `aes_decrypt_cfb`:
   * Extracts the Initialization Vector (IV) from the ciphertext.
   * Creates an AES cipher object using AES.new, selecting CFB mode, and initializing it with the provided key and IV.
   * Decrypts the ciphertext using the cipher object.
   * Removes the padding from the decrypted data to obtain the original plaintext.
   * Returns the decrypted plaintext string.
4. Generating the Key:
   * Generates a random 128-bit key.
5. Inputting the Plaintext:
   * Specifies the plaintext string to be encrypted.
6. Encryption:
   * Calls the `aes_encrypt_cfb` function, passing the key and plaintext to obtain the encrypted data.
7. Decryption:
   * Calls the `aes_decrypt_cfb` function, passing the key and the encrypted data to obtain the decrypted plaintext.
8. Printing the Results:
   * Outputs the encrypted data and the decrypted plaintext.

Considerations:

* Randomness of Initialization Vector (IV): The IV should be random and unpredictable to ensure security.
* Key Management: The generation and management of keys are crucial for security. Use a secure random number generator to generate keys and store them securely.
* Character Encoding: Consistent character encoding is important during encryption and decryption; UTF-8 is used here.
* Padding: Padding is used to ensure that the size of the plaintext conforms to the block size requirements of the cryptographic algorithm. The PKCS7 padding scheme is utilized here.

In summary, this code demonstrates how to encrypt and decrypt using the AES algorithm's CFB mode and serves as a basic encryption example.

Learning Output Feedback Mode (OFB) Now

Output Feedback Mode (OFB): OFB is a block cipher mode used in symmetric encryption algorithms such as AES. In the OFB mode, an initialization vector (IV) is input into the encryption algorithm, and the output is used as a key stream. The key stream is then XORed with the plaintext to generate the ciphertext. The ciphertext, in turn, serves as the input for the next round, continuing the process.

Steps in the OFB Mode:

1. Initialization: The initialization vector (IV) is passed as input to the encryption algorithm, generating the first key stream.
2. Key Stream Generation: The first key stream is XORed with the plaintext to produce the first ciphertext. Simultaneously, the first ciphertext becomes the input for the next encryption round, generating the next key stream.
3. Repeat Process: Repeat the above process, using the previous round's ciphertext as input to generate a new key stream, and XOR it with the plaintext.
4. Decryption: Decryption follows the same process as encryption, with ciphertext replacing the position of plaintext.

Considerations and Recommendations:

* Importance of Initialization Vector (IV): The IV must be random and should not be reused under the same key. Leaking or reusing the IV can lead to security issues.
* Key Stream and XOR Operation: The security of OFB mode depends on the generated key stream. The nature of the XOR operation ensures that the same plaintext generates different ciphertexts during the encryption process, enhancing security.
* Passing of Ciphertext: During encryption and decryption, it is crucial to separate the initialization vector (IV) from the ciphertext to ensure the correct use of the same IV during decryption.
* Character Encoding and Padding: Similar to the previously mentioned CFB mode, use consistent character encoding and padding schemes.

OFB mode is a common encryption mode that provides a secure and straightforward way to protect data. The schematic diagram of the OFB mode is shown below.

<figure><img src=".gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes

# Function to encrypt using AES in Output Feedback Mode (OFB)
def aes_encrypt_ofb(key, plaintext):
    # Generate a random initialization vector (IV)
    iv = get_random_bytes(AES.block_size)
    # Create an AES cipher object in OFB mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_OFB, iv)
    # Encrypt the plaintext and concatenate the IV with the ciphertext
    ciphertext = cipher.encrypt(plaintext.encode('utf-8'))
    return iv + ciphertext

# Function to decrypt using AES in Output Feedback Mode (OFB)
def aes_decrypt_ofb(key, ciphertext):
    # Extract the IV from the ciphertext
    iv = ciphertext[:AES.block_size]
    # Create an AES cipher object in OFB mode with the provided key and IV
    cipher = AES.new(key, AES.MODE_OFB, iv)
    # Decrypt the ciphertext, excluding the IV
    decrypted_data = cipher.decrypt(ciphertext[AES.block_size:])
    # Decode the decrypted data to obtain the plaintext
    plaintext = decrypted_data.decode('utf-8')
    return plaintext

# Generate a random 128-bit key (16 bytes)
key = get_random_bytes(16)

# Input plaintext
plaintext = "Hello, AES OFB mode!"

# Encrypt the plaintext using the AES OFB mode
encrypted_data = aes_encrypt_ofb(key, plaintext)
print("Encrypted Data:", encrypted_data)

# Decrypt the ciphertext using the AES OFB mode
decrypted_text = aes_decrypt_ofb(key, encrypted_data)
print("Decrypted Text:", decrypted_text)

```

This code utilizes Python's Crypto library to implement AES in Output Feedback (OFB) mode for both encryption and decryption.

1. AES OFB Encryption Function (`aes_encrypt_ofb`):
   * Generates a random Initialization Vector (IV).
   * Creates an AES OFB mode cipher object with the provided key and IV.
   * Encodes the plaintext in UTF-8 and encrypts it using AES OFB mode.
   * Returns the IV concatenated with the encrypted ciphertext.
2. AES OFB Decryption Function (`aes_decrypt_ofb`):
   * Extracts the Initialization Vector (IV) from the ciphertext.
   * Creates an AES OFB mode cipher object with the provided key and IV.
   * Decrypts the ciphertext, excluding the IV.
   * Decodes the decrypted data in UTF-8, obtaining the original plaintext.

This code primarily demonstrates how to perform encryption and decryption operations using AES in OFB mode. OFB mode is a stream cipher mode that achieves ciphertext feedback by using the output of the previous encryption block as the input for the key stream. During the encryption and decryption processes, there is no direct feedback between the ciphertext and plaintext; instead, an intermediate key stream is employed.

Now learning Counter (CTR) Mode

Counter (CTR) mode is a block cipher mode in symmetric encryption used to convert block ciphers into stream ciphers. CTR mode employs a counter to generate a key stream, which is then XORed with the plaintext to produce ciphertext.

1. Counter: CTR mode utilizes an incrementing counter as input, with a different counter value for each block. The initial value of the counter is chosen by the user of the encryption algorithm and gradually increases during the encryption process.
2. Key Stream Generation: The counter value is used by the encryption algorithm to generate a key stream (also known as a pseudo-random key stream). This key stream is XORed with the plaintext to obtain the ciphertext.
3. Block-by-Block Encryption: The plaintext is divided into blocks, and each block undergoes XOR with the generated key stream to produce the corresponding ciphertext block. This allows CTR mode to achieve parallel encryption and decryption since the encryption/decryption of each block is independent.
4. Decryption Process: As the encryption and decryption processes in CTR mode share the same steps, the decryption process is identical to encryption. The incrementing counter ensures that the same key stream is used for decryption, thereby restoring the ciphertext to plaintext.

Advantages of CTR mode include parallel encryption/decryption and the absence of padding since the counter always increments in blocks. However, CTR mode imposes high security requirements on the counter – it must not have duplicate values during use and must be kept confidential. Reusing the same counter value may lead to key stream reuse, exposing the security of the system.

The schematic diagram of CTR mode is shown below.

<figure><img src=".gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad
from Crypto.Util import Counter

# Function for AES encryption in Counter (CTR) mode
def aes_encrypt_ctr(key, plaintext):
    # Generate a random nonce (half the size of block)
    nonce = get_random_bytes(AES.block_size // 2)
    # Create a counter with a 64-bit prefix using the nonce
    counter = Counter.new(64, prefix=nonce)
    # Create an AES cipher object in CTR mode with the provided key and counter
    cipher = AES.new(key, AES.MODE_CTR, counter=counter)
    # Encrypt the plaintext and concatenate the nonce with the ciphertext
    ciphertext = cipher.encrypt(plaintext.encode('utf-8'))
    return nonce + ciphertext

# Function for AES decryption in Counter (CTR) mode
def aes_decrypt_ctr(key, ciphertext):
    # Extract the nonce from the ciphertext (half the size of block)
    nonce = ciphertext[:AES.block_size // 2]
    # Create a counter with a 64-bit prefix using the nonce
    counter = Counter.new(64, prefix=nonce)
    # Create an AES cipher object in CTR mode with the provided key and counter
    cipher = AES.new(key, AES.MODE_CTR, counter=counter)
    # Decrypt the ciphertext, excluding the nonce
    decrypted_data = cipher.decrypt(ciphertext[AES.block_size // 2:])
    # Decode the decrypted data to obtain the plaintext
    plaintext = decrypted_data.decode('utf-8')
    return plaintext

# Generate a random 128-bit key (16 bytes)
key = get_random_bytes(16)

# Input plaintext
plaintext = "Hello, AES CTR mode!"

# Encrypt the plaintext using AES CTR mode
encrypted_data = aes_encrypt_ctr(key, plaintext)
print("Encrypted Data:", encrypted_data)

# Decrypt the ciphertext using AES CTR mode
decrypted_text = aes_decrypt_ctr(key, encrypted_data)
print("Decrypted Text:", decrypted_text)

```

This code implements encryption and decryption using AES CTR mode.

1. Encryption Function:
   * Generate nonce: Generate a random nonce of half the length of the AES block size using `get_random_bytes`. The nonce serves as the prefix for the initial counter.
   * Create counter object: Use `Counter.new` to create a 64-bit counter object with the generated nonce as a prefix.
   * Create AES encryption object: Create an AES encryption object using the key and CTR mode.
   * Encrypt plaintext: Encrypt the UTF-8 encoded plaintext using the created AES object.
   * Return result: Return a byte string containing the nonce and the ciphertext.
2. Decryption Function:
   * Extract nonce: Extract the first half of the ciphertext as the nonce.
   * Create counter object: Create a 64-bit counter object using the extracted nonce.
   * Create AES decryption object: Create an AES decryption object using the key and CTR mode.
   * Decrypt ciphertext: Decrypt the remaining ciphertext using the created AES object.
   * Return result: Return the decrypted UTF-8 encoded plaintext.

## Reference

1\. [https://xilinx.github.io/Vitis\_Libraries/security/2019.2/guide\_L1/internals/ecb.html](https://xilinx.github.io/Vitis\_Libraries/security/2019.2/guide\_L1/internals/ecb.html)

2\. [https://www.cs.vsb.cz/ochodkova/courses/kpb/cryptography-and-network-security\_-principles-and-practice-7th-global-edition.pdf](https://www.cs.vsb.cz/ochodkova/courses/kpb/cryptography-and-network-security\_-principles-and-practice-7th-global-edition.pdf)
