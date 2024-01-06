# Mathematical Fundamentals Ⅰ

## Information theory

Claude Shannon's "Communication Theory of Secrecy Systems" is a classic work in the field of cryptography, published in 1949. This paper has had a profound impact on cryptography, laying the foundation for the development of information theory and modern cryptography. The following are the main contributions and influences of this paper on cryptography:

1. Foundation of Information Theory: Shannon's paper laid the groundwork for information theory. He introduced the concept of information entropy to measure the uncertainty of messages and the amount of information. Information entropy became a crucial metric for assessing the security of cryptographic systems, providing theoretical support for cryptography.
2. Integration of Information Theory and Cryptography: Shannon merged concepts from information theory with cryptography, introducing the definition of "perfect secrecy" and the fundamental principles of cryptographic systems. He demonstrated that using a one-time key (one-time pad) could achieve perfect secrecy, opening new directions for the theoretical development of cryptography.
3. Relationship between Key Length and Security: In the paper, Shannon discussed the relationship between key length and the security of cryptographic systems. He proposed that the key length should meet certain requirements relative to the message length to ensure system security. This had a profound impact on the design of symmetric key cryptographic systems.
4. Application of Information Theory: Shannon's information theory provided new theoretical tools for various aspects of cryptography, including key management and security analysis of cryptographic systems. These tools became indispensable in both the research and practical application of cryptography.
5. Driving Force for Cryptographic Development: Shannon's "Communication Theory of Secrecy Systems" injected new vitality and momentum into cryptography. His theoretical framework sparked widespread academic interest in cryptography, driving theoretical research and practical applications.

In summary, Claude Shannon's paper provided a theoretical foundation for cryptography, introducing information theory to the field and establishing a solid groundwork for cryptographic development. His work provided profound insights and research directions for later cryptographers.

Before we proceed, let's understand several methods for evaluating the security of cryptographic systems:

Firstly, there is computational security.

In cryptography, computational security refers to the strength and difficulty of resisting attacks in terms of computation. Computational security focuses on the difficulty attackers face in breaking a cryptographic system through computation and algorithmic analysis, typically related to the size of the algorithm's key space, cryptographic analysis techniques, and the availability of computational resources.

The key space represents the number of possible key combinations in a cryptographic algorithm. Computational security depends on the size of the key space because attackers need to try all possible keys to successfully break the encryption. The larger the key space, the more computational work is required to crack the system, making the cryptographic system more secure.

A crucial aspect of computational security is resistance against exhaustive attacks, where attackers attempt all possible keys. A secure cryptographic system should have a sufficiently large key space to make exhaustive attacks impractical.

Computational security is also influenced by cryptographic analysis techniques, involving the use of mathematical methods, statistical techniques, or algorithms to break the encryption. Cryptographic algorithms should exhibit robust resistance against various cryptographic analysis techniques.

With the development of quantum computing technology, the concept of computational security is evolving. Quantum computing poses potential threats to traditional cryptographic algorithms such as RSA and elliptic curve encryption. Hence, researchers are exploring cryptographic algorithms resistant to quantum computing attacks, known as "post-quantum cryptography."

Computational security also involves the time and resources required for cryptographic attacks. A secure cryptographic system should be challenging to break within a limited time and using finite computational resources, considering that attackers may employ more powerful hardware and advanced algorithms.

Overall, computational security is a key aspect of assessing a cryptographic system's resistance against computational attacks. When designing and selecting cryptographic algorithms, factors such as key space, cryptographic analysis techniques, and quantum computing need to be considered to ensure an adequate level of security in practical use.

Next, let's delve into Provable Security.

Provable Security is a method in cryptography that relies on mathematical proofs to ensure the security of a cryptographic system. Unlike traditional "security through trial and error" methods, Provable Security utilizes mathematical theorems and proofs to guarantee the security of cryptographic systems. This approach aims to provide stronger and more comprehensive security assurances, not solely relying on observations that current attacks have not been successful.

A key idea in Provable Security is the use of the "reduction model," where the security of a cryptographic problem is reduced to a known mathematical problem. This reduction proof indicates that if an attacker can successfully compromise a cryptographic system, they can also solve the underlying mathematical problem, which is considered secure.

Provable Security proofs typically depend on strict and formal definitions of security. For instance, in public-key encryption systems, security might be defined as the impossibility of a "chosen ciphertext attack (IND-CCA)." Formalized definitions contribute to a precise understanding of security.

In certain cryptographic protocols, Provable Security involves the concept of zero-knowledge proofs. Zero-knowledge proofs allow a prover to convince a verifier of the truth of a statement without revealing any additional information about the statement. This helps ensure that information in the protocol is not leaked.

In Provable Security, a precise model is often defined to describe the interaction between attackers and the protocol. This can include definitions for active adversaries and passive adversaries, as well as restrictions on the information attackers can obtain.

In cryptography, Provable Security proofs may sometimes rely on the random oracle model, where an ideal random function (oracle) is used to model the randomness available to attackers. This helps simplify the analysis of protocols.

In Provable Security proofs, a comparison is often made between the standard model and the ideal model. The standard model refers to the protocol actually running in the real world, while the ideal model is an idealized description of the protocol. By proving the security equivalence between the two models, one can ensure that the protocol is secure in practical applications.

In summary, Provable Security provides a more powerful and systematic way to assess the security of cryptographic protocols and algorithms. Through mathematical proofs, we can have greater confidence in the security of a cryptographic system in various attack scenarios.

Additionally, let's explore Unconditional Security.

In cryptography, Unconditional Security is an extremely powerful security concept based on information theory and mathematical principles, independent of computational capabilities. Even in situations where attackers have unlimited computational power, they cannot break the cryptographic system. This is an extremely robust security property, also known as information-theoretic security or absolute security.

Unconditional Security is founded on principles from information theory, primarily introduced by Claude Shannon in the 1940s. It utilizes concepts such as information entropy, information theory, and probability theory.

Information entropy measures the uncertainty contained in a message. In cryptography, information entropy is used to describe the size of the key space and the unpredictability of a cryptographic system. Unconditional security requires the system's information entropy to be sufficiently high, preventing attackers from gaining additional information about the key through any method.

The One-Time Pad is a typical example of unconditional security. In a One-Time Pad system, the key is as long as the message, and the key is entirely random. In ideal circumstances, a One-Time Pad system provides absolute security, even if the attacker has unlimited computational power.

Shannon introduced information-theoretic inequalities, such as Shannon's Inequality, which play a crucial role in describing information transmission and cryptographic security.

An important concept in unconditional security is Perfect Secrecy. In a system with perfect secrecy, attackers cannot obtain any information about the plaintext, regardless of the method they use on the ciphertext.

Unconditional security also involves key distribution issues. In many cases, achieving unconditional security requires ensuring secure key distribution, which may involve physical channels or other secure distribution means.

It is essential to note that while unconditional security provides a very powerful security guarantee, in practical applications, due to certain limitations and challenges, it is more common to adopt security based

on computational complexity, such as public-key cryptography. Unconditional security often relies on theoretical assumptions, such as keys being completely random, which can be challenging to implement in practice.

## Probability theory

Now, we are learning about the part of probability theory related to cryptography.

Probability theory is a branch of mathematics that studies the regularities and statistical regularities of random phenomena.

1. Random Experiment: A random experiment is a trial with uncertainty, and its results are random. For example, flipping a coin, rolling a die, and sampling are all random experiments.
2. Sample Space and Sample Points: The set of all possible outcomes of a random experiment is called the sample space, denoted by S. Each element in the sample space is called a sample point.
3. Event: A subset of the sample space is called an event. An event is a description of the results of a random experiment. Events are usually represented by capital letters A, B, C, ...
4. Probability: For each event A, a real number associated with event A is called the probability of event A, usually denoted by P(A). The probability ranges from \[0, 1], where 0 represents an impossible event, and 1 represents a certain event.
5. Properties of Probability:

* For any event A, 0 ≤ P(A) ≤ 1.
* The probability of a certain event is 1, i.e., P(S) = 1.
* The probability of an impossible event is 0, i.e., P(∅) = 0.
* For the complement of any event A denoted as A', P(A') = 1 - P(A).
* If events A and B are mutually exclusive (i.e., cannot occur simultaneously), then P(A∪B) = P(A) + P(B).

In cryptography, probability theory is often used to describe the security of cryptographic systems, especially in ana![](<.gitbook/assets/image (33).png>)lyzing the strength and resistance to attacks of cryptographic algorithms. Through probability theory, cryptographers can assess the likelihood of various attacks on cryptographic systems, thereby designing more secure cryptographic algorithms and protocols.

To gain a more intuitive understanding through the following diagram:

The core idea of Bayes' theorem is that by considering prior information and new observational data, we can update our beliefs about an event.

9. Random Variables and Probability Distribution: Random variables provide numerical descriptions of the outcomes of random experiments. Probability distribution describes the values a random variable can take and their corresponding probabilities.
10. Expectation and Variance:

* Expectation (Mean): The expectation of a random variable is the weighted average of all its possible values, denoted by E(X).

<figure><img src=".gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

* Variance: It measures the extent to which a random variable deviates from its expectation and is denoted by Var(X).

<figure><img src=".gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

These are fundamental concepts in probability theory, providing essential tools for understanding and applying probability theory.

Now, let's take a look at how to calculate joint probability and conditional probability using Python.

```
from collections import Counter

def calculate_joint_probability(data, event1, event2):
    # Calculate joint probability P(event1 and event2)
    total_occurrences = len(data)
    joint_occurrences = sum(1 for entry in data if entry == (event1, event2))
    joint_probability = joint_occurrences / total_occurrences
    return joint_probability

def calculate_conditional_probability(data, given_event, target_event):
    # Calculate conditional probability P(target_event | given_event)
    given_occurrences = sum(1 for entry in data if entry[0] == given_event)
    target_given_occurrences = sum(1 for entry in data if entry == (given_event, target_event))
    conditional_probability = target_given_occurrences / given_occurrences
    return conditional_probability

if __name__ == "__main__":
    # Example data
    data = [
        ("Male", "Smoker"),
        ("Female", "Non-Smoker"),
        ("Male", "Non-Smoker"),
        ("Female", "Smoker"),
        ("Male", "Smoker"),
        ("Male", "Non-Smoker"),
        ("Female", "Non-Smoker"),
        ("Male", "Smoker"),
        ("Female", "Smoker"),
        ("Male", "Non-Smoker"),
    ]

    # Calculate joint probability P(Male and Smoker)
    joint_prob_male_and_smoker = calculate_joint_probability(data, "Male", "Smoker")
    print(f"Joint Probability of Male and Smoker: {joint_prob_male_and_smoker:.4f}")

    # Calculate conditional probability P(Smoker | Male)
    cond_prob_smoker_given_male = calculate_conditional_probability(data, "Male", "Smoker")
    print(f"Conditional Probability of Smoker given Male: {cond_prob_smoker_given_male:.4f}")

```

This code segment primarily involves probability calculations, including the computation of joint probability and conditional probability.

1. `calculate_joint_probability` function:
   * Parameters: `data` is the dataset containing pairs of events, `event1` and `event2` are two events.
   * Approach: First, calculate the total number of occurrences of events in the dataset (`total_occurrences`). Then, use a Counter object to count the occurrences of the event `(event1, event2)`. Finally, calculate the joint probability by division.
   * Return value: Returns the joint probability of events `event1` and `event2`.
2. `calculate_conditional_probability` function:
   * Parameters: `data` is the dataset containing pairs of events, `given_event` is the given event, and `target_event` is the target event.
   * Approach: Initially, use a Counter to count the occurrences of the given event (`given_occurrences`). Then, count the occurrences of the target event (`target_given_occurrences`) when the given event occurs. Finally, calculate the conditional probability by division.
   * Return value: Returns the conditional probability of the event `target_event` given the occurrence of the event `given_event`.
3. Sample data:
   * `data` contains pairs of observed events related to gender and smoking habits.
4. Sample output:
   * Calculates the joint probability of events "Male" and "Smoker" and the conditional probability of event "Smoker" given the occurrence of "Male".

Overall, this code demonstrates how to use fundamental concepts of probability to calculate joint and conditional probabilities. In practical applications, these concepts find widespread use in statistics, probability theory, and various fields such as data analysis, machine learning, and cryptography.

## Bayesian theorem

The following is the code related to the computation of Bayesian theorem:

```
def bayes_theorem(prior, likelihood, evidence):
    # Implementation of Bayes' theorem
    posterior = (likelihood * prior) / evidence
    return posterior

if __name__ == "__main__":
    # Example data
    # Prior probability P(A)
    prior_probability = 0.01
    
    # Likelihood P(B|A)
    likelihood = 0.9
    
    # Evidence P(B)
    evidence = (likelihood * prior_probability) + ((1 - prior_probability) * (1 - likelihood))
    
    # Calculate posterior probability P(A|B)
    posterior_probability = bayes_theorem(prior_probability, likelihood, evidence)
    
    print(f"Prior Probability P(A): {prior_probability}")
    print(f"Likelihood P(B|A): {likelihood}")
    print(f"Evidence P(B): {evidence}")
    print(f"Posterior Probability P(A|B): {posterior_probability}")

```



The following is code related to the calculation of Bayes' theorem. Below is a detailed explanation of the code:

1. **bayes\_theorem Function:**
   * **Parameters:** `prior` is the prior probability (prior belief), `likelihood` is the likelihood (contribution of new evidence to known information), and `evidence` is the marginal probability (total probability of new evidence).
   * **Approach:** According to Bayes' theorem, calculate the posterior probability. The formula is `posterior = (likelihood * prior) / evidence`.
   * **Return Value:** Returns the calculated posterior probability.
2. **Example Data:**
   * `prior_probability` is the prior probability, representing belief in the absence of new evidence.
   * `likelihood` is the likelihood, indicating the contribution of new evidence to known information.
   * `evidence` is the marginal probability, representing the total probability of new evidence.
3. **Calculation Process:**
   * The calculation of `evidence` uses the total probability formula, considering two possible cases: the occurrence and non-occurrence of new evidence.
   * The calculation of `posterior_probability` uses Bayes' theorem.
4. **Example Output:**
   * Outputs the prior probability, likelihood, marginal probability, and the calculated posterior probability.

In summary, this code demonstrates how to use Bayes' theorem to update prior probabilities and calculate posterior probabilities in conjunction with new evidence.

In information theory, a cryptographic system is considered to have perfect secrecy if, for any ciphertext and any possible plaintext, no additional information about the plaintext can be obtained based on the ciphertext. This means that in a system with perfect secrecy, the ciphertext does not provide any statistical information about the plaintext, i.e., no algorithm can gain more information about the plaintext.

In this case, it can be said that the posterior probability of the plaintext given any ciphertext is equal to the prior probability. This is because, under perfect secrecy, providing the ciphertext does not yield any new information about the plaintext. Therefore, in the context of Bayesian inference, the posterior probability remains unchanged.

In summary, perfect secrecy is an ideal state in cryptography, and not all cryptographic systems in practice achieve perfect secrecy. In practical applications, we often consider more realistic security concepts, such as the security of Shannon's information theory in information security and computational security in computer science.

## Information entropy

Now let's learn about information entropy.

<figure><img src=".gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Information entropy is an important concept in information theory, proposed by Claude Shannon in the 1940s. It is used to measure the uncertainty or information content of a random variable. The higher the information entropy, the more uncertain the random variable, indicating a greater amount of information.

Basic Concepts:

* Random Variable: In information theory, we focus on the uncertainty of an event or information. This uncertainty can be represented by a random variable, which can take different values, each with a different probability.
* Information: Represents the amount of information obtained when an event occurs. Events with lower probabilities have higher information content.
* Measurement of Information: Information quantity is measured using logarithms, typically using logarithms to the base 2, known as binary logarithms, and the unit is bits.

Definition of Information Entropy:

* For a random variable X, its information entropy H(X) is defined as the expected (average) information content over all possible values of x. The formula is:

\[ H(X) = - \sum\_{i=1}^{n} P(x\_i) \cdot \log\_2 P(x\_i) ]

Where:

* ( P(x\_i) ) is the probability of the ith value of the random variable.
* The sum is taken over all possible values of the random variable.

In summary, information entropy quantifies the amount of uncertainty or randomness associated with a random variable, providing a measure of the average information content.

* Minimum Information Entropy: When the random variable X is deterministic (one probability in the probability distribution is 1, and others are 0), the information entropy is 0. This is because there is no uncertainty about the occurrence of this event.
* Maximum Information Entropy: When the random variable follows a uniform distribution, meaning each event has an equal probability of occurrence, the information entropy is maximum. This is because we have the same level of uncertainty for each event.
* Higher Information Entropy Indicates Greater Uncertainty: An increase in information entropy signifies an increase in uncertainty about the random variable, requiring more information for description.

Applications:

* Communication: Information entropy is used to describe the average length of messages, optimizing the design of communication systems.
* Cryptography: Information entropy is directly related to the security of cryptographic systems. Concepts in cryptography, such as "perfect secrecy" and the "difficulty of breaking a cryptographic system," can be understood and measured using information entropy.
* Data Compression: Information entropy provides a theoretical foundation for designing efficient data compression algorithms.

Information entropy is a powerful and widely applied concept in information theory. Its applications in various fields contribute to understanding and optimizing information processing systems.

The following is code for calculating information entropy.

```
import math
from collections import Counter

def calculate_entropy(data):
    # Calculate information entropy
    n = len(data)
    counter = Counter(data)
    entropy = -sum((count / n) * math.log2(count / n) for count in counter.values())
    return entropy

if __name__ == "__main__":
    # Example data
    data = [1, 1, 2, 3, 3, 3, 4, 4, 4, 4]

    # Calculate information entropy
    entropy = calculate_entropy(data)
    print(f"Entropy: {entropy:.4f}")

```

This code implements the calculation of information entropy for a given dataset. Information entropy is a measure of the uncertainty or disorder in the information within the dataset.

```python
def calculate_entropy(data):
    # Calculate information entropy
    n = len(data)  # Size of the dataset
    counter = Counter(data)  # Use Counter to count the occurrences of each element
    entropy = -sum((count / n) * math.log2(count / n) for count in counter.values())
    return entropy
```

* `data`: Input dataset.
* `n`: Size of the dataset.
* `counter`: Counter object used to count the occurrences of each element in the dataset.
* `entropy`: The formula for calculating information entropy is

<figure><img src=".gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

Where k is the number of distinct elements, ( P(x\_i) ) is the probability of the element ( x\_i ).

Example data:

```python
data = [1, 1, 2, 3, 3, 3, 4, 4, 4, 4]
```

The example data contains elements 1, 2, 3, and 4, with respective occurrence counts of 2, 1, 3, and 4.

Calculation process:

1. Calculate the probability of each element:

<figure><img src=".gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

2. Calculate using the information entropy formula:

<figure><img src=".gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

3. Obtain the information entropy. This code is used to calculate the information entropy of a given dataset. The higher the information entropy, the greater the uncertainty in the dataset, while lower entropy indicates a more structured dataset.

## Reference

1\. [https://blog.4d.com/cryptokey-encrypt-decrypt-sign-and-verify/](https://blog.4d.com/cryptokey-encrypt-decrypt-sign-and-verify/)

2\. [https://www.geeksforgeeks.org/cryptanalysis-and-types-of-attacks/](https://www.geeksforgeeks.org/cryptanalysis-and-types-of-attacks/)

3\. [https://www.ques10.com/p/28098/list-and-explain-various-types-of-attacks-on-enc-1/](https://www.ques10.com/p/28098/list-and-explain-various-types-of-attacks-on-enc-1/)

4\. [https://www.geeksforgeeks.org/substitution-cipher/](https://www.geeksforgeeks.org/substitution-cipher/)

5\. [https://www.geeksforgeeks.org/columnar-transposition-cipher/](https://www.geeksforgeeks.org/columnar-transposition-cipher/)

6\. [https://gkaccess.com/support/information-technology-wiki/caesar-cipher/](https://gkaccess.com/support/information-technology-wiki/caesar-cipher/)

7\. [https://learn.parallax.com/tutorials/robot/cyberbot/cybersecurity-brute-force-attacks-defenses/crack-cipher-brute-force](https://learn.parallax.com/tutorials/robot/cyberbot/cybersecurity-brute-force-attacks-defenses/crack-cipher-brute-force)
