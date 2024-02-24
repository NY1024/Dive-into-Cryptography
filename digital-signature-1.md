# Digital Signature Ⅱ

## Targeted Chosen Message Attack

Targeted Chosen Message Attack is a cryptographic attack similar to a general chosen message attack, with the difference being that the attacker (C) selects the attack message before the generation of the signature, after obtaining user A's public key. This attack is more targeted as the attacker determines the attack message before obtaining the signature.

The general steps and characteristics of a targeted chosen message attack are as follows:

1. Obtaining the public key:
   * Attacker C obtains the public key of user A, which may be achieved through network interception, phishing attacks, social engineering, or other means.
2. Choosing the attack message:
   * After obtaining A's public key, attacker C selects specific attack messages, designed to achieve particular attack goals or objectives.
3. Pre-signature generation attack:
   * The attacker selects the attack message before A signs the specific message. This allows the attacker to manipulate the attack target before signature generation, possibly for deception, identity forgery, or communication tampering purposes.
4. Obtaining a legitimate signature:
   * Attacker C acquires a legitimate signature for the chosen attack message, which may involve directly requesting user A's signature or obtaining it through other means.
5. Attack objectives:
   * The objectives of a targeted chosen message attack may include forging legitimate signatures, impersonating user A's identity, tampering with communication content, among others. The attacker typically has specific objectives when selecting the attack message.
6. Signature verification:
   * The attacker may attempt to use the obtained legitimate signature to verify the corresponding message's authenticity, possibly to deceive other communication parties into believing the attacker has a legitimate identity.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding

def targeted_chosen_message_attack(private_key, public_key, chosen_messages):
    signatures = {}

    # C chooses some messages and signs them under A's public key
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

    # C attempts to verify these signatures using A's public key
    for message, signature in signatures.items():
        try:
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
public_key_a = private_key_a.public_key()

# C chooses some messages and verifies the signatures under A's public key
chosen_messages = [b"Hello, World!", b"Goodbye, World!", b"Attack Message!"]
targeted_chosen_message_attack(private_key_a, public_key_a, chosen_messages)

```



1. **Imports**:
   * The code imports necessary modules from the `cryptography` library, including `default_backend`, `hashes`, `rsa`, and `padding`.
2. **Function Definition: `targeted_chosen_message_attack`**:
   * This function takes three parameters: `private_key`, `public_key`, and `chosen_messages`.
   * It initializes an empty dictionary called `signatures` to store message-signature pairs.
   * The function iterates over each message in the `chosen_messages` list.
     * For each message, it generates a signature using the private key `private_key`.
     * The signature is computed using the PSS padding scheme with SHA256 as the hash algorithm.
     * The generated signature is stored in the `signatures` dictionary with the corresponding message as the key.
   * After generating signatures for all chosen messages, the function attempts to verify each signature using the public key `public_key`.
   * If verification is successful, it prints a message indicating that the signature is valid; otherwise, it catches and prints any exceptions raised during verification.
3. **Key Pair Generation for User A**:
   * The code generates a RSA key pair for user A (`private_key_a`), consisting of a private key and a corresponding public key (`public_key_a`).
   * The private key is generated with a public exponent of 65537 and a key size of 2048 bits.
4. **Targeted Chosen Message Attack Simulation**:
   * A list of chosen messages (`chosen_messages`) is created, containing three byte strings.
   * The `targeted_chosen_message_attack` function is called with the private key, public key, and chosen messages as arguments, simulating a targeted chosen message attack.
   * The function attempts to sign and verify the chosen messages under user A's public key, demonstrating the attack scenario.

Overall, the code demonstrates how an attacker (denoted as C) can perform a targeted chosen message attack by generating signatures for specific messages and attempting to verify them using the target user's public key. This highlights the importance of secure key management and robust signature verification mechanisms in cryptographic systems.

## Adaptive Chosen Message Attack

Now, we'll learn about Adaptive Chosen Message Attacks against signature schemes.

Adaptive Chosen Message Attack is a cryptographic attack where the attacker (C) is allowed to treat the targeted user A as an oracle for querying. This means that attacker C can request A to sign specific messages, and these messages may be related to message-signature pairs obtained by the attacker earlier. This is a more powerful and flexible form of attack because the attacker can adjust their attack strategy based on previous feedback.

Below are the general steps and characteristics of an Adaptive Chosen Message Attack:

1. **Obtaining the Public Key**:
   * Attacker C obtains the public key of user A. This may be achieved through various means such as intercepting communications or stealing stored public keys.
2. **Adaptive Querying A**:
   * Attacker C sends requests to A in an adaptive manner, asking for signatures on specific messages. The attacker can observe A's signature feedback from previous requests and adjust their request strategy accordingly.
3. **Selecting and Adjusting Messages**:
   * Attacker C can choose some messages and adjust them adaptively. Messages may be adjusted based on A's feedback, specific signature scheme properties, or vulnerabilities.
4. **Signature Generation**:
   * A signs the messages provided by the attacker and returns the signatures. Upon receiving the signature, the attacker can further adjust their strategy or choose the next message to be signed.
5. **Attack Objectives**:
   * The objectives of an Adaptive Chosen Message Attack may include forging legitimate signatures, impersonating user A's identity, or tampering with communication content. Attackers can improve the efficiency and success rate of the attack by continuously adapting to A's feedback.
6. **Signature Verification**:
   * The attacker may attempt to verify the obtained signatures for corresponding messages to confirm their validity. This may also be done to deceive other communication parties into believing the attacker has a legitimate identity.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding

def adaptive_chosen_message_attack(private_key, public_key, messages_to_sign):
    signatures = {}

    # C adaptively selects messages and obtains signatures from A
    for message in messages_to_sign:
        signature = private_key.sign(
            message,
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        signatures[message] = signature

    # C attempts to verify these signatures using A's public key
    for message, signature in signatures.items():
        try:
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
public_key_a = private_key_a.public_key()

# C adaptively chooses messages and performs signature verification under A's public key
messages_to_sign = [b"Hello, World!", b"Goodbye, World!", b"Adaptive Attack Message!"]
adaptive_chosen_message_attack(private_key_a, public_key_a, messages_to_sign)

```



1. **Imports**:
   * The code imports necessary modules from the `cryptography` library, including `default_backend`, `hashes`, `rsa`, and `padding`.
2. **Function Definition: `adaptive_chosen_message_attack`**:
   * This function takes three parameters: `private_key`, `public_key`, and `messages_to_sign`.
   * It initializes an empty dictionary called `signatures` to store message-signature pairs.
   * The function iterates over each message in the `messages_to_sign` list.
     * For each message, it generates a signature using the private key `private_key`.
     * The signature is computed using the PSS padding scheme with SHA256 as the hash algorithm.
     * The generated signature is stored in the `signatures` dictionary with the corresponding message as the key.
   * After generating signatures for all messages to sign, the function attempts to verify each signature using the public key `public_key`.
   * If verification is successful, it prints a message indicating that the signature is valid; otherwise, it catches and prints any exceptions raised during verification.
3. **Key Pair Generation for User A**:
   * The code generates an RSA key pair for user A (`private_key_a`), consisting of a private key and a corresponding public key (`public_key_a`).
   * The private key is generated with a public exponent of 65537 and a key size of 2048 bits.
4. **Adaptive Chosen Message Attack Simulation**:
   * A list of messages to sign (`messages_to_sign`) is created, containing three byte strings.
   * The `adaptive_chosen_message_attack` function is called with the private key, public key, and messages to sign as arguments, simulating an adaptive chosen message attack.
   * The function attempts to sign and verify the provided messages under user A's public key, demonstrating the attack scenario.

Overall, the code demonstrates how an attacker (denoted as C) can perform an adaptive chosen message attack by adaptively selecting messages and obtaining signatures from the target user. This highlights the importance of robust signature schemes and secure key management in cryptographic systems.

## Schnorr digital signature

Now we are learning about the Schnorr digital signature scheme.

The Schnorr digital signature scheme is a digital signature algorithm based on the discrete logarithm problem. It was proposed by Claus Schnorr in 1991 and is a secure, simple, and efficient digital signature scheme. The security of Schnorr signatures is based on the computational difficulty of the elliptic curve discrete logarithm problem.

<figure><img src=".gitbook/assets/image (78).png" alt=""><figcaption></figcaption></figure>

The basic process of the Schnorr digital signature scheme is as follows:

1. **Key Generation**:
   * Choose a large prime number ( p ) and a generator ( G ), ensuring that ( p ) is prime and ( G ) is a generator of a subgroup of order ( p ).
   * Randomly select a private key ( x ) (( 0 < x < p-1 )).
   * Compute the public key ( y = G^x \mod p ).
   * Public key is represented as ((p, G, y)), and the private key is ( x ).
2. **Signature Generation**:
   * Randomly select a random number ( k ) (( 1 < k < p-1 )).
   * Compute ( r = (G^k \mod p) \mod q ), where ( q ) is a prime factor of ( p ).
   * Compute ( e = H(\text{message} || r) ), where ( H ) is a hash function, and (\text{message}) is the message to be signed.
   * Compute ( s = (k - x \cdot e) \mod q ), where ( x ) is the private key.
   * The signature is ((r, s)).
3. **Signature Verification**:
   * Compute ( e = H(\text{message} || r) ).
   * Compute ( w = s^{-1} \mod q ).
   * Compute ( u1 = e \cdot w \mod q ) and ( u2 = r \cdot w \mod q ).
   * Compute the point ((x1, y1) = u1 \cdot G + u2 \cdot y ).
   * If ( r \equiv x1 \mod q ), then the signature is valid; otherwise, it's invalid.

The security of the Schnorr signature scheme relies on the difficulty of the discrete logarithm problem on elliptic curves, i.e., given a base point ( G ) and a point ( Q = k \cdot G ) on an elliptic curve, finding the unknown ( k ). Due to the hardness of the discrete logarithm problem, the Schnorr signature scheme is considered secure.

Advantages of the Schnorr signature scheme include simplicity, efficiency, and natural support for multi-signatures. Due to its superior performance and security, the Schnorr signature scheme has been widely adopted in the field of cryptography and blockchain technology.

```
import hashlib
import ecdsa

def generate_key_pair():
    # Choose the elliptic curve
    curve = ecdsa.curves.SECP256k1
    # Generate private key and corresponding public key
    private_key = ecdsa.SigningKey.generate(curve=curve)
    public_key = private_key.get_verifying_key()
    return private_key, public_key

def sign_message(private_key, message):
    # Hash the message using SHA-256
    message_hash = hashlib.sha256(message).digest()
    # Sign the message using the private key
    signature = private_key.sign(message_hash)
    return signature

def verify_signature(public_key, message, signature):
    # Hash the message using SHA-256
    message_hash = hashlib.sha256(message).digest()
    try:
        # Verify the signature using the public key
        public_key.verify(signature, message_hash)
        return True
    except ecdsa.BadSignatureError:
        return False

# Generate key pair
private_key, public_key = generate_key_pair()

# Message to sign
message = b"Hello, Schnorr!"

# Sign the message
signature = sign_message(private_key, message)

# Verify the signature
verification_result = verify_signature(public_key, message, signature)

# Print the results
print(f"Original Message: {message}")
print(f"Signature: {signature.hex()}")
print(f"Verification Result: {verification_result}")

```



1. **Imports**:
   * The code imports necessary modules, including `hashlib` for cryptographic hashing and `ecdsa` for Elliptic Curve Digital Signature Algorithm functionality.
2. **Function Definitions**:
   * `generate_key_pair()`: This function generates a key pair consisting of a private key and its corresponding public key. It chooses the SECP256k1 elliptic curve.
   * `sign_message(private_key, message)`: This function signs a given message using the provided private key. It first hashes the message using SHA-256 and then signs the hash with the private key.
   * `verify_signature(public_key, message, signature)`: This function verifies the signature of a given message using the provided public key. It hashes the message using SHA-256, then verifies the signature against the hash and the public key. If the signature is valid, it returns `True`; otherwise, it returns `False`.
3. **Key Pair Generation**:
   * The code generates a key pair using the `generate_key_pair()` function, resulting in a private key and a corresponding public key.
4. **Message Signing and Verification**:
   * A message (`message`) to be signed is defined.
   * The message is signed using the `sign_message()` function with the previously generated private key, resulting in a digital signature (`signature`).
   * The signature is then verified using the `verify_signature()` function with the corresponding public key. The result (`verification_result`) indicates whether the signature is valid or not.
5. **Print Results**:
   * The original message, the signature, and the verification result are printed to the console.

This code demonstrates the generation of key pairs, signing of messages, and verification of signatures using the ECDSA algorithm with the SECP256k1 elliptic curve.

## ECDSA

Now we are learning about Elliptic Curve Digital Signature Algorithm (ECDSA).

The Elliptic Curve Digital Signature Algorithm (ECDSA) is a digital signature algorithm that utilizes elliptic curve cryptography (ECC). ECDSA offers a secure and efficient means of ensuring message integrity and authenticating the signer's identity.

<figure><img src=".gitbook/assets/image (79).png" alt=""><figcaption></figcaption></figure>

1. Key Generation:
   * Select an elliptic curve, typically using standard curves like SECP256k1.
   * Randomly choose a private key (integer (d)), where (0 < d <) the order of the curve (number of points on the curve).
   * Compute the public key (point (Q)) as (G \times d), where (G) is the base point on the curve.
   * The public key consists of the curve parameters and point (Q), and the private key is the integer (d).
2. Message Signature Generation:
   * Randomly select a temporary private key (k), where (0 < k <) the order of the curve.
   * Compute the temporary public key point (R = G \times k).
   * Compute the hash value of the message (typically using SHA-256 or SHA-3): (e = \text{Hash(message)}).
   * Compute the first part of the signature: (s1 = (e + d \times r) \mod) the order of the curve, where (r) is the x-coordinate of point (R).
   * Compute the second part of the signature: (s2 = k^{-1} \mod) the order of the curve, where (k^{-1}) is the modular inverse of (k) modulo the order of the curve.
   * The signature is ((r, s1 \times s2)).
3. Signature Verification:
   * Receive the signature ((r, s)) and the message.
   * Compute the hash value of the message: (e = \text{Hash(message)}).
   * Calculate the modular inverse of (s) modulo the order of the curve: (s2 = s^{-1} \mod) the order of the curve.
   * Calculate (w = s2).
   * Calculate (u1 = (e \times w) \mod) the order of the curve and (u2 = (r \times w) \mod) the order of the curve.
   * Calculate the public key point (P = u1 \times G + u2 \times Q).
   * If the x-coordinate of (P) equals (r), then the signature is valid; otherwise, it's invalid.

The security of ECDSA is based on the computational difficulty of the discrete logarithm problem on elliptic curves, i.e., given a base point (G) and a point (Q = k \times G) on the elliptic curve, finding the unknown (k). Due to the difficulty of this problem, ECDSA is widely used in practical applications such as digital currencies (e.g., Bitcoin) and other secure communication protocols.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import ec
from cryptography.hazmat.primitives import serialization

# Generate key pair
private_key = ec.generate_private_key(
    ec.SECP256R1(),  # Use the secp256r1 curve
    default_backend()
)
public_key = private_key.public_key()

# Sign the message
message = b"Hello, world!"
signature = private_key.sign(
    message,
    ec.ECDSA(hashes.SHA256())
)

# Verify the signature
try:
    public_key.verify(
        signature,
        message,
        ec.ECDSA(hashes.SHA256())
    )
    print("Signature is valid.")
except Exception as e:
    print("Signature is invalid:", e)

# Print the public and private keys
private_key_pem = private_key.private_bytes(
    encoding=serialization.Encoding.PEM,
    format=serialization.PrivateFormat.PKCS8,
    encryption_algorithm=serialization.NoEncryption()
)

public_key_pem = public_key.public_bytes(
    encoding=serialization.Encoding.PEM,
    format=serialization.PublicFormat.SubjectPublicKeyInfo
)

print("Private Key:\n", private_key_pem.decode())
print("Public Key:\n", public_key_pem.decode())

```



1. **Imports**:
   * The code imports necessary modules from the `cryptography` library, including backends for cryptographic operations, hashing primitives, asymmetric elliptic curve cryptography (EC), and serialization for key formats.
2. **Key Generation**:
   * The code generates a key pair consisting of a private key and its corresponding public key using the SECP256R1 elliptic curve.
3. **Message Signing**:
   * A message "Hello, world!" is defined.
   * The private key signs the message using the ECDSA algorithm with SHA-256 as the hashing algorithm.
4. **Signature Verification**:
   * The public key verifies the signature against the original message using the ECDSA algorithm with SHA-256 hashing.
5. **Print Keys**:
   * The private and public keys are serialized into PEM format and printed to the console.
6. **Comments**:
   * Throughout the code, comments have been added to explain each step and operation in English.

This code demonstrates the generation of an ECDSA key pair, signing of a message, verification of the signature, and serialization of the keys into PEM format.



## Reference

1\. [https://en.wikipedia.org/wiki/ElGamal\_signature\_scheme](https://en.wikipedia.org/wiki/ElGamal\_signature\_scheme)

2\. [https://medium.com/@shayanmakwana10/elgamal-digital-signature-scheme-860cfb177388](https://medium.com/@shayanmakwana10/elgamal-digital-signature-scheme-860cfb177388)

3\. [https://resources.saylor.org/wwwresources/archived/site/wp-content/uploads/2011/03/ElGamal-signature-scheme.pdf](https://resources.saylor.org/wwwresources/archived/site/wp-content/uploads/2011/03/ElGamal-signature-scheme.pdf)

4\. [https://www.hypr.com/security-encyclopedia/elliptic-curve-digital-signature-algorithm](https://www.hypr.com/security-encyclopedia/elliptic-curve-digital-signature-algorithm)

5\. [https://www.encryptionconsulting.com/education-center/what-is-ecdsa/](https://www.encryptionconsulting.com/education-center/what-is-ecdsa/)

6\. [https://blog.cloudflare.com/ecdsa-the-digital-signature-algorithm-of-a-better-internet/](https://blog.cloudflare.com/ecdsa-the-digital-signature-algorithm-of-a-better-internet/)

7\. [https://academy.bit2me.com/en/what-is-ecdsa-elliptic-curve/](https://academy.bit2me.com/en/what-is-ecdsa-elliptic-curve/)

8\. [https://www.cs.miami.edu/home/burt/learning/Csc609.142/ecdsa-cert.pdf](https://www.cs.miami.edu/home/burt/learning/Csc609.142/ecdsa-cert.pdf)

9\. [https://www.researchgate.net/figure/ECDSA-signature-and-verification-steps\_fig1\_271555939](https://www.researchgate.net/figure/ECDSA-signature-and-verification-steps\_fig1\_271555939)

10\. [https://asecuritysite.com/schnorr](https://asecuritysite.com/schnorr)
