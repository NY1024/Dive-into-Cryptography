# Mathematical Fundamentals Ⅲ

Classical cryptography and modern cryptography represent two stages in the history of cryptographic development, with significant differences in methods, techniques, and applications.

<figure><img src=".gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

Classical Cryptography:

Classical cryptography refers to the cryptographic phase before the advent of computers, primarily involving manual and mechanical encryption techniques. During this period, cryptography focused on ensuring the confidentiality of communications. Here are some characteristics and representative methods of classical cryptography:

1. Representative Methods: Representative methods in classical cryptography include the Caesar cipher, simple substitution ciphers (such as monoalphabetic substitution ciphers), and polyalphabetic substitution ciphers (such as the Vigenère cipher).
2. Based on Substitution and Transposition: Most classical cryptographic systems are based on substitution and transposition operations on letters. These operations are performed manually or mechanically to confuse and protect the content of communications.
3. Relatively Simple: The methods of classical cryptography are relatively simple, and their security primarily relies on the secrecy of algorithms rather than the security of keys.

Modern Cryptography:

Modern cryptography refers to the era when computers are widely used, employing more complex encryption techniques with a stronger mathematical foundation. Modern cryptography primarily focuses on ensuring the confidentiality, integrity, and authentication of information. Here are some characteristics and representative methods of modern cryptography:

1. Representative Methods: Modern cryptography includes symmetric encryption algorithms (such as AES), asymmetric encryption algorithms (such as RSA), hash functions (such as SHA-256), and digital signatures.
2. Mathematical Foundation: Modern cryptography is based on complex mathematical principles, such as modular arithmetic, discrete logarithm problems, and elliptic curve cryptography. These mathematical principles provide stronger security for cryptography.
3. Key Management: Modern cryptography emphasizes secure key management. Key generation, distribution, and storage are critical aspects of the modern cryptographic system.
4. Widely Applied: Modern cryptography is widely used in computer networks, e-commerce, mobile communications, etc., protecting the confidentiality and integrity of information while supporting functions like authentication and digital signatures.

Overall, modern cryptography has more advantages in terms of security, scalability, and practicality, making it the cornerstone of today's information security field. The upcoming series of experiments will be related to modern cryptography, so it is necessary to have a certain understanding of mathematical foundations, including number theory and finite fields.

## Division

Let's start by understanding divisibility and division.

Divisibility and division are concepts in mathematics related to the division operation.

<figure><img src=".gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

Divisibility:

Divisibility refers to the property of one integer being divisible by another integer, meaning the result of the division is an integer without any remainder. If an integer a can be divided by another integer b without leaving a remainder, then a is a multiple of b, and b is a factor of a. For example:

* 12 is divisible by 3 because 12 ÷ 3 = 4, and the result is the integer 4.
* 15 is not divisible by 7 because 15 ÷ 7 = 2 with a remainder of 1, and the result is not an integer.

In mathematical notation, if a can be divided by b, it is commonly written as a mod b = 0, where "mod" denotes the modulus operation, indicating that the remainder when a is divided by b is 0.

Division:

Division is a fundamental arithmetic operation that involves splitting a number into several equal parts or finding the multiples that can evenly divide a given number. The general form of division is a ÷ b, where a is the dividend, b is the divisor, and the result is the quotient.

For example:

* 12 ÷ 3 = 4 represents dividing 12 into three equal parts, each part being 4.
* 15 ÷ 7 = 2 represents dividing 15 by 7, with a quotient of 2 and a remainder of 1.

In integer division, the quotient can be an integer or a decimal. In the context of divisibility, if the quotient is an integer, it is referred to as a perfect division.

It's important to note that in computer science, the concepts of divisibility and division also have corresponding operators. For instance, in many programming languages, "a mod b" can be used to check if a is divisible by b, while "a ÷ b" represents floating-point division, and the result may include a decimal part.

```
dividend = 15
divisor = 4

# Use the divmod() function to obtain quotient and remainder
quotient, remainder = divmod(dividend, divisor)

# Print the results
print(f"Quotient: {quotient}")
print(f"Remainder: {remainder}")

```

## Euclid Algorithm

One very classic algorithm in number theory is the Euclidean algorithm.

<figure><img src=".gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

Translation: The Euclidean algorithm, also known as the division-based method, is a method for calculating the greatest common divisor (GCD) of two integers.

What is the greatest common divisor?

The greatest common divisor (GCD) is the largest common factor among two or more integers. A more general term is the greatest common factor, which can be used to describe the highest degree factor of multiple polynomials.

The greatest common divisor has some important properties and applications:

1. Divisibility Property: If d is the greatest common divisor of a and b, then d also divides all integer multiples of a and b.
2. Linear Combination: For any integers a, b, and their greatest common divisor d, there exist integers x and y such that ax + by = d. This is known as Bézout's identity.
3. Coprime Relationship: If the greatest common divisor of a and b is 1, i.e., GCD(a, b) = 1, then a and b are called coprime or relatively prime. Coprime numbers have no common factors other than 1, and no positive integer other than 1 can divide both of them simultaneously.

There are several methods for calculating the greatest common divisor, including:

* Euclidean Algorithm: Applicable for computing the greatest common divisor of two integers, based on the principle of successive division.
* Prime Factorization: Factorizing the two numbers into prime factors and then taking the product of their common prime factors.
* Subtraction Method: Repeatedly subtracting the smaller number from the larger one until they become equal, and the common value is their greatest common divisor.

The greatest common divisor has wide applications in mathematics, such as simplifying fractions, solving equations, modular arithmetic in cryptography, etc. In cryptography, the properties of the greatest common divisor are often used in designing and analyzing encryption algorithms.

The basic idea of the Euclidean algorithm is to calculate the greatest common divisor by iteratively taking the remainder when dividing two numbers until the remainder becomes zero.

Here are the detailed steps of the Euclidean algorithm:

1. Initialization: Given two integers a and b, where a >= b.
2. Calculate Remainder: Compute the remainder r when a is divided by b (r = a mod b).
3. Check Remainder: If r is zero, then b is the greatest common divisor, and the algorithm terminates.
4. Update Variables: Set a = b, b = r, and return to step 2.
5. Repeat: Repeat steps 2 to 4 until the remainder is zero.

Ultimately, when the remainder becomes zero, the last value of b is the greatest common divisor of a and b.

Now, let's take a look at the corresponding code.

```
def euclidean_algorithm(a, b):
    # Iterate while b is not zero
    while b:
        # Calculate quotient
        q = a // b
        # Update variables using the remainder
        a, b = b, a % b
    # Return the greatest common divisor (GCD)
    return a

# Example usage
num1 = 48
num2 = 18

# Calculate the GCD using the Euclidean algorithm
gcd = euclidean_algorithm(num1, num2)

# Print the greatest common divisor (GCD)
print(f"Greatest Common Divisor (GCD): {gcd}")

```

## Mod

## Continuing with our study of concepts related to modular arithmetic:

Modular arithmetic is a mathematical operation usually denoted by the symbol "%". In modular arithmetic, we calculate the remainder when one number is divided by another. Formally, for integers a and b (where b is not equal to zero), the result of a modulo b is the remainder, commonly expressed as a mod b or a % b.

Specifically, for integers a and b, the calculation of a modulo b is as follows: \[a \mod b = r] where r is the remainder when a is divided by b. For example, (17 \mod 5 = 2), because the remainder when 17 is divided by 5 is 2.

Modular arithmetic has some important properties:

1. Congruence Relation: If two integers a and b are congruent modulo m, denoted as (a \equiv b \pmod{m}), then their remainders are the same when divided by m.
2. Modular Addition: If (a \equiv b \pmod{m}) and (c \equiv d \pmod{m}), then (a + c \equiv b + d \pmod{m}).
3. Modular Multiplication: If (a \equiv b \pmod{m}) and (c \equiv d \pmod{m}), then (a \cdot c \equiv b \cdot d \pmod{m}).
4. Modular Exponentiation: If (a \equiv b \pmod{m}), then (a^n \equiv b^n \pmod{m}) for any non-negative integer n.

Modular arithmetic is widely used in various fields, including cryptography, computer science, and number theory. It plays a crucial role in ensuring data integrity and security in cryptographic algorithms, where it is applied to operations on large integers within a specific modulus.

Modular arithmetic has extensive applications in cryptography, computer science, and mathematics. In cryptography, modular arithmetic is often utilized in computations within encryption algorithms, especially when operating in finite fields.

Let's take a look at the code for checking congruence:\


```
def check_congruence(a, b, m):
    # Check if the remainders of a and b are equal when divided by m
    return a % m == b % m

# Example usage
a = 17
b = 29
m = 6

# Check if a and b are congruent modulo m
congruent = check_congruence(a, b, m)

# Print whether a and b are congruent modulo m
print(f"{a} and {b} are congruent modulo {m}: {congruent}")

```

## Euclidean Theorem

The Euclidean algorithm is based on Euclid's theorem, also known as the division-based method. The theorem is stated as follows: For any two non-negative integers a and b, their greatest common divisor is equal to the greatest common divisor of b and (a \mod b). This theorem provides a recursive approach. At each step, we replace a with b and replace b with (a \mod b), until b becomes 0. At this point, a is the greatest common divisor. The Euclidean algorithm can be rewritten as a recursive version because it exhibits the property of repeated subproblems. At each step, we are dealing with the same problem (finding the greatest common divisor of two integers), but the size of the problem keeps reducing. The recursive version continuously breaks down the problem into smaller subproblems until it reaches the base case (when one of the numbers is 0). The advantage of the recursive version lies in its simplicity and readability, but it may lead to a large recursion depth in extreme cases. Performance and potential stack overflow issues should be considered. Let's take a look at the modified code for the Euclidean algorithm in its recursive version.

```
def euclidean_algorithm_recursive(a, b):
    # Base case: if b is 0, the greatest common divisor is a
    if b == 0:
        return a
    else:
        # Recursive call to the Euclidean algorithm, swapping the positions of parameters
        return euclidean_algorithm_recursive(b, a % b)

# Example usage
num1 = 48
num2 = 18

result = euclidean_algorithm_recursive(num1, num2)
print(f"Greatest Common Divisor({num1}, {num2}) = {result}")

```

This recursive version of the Euclidean algorithm employs a base case and recursive calls. When b equals 0, the function directly returns a because the greatest common divisor of any number with 0 is the number itself. Otherwise, the function makes a recursive call, swapping the positions of the parameters, and continues to search for the greatest common divisor.

Now, let's learn about the Extended Euclidean Algorithm.

The Extended Euclidean Algorithm is a method used to solve linear Diophantine equations. Typically, it is employed to calculate the greatest common divisor of two integers, a and b, while simultaneously finding a pair of integers, x and y, such that (ax + by) equals the greatest common divisor.

The Extended Euclidean Algorithm is based on the property of the following linear Diophantine equation:

For any integers a, b, and their greatest common divisor d, there exist integers x and y such that (ax + by = d).

The steps of the algorithm are as follows:

1. Recursive Base: If (b = 0), then (d = a), (x = 1), and (y = 0). At this point, (ax + by = d).
2. Recursive Step: Otherwise, recursively call the algorithm to obtain (d'), (x'), and (y'), where (d' = b), (x' = 1), and (y' = 0).
3. Update Results: Update the results using the following formulas:

\[d = d', \quad x = y', \quad y = x' - \left\lfloor \frac{a}{b} \right\rfloor \cdot y']

These steps are repeated until (b) becomes 0, and the final result is the greatest common divisor (d) along with the corresponding values of (x) and (y).

Let's take a look at the corresponding code.

```
def extended_gcd(a, b):
    # Recursive base case: if b is 0, return a, x=1, y=0
    if b == 0:
        return a, 1, 0
    else:
        # Recursive step: call the algorithm recursively and update results
        d, x, y = extended_gcd(b, a % b)
        return d, y, x - (a // b) * y

def find_inverse(a, m):
    # Find the extended GCD of a and m
    d, x, y = extended_gcd(a, m)
    # Check if the inverse exists
    if d != 1:
        raise ValueError(f"The inverse does not exist for {a} modulo {m}")
    else:
        # Return the inverse modulo m
        return x % m

# Example
a = 35
b = 15

# Calculate the extended GCD of a and b
d, x, y = extended_gcd(a, b)

# Print the GCD and the coefficients x, y
print(f"The GCD of {a} and {b} is {d}")
print(f"x = {x}, y = {y}")

# Verify ax + by = d
print(f"Verification: {a}*{x} + {b}*{y} = {a*x + b*y}")

# Find the inverse of a modulo m
m = 17
inverse_a = find_inverse(a, m)
print(f"The inverse of {a} modulo {m} is {inverse_a}")

```

This code implements the Extended Euclidean Algorithm and the functionality to find the inverse of an integer a modulo m.

1. **extended\_gcd Function:** This function calculates the greatest common divisor of a and b, returning the result of the Extended Euclidean Algorithm. If b is 0, a is the greatest common divisor, and it returns (a, 1, 0). Otherwise, it recursively calls the extended\_gcd function and calculates the values of x and y using Bézout's identity, where (ax + by = d), and d is the greatest common divisor.
2. **find\_inverse Function:** This function uses the extended\_gcd function to find the inverse of the integer a modulo m. If a and m are not coprime (i.e., the greatest common divisor is not 1), it raises an exception since the inverse does not exist in this case. Otherwise, it returns (x \mod m) as the inverse.
3. **Example Usage:** The example demonstrates how to call these functions to calculate the greatest common divisor of integers and find the inverse of an integer a modulo m. For instance, given (a = 35) and (b = 15), calling the extended\_gcd function yields their greatest common divisor along with integers x and y satisfying Bézout's identity. Then, calling the find\_inverse function finds the inverse of a modulo (m = 17).

These functionalities are particularly useful in cryptography for modular arithmetic and key generation scenarios.

## Modern Algebra

Now let's explore the background of modern algebra.

Modern algebra is a branch of algebra that primarily investigates abstract algebraic structures, particularly algebraic structures such as groups, rings, fields, and more. Research in this field often involves the abstract properties of algebraic systems without relying on specific mathematical structures, such as numbers or geometric shapes. This characteristic gives modern algebra broad applicability, extending beyond any specific mathematical domain.

<figure><img src=".gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

The main objects of study in modern algebra include:

1. **Group Theory:** Group theory is a fundamental concept in modern algebra, studying algebraic structures with specific operations. Group theory finds extensive applications in cryptography, physics, chemistry, and other fields.
2. **Ring Theory:** Rings are algebraic structures more general than groups, involving two binary operations, usually addition and multiplication. Ring theory investigates the properties and relationships of these structures.
3. **Field Theory:** Fields are algebraic structures containing both addition and multiplication, more general than rings. Field theory studies the properties and structures of fields.
4. **Linear Algebra:** Methods and theories from modern algebra are widely applied in linear algebra. Linear algebra explores structures such as vector spaces and linear mappings.

There is a close relationship between modern algebra and cryptography. Cryptography is the study of securing communication and information, and modern algebra provides many tools and concepts widely used in cryptography. Here are some relationships between modern algebra and cryptography:

1. **Applications of Group Theory in Cryptography:** Concepts from group theory find extensive applications in cryptography, especially in symmetric cryptography. Permutation groups and substitution groups in symmetric cryptographic systems are examples of group theory applications. The properties and operations of groups are crucial for designing and analyzing encryption algorithms.
2. **Finite Fields and Algorithms on Finite Fields:** The concept of finite fields is foundational in cryptography. Algorithms on finite fields, such as elliptic curve group operations in elliptic curve cryptography, are key technologies in modern cryptography.
3. **Linear Algebra and Matrix Operations:** Methods from linear algebra have broad applications in cryptography, particularly in public-key cryptography. Matrix operations and linear transformations play a crucial role in designing public-key cryptographic algorithms and solving cryptographic problems.
4. **Permutation and Substitution Ciphers:** Permutation and substitution ciphers are common types of symmetric cryptographic algorithms. These algorithms involve permutation and substitution operations based on group theory and operations in finite fields.
5. **Digital Signatures and Public-Key Cryptography:** Mathematical structures used in public-key cryptography, such as the problem of factoring large integers and the discrete logarithm problem on elliptic curves, involve complex number theory concepts from modern algebra.

In summary, modern algebra provides powerful mathematical tools and abstract frameworks for cryptography, enabling cryptographers to design more secure and efficient encryption algorithms while better understanding and analyzing various cryptographic problems. In upcoming experiments, we will delve deeper into practical applications.

## Reference

1\. [https://www.researchgate.net/figure/Overview-of-Cryptography-6\_fig1\_295256333](https://www.researchgate.net/figure/Overview-of-Cryptography-6\_fig1\_295256333)

2\. [https://www.javatpoint.com/java-modulo](https://www.javatpoint.com/java-modulo)

3\. [https://sites.math.rutgers.edu/\~greenfie/gs2004/euclid.html](https://sites.math.rutgers.edu/\~greenfie/gs2004/euclid.html)

4\. [https://www.inchcalculator.com/euclidean-algorithm-calculator/](https://www.inchcalculator.com/euclidean-algorithm-calculator/)

5\. [https://www.scaffoldedmath.com/2018/02/euclidean-algorithm.html](https://www.scaffoldedmath.com/2018/02/euclidean-algorithm.html)

6\. [https://math.stackexchange.com/questions/4345249/are-there-any-diagrams-or-tables-of-relationships-like-with-groups-to-magmas-bu](https://math.stackexchange.com/questions/4345249/are-there-any-diagrams-or-tables-of-relationships-like-with-groups-to-magmas-bu)

