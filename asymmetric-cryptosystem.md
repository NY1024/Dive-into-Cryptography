# Asymmetric Cryptosystem Ⅰ

## Prime number

Prime numbers are positive integers that can only be divided evenly by 1 and themselves, and cannot be divided by any other positive integer. Prime numbers are a fundamental and crucial concept in mathematics, playing a key role in number theory and many other branches of mathematics.

Every integer greater than 1 can be uniquely represented as the product of a set of prime numbers. This is known as the famous Unique Factorization Theorem or Prime Factorization.

Prime numbers are infinite, a fact proven by the ancient Greek mathematician Euclid around 300 BCE.

Some of the first prime numbers include 2, 3, 5, 7, 11, 13, 17, 19, etc. Among them, 2 is the only even prime number, while the others are odd.

In general, prime numbers have wide applications in mathematics, including cryptography, computer science, number theory, and more. Their unique properties and distribution patterns have always been important topics of mathematical research.

Now, let's discuss how to determine whether a number is a prime number:

1. **Trial Division Method:** This is the simplest and most intuitive method. For a given positive integer n, start checking from 2 to the square root of n to see if any number within this range can divide n evenly. If a divisor is found, then n is not a prime number; otherwise, n is a prime number. This is because if n is not a prime, it can be factored into two numbers, a and b, and at least one of them must be less than or equal to the square root of n.
2. **Sieve of Eratosthenes:** This method is used to generate prime numbers rather than determining if a given number is prime. It involves marking multiples of each prime, starting from 2, as composite numbers. This process is repeated until no more unmarked numbers are left.
3. **Fermat's Little Theorem:** This is a probabilistic primality testing method. If a random integer a (1 < a < n) is chosen, and (a^{(n-1)} \mod n) is not equal to 1, then n is definitely composite. If the result is 1, then n may be prime, but it's not certain. This method is commonly used for probabilistic primality testing for large numbers.
4. **Miller-Rabin Primality Test:** Similar to Fermat's Little Theorem, this is another probabilistic primality testing method. It involves multiple iterations to improve the accuracy of the test.
5. **AKS Primality Test:** This is a deterministic primality testing algorithm proposed by Indian mathematicians Agrawal, Kayal, and Saxena in 2002. While it ensures accuracy, its time complexity is relatively high compared to probabilistic methods, making it less efficient for large numbers.

In practical applications, probabilistic primality testing methods are often preferred, especially when dealing with large numbers. For general purposes like key generation, known and validated algorithms for prime number generation are commonly used instead of primality testing.

```
# Function to check if a number is prime
def is_prime(num):
    if num < 2:
        return False  # Numbers less than 2 are not prime
    for i in range(2, int(num**0.5) + 1):
        if num % i == 0:
            return False  # If the number is divisible by any i, it's not prime
    return True  # If no divisors found, the number is prime

# Example usage
number = 17
if is_prime(number):
    print(f"{number} is a prime number.")
else:
    print(f"{number} is not a prime number.")

```

1. **Function `is_prime`**:
   * The function takes an integer `num` as an argument.
   * It first checks if `num` is less than 2. If so, it returns `False` because numbers less than 2 are not prime.
   * The function then iterates through the range from 2 to the square root of `num` (inclusive).
   * Inside the loop, it checks if `num` is divisible by the current value of `i`. If so, it returns `False` since `num` is not a prime number.
   * If no divisors are found in the loop, the function returns `True`, indicating that the number is prime.
2. **Example Usage**:
   * The variable `number` is assigned the value 17.
   * The `is_prime` function is called with `number` as an argument.
   * Depending on the result, it prints whether the number is prime or not.
3. **Explanation**:
   * In the example, since 17 is a prime number, the function returns `True`, and the message "17 is a prime number." is printed.
   * The function utilizes the fact that if a number has any divisors, they will be found within the range of 2 to the square root of the number. This optimization improves the efficiency of the prime-checking process.

Overall, the code provides a simple and efficient way to determine whether a given integer is a prime number.

## Fermat's Little Theorem

Now, we are learning about Fermat's Theorem and Euler's Theorem, both of which hold significant positions in public-key cryptography.

**Fermat's Little Theorem** was proposed by the 17th-century French mathematician Pierre de Fermat. The original statement of the theorem was in a letter written to the mathematician Marin Mersenne. The specific form is as follows:

If ( p ) is a prime number, ( a ) is any integer, and ( a ) is not a multiple of ( p ) (meaning ( a ) is coprime with ( p )), then

\[ a^{p-1} \equiv 1 \pmod{p} ]

This means that raising ( a ) to the power of ( p-1 ) and taking the remainder when divided by ( p ) results in 1. The theorem is crucial in number theory and is widely used in various applications, including cryptography.

To understand Fermat's Little Theorem, it can be detailed through the following points:

1. **Prime Number Condition:**
   * The premise of the theorem is that ( p ) must be a prime number. If ( p ) is not a prime number, the theorem may not hold.
2. **Coprime Condition:**
   * ( a ) must be any integer that is not a multiple of ( p ), i.e., ( a ) is coprime with ( p ). If ( a ) and ( p ) are not coprime, meaning they share a common factor other than 1, then ( a ) might be a multiple of ( p ), and in such cases, Fermat's Little Theorem may not necessarily hold.
3. **Congruence Condition:**
   * The conclusion of the theorem is expressed as a modular congruence relationship, namely,

\[ a^{p-1} \equiv 1 \pmod{p} ]

Fermat's Little Theorem finds widespread applications in cryptography, random number generation, coding theory, and various other fields. In the RSA public-key cryptosystem, for instance, prime number testing and generation are based on Fermat's Little Theorem. An extended form of Fermat's Little Theorem is also employed in probabilistic prime testing algorithms like the Miller-Rabin primality test.

While Fermat's Little Theorem is a powerful tool, it's essential to note that not all integers satisfy the theorem for a given prime ( p ). Fermat's Little Theorem provides a useful conclusion only under specific conditions.

We can use Fermat's Little Theorem to check for primality.

```
def is_prime(n):
    """Check if a number is prime using Fermat's Little Theorem."""
    if n <= 1:
        return False  # Numbers less than or equal to 1 are not prime

    # Choose a random number a (1 < a < n)
    a = 2  # You can choose a different value if needed

    # Check if a^(n-1) is congruent to 1 modulo n
    if pow(a, n - 1, n) != 1:
        return False  # If not congruent, the number is not prime

    return True

# Example usage
number = 17
if is_prime(number):
    print(f"{number} is a prime number.")
else:
    print(f"{number} is not a prime number.")

```



1. **Function `is_prime`**:
   * The function takes an integer `n` as an argument.
   * It returns `False` for numbers less than or equal to 1, as they are not prime.
   * The function then chooses a random number ( a ) (in this case, ( a = 2 )). You can modify this value as needed.
   * It checks if ( a^{(n-1)} ) is congruent to 1 modulo ( n ) using the `pow` function. If the congruence is not satisfied, the function returns `False`, indicating that the number is not prime.
   * If the congruence holds, the function returns `True`, indicating that the number is prime.
2. **Example Usage**:
   * The variable `number` is assigned the value 17.
   * The `is_prime` function is called with `number` as an argument.
   * Depending on the result, it prints whether the number is prime or not.
3. **Explanation**:
   * The code implements a primality test based on Fermat's Little Theorem. It checks whether a randomly chosen number ( a ) raised to the power of ( n-1 ) is congruent to 1 modulo ( n ). If this condition is not met, the number is not prime; otherwise, it is considered prime.
   * While Fermat's Little Theorem is a powerful tool, this implementation is a probabilistic primality test. It may produce false positives (indicating a composite number as prime), although the chances are low for randomly chosen ( a ). For cryptographic applications, deterministic tests like the AKS algorithm are preferred.

Overall, the code provides a simple implementation of a primality test using Fermat's Little Theorem.

## Euler's totient function

Euler's totient function, commonly denoted by ( \phi(n) ), is a significant number theory function associated with a positive integer ( n ). The Euler's totient function is defined as the count of positive integers less than or equal to ( n ) that are coprime with ( n ), i.e.,

\[ \phi(n) = |{k \in \mathbb{Z}^+ \mid 1 \leq k \leq n \text{ and } \text{gcd}(n, k) = 1}| ]

where ( \text{gcd}(n, k) ) represents the greatest common divisor of ( n ) and ( k ). Euler's totient function possesses several important properties and applications:

1. **Coprime Primes Case:**
   * If ( p ) is a prime number, then

\[ \phi(p) = p - 1 ]

This is because for any ( 1 \leq k \leq p ), ( k ) is coprime to ( p ).

2. **Composite Case:**
   * If ( n ) can be factored into the product of primes, i.e.,

\[ n = p\_1^{a\_1} \cdot p\_2^{a\_2} \cdot \ldots \cdot p\_k^{a\_k} ]

then the formula for Euler's totient function is:

\[ \phi(n) = n \left(1 - \frac{1}{p\_1}\right) \left(1 - \frac{1}{p\_2}\right) \ldots \left(1 - \frac{1}{p\_k}\right) ]

where ( \phi(p\_i) ) is calculated for each prime factor ( p\_i ).

Euler's totient function plays a crucial role in the RSA public-key cryptosystem. In this system, ( n ) is the product of two large primes, and ( \phi(n) ) is used in the selection of public and private keys.

The Euler's totient function plays a crucial role in solving number theory problems related to modular arithmetic, congruences, and other related concepts.

Calculating the Euler's totient function can be approached through various methods, depending on the form and size of ( n ). In practical applications, to enhance efficiency, precomputed values of the Euler's totient function or properties of Euler's theorem are often utilized.

```
def euler_phi(n):
    result = n  # Initialize result as n

    # Check for each prime factor p of n
    p = 2
    while p * p <= n:
        if n % p == 0:
            # p is a prime factor of n
            while n % p == 0:
                n //= p
            result -= result // p
        p += 1

    # If n is a prime number greater than 1
    if n > 1:
        result -= result // n

    return result

# Example usage
number = 12
phi_value = euler_phi(number)

print(f"Euler's Totient Function ({number}): {phi_value}")

```



1. **Function `euler_phi`**:
   * The function calculates Euler's totient function for a given integer ( n ).
   * Initializes the result as ( n ).
   * Uses a loop to check for each prime factor ( p ) of ( n ).
   * If ( p ) is a prime factor, it updates the result based on Euler's totient function formula (( \phi(n) = n \prod\_{p|n} (1 - \frac{1}{p}) )).
   * The nested while loop handles repeated occurrences of the prime factor ( p ).
   * Continues the process until ( p ) exceeds the square root of ( n ).
   * If ( n ) is a prime number greater than 1, adjusts the result accordingly.
2. **Example Usage**:
   * The variable `number` is assigned the value 12.
   * The `euler_phi` function is called with `number` as an argument.
   * Prints the result of Euler's totient function for the given number.
3. **Explanation**:
   * The code efficiently calculates Euler's totient function by iterating through prime factors of ( n ) using a while loop.
   * The outer loop iterates over potential prime factors, and the inner loop handles repeated occurrences of a prime factor.
   * The formula ( \phi(n) = n \prod\_{p|n} (1 - \frac{1}{p}) ) is used to update the result.
   * The final result is printed for the example usage with ( n = 12 ).

## Euler's Theorem

**Euler's Theorem** is an important theorem in number theory, and it is a corollary of Euler's totient function. The theorem describes a property of modular exponentiation, particularly useful in solving congruence equations. Euler's Theorem is stated as follows:

For any positive integer (a) coprime with a positive integer (n), Euler's Theorem gives the following congruence relation:

\[ a^{\phi(n)} \equiv 1 \pmod{n} ]

where (\phi(n)) is the Euler's totient function, representing the count of positive integers less than or equal to (n) that are coprime with (n).

The proof of Euler's Theorem involves advanced mathematical concepts, including group theory and properties of modular arithmetic.

The prerequisite for Euler's Theorem is that (a) and (n) must be coprime, meaning their greatest common divisor (\text{gcd}(a, n) = 1). This is crucial because if (a) and (n) are not coprime, their common divisors would affect the validity of the congruence relation.

The theorem states that (a^{\phi(n)}) is congruent to 1 modulo (n). This implies that the remainder when (a^{\phi(n)}) is divided by (n) equals 1.

Euler's Theorem finds widespread applications in cryptography and number theory. In the RSA public-key cryptosystem, Euler's Theorem is used to derive (d) for a given choice of public exponent (e) and modulus (n). Additionally, Euler's Theorem is highly useful in solving congruence equations and modular exponentiation problems.

Overall, Euler's Theorem is a fundamental and powerful theorem in number theory, providing an essential tool for understanding and addressing problems related to modular arithmetic.

```
def gcd(a, b):
    # Calculate the greatest common divisor using Euclidean algorithm
    while b:
        a, b = b, a % b
    return a

def euler_phi(n):
    result = n  # Initialize result as n

    # Check for each prime factor p of n
    p = 2
    while p * p <= n:
        if n % p == 0:
            # p is a prime factor of n
            while n % p == 0:
                n //= p
            result -= result // p
        p += 1

    # If n is a prime number greater than 1
    if n > 1:
        result -= result // n

    return result

def verify_euler_theorem(a, n):
    # Check if a and n are coprime
    if gcd(a, n) != 1:
        return False

    # Calculate a^phi(n) % n using Euler's theorem
    phi_n = euler_phi(n)
    result = pow(a, phi_n, n)

    # Verify if a^phi(n) is congruent to 1 modulo n
    return result == 1

# Example usage
a = 3
n = 7

if verify_euler_theorem(a, n):
    print(f"Euler's Theorem holds for {a} and {n}")
else:
    print(f"Euler's Theorem does not hold for {a} and {n}")

```



1. **Function `gcd`**:
   * Implements the Euclidean algorithm to calculate the greatest common divisor ((\text{gcd}(a, b))).
   * The while loop continues until (b) becomes 0.
2. **Function `euler_phi`**:
   * Calculates Euler's totient function ((\phi(n))) for a given integer (n).
   * Iterates through prime factors to update the result based on Euler's totient function formula.
3. **Function `verify_euler_theorem`**:
   * Checks if (a) and (n) are coprime using the gcd function.
   * Calculates (a^{\phi(n)} \mod n) using Euler's theorem.
   * Verifies if (a^{\phi(n)}) is congruent to 1 modulo (n).
4. **Example Usage**:
   * Chooses (a = 3) and (n = 7) for demonstration.
   * Verifies whether Euler's Theorem holds for the selected values and prints the result.

**Overall Explanation:**

* The code provides functions to calculate the greatest common divisor, Euler's totient function, and to verify Euler's Theorem.
* The example usage demonstrates the application of Euler's Theorem for specific values of (a) and (n).
* This code is useful for understanding and verifying Euler's Theorem in a modular arithmetic context.

```
def euler_phi(n):
    result = n  # Initialize result as n

    # Check for each prime factor p of n
    p = 2
    while p * p <= n:
        if n % p == 0:
            # p is a prime factor of n
            while n % p == 0:
                n //= p
            result -= result // p
        p += 1

    # If n is a prime number greater than 1
    if n > 1:
        result -= result // n

    return result

def euler_theorem(a, n):
    # Calculate a^phi(n) % n using Euler's theorem
    phi_n = euler_phi(n)
    result = pow(a, phi_n, n)
    return result

# Example usage
a = 3
n = 7

result = euler_theorem(a, n)

print(f"{a}^{euler_phi(n)} mod {n} = {result}")

```



1. **Function `euler_phi`**:
   * Calculates Euler's totient function ((\phi(n))) for a given integer (n).
   * Initializes the result as (n).
   * Iterates through prime factors to update the result based on Euler's totient function formula.
   * Handles the case where (n) is a prime number greater than 1.
2. **Function `euler_theorem`**:
   * Utilizes the `euler_phi` function to calculate (\phi(n)) for Euler's theorem.
   * Applies Euler's theorem to compute (a^{\phi(n)} \mod n).
3. **Example Usage**:
   * Chooses (a = 3) and (n = 7) for demonstration.
   * Calls the `euler_theorem` function with these values.
   * Prints the result of (3^{\phi(7)} \mod 7) using Euler's theorem.

**Overall Explanation:**

* The code demonstrates the use of Euler's totient function and Euler's theorem to efficiently calculate modular exponentiation.
* It showcases a practical application of these concepts by verifying Euler's theorem for specific values of (a) and (n).
* The code provides a concise and modular implementation for handling modular exponentiation in the context of Euler's theorem.

## Miller-Rabin

Primality testing is an algorithmic or methodological approach to determine whether a number is prime. In fields such as computer science and cryptography, primality testing is a significant problem due to the widespread applications of prime numbers in these domains.

Let's start by learning about the Miller-Rabin algorithm.

The Miller-Rabin primality testing algorithm is a probabilistic method widely employed in the fields of computer science and cryptography. It draws inspiration from Fermat's Little Theorem but enhances the probability by conducting multiple tests with different random numbers.

The basic idea and steps of the Miller-Rabin algorithm are as follows:

The algorithm is an extension of Fermat's Little Theorem.

For a given odd number (n), a random integer (a) is chosen, and it is considered whether

If this condition is not satisfied, then (n) is definitely composite. If this condition is satisfied, the algorithm cannot determine whether (n) is prime, but it might be prime.

3. Multiple Tests: To enhance accuracy, the algorithm performs multiple tests with different random numbers (a). If any test indicates that (n) is composite, then (n) is deemed composite. Otherwise, (n) is considered probably prime.

Specific Steps:

1. Input: Given odd number (n) and the number of tests (k).
2. Factorize (n-1): Express (n-1) as (2^s \cdot d), where (s) is the largest non-negative integer and (d) is odd.
3. Multiple Tests:
   * For (i) from 1 to (k):
     * Randomly choose an integer (a) satisfying
     * Calculate (x = a^d \mod n).
     * If or continue to the next test.
     * For (r) from 1 to (s-1):
       * Calculate
       * If (n) is deemed composite.
       * If continue to the next test.
       * If (n) is deemed composite.
4. Output: If all tests pass, then (n) is considered probably prime.

The Miller-Rabin algorithm is a probabilistic algorithm, and by choosing an appropriate number of tests (k), the probability of error can be made extremely small.

In practical applications, the Miller-Rabin algorithm is often used for primality testing of large numbers, especially in cryptographic algorithms like RSA.

The algorithm performs well, particularly in handling large numbers.

It's important to note that while the Miller-Rabin algorithm is probabilistic, in real-world applications, by selecting a sufficient number of tests, the error probability can be made very small, meeting practical requirements.

```
import random

def miller_rabin_test(n, k=5):
    if n <= 1:
        return False

    # Write n as 2^r * d + 1
    r, d = 0, n - 1
    while d % 2 == 0:
        r += 1
        d //= 2

    # Witness loop
    for _ in range(k):
        a = random.randint(2, n - 2)
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = pow(x, 2, n)
            if x == n - 1:
                break
        else:
            return False  # n is definitely composite

    return True  # n is probably prime

# Example usage
number_to_test = 17

if miller_rabin_test(number_to_test):
    print(f"{number_to_test} is probably prime.")
else:
    print(f"{number_to_test} is composite.")

```



1. **`miller_rabin_test` Function:**
   * **Input Parameters:** `n` is the number to be tested, and `k` is the number of tests (default is 5).
   * **Base Case Check:** If `n` is less than or equal to 1, the function returns `False` indicating that numbers less than or equal to 1 are not considered prime.
   * **Factorization:** Expresses `n` as (2^r \cdot d + 1), where `r` is the highest power of 2 in the factorization of `n - 1`, and `d` is an odd number.
   * **Witness Loop:** Performs the Miller-Rabin test loop for `k` iterations.
     * Randomly selects an integer `a` in the range \[2, n - 2].
     * Computes (x = a^d \mod n).
     * Checks conditions to determine if `n` is a probable prime or composite.
     * If (x) is neither 1 nor (n - 1), performs additional modular exponentiation to check for conditions that would indicate `n` is definitely composite.
   * **Output:** Returns `True` if `n` is probably prime after passing all tests; otherwise, returns `False`.
2. **Example Usage:**
   * Tests the number 17 using the `miller_rabin_test` function.
   * Prints the result indicating whether 17 is probably prime or composite.

This code implements the Miller-Rabin algorithm, a probabilistic primality testing method, to assess the primality of a given number. The use of random witnesses and multiple tests enhances the algorithm's accuracy. The example usage demonstrates how to apply the function to determine if a specific number (17 in this case) is probably prime or composite.

## Chinese Remainder Theorem

The Chinese Remainder Theorem (CRT) is an important theorem in number theory that provides a method for combining solutions of a system of simultaneous congruences into a single solution. This theorem finds widespread applications in cryptography, coding theory, and computer science.

**Theorem Statement:** Let (m\_1, m\_2,..., m\_k) be pairwise coprime positive integers, and let (M = m\_1 \cdot m\_2 \cdot ... \cdot m\_k). For any integers (a\_1, a\_2, ..., a\_k), the system of simultaneous congruences:

\[ \begin{cases} x \equiv a\_1 \pmod{m\_1} \ x \equiv a\_2 \pmod{m\_2} \ \vdots \ x \equiv a\_k \pmod{m\_k} \end{cases} ]

has a unique solution modulo (M), and the solution is given by:

\[ x \equiv a\_1N\_1 + a\_2N\_2 + ... + a\_kN\_k \pmod{M} ]

where

\[ N\_i = \frac{M}{m\_i} \cdot (M\_i)^{-1} ]

and (N\_i) is the solution to the congruence (M\_iN\_i \equiv 1 \pmod{m\_i}).

**Detailed Steps:**

1. **Calculate (M):** Compute the modulus (M) as (m\_1 \cdot m\_2 \cdot ... \cdot m\_k).
2. **Calculate (M\_i):** For each (m\_i), compute (M\_i = \frac{M}{m\_i}).
3. **Calculate (N\_i):** For each (m\_i), calculate (N\_i) satisfying (M\_iN\_i \equiv 1 \pmod{m\_i}). This can be done using the Extended Euclidean Algorithm.
4. **Calculate the Solution (x):** The final solution is given by:

\[ x \equiv a\_1N\_1 + a\_2N\_2 + ... + a\_kN\_k \pmod{M} ]

&#x20;**Applications:**

1. **Cryptography:** In the RSA encryption algorithm, the Chinese Remainder Theorem can be used to optimize the decryption process.
2. **Coding Theory:** In erasure codes and error-correcting codes, CRT can be applied to recover lost data.
3. **Parallel Computing:** CRT can enhance computational efficiency in certain parallel computing scenarios.

Overall, the Chinese Remainder Theorem is a powerful tool that can be employed to solve a specific class of simultaneous congruence problems, particularly when dealing with large numbers.

```
def extended_gcd(a, b):
    """
    Extended Euclidean Algorithm to find g, x, y such that g = gcd(a, b) and ax + by = g.
    
    Parameters:
    - a, b: Integers
    
    Returns:
    Tuple (g, x, y)
    """
    if a == 0:
        return b, 0, 1
    else:
        g, x, y = extended_gcd(b % a, a)
        return g, y - (b // a) * x, x

def chinese_remainder_theorem(remainders, moduli):
    """
    Chinese Remainder Theorem to solve a system of simultaneous congruences.
    
    Parameters:
    - remainders: List of remainders
    - moduli: List of moduli
    
    Returns:
    Integer: Solution for x
    """
    if len(remainders) != len(moduli):
        raise ValueError("Number of remainders must be equal to the number of moduli")

    # Calculate the product of all moduli
    product = 1
    for modulus in moduli:
        product *= modulus

    result = 0
    for i in range(len(remainders)):
        ai = remainders[i]
        mi = moduli[i]
        bi = product // mi

        _, xi, _ = extended_gcd(mi, bi)
        result += ai * xi * bi

    return result % product

remainders = [2, 3, 1]
moduli = [3, 4, 5]

x = chinese_remainder_theorem(remainders, moduli)

print(f"The solution for x is {x}")

```



**Analysis:**

1. **`extended_gcd` function:**
   * Calculates the extended Euclidean algorithm to find the gcd and coefficients for a linear combination.
   * Parameters `a` and `b` are integers.
   * Returns a tuple `(g, x, y)` where `g` is the gcd, and `ax + by = g`.
2. **`chinese_remainder_theorem` function:**
   * Applies the Chinese Remainder Theorem to solve a system of simultaneous congruences.
   * Parameters are lists of remainders and moduli.
   * Returns the solution for `x` based on the Chinese Remainder Theorem.
3. **Example Usage:**
   * Sets up an example with `remainders = [2, 3, 1]` and `moduli = [3, 4, 5]`.
   * Calls `chinese_remainder_theorem` with the example values.
   * Prints the solution for `x`.

The code demonstrates the application of the Chinese Remainder Theorem to efficiently find a unique solution for a system of congruences. The extended Euclidean algorithm is used as a subroutine to calculate coefficients in the process. The example showcases solving a specific system of congruences.

## **Logarithm**

Now we are going to learn about logarithms.

In mathematics, logarithms are a mathematical operation used to represent exponentiation. Common logarithms include the base-10 logarithm (often referred to as "log") and the natural logarithm with the base e.

**Common Logarithm (Base-10 Logarithm):** The general form of a logarithm is:

\[ \log\_b(x) = y ]

where ( b ) is the base, ( x ) is the true number, and ( y ) is the exponent. In common logarithms, the base ( b ) is usually 10. Some common representations of common logarithms include:

* Standard notation: ( \log(x) ), representing the base-10 logarithm.
* Special notation for base 10: ( \log\_{10}(x) ) or ( \lg(x) ).

**Natural Logarithm (Base-e Logarithm):** The general form of a natural logarithm is:

\[ \ln(x) = y ]

where ( e ) is the base of the natural logarithm, ( x ) is the true number, and ( y ) is the exponent.

In summary, logarithms are a powerful mathematical tool that simplifies exponentiation, transforming complex multiplication and division operations into simpler addition and subtraction operations. As a result, logarithms find widespread applications in various fields.

```
import math

# Calculate the common logarithm
number = 100
log_result = math.log10(number)

print(f"The common logarithm of {number} is: {log_result}")

```

## Discrete Logarithm

Completion and translation into English:

The Discrete Logarithm is a concept in number theory that involves logarithmic operations but is performed in a discrete environment. Specifically, given a base g, a modulus p, and an integer y, the discrete logarithm problem is to find an integer x such that

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

. Here,

<figure><img src=".gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

represents the remainder obtained when g^x is divided by p. The discrete logarithm is commonly denoted by the following symbols:

<figure><img src=".gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

Here, x is the discrete logarithm, g is the base, y is the true number, and p is the modulus. This implies

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

The discrete logarithm problem plays a crucial role in cryptography. For example, in elliptic curve cryptography and Diffie-Hellman key exchange, the discrete logarithm problem is employed to ensure the security of information.



In digital signature algorithms, the discrete logarithm problem is also utilized to verify the legitimacy of signatures. The discrete logarithm problem is a challenging computational problem, especially when p is a large prime number. There are no known efficient algorithms that can solve the general discrete logarithm problem in polynomial time, making it a secure foundation in cryptography.

It is important to note that with advancements in computer science and mathematics, cryptographic systems often choose to use algorithms based on the discrete logarithm problem. This is because, at the current level of technology, solving the discrete logarithm problem is quite difficult.

```
from sympy import mod_inverse

def discrete_log(g, a, p):
    # Calculate the solution for g^x mod p = a
    x = 1
    while True:
        if pow(g, x, p) == a:
            return x
        x += 1

# Example
g = 3  # Primitive root modulo p
a = 5  # Right-hand side of the equation to solve
p = 17  # Prime number

x = discrete_log(g, a, p)
print(f"The discrete logarithm of {a} to the base {g} (mod {p}) is: {x}")

# Verification
result = pow(g, x, p)
print(f"Verification: {g}^{x} mod {p} = {result}")

```



1. The code imports the `mod_inverse` function from the `sympy` library. However, it is not used in the provided code.
2. The `discrete_log` function takes three parameters: `g` (base), `a` (right-hand side), and `p` (prime modulus). It uses a simple brute-force approach to find the solution `x` for the equation (g^x \mod p = a). It increments `x` until the correct solution is found and then returns it.
3. In the example section, specific values are assigned to `g`, `a`, and `p`. The `discrete_log` function is called with these values, and the result is stored in the variable `x`.
4. The result, i.e., the discrete logarithm, is printed to the console.
5. A verification step is included where it calculates `g^x mod p` using the obtained discrete logarithm (`x`) and prints the result. This is to confirm that the calculated discrete logarithm is indeed correct.

It's important to note that the provided `discrete_log` function uses a straightforward approach and may not be efficient for large prime numbers. More advanced algorithms, such as Pollard's rho algorithm or baby-step giant-step, are often used in practice for more efficient discrete logarithm calculations.

## Reference

1\. [https://blog.4d.com/cryptokey-encrypt-decrypt-sign-and-verify/](https://blog.4d.com/cryptokey-encrypt-decrypt-sign-and-verify/)

2\. [https://www.geeksforgeeks.org/cryptanalysis-and-types-of-attacks/](https://www.geeksforgeeks.org/cryptanalysis-and-types-of-attacks/)

3\. [https://www.ques10.com/p/28098/list-and-explain-various-types-of-attacks-on-enc-1/](https://www.ques10.com/p/28098/list-and-explain-various-types-of-attacks-on-enc-1/)

4\. [https://www.geeksforgeeks.org/substitution-cipher/](https://www.geeksforgeeks.org/substitution-cipher/)

5\. [https://www.geeksforgeeks.org/columnar-transposition-cipher/](https://www.geeksforgeeks.org/columnar-transposition-cipher/)

6\. [https://gkaccess.com/support/information-technology-wiki/caesar-cipher/](https://gkaccess.com/support/information-technology-wiki/caesar-cipher/)

7\. [https://learn.parallax.com/tutorials/robot/cyberbot/cybersecurity-brute-force-attacks-defenses/crack-cipher-brute-force](https://learn.parallax.com/tutorials/robot/cyberbot/cybersecurity-brute-force-attacks-defenses/crack-cipher-brute-force)

8\. [https://www.quora.com/Which-is-the-easiest-way-to-explain-Fermats-Little-Theorem](https://www.quora.com/Which-is-the-easiest-way-to-explain-Fermats-Little-Theorem)
