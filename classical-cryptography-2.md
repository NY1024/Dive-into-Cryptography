# Classical Cryptography Ⅲ

## Vernam cipher

Now we are learning Vernam cipher.

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>

The Vernam cipher, also known as the One-Time Pad, is a unique encryption method renowned for its theoretical unbreakability. Invented by Gilbert Vernam in 1917, this cryptographic system is utilized to provide the highest level of confidentiality in communication.

Principles and characteristics:

1. Key and plaintext of equal length: In the Vernam cipher, the key and plaintext must have the same length. Each plaintext character is encrypted using the corresponding key character.
2. One-time use: Each key can only be used once. If the same key is reused for encrypting other messages, the security of the system will be compromised.
3. Randomness: The key must be truly random, and each character should occur with equal probability. This can be achieved through physically generated random numbers or highly complex pseudo-random number generators.
4. Absolute security: The Vernam cipher provides absolute information-theoretic security. This means that even with unlimited computing power and time, an attacker cannot decrypt the ciphertext or predict the key used in the next instance.

Encryption and decryption process:

1. Encryption: For a given position, the encryption operation for plaintext character P and key character K involves performing P XOR K (bitwise exclusive OR) to obtain the ciphertext character C.

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

2. Decryption: The decryption process is identical to the encryption operation. To recover the plaintext character, perform P XOR K (bitwise exclusive OR) on the ciphertext character C and the corresponding key character K at the same position.

<figure><img src=".gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Security and Challenges:

1. Absolute Security: The absolute security of the Vernam cipher relies on the genuine randomness of the key and the one-time use condition. This prevents attackers from deducing any information about the plaintext without obtaining the key.
2. Key Distribution: One challenge of the Vernam cipher is securely distributing the key. The key must be exchanged between communicating parties in a secure manner, which often poses a complex problem.
3. Key Management: Due to the nature of the one-time pad, a sufficient length of random keys must be prepared in advance, which can become impractical in practice.
4. Physical Limitations: Achieving true randomness may require physical processes, such as quantum effects, which could be constrained in certain situations.

In summary, the Vernam cipher offers theoretically absolute security; however, practical issues like key management and distribution limit its widespread use. One can experience encryption and decryption functionality through the following online website: [https://demonstrations.wolfram.com/VernamCipherOneTimePad/](https://demonstrations.wolfram.com/VernamCipherOneTimePad/)

<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

```
import random

def generate_key(length):
    # Generate a random key with the same length as the plaintext
    return ''.join(random.choice('ABCDEFGHIJKLMNOPQRSTUVWXYZ') for _ in range(length))

def vernam_cipher(plain_text, key, mode='encrypt'):
    plain_text = plain_text.upper()
    key = key.upper()

    if len(plain_text) != len(key):
        raise ValueError("Length of plaintext and key must be the same.")

    result_text = ''
    for i in range(len(plain_text)):
        char = plain_text[i]
        key_char = key[i]
        if char.isalpha():
            if mode == 'encrypt':
                encrypted_char = chr((ord(char) + ord(key_char) - 2 * ord('A')) % 26 + ord('A'))
            elif mode == 'decrypt':
                decrypted_char = chr((ord(char) - ord(key_char) + 26) % 26 + ord('A'))
            else:
                raise ValueError("Invalid mode. Use 'encrypt' or 'decrypt'.")
            result_text += encrypted_char if mode == 'encrypt' else decrypted_char
        else:
            result_text += char

    return result_text

# Example
plaintext = "HELLOVERNAM"
key = generate_key(len(plaintext))

encrypted_text = vernam_cipher(plaintext, key, 'encrypt')
decrypted_text = vernam_cipher(encrypted_text, key, 'decrypt')

print("Original Text:", plaintext)
print("Key:", key)
print("Encrypted:", encrypted_text)
print("Decrypted:", decrypted_text)

```

This is an example code implementing the Vernam cipher in Python.

1. `generate_key` function: Generates a random key of the given length. It utilizes the `random.choice` function in Python to select characters from the set of uppercase letters.
2. `vernam_cipher` function: Implements encryption and decryption of the Vernam cipher. For each character, it performs the following operations:

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>

Here, C represents the ciphertext character, P is the plaintext character, and K is the key character. It ensures that encryption and decryption operations are performed only on alphabetical characters, ignoring other characters.

3. Example Section: In this part, a random key of the same length as the plaintext is first generated. Then, the `vernam_cipher` function is used to encrypt and decrypt an example plaintext, and the original text, key, encrypted text, and decrypted text are printed.

## One-time pad

One-Time Pad (OTP) is an extremely special and secure encryption technique with the following notable characteristics:

1. Information-theoretic security: OTP is considered information-theoretically secure. Even with any length of ciphertext, there is not enough information to infer the key or plaintext. This is because each key is completely random and used only once.
2. Absolute security: In theory, properly implemented OTP is absolutely secure. This means that even with any powerful computing capability and resources, attackers cannot crack a correctly implemented OTP system.
3. Key length equals plaintext length: To maintain security, the key length in OTP must be exactly the same as the plaintext length. This requires sharing a key of identical length in advance between communicating parties.
4. Key used only once: To ensure security, the key in OTP is used only once for each encryption. This means that communicating parties must regularly change keys, and each key can only be used for one plaintext.
5. Randomness: The key in OTP must be truly random, not pseudo-random. This ensures that for the same plaintext, each generated ciphertext is different.
6. Ciphertext provides no information: Even if attackers know or guess part of the plaintext, due to the randomness of the key, they still cannot determine the content of other parts.
7. Security depends on key secrecy: The security of OTP largely depends on the secrecy of the key. If the key is compromised or attackers can obtain the key, the security of the system is threatened.

Despite being theoretically secure, OTP faces practical challenges such as secure key distribution and management, as well as ensuring each key is used only once. Due to these practical challenges, the application of OTP in modern encryption is limited, but it is still used in specific scenarios, especially in highly sensitive communications.

## Rail fence Cipher

Now let's study ciphers related to substitution techniques. A typical example is the Rail Fence Cipher. The Rail Fence Cipher is a substitution cipher that is easy to apply, quickly and conveniently scrambling the order of letters in a message. It also has the security of a key, making decryption a bit more challenging. The working principle of the Rail Fence Cipher is to write the message on alternate rows on a page and then read each row sequentially. For example, the plaintext "defend the east wall" is written as follows, with all spaces removed.

<figure><img src=".gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

The simplest Rail Fence Cipher, where each letter is written on the page in a zigzag pattern. Then, the ciphertext is obtained by reading the message in a zigzag manner, starting by writing the top row and then the bottom row, resulting in "DFNTEATALEEDHESWL".

To encrypt a message using the Rail Fence Cipher, you need to write out the message along the page in a zigzag pattern and then read it row by row. First, a key is required, which is the number of rows to be created. Then, start writing the letters of the plaintext along the rightward diagonal until reaching the specified number of rows by the key. Next, bounce back along the diagonal until reaching the first row again. This process continues until the end of the plaintext.

For the plaintext "defend the east wall" used above, with a key of 3, the encryption process is as follows.

<figure><img src=".gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

Rail Fence Cipher with a key of 3. Note the added blank characters at the end of the message to reach the correct length. Also, note that two "X" characters are inserted at the end of the message. These are called null characters and act as placeholders. We do this to neatly fit the message into the rail fence grid (ensuring the same number of letters on the top and bottom rows). While not necessary, this makes the decryption process easier if the message follows this layout. The ciphertext is read row by row, resulting in "DNETLEEDHESWLXFTAAX".

The decryption process of the Rail Fence Cipher involves reconstructing the diagonal grid used to encrypt the message. We start writing the message but place a dash in the yet unoccupied spaces. Gradually, all dashes can be replaced with the corresponding letters and the plaintext can be read from the table. We first create a grid with the same number of rows as the key and the same number of columns as the length of the ciphertext. Then place the first letter in the top-left cell and add dashes where the letters will appear. When we reach the top row again, place the next letter in the ciphertext. Continue this process along the entire row, starting the next row when reaching the end. For example, if we receive the ciphertext "TEKOOHRACIRMNREATANFTETYTGHH" encrypted with a key of 4, we start by placing "T" in the first cell. Then, by filling in the downward diagonal spaces until we return to the top row, place "E" here. Continuing to fill the top row, we get the following pattern.

<figure><img src=".gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>

The above image is the first row of the decryption process for the Rail Fence Cipher. We have a table with 4 rows (as the key is 4) and 28 columns (as the length of the ciphertext is 28). Continuing row by row, we get the consecutive stages shown below.

<figure><img src=".gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

The above image is the second stage of the decryption process.

<figure><img src=".gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

The above image is the third stage of the decryption process.

The above image is the fourth stage of the decryption process. From here, we can now read the plaintext along the diagonal and obtain "they are attacking from the north."

The Rail Fence Cipher is a very simple substitution cipher. However, it is not particularly secure due to the limited number of available keys, especially for short messages (the length of the message needs to be at least twice the length of the key, preferably three times for sufficient letter movement). These can be quickly deciphered manually and even faster using computers.

The impact on security can also be mitigated by using padding characters, as interceptors can use them to identify the ends of rows, providing a reasonable guess for the key. This can be avoided by using more common letters (such as "E") to fill the blank spaces, as they would still be clearly not part of the message for the recipient, appearing at the end of the plaintext. The Rail Fence Cipher can also be used without the use of padding characters.

One method to slightly enhance the security of encryption is to preserve spaces as characters and include them in the encryption table. They are treated in the same way as any other letters. For example, using the plaintext "defend the east wall" and a key of 3, but this time including spaces, we get the table below.

<figure><img src=".gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

The above image shows the Rail Fence Cipher with spaces retained. The positions of spaces are emphasized using color, contrasting with the blank cells in the table. Thus, the ciphertext is read as "DNHAWXEEDTEES ALF TL."

The implementation of the Rail Fence Cipher in a practical environment.

```
def rail_fence_cipher(text, key, mode='encrypt'):
    text = text.upper()
    result_text = ''

    if mode == 'encrypt':
        # Encryption
        for i in range(key):
            result_text += text[i::key]
    elif mode == 'decrypt':
        # Decryption
        cycle = 2 * (key - 1)
        full_cycles = len(text) // cycle
        remainder = len(text) % cycle

        for i in range(full_cycles):
            result_text += text[i * cycle] + text[i * cycle + cycle - 1]
        
        if remainder > 0:
            result_text += text[full_cycles * cycle]
        if remainder > key - 1:
            result_text += text[full_cycles * cycle + key - 1]

        for i in range(1, key - 1):
            for j in range(full_cycles):
                result_text += text[j * cycle + i] + text[(j + 1) * cycle - i]

                if j == full_cycles - 1 and remainder > i:
                    result_text += text[j * cycle + i]

    return result_text

# Example
plaintext = "HELLORAILFENCE"
key = 3

encrypted_text = rail_fence_cipher(plaintext, key, 'encrypt')
decrypted_text = rail_fence_cipher(encrypted_text, key, 'decrypt')

print("Original Text:", plaintext)
print("Encrypted Text:", encrypted_text)
print("Decrypted Text:", decrypted_text)

```

This is a Python code for implementing the Rail Fence Cipher. Here is a detailed analysis of the code:

1. `rail_fence_cipher` function:
   * Parameters:
     * `text`: The text to be encrypted or decrypted.
     * `key`: The number of rows in the rail fence.
     * `mode`: The mode, which can be 'encrypt' or 'decrypt'. Defaults to 'encrypt'.
   * Returns:
     * The text after encryption or decryption.
2. Internal Logic of the function:
   * Convert `text` to uppercase to ensure case consistency.
   * If `mode` is 'encrypt', perform encryption.
   * If `mode` is 'decrypt', perform decryption.
3. Encryption Logic:
   * Iterate through each row using a loop, sequentially adding characters from the text to the result string with a step size of `key`.
   * For example, for "HELLO" and `key` of 3, the encrypted result would be "HL EOL".
4. Decryption Logic:
   * Calculate the cycle, where `cycle = 2 * (key - 1)`.
   * Process both the complete cycles and the remainder part separately.
   * For complete cycles, add two characters to the result string in each iteration.
   * For the remainder part, handle the first and last rows separately.
   * For example, for "HL EOL" and `key` of 3, the decrypted result would be "HELLO".
5. Example Usage:
   * Encrypt with the example text "HELLORAILFENCE" and a key of 3.
   * Decrypt using the same key.
   * Output the original text, encrypted text, and decrypted text.

This code implements the basic functionality of the Rail Fence Cipher, including both encryption and decryption operations.

## Stream cipher

Now let's explore Stream Ciphers and Block Ciphers.

A Stream Cipher is a type of symmetric key cryptographic system, contrasting with block cipher systems. In a stream cipher, encryption and decryption are performed bit by bit (or byte by byte) rather than operating on fixed-size blocks. Stream ciphers use a continuously generated.

<figure><img src=".gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Key stream and the plaintext stream using XOR operation, producing the ciphertext stream.

The main features of stream ciphers include:

1. Key Stream Generator: Stream cipher systems use a key stream generator to produce a continuous key stream. The key stream can be pseudo-random, generated from a relatively short key. This allows the key stream to change continuously throughout the communication process.
2. Encryption and Decryption: Stream ciphers use XOR operation between the key stream and the plaintext to produce ciphertext. During decryption, the ciphertext is XORed again with the same key stream to recover the plaintext.
3. Synchronization: In stream ciphers, both the encryptor and decryptor must remain synchronized, meaning they use the same key stream. This requires initializing the key stream generator at the beginning of communication and ensuring synchronization throughout the process.
4. Real-time Encryption: As stream ciphers operate bit by bit, they can perform real-time encryption and decryption, suitable for securing real-time streaming data, such as communication or audio/video streams.
5. Efficiency: Stream ciphers are generally efficient in both hardware and software implementations since they operate on a single bit or byte without handling large blocks of data.

Classic stream cipher algorithms include RC4 and A5/1, which were widely used in practical applications. However, due to security issues with some stream cipher algorithms, modern cryptography tends to favor block cipher algorithms, especially in protocols like TLS/SSL.

In summary, stream ciphers offer a lightweight and efficient encryption solution suitable for securing real-time streaming data. The security of stream ciphers largely depends on the quality of the key stream and the security of key management.

Block Cipher is a symmetric key cryptographic system that divides plaintext into fixed-size blocks and encrypts each block independently. In both encryption and decryption processes, the same key is used, but each block undergoes encryption and decryption independently.

<figure><img src=".gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

The core idea of block ciphers is to map each block of plaintext to the corresponding block of ciphertext, and this mapping is determined by the key.

Some key features and concepts of block ciphers include:

1. Block Size: Block ciphers divide plaintext into fixed-size blocks, usually in bits. The block size is a crucial parameter for block ciphers, with typical sizes being 64 bits (e.g., DES) or 128 bits (e.g., AES).
2. Rounds: Block ciphers typically use an iterative round structure, applying different keys in each round. Each round involves operations such as substitution, permutation, and confusion. The number of iteration rounds depends on the cipher's design, with AES using 10 rounds (128-bit key) or 14 rounds (256-bit key).
3. Initial and Final Permutation: Block ciphers often apply initial and final permutations at the beginning and end of the encryption and decryption processes. These permutations are used to confuse the initial input and restore the final output.
4. Modes: Block ciphers can be applied to plaintext using different modes. Common modes include Electronic Codebook (ECB), Cipher Block Chaining (CBC), Counter (CTR), etc. These modes determine the relationships between blocks and how ciphertext feedback is handled.
5. Key: Block ciphers use symmetric keys, meaning the same key is used for both encryption and decryption. The key length depends on the cipher algorithm's design, with typical lengths being 128, 192, or 256 bits.
6. Security: The security of block ciphers relies on the secrecy of the key and the strength of the cipher algorithm. Generally, longer key lengths and robust cipher algorithms contribute to the security of block ciphers. Classic block cipher algorithms include DES, 3DES, AES, Blowfish, etc. AES (Advanced Encryption Standard) is one of the most widely used block cipher algorithms, providing high security and efficiency.

In summary, block ciphers offer a universal and flexible encryption framework suitable for various applications. However, due to their block-wise data processing, they are not suitable for real-time streaming data encryption, where stream ciphers are more appropriate. Block ciphers find frequent applications in TLS/SSL, IPsec, file encryption, and other areas.

## Reference

1\. [https://demonstrations.wolfram.com/VernamCipherOneTimePad/](https://demonstrations.wolfram.com/VernamCipherOneTimePad/)

2\. [https://www.cryptomuseum.com/crypto/vernam.htm](https://www.cryptomuseum.com/crypto/vernam.htm)

3\. [https://crypto.interactive-maths.com/rail-fence-cipher.html](https://crypto.interactive-maths.com/rail-fence-cipher.html)

4\. [https://www.okta.com/identity-101/stream-cipher/](https://www.okta.com/identity-101/stream-cipher/)

5\. [https://www.javatpoint.com/block-cipher-vs-stream-cipher](https://www.javatpoint.com/block-cipher-vs-stream-cipher)
