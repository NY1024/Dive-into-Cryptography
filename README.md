# Classical Cryptography

## Basic

What is cryptography? Cryptography is the study of how to secure information in communication or computer systems. It encompasses the theory and practice of developing and using cryptographic techniques to ensure that only authorized individuals can access information, while unauthorized individuals are unable to comprehend or alter the information. Cryptography typically includes the following aspects:

1. Encryption Algorithms: Encryption algorithms are a core component of cryptography, used to transform readable plaintext into unreadable ciphertext to protect the confidentiality of information. Common encryption algorithms include symmetric and asymmetric encryption algorithms.
2. Key Management: Keys are special codes used during the encryption and decryption processes. Cryptography focuses on how to generate, distribute, use, and manage keys to ensure security and reliability.
3. Digital Signatures: Digital signatures are a technique used to verify the integrity and authenticity of files. They use asymmetric encryption to generate a unique digital digest for a file, ensuring that the file has not been tampered with during transmission.
4. Security Protocols: Security protocols are a set of rules and steps designed to ensure the security of communication and information exchange. For example, the SSL/TLS protocol is used for secure website communication.
5. Authentication: Cryptography is employed to develop authentication methods, ensuring that only authorized users can access systems or data.

Cryptography plays a crucial role in the field of information security, with applications spanning network communication, e-commerce, database management, email, and various other areas. Its goal is to provide security services such as confidentiality, integrity, and authentication, ensuring that data and communication are not subject to unauthorized access or modification during transmission and storage.

When studying cryptography, it is essential to grasp fundamental concepts, including plaintext, ciphertext, encryption, and decryption.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

\
Plaintext : Plaintext refers to the original data or text in its raw form, without any encryption or transformation. It is the raw representation of information without any form of protection or concealment.

Ciphertext: Ciphertext refers to data or text that has undergone processing by an encryption algorithm. It is transformed from plaintext into a form that is not easily understood or recognized by using an encryption key, providing confidentiality and security.

Encryption: Encryption is the process of transforming plaintext into ciphertext. It involves using specific encryption algorithms and keys to convert information, ensuring that only those with the correct key can decrypt and restore the original information.

Decryption: Decryption is the reverse process of encryption, converting ciphertext back into plaintext. Only individuals with the correct decryption key can effectively perform the decryption operation.

In encryption systems, the selection of encryption algorithms and keys is crucial. Robust encryption algorithms and secure key management are key factors in ensuring information security. Encryption plays a vital role in protecting communication, data storage, and information transmission, especially in the fields of network security and privacy protection.

In cryptography, there are two main branches: Cryptanalysis and Cryptography.

1. Cryptography: This is the design aspect of cryptography, focusing on developing secure encryption algorithms and systems to ensure the confidentiality, integrity, and availability of information during transmission and storage. Cryptography covers aspects such as the design of encryption algorithms, key management, digital signatures, security protocols, etc. Its goal is to provide a way to protect data from unauthorized access or tampering.
2. Cryptanalysis: This is the attack aspect of cryptography, aiming to study and develop techniques to break cryptographic systems, crack encryption algorithms, or obtain encrypted data. The goal of cryptanalysis is to understand the weaknesses of cryptographic systems in order to propose effective attack methods. Key recovery, plaintext attacks, and chosen plaintext attacks are all within the scope of cryptanalysis.

There is a competitive relationship between these two branches: cryptography strives to design more powerful encryption systems, while cryptanalysis attempts to find and exploit weaknesses in these systems. This competition drives continuous development in the field of cryptography to adapt to increasingly complex information security challenges. In practice, a successful cryptographic system needs to undergo extensive cryptanalysis testing to verify its security.

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

\
Cryptography is a branch of cryptology that focuses on the design and development of encryption algorithms and systems to safeguard information security. The goal of cryptography is to ensure confidentiality, integrity, and availability during the processes of information transmission and storage.

1. Encryption Algorithms: Encryption algorithms are the core of cryptography. They are responsible for converting plaintext into ciphertext, ensuring that only authorized users can comprehend and restore the original information. There are two main types of encryption algorithms:
   * Symmetric Encryption Algorithm: Uses the same key for both encryption and decryption. Common symmetric encryption algorithms include Advanced Encryption Standard (AES) and Data Encryption Standard (DES).
   * Asymmetric Encryption Algorithm: Uses a pair of keys, namely a public key and a private key, for encryption and decryption. RSA (Rivest-Shamir-Adleman) is a common asymmetric encryption algorithm.
2. Key Management: Keys are a critical component used during the encryption and decryption processes. Cryptography focuses on how to generate, distribute, use, and manage keys to ensure their security. Key management also includes aspects such as key updates, revocation, and storage.
3. Digital Signatures: Digital signatures are a technology used to verify the integrity of files and confirm their source. Cryptography utilizes asymmetric encryption algorithms to generate digital signatures, ensuring the authenticity and non-tampering of information. Digital signatures play a crucial role in authentication and data integrity verification.
4. Security Protocols: Security protocols are rules and procedures designed to protect information security during communication. The SSL/TLS protocol is a common security protocol used for encrypting network communication, ensuring the confidentiality and integrity of data.
5. Authentication: Cryptography involves designing methods to ensure that only authorized users can access systems or data. This includes techniques such as password authentication, two-factor authentication, and biometrics.
6. Randomness: Randomness in cryptography refers to the introduction of random elements during the encryption process to enhance the security of cryptographic systems. This may include initialization vectors (IV) and other random elements.

Cryptography is a crucial component of information security, aiming to provide a reliable way to protect sensitive information from unauthorized access and tampering. In practice, cryptography needs to evolve continuously to adapt to evolving threats and attack techniques.

Cryptanalysis is a branch of cryptography that focuses on studying and developing techniques to break cryptographic systems, crack encryption algorithms, or obtain encrypted data. Its goal is to understand the weaknesses of cryptographic systems in order to propose effective attack methods. Cryptanalysis involves various attack techniques and methods:

1. **Key Recovery:** Cryptanalysis attempts to recover the key used by analyzing the relationship between ciphertext, plaintext, or the key itself. This may include known-plaintext attacks, chosen-plaintext attacks, or other methods to obtain key information.
2. **Known-Plaintext Attack:** In a known-plaintext attack, the attacker has access to some plaintext and the corresponding ciphertext. By analyzing these pairs, the attacker tries to deduce information about the encryption algorithm or key. Some simplified versions of symmetric encryption algorithms may be susceptible to known-plaintext attacks.
3. **Chosen-Plaintext Attack:** Unlike known-plaintext attacks, in a chosen-plaintext attack, the attacker can choose some plaintext and obtain the corresponding ciphertext. This attack model is more challenging as the attacker can selectively choose plaintext to gain a better understanding of the internal mechanisms of the cryptographic system.
4. **Ciphertext-Only Attack:** In a ciphertext-only attack, the attacker only has access to the encrypted ciphertext and cannot obtain corresponding plaintext or other information. This is a more challenging attack model as the attacker needs to deduce the key or plaintext through ciphertext analysis.
5. **Differential Cryptanalysis:** Differential cryptanalysis is an attack technique that attempts to deduce part or the entire key of a cryptographic algorithm by observing differences between input and output. This method is often used against symmetric encryption algorithms.
6. **Side-Channel Attacks :** Side-channel attacks exploit information related to the implementation of a cryptographic system, such as power consumption, execution time, or electromagnetic radiation, to gather information about the key or plaintext.

The development of cryptanalysis is an important aspect of cryptographic progress. Successful cryptanalysis often leads to improvements in cryptographic systems to resist new attack techniques. Therefore, designers and researchers in cryptography must continually consider and address the challenges posed by cryptanalysis to enhance the security of cryptographic systems.



| **Attack Type**            | **Description**                                                              |
| -------------------------- | ---------------------------------------------------------------------------- |
| Known-Plaintext Attack     | Attacker has access to some plaintext and corresponding ciphertext.          |
| Chosen-Plaintext Attack    | Attacker can choose plaintext and obtain corresponding ciphertext.           |
| Ciphertext-Only Attack     | Attacker only has access to encrypted ciphertext.                            |
| Differential Cryptanalysis | Attack technique observing differences between input and output.             |
| Side-Channel Attacks       | Exploit information related to the implementation of a cryptographic system. |

The table below lists types of attacks on encrypted information:

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

* Ciphertext-Only Attack: In a ciphertext-only attack, the attacker only has access to encrypted ciphertext and cannot obtain corresponding plaintext or other information. This is a more challenging attack model as the attacker needs to deduce information about the key or plaintext through the analysis of ciphertext.
* Known-Plaintext Attack: In this type of attack, the attacker can acquire some known plaintext and the corresponding ciphertext. By analyzing these known plaintext-ciphertext pairs, the attacker attempts to deduce information about the encryption algorithm or key, making it easier to decrypt other messages.
* Chosen-Plaintext Attack: In this attack, the attacker can choose some plaintext and obtain the corresponding ciphertext. The attacker has the ability to selectively choose plaintext with a purpose, gaining a better understanding of the workings of the cryptographic system.
* Chosen-Ciphertext Attack: This is a more advanced attack where the attacker can choose some ciphertext and obtain the corresponding plaintext. This attack often involves a deep understanding of the encryption or decryption process, allowing the attacker to selectively obtain the results of decryption.
* Chosen-Text Attack : In a chosen-text attack, the attacker can choose some ciphertext and obtain the corresponding decrypted result. The attacker selectively submits specific ciphertext and then observes or obtains the decrypted plaintext. This attack model often considers the possibility that the attacker may have access to decryption services or devices.

## Substitution and **Transposition**

After understanding some essential basics of cryptography, we now turn to the study of classical ciphers.

The two most commonly used techniques in classical ciphers are substitution and permutation, used to change the positions or values of letters or bits in the plaintext to increase encryption complexity.

**Substitution :** Substitution refers to the process of replacing elements in the plaintext (usually letters) with another set of elements. This substitution can be one-to-one, many-to-one, or one-to-many. Substitution ciphers are among the earliest ciphers, where each letter or group of letters is mapped to another letter or group of letters. The Caesar cipher is a simple example, where letters are replaced according to a fixed offset.

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Transposition:** Transposition refers to the rearrangement of elements in the plaintext rather than replacing them. In transposition ciphers, the ciphertext is typically created by rearranging or swapping the letters in the plaintext. The rail fence cipher is a simple transposition cipher where letters are arranged on a rail or fence according to a specific rule, and then read off either row-wise or column-wise to generate the ciphertex.

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

These two techniques are often combined to form more complex cryptographic systems. For instance, some modern cryptographic algorithms use the structure of a Substitution-Permutation Network (SPN), where substitution and permutation operations alternate to enhance the security of the cryptographic system. This combination leverages the confusion from substitution and the diffusion from permutation, making it more challenging for attackers to comprehend and reverse-engineer the cryptographic algorithm.

## Caesar cipher

Let's begin by learning about the Caesar Cipher.

The Caesar Cipher is a simple substitution cipher, also known as a shift cipher. It is one of the earliest ciphers in classical cryptography and is named after the ancient Roman military leader Julius Caesar. It is said that Caesar used this encryption technique in military communications. The basic idea of the Caesar Cipher is to encrypt text by replacing letters with a fixed offset.

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

The specific steps are as follows:

1. **Choose Offset:** Select an integer as the offset, often referred to as the key of the Caesar Cipher. This offset determines the letter substitution rule.
2. **Encrypting the Plaintext:** For the text to be encrypted, replace each letter with the offset. For the English alphabet, a positive offset means a right shift, and a negative offset means a left shift. When a letter goes beyond the boundaries, it can wrap around. For example, if the chosen offset is 3, the letter "A" in the plaintext would be replaced with the letter "D" in the ciphertext, "B" would be replaced with "E," and so on.
3. **Decrypting the Ciphertext:** The decryption process is the reverse of encryption. Use the same offset to replace the letters in the ciphertext back to plaintext.

The encryption process of the Caesar Cipher can be represented using mathematical symbols: \[E(x) = (x + k) \mod 26] Where:

* (E(x)) represents the result of encrypting the letter (x).
* (k) is the offset.
* (\mod 26) ensures wrapping within the alphabet.

The security of the Caesar Cipher is very low as it has only 26 possible keys, and it can be easily cracked through a simple exhaustive search. In modern cryptography, the Caesar Cipher is used solely for educational purposes and not for securing sensitive information.

Exhaustive Search Attack, also known as a Brute Force Attack, is a fundamental cryptographic attack method. For the Caesar Cipher, conducting an exhaustive search is straightforward due to its limited key space.

Here are the general steps for an exhaustive search attack on the Caesar Cipher:

1. **Identify the Ciphertext:** The attacker obtains the encrypted ciphertext, the message encrypted using the Caesar Cipher.
2. **Exhaustive Offset Trial:** The attacker tries all possible offsets, ranging from 1 to 25 (as an offset of 0 is equivalent to no encryption, and an offset of 26 is the same as returning to the original text).
3. **Decrypt and Check Results:** For each offset, the attacker decrypts the ciphertext and checks if the resulting decryption is meaningful. This often involves using a dictionary or language model to determine which decrypted result makes sense.
4. **Determine the Correct Offset:** The attacker identifies the offset that produces a meaningful plaintext, successfully breaking the Caesar Cipher.

The time complexity of an exhaustive search attack is O(25) since there are 25 possible offsets. While this is a very inefficient attack method, it is often feasible due to the weaknesses of the Caesar Cipher.

One method to counteract exhaustive search attacks is to use more complex cryptographic algorithms with larger key spaces, making it impractical to attempt all possible keys. In modern cryptography, strong symmetric encryption algorithms and hash functions typically have sufficiently large key spaces, rendering exhaustive search attacks very difficult.

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
