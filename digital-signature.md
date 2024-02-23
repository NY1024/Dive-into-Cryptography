# Digital Signature

## Digital Signature

The goal of digital signatures is to authenticate and verify files and data. This is necessary to prevent tampering, alterations, or forgery during the transmission of official documents.

With the exception of one scenario, digital signatures are all based on public key cryptography systems. Typically, asymmetric key systems use a public key for encryption and a private key for decryption. However, for digital signatures, the situation is reversed. Signatures are encrypted using a private key and decrypted using a public key. Because the keys are related, decoding with the public key can verify that the file has been signed with the correct private key, thus authenticating the source of the signature.

<figure><img src=".gitbook/assets/image (75).png" alt=""><figcaption></figcaption></figure>

M - Plaintext H - Hash function h - Hash digest ‘+’ - Concatenation of plaintext and digest E - Encryption D - Decryption

The above diagram illustrates the entire process, from signing the key to its verification. So, let's understand each step gradually to thoroughly comprehend the process.

Step 1: The original message M is first passed to the hash function represented by H# to create a digest. Step 2: Next, the message is concatenated with the hash digest h and encrypted using the sender's private key. Step 3: The encrypted bundle is then sent to the receiver, who can decrypt it using the sender's public key. Step 4: Once the message is decrypted, it is passed through the same hash function (H#) to generate a similar digest. Step 5: The newly generated hash is compared with the concatenated hash value received along with the message. If they match, data integrity is verified.

There are two industry-standard ways to implement the above method, they are:

RSA Algorithm DSA Algorithm

Both of these algorithms achieve the same purpose, but there are significant differences in the encryption and decryption functionalities. We will focus on the DSA algorithm.

Digital signatures are a type of electronic signature based on cryptography, used to verify the identity of the message sender or document signer and ensure that the original content of the message or document has not been altered.

<figure><img src=".gitbook/assets/image (76).png" alt=""><figcaption></figcaption></figure>

The information in the above diagram is explained as follows:

Message or Document: The data or document that needs to be signed.

Hash: The document or message is transformed into a fixed-length hash using a cryptographic hash function.

Encryption: The hash is then encrypted using the sender's private key.

Signature: The encrypted hash becomes the digital signature of the document.

Verification: The signature is then verified using the sender's public key to ensure the validity of the signature and the authenticity of the document.

The diagram outlines the process of creating and verifying digital signatures.

The process begins with one party sending a message to another party. The message is then passed through a cryptographic hash algorithm, generating a hash of the message.

Next, the hash is encrypted using the sender's private key. The encrypted hash is then appended to the original message, forming the digital signature.

The receiving party then decrypts the hash using the sender's public key and compares it with a new hash generated from the original message to verify the digital signature. This decryption process ensures that the message has not been altered during transmission and can be trusted to come from the sender. The digital signature again proves that the data is from a trusted source and has not been altered.

Digital signatures are used for authentication or verification. They are used to verify the authenticity and integrity of digital documents or messages.

The significant role of digital signatures is to ensure the integrity and authenticity of digital documents or messages. Digital signatures are essentially a form of electronic signature, providing means for undeniable and authenticated communication between two parties. They serve as evidence that both parties have agreed to specific terms and conditions and mutually verified the content of the document or message.

Digital signatures can also protect confidential information, ensuring that only authorized personnel can access the data.

Additionally, digital signatures can be used to trace the source of digital transactions and ensure that they have not been altered.

Digital Signature Encryption

Encryption is the process of transforming information (called plaintext) into a form that can only be read by those with special knowledge (usually called a key). The result of this process is encrypted information. In cryptography, this encrypted information is called ciphertext.

## DSA

What is the DSA Algorithm?

The Digital Signature Algorithm is a Federal Information Processing Standard (FIPS) for digital signatures. It was proposed in 1991 and standardized globally by the National Institute of Standards and Technology (NIST) in 1994. It is based on the framework of modular exponentiation and the discrete logarithm problem, which are difficult to compute in brute-force systems.

The DSA algorithm provides three benefits:

Message Authentication: You can authenticate the identity of the sender using the correct key pair.

Integrity Verification: Because this prevents the entire bundle from being decrypted, you cannot tamper with the message.

Non-Repudiation: If the signature is verified, the sender cannot claim that they never sent the message.

<figure><img src=".gitbook/assets/image (77).png" alt=""><figcaption></figcaption></figure>

The above diagram illustrates the entire process of the DSA algorithm. Here, you'll use two different functions: a signing function and a verification function. The typical digital signature verification process differs from the above diagram in the encryption and decryption parts. They have different parameters.

Steps of the DSA algorithm:

Continue to understand how the entire process works, from creating key pairs to finally verifying the signature.

1. Key Generation The key generation process is divided into two steps: parameter generation and key generation for each user.

Parameter Generation Initially, users need to choose an encryption hash function (H) and an output length in bits |H|. When the output length |H| is large, a modulus length N is used.

Next, choose a key length L, which should be a multiple of 64 and between 512 and 1024, according to the original DSS length. However, NIST recommends using key lengths of 2048 or 3072 to ensure security throughout the lifecycle.

According to FIPS 186-4, values for L and N need to be chosen between (1024, 60), (2048, 224), (2048, 256), or (3072, 256). Additionally, users should choose the modulus length N such that the modulus length N should be less than the key length (N < L).

Then, users can choose an N-bit prime number q and another L-bit prime number p, such that p-1 is a multiple of q. Then choose h as an integer in the list (2……..p-2).

Once the values of p and q are obtained, compute g = h^(p-1)/q\*mod(p). If g = 1 is obtained, try another value of h and recalculate g, excluding any value other than 1.

p, q, and g are algorithm parameters shared among different users in the system.

Key Generation for Each User To compute key parameters for an individual user, first choose an integer x (private key) from the list, then compute the public key y = g^(x)\*mod(p).

2. Signature Generation It processes the original message (M) through the hash function (H#) to obtain our hash digest (h).

Pass the digest as input to the signing function, which aims to produce two variables as output, namely s and r.

In addition to the digest, a random integer k is used, such that 0 < k < q.

To compute the value of r, use the formula r = (gk mod p) mod q.

To compute the value of s, use the formula s = \[K-1(h+x . R)mod q].

Then package the signature as {r, s}.

The entire message and signature package {M, r, s} are sent to the receiver.

3. Key Distribution When distributing keys, the signer should keep the confidentiality of the private key (x) and make the public key (y) public and send the public key (y) to the receiver without using any confidentiality mechanism.

Signing The signature for message m should be done as follows:

First, choose an integer k from (1……q-1).

Compute r = g^(k)\*mod(p)\*mod(q). If r = 0 is obtained, try using another random value of k, excluding 0, and compute r again.

Compute s=(k^(-1)\*(H(m)+xr))\*mod(q). If s = 0 is obtained, try using another random value of k, excluding 0, and compute s again.

The signature is defined by two key elements (r, s). Additionally, the key elements k and r are used to create a new message. However, computing r using modular exponentiation is a very expensive process and is computed before the message is known. The computation is done using the Euclidean algorithm and Fermat's little theorem.

4. Signature Verification You use the same hash function (H#) to generate the digest h.

Then pass this digest to the verification function, which also requires additional variables as parameters.

Compute the value w, such that s\*w mod q = 1.

Calculate the value of u1 using the formula u1 = h\*w mod q.

Calculate the value of u2 using the formula u2 = r\*w mod q.

The final verification component v is calculated as v = \[((gu1 . yu2) mod p) mod q].

Compare the value of v with the received value of r.

If they match, the signature verification is complete.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import dsa

def generate_key_pair():
    # Generate private key
    private_key = dsa.generate_private_key(
        key_size=1024,
        backend=default_backend()
    )
    # Derive public key from private key
    public_key = private_key.public_key()
    return private_key, public_key

def sign_message(private_key, message):
    # Sign the message using the private key
    signature = private_key.sign(
        message,
        hashes.SHA256()
    )
    return signature

def verify_signature(public_key, message, signature):
    try:
        # Verify the signature using the public key
        public_key.verify(
            signature,
            message,
            hashes.SHA256()
        )
        return True
    except Exception as e:
        print(f"Signature verification failed: {e}")
        return False

# Generate key pair
private_key, public_key = generate_key_pair()

# Message to sign
message = b"Hello, DSA!"

# Sign the message
signature = sign_message(private_key, message)

# Verify the signature
if verify_signature(public_key, message, signature):
    print("Signature is valid.")
else:
    print("Signature is invalid.")

```

The provided Python code demonstrates how to generate a key pair, sign a message with a private key, and verify the signature using the corresponding public key in the context of the Digital Signature Algorithm (DSA).

Here's a breakdown and explanation of the code:

1. **Imports**:
   * The code imports necessary modules from the `cryptography` library, including backends for cryptographic operations, hash functions, and the DSA module for asymmetric cryptography.
2. **Key Pair Generation** (`generate_key_pair` function):
   * This function generates a DSA key pair consisting of a private key and a corresponding public key.
   * It uses the `generate_private_key` method from the `dsa` module to create a private key with a specified key size (1024 bits in this case) and default backend.
   * The public key is derived from the private key using the `public_key` method.
3. **Message Signing** (`sign_message` function):
   * This function signs a given message using a private key.
   * It utilizes the `sign` method of the private key object, which takes the message to be signed and a hash algorithm (SHA-256 in this case) as parameters.
   * The resulting signature is returned.
4. **Signature Verification** (`verify_signature` function):
   * This function verifies the validity of a signature for a given message using a public key.
   * It attempts to verify the signature using the `verify` method of the public key object, passing the signature, message, and hash algorithm as parameters.
   * If the verification succeeds, the function returns `True`; otherwise, it catches any exceptions raised during the verification process, prints an error message, and returns `False`.
5. **Execution**:
   * The code then proceeds to:
     * Generate a key pair (`private_key`, `public_key`).
     * Define a message to sign (`message`).
     * Sign the message using the private key, resulting in a signature (`signature`).
     * Verify the signature using the public key and the original message, outputting whether the signature is valid or invalid.

Overall, this code showcases the basic process of generating key pairs, signing messages, and verifying signatures using the DSA algorithm within the `cryptography` library in Python.

## RSA

RSA (Rivest-Shamir-Adleman) is an asymmetric encryption algorithm widely used for digital signatures and encrypted communication. In digital signatures, RSA is employed to ensure the integrity, authenticity, and non-repudiation of messages.

1. Key Generation:
   * Digital signatures utilize RSA's asymmetric key pair, consisting of a public key and a private key.
   * The public key is used for signature verification, while the private key is used for signature generation.
2. Digital Signature Generation Process:
   * The sender employs a hash function (such as SHA-256) on the message to generate a digest or hash value.
   * Using their private key, the sender encrypts this hash value, forming the digital signature.
   * The digital signature is essentially the encrypted result of the message digest by the sender.
3. Digital Signature Verification Process:
   * The recipient obtains the sender's public key for signature verification.
   * Using the same hash function, the recipient generates a digest for the received message.
   * The recipient decrypts the digital signature using the sender's public key, obtaining the decrypted hash value.
   * The recipient compares the decrypted hash value with the digest they computed.
   * If they match, it signifies the signature is valid, the message is unaltered, and indeed, the sender holds the private key.

The security of RSA digital signatures relies on the difficulty of the integer factorization problem, i.e., calculating the corresponding private key given the known public key and encrypted text is a challenging task.

The security of RSA depends on the chosen key length, typically requiring sufficiently long keys to withstand risks posed by increasing computational power.

Overall, RSA digital signatures provide a reliable means to ensure the integrity and authenticity of data. Its asymmetric nature ensures that only the holder of the private key can generate valid digital signatures, thus ensuring the trustworthiness of the signatures.

```
from cryptography.hazmat.backends import default_backend  # Importing default backend for cryptographic operations
from cryptography.hazmat.primitives import hashes  # Importing hash functions
from cryptography.hazmat.primitives.asymmetric import rsa, padding  # Importing RSA asymmetric encryption and padding
from cryptography.hazmat.primitives import serialization  # Importing serialization module

def generate_key_pair():
    # Generating RSA key pair
    private_key = rsa.generate_private_key(
        public_exponent=65537,
        key_size=2048,
        backend=default_backend()
    )
    # Extracting public key from private key
    public_key = private_key.public_key()
    return private_key, public_key

def sign_message(private_key, message):
    # Signing the message using private key and PSS padding
    signature = private_key.sign(
        message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )
    return signature

def verify_signature(public_key, message, signature):
    try:
        # Verifying the signature using public key and PSS padding
        public_key.verify(
            signature,
            message,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        print("Signature is valid.")
        return True
    except Exception as e:
        print(f"Signature verification failed: {e}")
        return False

# Generating RSA key pair
private_key, public_key = generate_key_pair()

# Message to be signed
message_to_sign = b"Hello, this is a message to sign."

# Creating digital signature
signature = sign_message(private_key, message_to_sign)

# Verifying the signature
verify_signature(public_key, message_to_sign, signature)

```



1. **Imports**:
   * The code imports necessary modules from the `cryptography` library, including backends for cryptographic operations, hash functions, RSA encryption, padding, and serialization.
2. **Key Pair Generation** (`generate_key_pair` function):
   * This function generates an RSA key pair, which includes a private key and a corresponding public key.
   * It uses the `generate_private_key` method from the `rsa` module to create a private key with specified parameters such as public exponent and key size.
   * The public key is extracted from the private key using the `public_key` method.
3. **Message Signing** (`sign_message` function):
   * This function signs a given message using the private key and PSS (Probabilistic Signature Scheme) padding.
   * It utilizes the `sign` method of the private key object, which takes the message, padding scheme, and hash algorithm as parameters.
   * PSS padding is used to enhance security and randomness in the signature generation process.
4. **Signature Verification** (`verify_signature` function):
   * This function verifies the validity of a signature for a given message using the public key and PSS padding.
   * It attempts to verify the signature using the `verify` method of the public key object, passing the signature, message, padding scheme, and hash algorithm as parameters.
   * If the verification succeeds, it prints "Signature is valid"; otherwise, it catches any exceptions raised during the verification process and prints an error message.
5. **Execution**:
   * The code then proceeds to:
     * Generate an RSA key pair (`private_key`, `public_key`).
     * Define a message to be signed (`message_to_sign`).
     * Create a digital signature for the message using the private key and PSS padding.
     * Verify the signature using the public key, message, and signature.

Overall, this code demonstrates the process of generating RSA key pairs, signing messages, and verifying signatures using RSA encryption with PSS padding, which enhances the security of the digital signature scheme.

## Key-only attack

Now we learn about attacks that signature schemes may face. First is the Key-Only Attack. A Key-Only Attack refers to a scenario where an attacker (C) only knows the public key of a user (A) without any other information about A. In this attack scenario, the attacker attempts to derive the user's private key based on their knowledge of the public key, thus compromising the security of the cryptographic system. In the RSA encryption system, the Key-Only Attack is based on the difficulty of the integer factorization problem, which involves decomposing large integers into their prime factors. The security of RSA relies on this problem, and when an attacker only knows the public key, they seek to obtain the private key through various means. Below are some attack methods attackers may employ:

1. Factoring the modulus of the public key:

* RSA's public key consists of two parts: the modulus (n) and the public exponent (e).
* Attackers may attempt to factorize the modulus n, aiming to find two prime numbers p and q such that n = p \* q.
* If the attacker successfully factors n, they can obtain the private key, as the private key is calculated based on p and q.

2. Attempting to factorize the public exponent:

* Attackers may try to guess or deduce the value of the public exponent e.
* If the attacker successfully guesses e, they can attempt to solve the discrete logarithm problem to deduce the private key d.

3. Chosen plaintext-ciphertext pairs attack:

* Attackers can select some known plaintext-ciphertext pairs and try to deduce the private key from them.
* By analyzing multiple plaintext-ciphertext pairs, attackers may gather enough information to guess the private key. It's important to note that performing a Key-Only Attack on RSA is a complex task, depending on the length of the keys used and the security of the encryption algorithm. Longer key lengths are generally more secure because factoring them requires more computational resources and time. To resist Key-Only Attacks, it's recommended to use sufficiently long RSA keys and regularly update the keys to maintain system security. Additionally, the key generation and storage processes should be subject to strict security controls to prevent private key leakage.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives import serialization  # Add this import
from cryptography.hazmat.primitives.asymmetric import rsa, padding

def simulate_only_public_key_attack():
    # A generates key pair
    private_key_a = rsa.generate_private_key(
        public_exponent=65537,
        key_size=2048,
        backend=default_backend()
    )
    public_key_a = private_key_a.public_key()

    # C only knows A's public key
    public_key_c = public_key_a.public_bytes(
        encoding=serialization.Encoding.PEM,
        format=serialization.PublicFormat.SubjectPublicKeyInfo
    )

    # A signs a message with private key
    message = b"Secret message"
    signature = private_key_a.sign(
        message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )

    print("Signature:", signature)

    # C tries to verify the signature, but without A's private key cannot generate a valid signature
    try:
        public_key_c = serialization.load_pem_public_key(public_key_c, backend=default_backend())
        public_key_c.verify(
            signature,
            message,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        print("Signature is valid.")
    except Exception as e:
        print(f"Signature verification failed: {e}")

simulate_only_public_key_attack()

```

This Python script demonstrates a scenario known as a "Key-Only Attack":

1. **Key Pair Generation**:
   * User A generates an RSA key pair comprising a private key and its corresponding public key. This is a typical step in establishing asymmetric cryptography.
2. **Public Key Knowledge**:
   * Attacker C, in this simulation, only has access to the public key of user A. This represents a scenario where an attacker gains access to the public key through various means, but does not have access to the private key.
3. **Signing Message**:
   * User A signs a message ("Secret message") using their private key. This operation is performed to create a digital signature, which can later be verified using the corresponding public key.
4. **Verification Attempt**:
   * Attacker C attempts to verify the signature using the public key of user A. However, since the public key can only be used for signature verification and not for signature generation, the verification fails. This illustrates the security principle that knowing only the public key should not allow an attacker to forge valid signatures.
5. **Output**:
   * The script prints the signature generated by user A, and then indicates whether the signature verification succeeds or fails. In this case, the verification fails due to the inability to generate a valid signature without access to the private key.

This scenario highlights the importance of protecting private keys in asymmetric cryptography systems, as the security of the system relies on the secrecy of the private key. Even if an attacker gains access to the public key, they should not be able to generate valid signatures without the corresponding private key.

## Chosen Message Attack

The Chosen Message Attack (CMA) is a cryptographic attack in which the attacker (C) selects some messages and obtains legitimate signatures for those messages without needing to know the public key of the targeted user (A). This attack is powerful and flexible because the attacker can freely choose the messages to be signed, without being restricted by the public key.

1. **Message Selection**:
   * Attacker C can freely choose messages, which can be of interest to them or tailored to meet the attack objectives.
2. **Obtaining Legitimate Signatures**:
   * Attacker C somehow obtains legitimate signatures for the chosen messages. This may involve intercepting communication, stealing stored signatures, or other means.
3. **Analyzing the Signature Scheme**:
   * Attacker C may analyze the signature scheme to understand its internal workings, such as the signature calculation process, the hash algorithms used, and padding schemes.
4. **Forgery of Signatures**:
   * Utilizing the obtained legitimate signatures, Attacker C can forge signatures for other messages. Due to the nature of the signature scheme, the attacker does not need to know the private key; knowledge of the public key is sufficient to forge signatures.
5. **Attack Objectives**:
   * The objectives of a chosen message attack may include forging legitimate signatures, impersonating the identity of user A, tampering with communication content, and more.

This attack is generic because it does not rely on the public key information of user A but exploits the general properties of the signature scheme. The success of this attack depends on the security and complexity of the signature scheme, as well as the attacker's understanding of the signature scheme.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding

def chosen_message_attack(private_key, chosen_messages):
    signatures = {}

    # C chooses some messages and obtains legitimate signatures from A
    for message in chosen_messages:
        signature = private_key.sign(
            message,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        signatures[message] = signature

    # C attempts to verify the signatures, but without A's public key, cannot generate valid signatures
    for message, signature in signatures.items():
        try:
            public_key = private_key.public_key()
            public_key.verify(
                signature,
                message,
                padding.PSS(
                    mgf=padding.MGF1(hashes.SHA256()),
                    salt_length=padding.PSS.MAX_LENGTH
                ),
                hashes.SHA256()
            )
            print(f"Signature for the message '{message}' is valid.")
        except Exception as e:
            print(f"Signature verification failed for the message '{message}': {e}")

# A generates a key pair
private_key_a = rsa.generate_private_key(
    public_exponent=65537,
    key_size=2048,
    backend=default_backend()
)

# C selects some messages and performs signature verification
chosen_messages = [b"Hello, World!", b"Goodbye, World!", b"Attack Message!"]
chosen_message_attack(private_key_a, chosen_messages)

```

In the provided Python code:

1. **Function `chosen_message_attack(private_key, chosen_messages)`:**
   * This function simulates a chosen message attack scenario where an attacker (C) selects some messages and attempts to verify signatures without knowing the public key of the user being attacked (A).
   * It takes `private_key` (belonging to A) and a list of `chosen_messages` as input.
   * It iterates through each `chosen_message`, signs it using A's private key, and stores the message-signature pairs in a dictionary.
   * It then attempts to verify each signature using A's public key derived from A's private key. If verification succeeds, it prints a message indicating that the signature is valid. If verification fails, it prints an error message.
2. **Key Generation (`private_key_a = rsa.generate_private_key(...)`):**
   * A generates a key pair consisting of a private key (`private_key_a`) and a corresponding public key.
3. **Chosen Messages Selection (`chosen_messages = [...]`):**
   * The attacker (C) selects a list of messages (`chosen_messages`) to be used in the chosen message attack.
4. **Execution (`chosen_message_attack(private_key_a, chosen_messages)`):**
   * The chosen message attack function is called with A's private key and the selected messages as parameters to simulate the attack scenario.

This code demonstrates the vulnerability of RSA signatures to chosen message attacks, where an attacker can obtain legitimate signatures for chosen messages without needing knowledge of the signer's public key, thereby undermining the security of the signature scheme.\


## Reference

1\. [https://en.wikipedia.org/wiki/Digital\_signature](https://en.wikipedia.org/wiki/Digital\_signature)

2\. [https://www.simplilearn.com/tutorials/cryptography-tutorial/digital-signature-algorithm](https://www.simplilearn.com/tutorials/cryptography-tutorial/digital-signature-algorithm)
