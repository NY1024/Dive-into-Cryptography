# Asymmetric Cryptosystem Ⅲ

Now we are learning the Diffie-Hellman key exchange protocol. Before delving into the specific principles, let's first look at an example. The Diffie-Hellman key exchange establishes a shared secret between two parties, enabling secure communication over a public network. Using colors instead of extremely large numbers, an analogy diagram illustrates the concept of public key exchange:

The process begins with two parties, Alice and Bob, openly agreeing on an arbitrary starting color, which doesn't need to be kept secret. In this example, the color is yellow. Each person also chooses a secret color they keep to themselves - in this case, red and cyan. The key part of this process is that Alice and Bob respectively blend their secret color with the color they share in common, resulting in orange and light blue mixtures. They then publicly exchange these two mixed colors. Finally, each person blends the color they received from the other party with their own secret color. The result is the ultimate color mixture (in this case, yellow-brown), matching their partner's final color mixture.

If a third party eavesdrops on this exchange, they would only know the public color (yellow) and the color mixed in the first instance (orange and light blue). However, it would be challenging for them to deduce the ultimate secret color (yellow-brown). Bringing this analogy back to real-life exchanges using large numbers instead of colors, this decision is computationally expensive. Even for modern supercomputers, calculating it within a practical timeframe is impossible.

The entire process is illustrated in the diagram below.

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now let's take a look at the specific steps of the protocol:

1. Parameter Selection: Between the communicating parties, first, two large prime numbers, p and g, are selected. Here, p is a prime number, and g is an integer between 1 < g < p-1.
2. Publicizing Parameters: The communicating parties publicly disclose the selected p and g.
3. Private Key Generation: Each communicating party generates a private random number (private key). Assume Alice chooses private key a, and Bob chooses private key b.
4. Public Key Generation: Each communicating party calculates a public key based on the selected p, g, and their own private key. Alice calculates A = g^a mod p, and Bob calculates B = g^b mod p.
5. Public Key Exchange: The communicating parties exchange the calculated public keys. Alice sends A to Bob, and Bob sends B to Alice.
6. Key Calculation: Each communicating party uses the received public key from the other party and their own private key to calculate the shared key. Alice calculates K = B^a mod p, and Bob calculates K = A^b mod p.
7. Key Determination: Since K is the same on both ends, Alice and Bob now have a shared key that can be used for symmetric key encryption in communication.

<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Example illustration:

Assume:

* p = 23 (prime number)
* g = 5 (generator) Alice chooses private key a = 6, calculates A = 5^6 mod 23 = 8, and sends A to Bob. Bob chooses private key b = 15, calculates B = 5^15 mod 23 = 19, and sends B to Alice. Alice, upon receiving B, calculates K = B^a mod 23 = 19^6 mod 23 = 2. Bob, upon receiving A, calculates K = A^b mod 23 = 8^15 mod 23 = 2. Now, Alice and Bob both have the same shared key K = 2, which can be used for symmetric key encryption in communication.

The security of the Diffie-Hellman key exchange protocol is based on the difficulty of the discrete logarithm problem. The protocol's security relies on a mathematical problem, specifically the difficulty of finding x for g^x mod p in a finite field. In the Diffie-Hellman protocol, g is the generator, p is the prime number, and g^x mod p is the public part. Finding x is challenging, especially when p and g are chosen to be sufficiently large.

The computational complexity of solving the discrete logarithm problem is exponential, meaning that for large prime numbers p, even with the most powerful computers, it takes a considerable amount of time to calculate x. This makes it impractical for an attacker to easily find the key through brute force.

Diffie-Hellman provides perfect forward secrecy. Even if the long-term secret key is compromised, past and future keys remain secure. This is because each session uses a unique private key that does not directly impact other sessions.

While the security of the Diffie-Hellman protocol regarding the discrete logarithm problem is well-established, it is essential to note that if the chosen p and g are not sufficiently large, there may be threats from some specific attacks. Therefore, to ensure security, it is recommended to choose sufficiently large prime numbers p and appropriate generator g.

```
import random

def mod_exp(base, exp, mod):
    # Modular exponentiation to avoid direct use of the pow function
    result = 1
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        base = (base * base) % mod
        exp //= 2
    return result

def generate_keypair(prime, generator):
    # Generate Diffie-Hellman key pair
    private_key = random.randint(2, prime - 1)
    public_key = mod_exp(generator, private_key, prime)
    return private_key, public_key

def generate_shared_secret(private_key, other_public_key, prime):
    # Calculate the shared secret
    shared_secret = mod_exp(other_public_key, private_key, prime)
    return shared_secret

if __name__ == "__main__":
    # Choose prime number and generator
    prime = 23
    generator = 5

    # Alice generates key pair
    alice_private_key, alice_public_key = generate_keypair(prime, generator)

    # Bob generates key pair
    bob_private_key, bob_public_key = generate_keypair(prime, generator)

    # Alice and Bob exchange public keys and calculate shared secret
    alice_shared_secret = generate_shared_secret(alice_private_key, bob_public_key, prime)
    bob_shared_secret = generate_shared_secret(bob_private_key, alice_public_key, prime)

    # Shared secrets should be equal
    assert alice_shared_secret == bob_shared_secret

    print("Prime (p):", prime)
    print("Generator (g):", generator)
    print("Alice's private key:", alice_private_key)
    print("Alice's public key:", alice_public_key)
    print("Bob's private key:", bob_private_key)
    print("Bob's public key:", bob_public_key)
    print("Shared Secret:", alice_shared_secret)

```

The provided Python code implements the Diffie-Hellman key exchange protocol. Here's an analysis of the code:

1. **Modular Exponentiation Function (`mod_exp`):**
   * This function is defined to perform modular exponentiation efficiently.
   * It uses the square-and-multiply method to calculate `base^exp mod mod`.
   * It iterates through the binary representation of `exp` to compute the result.
2. **Key Pair Generation Function (`generate_keypair`):**
   * Generates a Diffie-Hellman key pair consisting of a private key and a corresponding public key.
   * The private key is a random integer between 2 and `prime - 1`.
   * The public key is calculated using the modular exponentiation function.
3. **Shared Secret Calculation Function (`generate_shared_secret`):**
   * Calculates the shared secret between two parties using their private key and the other party's public key.
   * It uses the modular exponentiation function for the calculation.
4. **Main Section (\`if name == "main":)**
   * Chooses a prime number (`prime`) and a generator (`generator`).
   * Generates key pairs for both Alice and Bob using the `generate_keypair` function.
   * Exchanges public keys between Alice and Bob and computes the shared secrets using the `generate_shared_secret` function.
   * Asserts that the shared secrets computed by Alice and Bob are equal.
   * Prints the prime number, generator, private and public keys for both Alice and Bob, and the shared secret.
5. **Output:**
   * The code outputs information about the prime number, generator, private and public keys for both Alice and Bob, and the shared secret.
6. **Security Considerations:**
   * The code uses a fixed prime number (`prime = 23`) and generator (`generator = 5`). For real-world applications, it is crucial to choose larger prime numbers and suitable generators to enhance security.
   * Randomness in key generation is achieved using the `random` module.

Overall, the code demonstrates the key steps of the Diffie-Hellman key exchange protocol and provides a foundation for understanding the process of generating shared secrets securely.

## Implementing using existing libs

```
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
from Crypto.Util.Padding import pad, unpad

def diffie_hellman():
    # Alice generates private key and public key
    alice_private_key = get_random_bytes(16)
    alice_public_key = pow(2, int.from_bytes(alice_private_key, 'big'), 19)

    # Bob generates private key and public key
    bob_private_key = get_random_bytes(16)
    bob_public_key = pow(2, int.from_bytes(bob_private_key, 'big'), 19)

    # Alice and Bob exchange public keys and calculate shared keys
    shared_key_alice = pow(bob_public_key, int.from_bytes(alice_private_key, 'big'), 19)
    shared_key_bob = pow(alice_public_key, int.from_bytes(bob_private_key, 'big'), 19)

    # Shared keys should be equal
    assert shared_key_alice == shared_key_bob

    return shared_key_alice

def encrypt_decrypt(shared_key, message):
    # Encrypt and decrypt using the shared key
    cipher = AES.new(shared_key.to_bytes(16, 'big'), AES.MODE_CBC)
    ciphertext = cipher.encrypt(pad(message.encode(), AES.block_size))

    # Recreate a new AES object for decryption
    decrypt_cipher = AES.new(shared_key.to_bytes(16, 'big'), AES.MODE_CBC, iv=cipher.iv)
    decrypted_message = unpad(decrypt_cipher.decrypt(ciphertext), AES.block_size)

    return ciphertext, decrypted_message.decode()

if __name__ == "__main__":
    # Perform Diffie-Hellman key exchange
    shared_key = diffie_hellman()

    # Encrypt and decrypt a message
    message = "Hello, Diffie-Hellman!"
    ciphertext, decrypted_message = encrypt_decrypt(shared_key, message)

    print("Original Message:", message)
    print("Ciphertext:", ciphertext.hex())
    print("Decrypted Message:", decrypted_message)

```

The provided Python code demonstrates a simple implementation of a secure communication scheme using the Diffie-Hellman key exchange protocol and AES encryption. Here's an analysis of the code:

1. **Diffie-Hellman Key Exchange (`diffie_hellman` function):**
   * Alice and Bob each generate a random 128-bit private key (`alice_private_key` and `bob_private_key`) using the `get_random_bytes` function.
   * They compute their respective public keys (`alice_public_key` and `bob_public_key`) using modular exponentiation with a fixed base of 2 and a prime modulus 19.
   * They exchange public keys and calculate a shared key using modular exponentiation.
2. **AES Encryption and Decryption (`encrypt_decrypt` function):**
   * The `encrypt_decrypt` function takes a shared key and a message as input.
   * It uses AES encryption in Cipher Block Chaining (CBC) mode to encrypt the message.
   * The encrypted message is then decrypted using the same shared key.
3. **Main Section (\`if name == "main":):**
   * The main section performs the Diffie-Hellman key exchange using the `diffie_hellman` function.
   * It then encrypts and decrypts a sample message using the shared key and prints the results.
4. **Security Considerations:**
   * The use of random private keys in Diffie-Hellman adds a level of security.
   * AES encryption provides confidentiality to the exchanged messages.
   * The shared key calculated in Diffie-Hellman is used to derive an AES key for secure communication.
5. **Output:**
   * The code outputs the original message, the ciphertext in hexadecimal format, and the decrypted message.
6. **Note:**
   * The prime modulus (`19`) used in the Diffie-Hellman key exchange is relatively small for demonstration purposes. In real-world scenarios, larger prime numbers should be used for better security.

Overall, the code illustrates the integration of Diffie-Hellman key exchange and AES encryption for secure communication between two parties.

## MITM attack

Man-in-the-middle attack is a security attack where the attacker intercepts and potentially modifies information transmitted between two communicating parties. For the Diffie-Hellman key exchange protocol, a man-in-the-middle attack typically involves the following steps:

1. Eavesdropping on Public Key Exchange:
   * When Alice and Bob exchange public keys in the Diffie-Hellman key exchange, the man-in-the-middle, Eve, intercepts these public keys.
   * Eve may send her own public key to Alice while sending Alice's public key to Bob, making Alice and Bob both believe they are exchanging keys with Eve.
2. Forging Shared Keys:
   * Eve intercepts the steps where Alice and Bob calculate the shared key.
   * Eve uses her own private key to calculate a pseudo-shared key with Alice and uses Alice's public key to calculate a pseudo-shared key with Bob.
   * This way, Eve ends up with two distinct shared keys corresponding to Alice and Bob, while Alice and Bob believe they are sharing the same key.
3. Completion of the Man-in-the-Middle Attack:
   * Now, Eve can intercept, modify, or view encrypted communication between Alice and Bob without being detected.
   * Because Eve knows two different shared keys, she can decrypt messages from Alice to Bob and from Bob to Alice, and re-encrypt them to maintain the integrity of the communication.

Let's take a look at the code.

```
import random

def diffie_hellman(p, g):
    # Generate private key and public key
    private_key = random.randint(2, p - 1)
    public_key = pow(g, private_key, p)
    return private_key, public_key

def simulate_man_in_the_middle(p, g):
    # The man-in-the-middle intercepts Alice's public key to Bob and replaces it with their own public key
    alice_private_key, alice_public_key = diffie_hellman(p, g)
    malory_private_key, malory_public_key = diffie_hellman(p, g)
    
    # The man-in-the-middle intercepts Bob's public key to Alice and replaces it with their own public key
    bob_private_key, bob_public_key = diffie_hellman(p, g)
    malory_shared_key_with_alice = pow(bob_public_key, malory_private_key, p)

    # The man-in-the-middle intercepts Alice's public key to Bob and replaces it with their own public key
    malory_shared_key_with_bob = pow(alice_public_key, malory_private_key, p)

    return malory_shared_key_with_alice, malory_shared_key_with_bob

if __name__ == "__main__":
    # Simplified Diffie-Hellman parameters
    p = 23
    g = 5

    # Alice and Bob perform Diffie-Hellman key exchange
    alice_private_key, alice_public_key = diffie_hellman(p, g)
    bob_private_key, bob_public_key = diffie_hellman(p, g)

    # Man-in-the-middle attack
    malory_shared_key_with_alice, malory_shared_key_with_bob = simulate_man_in_the_middle(p, g)

    # Print results
    print("Alice's shared key:", pow(bob_public_key, alice_private_key, p))
    print("Bob's shared key:", pow(alice_public_key, bob_private_key, p))
    print("Malory's shared key with Alice:", malory_shared_key_with_alice)
    print("Malory's shared key with Bob:", malory_shared_key_with_bob)

```

The provided Python code demonstrates a simplified Diffie-Hellman key exchange and a simulation of a man-in-the-middle attack. Here's an analysis of the code:

1. **Diffie-Hellman Key Exchange (`diffie_hellman` function):**
   * This function generates a private key and a corresponding public key for a given prime `p` and generator `g`.
   * The private key is a random integer between 2 and `p - 1`.
   * The public key is calculated using modular exponentiation.
2. **Simulation of Man-in-the-Middle Attack (`simulate_man_in_the_middle` function):**
   * This function simulates a man-in-the-middle attack on the Diffie-Hellman key exchange.
   * It intercepts the public keys exchanged between Alice and Bob and replaces them with the attacker's public key.
   * The shared keys between the attacker (Malory) and Alice/Bob are then calculated.
3. **Main Section (\`if name == "main":):**
   * Defines simplified Diffie-Hellman parameters (prime `p` and generator `g`).
   * Alice and Bob perform Diffie-Hellman key exchange.
   * Simulates a man-in-the-middle attack by calculating shared keys with the attacker.
   * Prints the shared keys for Alice, Bob, and the attacker.
4. **Security Considerations:**
   * The code illustrates a scenario where a man-in-the-middle attack is successful due to the interception and replacement of public keys.
   * In a real-world scenario, it emphasizes the importance of secure key exchange mechanisms and the need for authentication to prevent such attacks.
5. **Output:**
   * The code prints the shared keys for Alice, Bob, and the attacker, demonstrating the impact of the man-in-the-middle attack.

Overall, the code serves as a simple example to highlight the vulnerability of Diffie-Hellman key exchange to man-in-the-middle attacks when public keys are intercepted and replaced by an attacker.

## EIgamal

ElGamal is an asymmetric encryption algorithm named after its inventor, Taher ElGamal. This algorithm is based on the discrete logarithm problem in number theory and falls under the category of public-key cryptosystems, used for encryption, digital signatures, and key exchange. Its primary applications include secure communication, digital signatures, and key exchange.

The main steps of the ElGamal algorithm are as follows:

1. **Parameter Generation:**
   * Choose a large prime number (p) and a primitive root (g) (a primitive root modulo (p)). These two parameters can be publicly disclosed.
   * Randomly select a private key (x) with a range of \[1, (p-2)].
   * Calculate the public key (y) as follows:
2. **Encryption:**
   * Convert the plaintext message (M) into an integer (m), ensuring (m) is within the range \[0, (p-1)].
   * Randomly choose a temporary private key (k) with a range of \[1, (p-2)].
   * Calculate the first ciphertext component (c\_1):
   * Calculate the second ciphertext component (c\_2):
   * The ciphertext is represented as ((c\_1, c\_2)).
3. **Decryption:**
   * Receive the ciphertext ((c\_1, c\_2)).
   * Calculate the shared key (K):
   * Calculate the modular inverse:
   * Compute the plaintext (m):

The security of ElGamal relies on the difficulty of the discrete logarithm problem, specifically finding the value (x) for (g^x \mod p) in a finite field. This problem is considered computationally challenging, especially when (p) is a sufficiently large prime number.

It's important to note that the ElGamal algorithm does not provide message integrity verification. Therefore, additional measures are required in communication, such as using hash functions and digital signatures to ensure the integrity of messages.

Review the code.

```
import random

def mod_exp(base, exp, mod):
    # Modular exponentiation to avoid direct use of the pow function
    result = 1
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        base = (base * base) % mod
        exp //= 2
    return result

def generate_keypair(p, g, x):
    # Generate ElGamal key pair
    y = mod_exp(g, x, p)
    return (p, g, y), (p, x)

def elgamal_encrypt(plaintext, public_key):
    p, g, y = public_key
    k = random.randint(2, p - 2)
    c1 = mod_exp(g, k, p)
    s = mod_exp(y, k, p)
    c2 = (plaintext * s) % p
    return c1, c2

def elgamal_decrypt(ciphertext, private_key):
    p, x = private_key
    c1, c2 = ciphertext
    s = mod_exp(c1, x, p)
    plaintext = (c2 * mod_exp(s, p-2, p)) % p
    return plaintext

if __name__ == "__main__":
    # Choose prime and generator
    p = 23
    g = 5

    # Choose private key
    x = random.randint(2, p - 2)

    # Generate ElGamal key pair
    public_key, private_key = generate_keypair(p, g, x)

    # Message to be encrypted
    plaintext = 9

    # Encrypt
    ciphertext = elgamal_encrypt(plaintext, public_key)

    # Decrypt
    decrypted_plaintext = elgamal_decrypt(ciphertext, private_key)

    # Print results
    print("Original plaintext:", plaintext)
    print("Encrypted ciphertext:", ciphertext)
    print("Decrypted plaintext:", decrypted_plaintext)

```

The provided Python code implements the ElGamal encryption and decryption scheme. Here's an analysis of the code:

1. **Modular Exponentiation (`mod_exp` function):**
   * This function performs modular exponentiation to efficiently compute (a^b \mod c).
   * It uses the square-and-multiply method to avoid direct use of the `pow` function.
2. **Key Pair Generation (`generate_keypair` function):**
   * This function generates an ElGamal key pair given prime number (p), generator (g), and private key (x).
   * The public key (y) is calculated using modular exponentiation.
3. **ElGamal Encryption (`elgamal_encrypt` function):**
   * Encrypts a plaintext using the ElGamal algorithm.
   * Randomly generates a temporary key (k).
   * Calculates the first ciphertext component (c1) and the second component (c2).
4. **ElGamal Decryption (`elgamal_decrypt` function):**
   * Decrypts a ciphertext using the ElGamal algorithm.
   * Calculates the shared secret (s) using modular exponentiation.
   * Computes the plaintext using modular arithmetic.
5. **Main Section (\`if name == "main":):**
   * Chooses a prime (p) and generator (g).
   * Randomly selects a private key (x).
   * Generates the ElGamal key pair (public and private keys).
   * Defines a plaintext message.
   * Encrypts and then decrypts the message using ElGamal.
   * Prints the original plaintext, encrypted ciphertext, and decrypted plaintext.
6. **Security Considerations:**
   * The security of ElGamal relies on the difficulty of the discrete logarithm problem, specifically finding (x) for (g^x \mod p).
   * The code demonstrates the core operations of ElGamal encryption and decryption, which are secure under the assumption that the discrete logarithm problem is hard.
7. **Output:**
   * The code prints the original plaintext, encrypted ciphertext, and the decrypted plaintext to demonstrate the correctness of the ElGamal scheme.

## Reference

1\. [https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman\_key\_exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman\_key\_exchange)

2\. [https://www.techtarget.com/searchsecurity/definition/Diffie-Hellman-key-exchange](https://www.techtarget.com/searchsecurity/definition/Diffie-Hellman-key-exchange)

3\. [https://www.comparitech.com/blog/information-security/diffie-hellman-key-exchange/](https://www.comparitech.com/blog/information-security/diffie-hellman-key-exchange/)

4\. [https://www.ques10.com/p/33937/el-gamal-cryptography-algorithm/](https://www.ques10.com/p/33937/el-gamal-cryptography-algorithm/)
