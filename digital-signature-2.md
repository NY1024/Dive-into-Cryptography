# Digital Signature Ⅲ

## Blind Signatures

We first learn blind signatures.

<figure><img src=".gitbook/assets/image (80).png" alt=""><figcaption></figcaption></figure>

The concept of blind signatures was indeed first introduced by David Chaum at the 1982 American Cryptography Conference. The blind signature scheme proposed by Chaum is a cryptographic protocol that allows users to request a digital signature for a specific message from a signer, without the signer knowing the content of the message during the signing process. Even if the signer later discloses the message and signature, they cannot trace back to the specific signature request and its corresponding message.

The basic idea of blind signatures is to use a "blinding factor" to obscure the message to be signed, preventing the signer from learning the original message. Below is a simple protocol flow for blind signatures:

1. Key Pair Generation: The signer and the user each generate a key pair.
2. Blinding Factor Generation: The user generates a random blinding factor and combines it with the message to create a blinded message.
3. Sending Blinded Message: The user sends the blinded message to the signer.
4. Signing: The signer uses their private key to sign the blinded message, producing a digital signature.
5. Returning Signature: The signer returns the digital signature to the user.
6. Unblinding: The user uses the previously generated blinding factor to process the digital signature, obtaining the final signature.

The final signature is based on the original message, but the signer cannot know the content of the original message. This property makes blind signatures well-suited for protecting user privacy, applicable in scenarios such as anonymous voting, electronic cash, etc.

Let's take a look at blind signatures based on the RSA problem.

RSA-based blind signatures are a secure protocol used to ensure that the signer cannot learn the actual message content while signing user messages. In this process, the user generates a blinding factor, combines it with the message to be signed, and then sends the combined data to the signer. The signer signs this blinded data, and the user then uses the blinding factor to unblind the signature, obtaining the final signature.

Here is a detailed explanation of the RSA-based blind signature process:

1. Key Generation:
   * Firstly, both the user and the signer generate a pair of asymmetric keys, including public and private keys. The RSA algorithm is commonly used, where the public key is used for encryption and signature verification, while the private key is used for decryption and signature generation.
2. Blinding Factor Generation:
   * The user generates a random blinding factor, which is a random number used to obscure the message to be signed. The generation of the blinding factor typically occurs on the user's side to ensure that the user maintains control over the message.
3. Message Blinding:
   * The user combines the blinding factor with the message to be signed. The result of this combination is termed the blinded message. This process can be achieved through operations such as XOR or concatenation between the blinding factor and the message.
4. Blind Signature Request:
   * The user sends the blinded message to the signer. Since the blinding factor is generated by the user, the signer cannot know the content of the original message.
5. Blind Signature:
   * The signer uses the private key to digitally sign the blinded message. During this process, the signer only knows the blinded message and is unaware of the blinding factor or the content of the original message.
6. Signature Result Return:
   * The signer returns the result of the digital signature to the user.
7. Unblinding:
   * The user applies the previously generated blinding factor to the signature result, removing the blinding effect and obtaining the final digital signature.
8. Signature Verification:
   * The user can use the signer's public key to verify the validity of the digital signature, ensuring that the message has not been tampered with and was signed by the signer.

RSA-based blind signatures, while ensuring user privacy and data integrity, allow for applications of digital signatures such as anonymous voting systems, electronic cash systems, etc. This technology ensures that the signer cannot access the user's original message, thereby protecting user privacy.

```
# Import necessary modules
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.asymmetric import rsa
from cryptography.hazmat.primitives import serialization
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.asymmetric import padding

def blind_sign(message, private_key):
    # Load the private key from PEM format
    private_key = serialization.load_pem_private_key(
        private_key,
        password=None,
        backend=default_backend()
    )

    # Blind the message
    blinded_message = private_key.public_key().encrypt(
        message.encode(),
        padding.OAEP(
            mgf=padding.MGF1(algorithm=hashes.SHA256()),
            algorithm=hashes.SHA256(),
            label=None
        )
    )

    # Sign the blinded message using the private key
    signature = private_key.sign(
        blinded_message,
        padding.PSS(
            mgf=padding.MGF1(hashes.SHA256()),
            salt_length=padding.PSS.MAX_LENGTH
        ),
        hashes.SHA256()
    )

    return signature

def main():
    # Generate a key pair
    private_key = rsa.generate_private_key(
        public_exponent=65537,
        key_size=2048,
        backend=default_backend()
    )

    # Extract the public key
    public_key = private_key.public_key()

    # Serialize the public key to PEM format
    public_pem = public_key.public_bytes(
        encoding=serialization.Encoding.PEM,
        format=serialization.PublicFormat.SubjectPublicKeyInfo
    )

    # Serialize the private key to PEM format
    private_pem = private_key.private_bytes(
        encoding=serialization.Encoding.PEM,
        format=serialization.PrivateFormat.PKCS8,
        encryption_algorithm=serialization.NoEncryption()
    )

    # Message to be signed
    message = "Hello, World!"

    # Perform blind signature
    signature = blind_sign(message, private_pem)

    # Print the results
    print("Original message:", message)
    print("Blind signature:", signature.hex())
    print("Public key:", public_pem.decode())

if __name__ == "__main__":
    main()

```

Sure, here's an analysis of the provided Python code:

1. **Imports**: The code imports necessary modules from the `cryptography.hazmat` package, which provides cryptographic primitives and algorithms.
2. **Function `blind_sign`**:
   * This function takes a `message` and a `private_key` as input parameters.
   * It loads the private key from the provided PEM format using `serialization.load_pem_private_key`.
   * The message is then encrypted using the public key corresponding to the loaded private key. This step blinds the message to ensure privacy.
   * The blinded message is signed using the private key.
   * The signature is returned.
3. **Function `main`**:
   * This is the main function of the script.
   * It generates an RSA key pair using `rsa.generate_private_key`.
   * The public key is extracted from the generated private key.
   * Both the public and private keys are serialized to PEM format using `public_bytes` and `private_bytes` respectively.
   * A message "Hello, World!" is defined for signing.
   * The `blind_sign` function is called to obtain a blind signature for the message using the private key.
   * Finally, the original message, blind signature, and public key are printed.
4. **Execution**:
   * The `main` function is executed if the script is run as the main program.
5. **Output**:
   * The original message, blind signature (in hexadecimal format), and the public key in PEM format are printed to the console.

This code demonstrates the process of blind signing a message using an RSA key pair and the cryptography library in Python. Blind signatures are used to preserve the privacy of the message being signed, as the signer does not know the content of the message during the signing process.

## Group Signature

Now we're learning about group signatures.

![](<.gitbook/assets/image (1) (1).png>)\


Group Signature is a special type of digital signature scheme, first proposed by David Chaum and Eugeen van Heyst in 1991. This signature scheme possesses unique properties that allow members to anonymously sign on behalf of the entire group while still enabling the group manager to trace them when necessary.

Key features of the group signature scheme:

1. Anonymity:
   * Group signatures protect the identity of signing members, allowing verifiers to only determine that the signature is generated by a member of the group without identifying the specific member. This anonymity permits any member of the group to sign on behalf of the entire group, making it indistinguishable for outsiders to trace the true origin of the signature.
2. Traceability:
   * Despite providing anonymity, group managers can "open" signatures to reveal the identity of the signer when necessary. This ensures signers cannot deny their signature actions, thereby making signatures traceable. This feature is useful in dealing with illegal activities or disputes.
3. Unlinkability:
   * The group signature scheme also features unlinkability, meaning without opening the group signature, no one can determine if two group signatures are generated by the same member. This ensures there are no direct associations between different signatures, enhancing the privacy of signatures.
4. Revocability:
   * Some group signature schemes also include revocability, allowing group managers to revoke a member's signing authority in specific circumstances, rendering signatures generated by that member invalid.

Applications of group signature schemes include anonymous authentication, anonymous access control, digital cash systems, etc., where a balance between anonymity and traceability is required. Group signatures provide an effective means to protect individual privacy while also providing some tracking capabilities for institutions needing to investigate illegal activities.



Let's take a look at a simple group signature scheme:

1. System Initialization Algorithm:
   * The group manager (GM) is responsible for initializing the system. For each group member, the GM distributes a secret key table. These tables are disjoint, meaning each member can only use their own private key table. The GM collects all members' private keys and arranges the corresponding public keys in a random order to form a public key table, which is then made public. This way, each member has a private key table and a public key table for verification purposes.
2. Signature Generation:
   * When a group member needs to create a group signature, they select an unused private key from their private key table and use it to sign the message. This ensures that each private key can only be used once, preventing replay attacks on signatures.
3. Signature Verification:
   * When other members or verifiers need to verify a group signature generated by a group member, they attempt verification using each public key from the public key table. If a public key is found that successfully verifies the signature, it indicates the signature is valid.
4. Signature Opening:
   * In case of disputes or the need to trace the signer's identity, the group manager (GM) can recover the signer's identity based on the correspondence between the signature and the public key table. This process, known as signature opening, ensures the ability to trace the signer's identity when needed.

It's important to note that in this scheme, group members are fixed at system initialization, and each group member's private key can only be used once to preserve unlinkability. Additionally, this simple scheme assumes the group manager is trusted and will not attempt to forge group members' identities.

This is just a simple example in the field of group signatures. Actual applications may involve more complexity and security considerations. In real systems, it's common to address issues such as dynamic membership management, security proofs, revocation mechanisms, etc.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.asymmetric import rsa, padding, utils

class GroupSignatureSystem:
    def __init__(self, num_members):
        self.num_members = num_members
        self.private_keys = {}
        self.public_keys = []

    def initialize_system(self):
        for _ in range(self.num_members):
            private_key = rsa.generate_private_key(
                public_exponent=65537,
                key_size=2048,
                backend=default_backend()
            )
            public_key = private_key.public_key()
            
            private_key_bytes = private_key.private_bytes(
                encoding=serialization.Encoding.PEM,
                format=serialization.PrivateFormat.PKCS8,
                encryption_algorithm=serialization.NoEncryption()
            )
            
            self.private_keys[public_key] = private_key_bytes
            self.public_keys.append(public_key)

    def sign_message(self, message, member_index):
        private_key = self.private_keys[self.public_keys[member_index]]
        
        private_key = serialization.load_pem_private_key(
            private_key,
            password=None,
            backend=default_backend()
        )
        
        signature = private_key.sign(
            message.encode(),
            padding.PSS(
                mgf=padding.MGF1(hashes.SHA256()),
                salt_length=padding.PSS.MAX_LENGTH
            ),
            hashes.SHA256()
        )
        
        return signature

    def verify_signature(self, signature, message):
        for public_key in self.public_keys:
            try:
                public_key.verify(
                    signature,
                    message.encode(),
                    padding.PSS(
                        mgf=padding.MGF1(hashes.SHA256()),
                        salt_length=padding.PSS.MAX_LENGTH
                    ),
                    hashes.SHA256()
                )
                return True
            except:
                continue
        return False

def main():
    num_members = 5
    group_system = GroupSignatureSystem(num_members)
    group_system.initialize_system()

    message = "Hello, World!"

    # Each member signs the message
    for i in range(num_members):
        signature = group_system.sign_message(message, i)
        print(f"Signature from member {i}: {signature.hex()}")

    # Verify the signatures
    for i in range(num_members):
        signature = group_system.sign_message(message, i)
        verification_result = group_system.verify_signature(signature, message)
        print(f"Signature from member {i} is valid: {verification_result}")

if __name__ == "__main__":
    main()

```

Here's an analysis of the provided Python code:

1. **Imports**:
   * The code imports necessary modules from the `cryptography.hazmat` package, including `default_backend`, `serialization`, `hashes`, `rsa`, `padding`, and `utils`.
2. **Class `GroupSignatureSystem`**:
   * This class represents a group signature system.
   * It has attributes to store the number of members, private keys, and public keys.
   * The `initialize_system` method initializes the system by generating private-public key pairs for each member and storing them.
   * The `sign_message` method takes a message and a member index as input, retrieves the corresponding private key, signs the message using RSA-PSS with SHA-256 hashing, and returns the signature.
   * The `verify_signature` method takes a signature and a message as input, iterates over public keys to find the corresponding one, and verifies the signature using RSA-PSS with SHA-256 hashing. If verification succeeds for any public key, it returns `True`; otherwise, it returns `False`.
3. **Function `main`**:
   * This function serves as the main entry point of the script.
   * It initializes a group signature system with a specified number of members and then initializes the system.
   * It defines a message to be signed.
   * It iterates over each member, signs the message, and prints out the signature for each member.
   * It then verifies each signature against the original message and prints out the verification result.
4. **Execution**:
   * The `main` function is executed if the script is run as the main program.
5. **Output**:
   * For each member, the script prints the signature of the message.
   * After signing, it verifies each signature against the original message and prints whether each signature is valid.

Overall, this code demonstrates a basic implementation of a group signature system using RSA-PSS with SHA-256 hashing for signing and verification. It illustrates the initialization, signing, and verification processes within such a system.

## Ring Signatures

Now we're learning about ring signatures.

Ring Signature is a cryptographic technique first proposed by Ron Rivest and others in 2001. In a ring signature scheme, a signer can use the public keys of other members in the ring, along with their own private key, to sign a message. Verification of the signature can confirm its validity, but cannot determine which specific member in the ring generated the signature. This provides a certain degree of anonymity for the signer.

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

Characteristics:

1. Anonymity: Ring signature scheme provides anonymity for the signer. Members in the ring can collectively produce a signature, while verifiers can only confirm the validity of the signature without identifying the specific signer.
2. Irrevocable Anonymity: Unlike some other anonymity schemes, the anonymity provided by ring signatures is irrevocable. Even after signing, there is no way to determine the specific signer through any means.

Applications:

1. Anonymous Whistleblowing: Ring signatures can be used to anonymously provide information to the public or authorities without exposing individual identities. For example, within a government, members can collectively sign to anonymously disclose certain sensitive information.
2. Anonymous Voting: In digital voting systems, voters can use ring signatures to cast their votes, protecting their voting privacy while ensuring the validity of the votes.
3. Anonymous Authentication: Ring signatures can be used to achieve anonymous identity authentication, where an entity can prove its affiliation with a group without disclosing its specific identity.
4. Privacy-Preserving Digital Currency: Ring signatures have been widely used in some privacy-preserving digital currency systems, such as Monero. Users can conduct transactions without revealing their identities.

Overall, ring signatures provide a means to balance anonymity and trustworthiness, suitable for a range of scenarios where individual privacy needs protection while ensuring trustworthiness.

Let's take a look at a ring signature scheme without linkability.

A ring signature scheme without linkability is a special type of ring signature system where different ring signatures generated cannot be directly linked to the same signer. This means that observing two different ring signatures cannot determine if they share the same subset of members.

First, it's necessary to understand the basic concepts of a ring signature scheme:

Ring Signature Members: In a ring signature system, there is a set of members, each with a private key and a corresponding public key. Members can collectively generate a ring signature, but verifiers cannot identify which specific member in the ring generated the signature.

Ring Signature Generation: During ring signature generation, each member generates their own partial signature. The final ring signature is a combination of all members' partial signatures, without revealing their individual contributions.

Verifiers validate the validity of a ring signature by checking if an equation holds true. This equation utilizes bilinear mapping, ensuring the anonymity of the ring signature.

In a ring signature scheme without linkability, the generation and verification of ring signatures adhere to the following characteristics:

1. Unlinkability:
   * There is no direct link established between two different ring signatures. In other words, verifiers cannot determine if two ring signatures share the same subset of members by observing them.
2. Anonymity:
   * The ring signature scheme provides anonymity for the signer. Verifiers can only confirm the validity of the signature but cannot identify the specific signer.

Applications: A ring signature scheme without linkability is suitable for scenarios where signer anonymity needs protection, but signatures should not be traceable to the same entity. For example:

* Anonymous Whistleblowing: Allowing individuals to anonymously provide information to the public or authorities without revealing their identities.
* Anonymous Authentication: It can be used to achieve anonymous identity authentication, where an entity can prove its affiliation with a group without disclosing its specific identity.
* Privacy-Preserving Digital Currency: In some privacy-preserving digital currency systems, ring signatures without linkability can ensure transaction privacy.

Overall, a ring signature scheme without linkability provides a way to balance privacy protection and trustworthiness in specific scenarios.

Let's learn the basic steps.

A ring signature scheme without linkability is a special ring signature scheme designed to prevent different ring signatures from being directly linked to the same signer.

Ring Signature Generation Algorithm (ring-sign):

1. Compute Message Hash:
   * First, compute the hash value of the message m, typically denoted as H(m).
2. Choose Random Numbers:
   * For each member k in the ring, choose a random number ai ∈ Zq, where i ∈ {1, ..., n}, and compute a = a1 + a2 + ... + an.
3. Compute Partial Values of the Ring Signature:
   * For each member i, compute the partial signature σi = a \* Pi, where Pi is the public key point of member i.
4. Compute the Ring Signature Value:
   * Compute the ring signature value σ = Σ(aj \* Y \* xj) for j ∈ {1, ..., n}, where Y is the generator element of group G1, and xj is the private key of member j.
5. Output:
   * Output the ring signature value σ as the signature for the message m.

Ring Signature Verification Algorithm (ring-verify):

1. Compute Message Hash:
   * Compute the hash value of the message m, denoted as H(m).
2. Verify Equation:
   * For each member i in the ring, verify if the following equation holds true: e(H, P) = Πe(σi, Yi), where e is the bilinear map from group G1 to G2, Pi is the public key of member i, and Yi is the public key point of member i.
3. Accept/Reject:
   * If the equation holds true, accept the signature as valid; otherwise, reject the signature.



Characteristics of Non-Linkability: In a ring signature scheme without linkability, the ring signature generation process utilizes member private keys and an overall random number a, but does not use previous ring signature values. This results in each ring signature being independent of one another, making it impossible to determine if two ring signatures share the same subset of members by observing them.

Notes:

* A ring signature scheme without linkability typically ensures the independence of ring signatures by not introducing the previous ring signature value as input.
* This characteristic ensures that the signer's identity is anonymous and untraceable across different ring signatures.
* Bilinear mapping is used during signature generation and verification processes to ensure the correctness of the verification equation.

Overall, a ring signature scheme without linkability provides a means to balance anonymity and trustworthiness, suitable for scenarios where individual privacy needs protection without the requirement of linkability.

```
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives import serialization, hashes
from cryptography.hazmat.primitives.asymmetric import ec

class RingSignatureSystem:
    def __init__(self, num_members):
        self.num_members = num_members
        self.private_keys = {}
        self.public_keys = []

    def initialize_system(self):
        for _ in range(self.num_members):
            private_key = ec.generate_private_key(
                ec.SECP256R1(),  # You can choose an appropriate elliptic curve
                default_backend()
            )
            public_key = private_key.public_key()

            private_key_bytes = private_key.private_bytes(
                encoding=serialization.Encoding.PEM,
                format=serialization.PrivateFormat.PKCS8,
                encryption_algorithm=serialization.NoEncryption()
            )

            self.private_keys[public_key] = private_key
            self.public_keys.append(public_key)

    def ring_sign(self, message, member_index):
        private_key = self.private_keys[self.public_keys[member_index]]

        # Simplified signing process, actual process involves more complex cryptographic operations
        signature = private_key.sign(
            message.encode(),
            ec.ECDSA(hashes.SHA256())
        )

        return signature

    def ring_verify(self, signature, message):
        for public_key in self.public_keys:
            try:
                public_key.verify(
                    signature,
                    message.encode(),
                    ec.ECDSA(hashes.SHA256())
                )
                return True
            except:
                continue
        return False

def main():
    num_members = 5
    ring_system = RingSignatureSystem(num_members)
    ring_system.initialize_system()

    message = "Hello, World!"

    # Each member signs the message
    for i in range(num_members):
        signature = ring_system.ring_sign(message, i)
        print(f"Signature from member {i}: {signature.hex()}")

    # Verify the signatures
    for i in range(num_members):
        signature = ring_system.ring_sign(message, i)
        verification_result = ring_system.ring_verify(signature, message)
        print(f"Signature from member {i} is valid: {verification_result}")

if __name__ == "__main__":
    main()

```



The provided Python code implements a basic ring signature system using elliptic curve cryptography (ECC). Let's break down the code:

1. **Imports**:
   * The code imports necessary modules from the `cryptography.hazmat` package, including `default_backend`, `serialization`, `hashes`, and `ec` (elliptic curve).
2. **Class `RingSignatureSystem`**:
   * This class represents a ring signature system.
   * It has attributes to store the number of members, private keys, and public keys.
   * The `initialize_system` method initializes the system by generating private-public key pairs for each member and storing them.
   * The `ring_sign` method takes a message and a member index as input, retrieves the corresponding private key, and signs the message using ECDSA (Elliptic Curve Digital Signature Algorithm) with SHA-256 hashing. (Note: The actual signing process involves more complex cryptographic operations.)
   * The `ring_verify` method verifies the signature against the message for each public key in the ring.
3. **Function `main`**:
   * This function serves as the main entry point of the script.
   * It initializes a ring signature system with a specified number of members and then initializes the system.
   * It defines a message to be signed.
   * It iterates over each member, signs the message, and prints out the signature for each member.
   * It then verifies each signature against the original message and prints whether each signature is valid.
4. **Execution**:
   * The `main` function is executed if the script is run as the main program.

Overall, this code demonstrates a basic implementation of a ring signature system using ECC. It illustrates the initialization, signing, and verification processes within such a system.



## Reference

1\. [https://en.wikipedia.org/wiki/Blind\_signature](https://en.wikipedia.org/wiki/Blind\_signature)

2\. [https://www.educative.io/answers/what-is-a-blind-signature](https://www.educative.io/answers/what-is-a-blind-signature)

3\. [https://en.wikipedia.org/wiki/Ring\_signature](https://en.wikipedia.org/wiki/Ring\_signature)

4\. [https://www.researchgate.net/figure/Group-signature-modes\_fig1\_8645068](https://www.researchgate.net/figure/Group-signature-modes\_fig1\_8645068)

5\. [https://www.mdpi.com/2073-8994/12/12/2009](https://www.mdpi.com/2073-8994/12/12/2009)

6\. [https://devpost.com/software/ring-signatures-based-anonymous-voting](https://devpost.com/software/ring-signatures-based-anonymous-voting)

7\. [https://medium.com/ppio/ring-signatures-privacy-protection-so-you-can-hide-in-a-crowd-40180a732bce](https://medium.com/ppio/ring-signatures-privacy-protection-so-you-can-hide-in-a-crowd-40180a732bce)
