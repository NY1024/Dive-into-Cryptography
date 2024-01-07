# Mathematical Fundamentals Ⅱ

## Diffusion and confusion

Claude Shannon is the founder of modern cryptography, and he introduced the concepts of diffusion and confusion in cryptography. These principles are crucial elements in his groundbreaking work, "Communication Theory of Secrecy Systems," published in 1949.

<figure><img src=".gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

In cryptography, diffusion is a process of information propagation aimed at influencing as many bits of the ciphertext as possible by the plaintext. Diffusion ensures that small changes in the input result in significant changes in the output, thereby enhancing the security of the cipher.

The goal of diffusion is to induce changes in every bit of the ciphertext for each bit of the input, ensuring that minor alterations in the input manifest broadly in the output. If a single bit in the input changes, this alteration should be reflected in the ciphertext, ideally in a highly complex manner. Diffusion methods typically involve the movement, rotation, and mixing of bits.

Linear Diffusion: Linear diffusion is achieved through the use of linear operations, such as XOR and AND gates, where each output bit is influenced by a linear combination of input bits. In cryptography, linear diffusion propagates information bits using operators like XOR.

Nonlinear Diffusion: Nonlinear diffusion employs nonlinear operations, typically involving substitution and confusion. Substitution involves replacing input bits with values dependent on the key, while confusion introduces more complex nonlinear functions to increase the complexity of the cryptographic system. Nonlinear diffusion is more challenging to analyze, enhancing the security of the cipher.

Rotation and Shift: Another common diffusion method involves rotation (circular shifting) and shifting operations. This leads to changes in the positions of input bits in the output, achieving diffusion.

Overall, diffusion is a crucial concept in cryptographic algorithms, helping ensure that every part of the ciphertext undergoes sufficient confusion and influence, making the cipher more resistant to decryption attempts.

```
class DiffusionExample:
    def __init__(self):
        # Define the diffusion matrix, here is a simplified example
        self.diffusion_matrix = [
            [1, 2, 3, 4],
            [4, 3, 2, 1],
            [1, 3, 2, 4],
            [4, 2, 1, 3]
        ]

    def diffuse(self, input_bits):
        # Iterate using the diffusion matrix
        output_bits = ''
        for i in range(0, len(input_bits), 4):
            chunk = input_bits[i:i + 4]
            output_chunk = self.apply_diffusion(chunk)
            output_bits += output_chunk
        return output_bits

    def apply_diffusion(self, chunk):
        # Apply the diffusion matrix to a 4-bit chunk
        output_chunk = ''
        for i in range(4):
            output_chunk += str(sum(int(chunk[j]) * self.diffusion_matrix[i][j] for j in range(4)) % 2)
        return output_chunk

# Example usage
diffusion_example = DiffusionExample()

# Input plaintext bits
input_bits = '1101101010110010'

# Perform diffusion operation
output_bits = diffusion_example.diffuse(input_bits)

# Output the result after diffusion
print(f"Plaintext bits: {input_bits}")
print(f"After diffusion: {output_bits}")

```

This code demonstrates a simple diffusion process, using a diffusion matrix to map input bits to output bits. Diffusion is a cryptographic process designed to increase confusion, ensuring that a change in a single input bit will have a complex impact on the output.

1. `DiffusionExample` class:
   * `__init__` method: Initializes the class and defines a simplified diffusion matrix. The matrix is a 4x4 matrix used to illustrate the diffusion operation.
   * `diffuse` method: Takes input bits and iterates using the diffusion matrix. It divides the input bits into 4-bit chunks and applies the diffusion matrix to each chunk.
   * `apply_diffusion` method: Applies the diffusion matrix to a 4-bit chunk. It iterates through each row of the matrix, multiplies each bit of the input block by the corresponding matrix element, and takes the result modulo 2 to obtain each bit of the output block.
2. Example usage:
   * Creates an instance of the `DiffusionExample` class.
   * Defines input plaintext bits, `input_bits`.
   * Uses the `diffuse` method to iteratively perform diffusion on the input.
   * Prints the plaintext bits and the result after diffusion.

In summary, this code snippet illustrates a straightforward diffusion process where a diffusion matrix is employed to ensure that small changes in input bits result in more significant confusion in the output. This is a means of enhancing the security of cryptographic systems in cryptography. In practice, more complex diffusion matrices and operations are often applied in cryptographic algorithms.

In cryptography, confusion is a process that increases the relationship between ciphertext and the key by making the statistical structure of the ciphertext more complex and difficult to analyze, thereby enhancing the security of a cryptographic algorithm.

The goal of confusion is to ensure that each output bit in the ciphertext depends as evenly as possible on both the key and plaintext bits, making the cryptographic algorithm highly sensitive to minor changes in the input. Confusion is typically achieved through techniques such as substitution and permutation.

Here are some common methods of confusion:

1. **Substitution:** Substitution involves replacing input bits with values related to the key. This can be achieved using structures such as substitution boxes (S-boxes) or permutation boxes (P-boxes). Substitution is crucial as it introduces non-linearity, making the cryptographic algorithm more resistant to analysis.
2. **Permutation:** Permutation involves rearranging the input bits. This can be accomplished using permutation boxes or permutation networks. By rearranging the positions of input bits, permutation increases the confusion of the cryptographic algorithm.
3. **Confusion Network:** A confusion network is a structure that increases confusion by using multiple layers of substitution and permutation. The confusion network enhances security by introducing multiple non-linear functions.

Overall, confusion is a critical concept in cryptographic algorithms, ensuring that the statistical properties of the ciphertext become highly unpredictable. By introducing non-linearity and complexity, confusion enhances the ability of a cryptographic system to resist analysis and attacks. Confusion and diffusion are often combined to provide higher security.

Check out the code for confusion.

```
class ConfusionExample:
    def __init__(self):
        # Define the S-box, here is a simplified example
        self.s_box = {
            '0000': '1100',
            '0001': '0110',
            '0010': '1010',
            '0011': '0001',
            '0100': '1000',
            '0101': '0101',
            '0110': '1111',
            '0111': '0010',
            '1000': '0011',
            '1001': '1110',
            '1010': '1001',
            '1011': '0111',
            '1100': '0100',
            '1101': '1011',
            '1110': '1101',
            '1111': '0000',
        }

    def substitute(self, input_bits):
        # Perform substitution using the S-box
        output_bits = ''
        for i in range(0, len(input_bits), 4):
            chunk = input_bits[i:i+4]
            output_bits += self.s_box[chunk]
        return output_bits

# Example usage
confusion_example = ConfusionExample()

# Input plaintext bits
input_bits = '1101101010110010'

# Use substitution for confusion
output_bits = confusion_example.substitute(input_bits)

# Output after substitution
print(f"Plaintext bits: {input_bits}")
print(f"After substitution: {output_bits}")

```

This code demonstrates a simple confusion process where a simplified Substitution Box (S-box) is used to substitute input bits with corresponding output bits. Confusion is a cryptographic process aimed at introducing non-linearity by performing substitution operations on the input, adding complexity and confusion.

1. **ConfusionExample class:**
   * **init method:** Initializes the class and defines a simplified S-box, where each 4-bit input has a corresponding 4-bit output.
   * **substitute method:** Takes input bits and substitutes them using the S-box. It divides the input bits into 4-bit chunks and looks up the S-box, substituting each input chunk with the corresponding output chunk.
2. **Example usage:**
   * Creates an instance of the ConfusionExample class.
   * Defines input plaintext bits (input\_bits).
   * Uses the substitute method to perform substitution on the input.
   * Prints the plaintext bits and the resulting substitution.

Overall, this code snippet illustrates a simple confusion process where an S-box is employed to ensure that the substitution of input bits is not a straightforward linear operation, thereby introducing confusion and non-linearity. In practical cryptographic algorithms, more complex S-boxes and sophisticated confusion operations are often used to enhance the security of cryptographic systems. Confusion is commonly used in conjunction with diffusion to create robust cryptographic schemes.

## Feistel cipher

Feistel ciphers are a design model or structure used to construct various symmetric block ciphers, such as DES. This design model can include reversible, irreversible, and self-reversible components. Additionally, Feistel block ciphers employ the same encryption and decryption algorithms.

The Feistel structure is based on the Shannon structure proposed in 1945, demonstrating the implementation processes of confusion and diffusion. Confusion is achieved by using substitution algorithms to generate a complex relationship between ciphertext and encryption keys. On the other hand, diffusion creates intricate relationships between plaintext and ciphertext using permutation algorithms.

Feistel ciphers propose a structure that alternates between substitution and permutation implementations. Substitution involves replacing plaintext elements with ciphertext, while permutation changes the order of plaintext elements instead of substituting them with another element.

Feistel Cipher Encryption Example The Feistel cipher encryption process involves multiple rounds of processing the plaintext. Each round consists of a substitution step followed by a permutation step. The example below describes the encryption structure of this design model:

\[Here, you would provide the specific example or details illustrating the encryption structure of the Feistel cipher.]

Please note that the Feistel cipher design model offers a flexible and effective approach to building symmetric block ciphers with strong security properties.

<figure><img src=".gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

Step 1 – The first step involves dividing the plaintext into fixed-size blocks, with only one block processed at a time. The input to the encryption algorithm consists of a plaintext block and a key, denoted as K.

Step 2 – The plaintext block is split into two halves. The left half of the plaintext block is represented as LE0, and the right half of the block is represented as RE0. Both halves of the plaintext block (LE0 and RE0) undergo multiple rounds of processing to generate the ciphertext block.

For each round, the encryption function is applied to the right half REi of the plaintext block and the key Ki. The result of the function is then XORed with the left half LEj. XOR, a logical operator used in cryptography, compares two input bits and produces an output bit. The result of the XOR function becomes the new right half REi+1 for the next round. The previous right half REi becomes the new left half LEi+1 for the next round.

The same function is executed for each round, substituting the right half of the plaintext block with the round function. The result of this function is XORed with the left half of the block. Then, a permutation function is applied by swapping the two halves. The result of the permutation is provided to the next round. In essence, this is how the Feistel cipher model alternates between substitution and permutation steps, similar to the Shannon structure mentioned earlier.

Design considerations for Feistel cipher when using block ciphers: Block Size – A larger block size is considered more secure for block ciphers. However, larger block sizes may slow down the execution speed of encryption and decryption processes. Typically, block ciphers have a block size of 64 bits, but modern blocks like AES (Advanced Encryption Standard) use 128 bits.

Analyzability – Block ciphers should be analyzable to identify and address any weaknesses in cryptographic analysis, creating stronger algorithms.

Key Size – Similar to block size, a larger key size is considered more secure, with the trade-off being potential slowing of encryption and decryption processes. Modern ciphers use a 128-bit key, replacing the earlier 64-bit versions.

Number of Rounds – The number of rounds can also impact the security of block ciphers. While more rounds increase security, decryption becomes more complex. Therefore, the number of rounds depends on the desired level of data protection.

Round Function – A complex round function contributes to improving the security of block ciphers.

Subkey Generation Function – The complexity of the subkey generation function makes it more challenging for professional cryptographic analysts to decrypt the cipher.

Fast Software Encryption and Decryption – Using software applications that facilitate faster execution speed for block ciphers is beneficial.

Feistel cipher model uses the same algorithm for both encryption and decryption. There are several key rules to consider during the decryption process:

<figure><img src=".gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

As shown in the diagram above, the ciphertext block consists of two halves, the left half (LD0) and the right half (RD0). Similar to the encryption algorithm, the round function operates on the right half of the ciphertext block with the key K16. The result of the function is XORed with the left half of the ciphertext block. The output of the XOR function becomes the new right half (RD1), while RD0 and LD0 swap positions for the next round. In fact, each round uses the same function, and after a fixed number of rounds, the plaintext block is obtained. In summary, the Feistel structure is a widely used design model for constructing symmetric block ciphers with the following characteristics:

1. Reversibility: The design of the Feistel structure allows for the use of the same steps in both the encryption and decryption algorithms, achieving reversibility. This property is crucial for the implementation of block ciphers, as users need to be able to encrypt and decrypt using the same key.
2. Application of Nonlinear Functions: The key to the Feistel structure is the application of nonlinear functions, contributing to confusion. The use of nonlinear functions increases the complexity of the cipher, making it more challenging to analyze and break.
3. Alternate Substitution and Permutation: The Feistel structure alternately applies substitution and permutation operations during the encryption and decryption processes. This helps achieve confusion and diffusion, enhancing the security of the cipher.
4. Iteration of Round Function: Both encryption and decryption algorithms include multiple rounds, each involving the processing of data by the round function. The repeated iteration of the round function increases the strength of the cipher.
5. Key Expansion and Subkey Generation: The Feistel structure often requires key expansion and the generation of multiple subkeys. These subkeys are used in different rounds, adding complexity to the cipher.
6. Block Size: The block size of Feistel ciphers is a design parameter and is typically smaller. The choice of block size may impact the security and performance of the cipher.
7. Symmetry: The Feistel structure mandates that the steps of encryption and decryption are symmetric, meaning the same algorithm and key can be used for both processes. In conclusion, the Feistel structure is a flexible, reversible, and secure cipher design model that has been successfully applied to many classical ciphers, such as the Data Encryption Standard (DES).



```
def feistel_encrypt(text, rounds, key):
    for _ in range(rounds):
        left, right = text[:len(text)//2], text[len(text)//2:]
        new_right = xor(left, function(right, key))
        new_left = right
        text = new_left + new_right
    return text

def feistel_decrypt(text, rounds, key):
    for _ in reversed(range(rounds)):
        left, right = text[:len(text)//2], text[len(text)//2:]
        new_left = xor(right, function(left, key))
        new_right = left
        text = new_left + new_right
    return text

def function(data, key):
    # Nonlinear function, using simple bitwise shift operation here
    return ''.join(chr((ord(x) + ord(y)) % 256) for x, y in zip(data, key))

def xor(a, b):
    return ''.join([chr(ord(x) ^ ord(y)) for x, y in zip(a, b)])

# Example text
plaintext = "HelloFeistel"

# Generate key
import random
key = ''.join(random.choice('ABCDEFGHIJKLMNOPQRSTUVWXYZ') for _ in range(len(plaintext)//2))

# Encrypt
ciphertext = feistel_encrypt(plaintext, rounds=4, key=key)
print(f"Plaintext: {plaintext}")
print(f"Encrypted: {ciphertext}")

# Decrypt
decrypted_text = feistel_decrypt(ciphertext, rounds=4, key=key)
print(f"Decrypted: {decrypted_text}")

# Check if the decrypted result matches the original plaintext
assert decrypted_text == plaintext, "Decrypted result does not match the original plaintext"

```

This code implements an encryption and decryption algorithm based on the Feistel structure. The Feistel structure is a symmetric cryptographic design used to build block ciphers, where the encryption and decryption algorithms share the same structure.

1. **feistel\_encrypt Function**: This function performs Feistel encryption. It takes three parameters: `text` (the text to encrypt), `rounds` (the number of Feistel rounds), and `key` (the encryption key). In each round, the plaintext is split into left and right halves. The right half undergoes a non-linear function called `function` and is then XORed with the left half. After the specified number of rounds, the encrypted text is returned.
2. **feistel\_decrypt Function**: This function performs Feistel decryption. Similar to encryption, but the rounds are processed in reverse order. The plaintext is split into left and right halves, the right half undergoes the `function` process, and then it is XORed with the left half. After the specified number of rounds, the decrypted text is returned.
3. **function Function**: This is the non-linear function in the Feistel structure. Here, it uses a simple bitwise shift operation, adding the input character to the key character and then taking the modulo 256. In practice, this function can be a more complex non-linear transformation.
4. **xor Function**: Performs XOR operation on two strings.
5. **Sample Text and Key**: Provides an example plaintext "HelloFeistel" and a key generated randomly.
6. **Encryption and Decryption**: Uses the Feistel structure for encryption and decryption, printing the results.
7. **Check Decryption Result**: Uses the assert statement to verify if the decrypted result matches the original plaintext. This is a simple means of validating the correctness of the decryption.

Overall, this code demonstrates the encryption and decryption process based on the Feistel structure. One of the advantages of the Feistel structure is its ability to use the same structure for both encryption and decryption, making the implementation more convenient.

Here, everyone needs to note that the specific implementation of the Feistel cipher structure relies on the following key elements:

1. **Block Cipher Algorithm**: The Feistel cipher structure is typically used in block ciphers, where plaintext is divided into two equal blocks. These two blocks undergo a series of transformations in each round.
2. **Round Function**: The round function is the core of the Feistel cipher structure. It takes half of the plaintext (or ciphertext) as input and a subkey. The round function's role is to confuse and diffuse the input through a series of non-linear transformations and permutation operations. These transformations often include operations like S-box substitution, permutation, shifting, etc., to enhance the complexity and security of the cryptographic system.
3. **Subkey Generation**: To use different subkeys for each round, a key scheduling algorithm is needed to generate the subkeys required for the round function. This typically involves generating a number of different subkeys from the main key.
4. **Number of Rounds**: The design of the Feistel cipher structure allows for the application of the round function multiple times in each round. Both the security and performance of the cipher are related to the number of rounds. Increasing the number of rounds can enhance security but also increases computational costs.
5. **XOR Operation**: In the Feistel cipher structure, XOR (exclusive OR) operation is a fundamental logical operation. XOR is used to combine half of the data with the output of the round function, producing the input for the next round.
6. **Reversibility**: The Feistel cipher structure requires the round function to be reversible because encryption and decryption use the same structure and operations, with only the subkey order reversed. Therefore, the round function must be reversible to ensure the decryption process can correctly reconstruct the plaintext.

The combination and careful design of these elements enable the Feistel cipher structure to achieve effective diffusion and confusion, providing higher cryptographic strength.



## Reference

1\. [https://www.nku.edu/\~christensen/diffusionandconfusion.pdf](https://www.nku.edu/\~christensen/diffusionandconfusion.pdf)

2\. [https://crypto.stackexchange.com/questions/19000/shannon-confusion-and-diffusion-concept](https://crypto.stackexchange.com/questions/19000/shannon-confusion-and-diffusion-concept)

3\. [https://www.researchgate.net/figure/Confusion-and-Diffusion-Process\_fig8\_284001953.](https://www.researchgate.net/figure/Confusion-and-Diffusion-Process\_fig8\_284001953.)

4\. [https://www.tokenex.com/blog/vh-what-is-a-feistel-cipher/](https://www.tokenex.com/blog/vh-what-is-a-feistel-cipher/)
