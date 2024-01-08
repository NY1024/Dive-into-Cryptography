# Mathematical Fundamentals Ⅳ

## Group



Let's start by learning about groups. In modern algebra, a group is an abstract algebraic structure that consists of a set of elements and an associated binary operation. A group has the following properties:

1. Closure: For any two elements in the group, the result of the operation still belongs to the group.
2. Associativity: The operation in the group satisfies the associative law, meaning for any elements a, b, and c in the group, (a \* b) \* c = a \* (b \* c).
3. Identity Element: There exists a specific element in the group, called the identity element (usually denoted as e), such that for any element a in the group, a \* e = e \* a = a.
4. Inverse Element: For each element a in the group, there exists an inverse element b such that a \* b = b \* a = e. This implies that every element has an inverse element, and combining them results in the identity element.

If a set and its operation satisfy these four conditions, it is considered a group. Groups can be classified into finite groups and infinite groups, depending on the number of elements. In cryptography, finite groups are commonly used because dealing with finite data sets is more practical in computer science and encryption.

In cryptography, groups find applications in:

1. Group in Symmetric Cryptography: Permutation groups and substitution groups are two important concepts in symmetric cryptography. In permutation ciphers, elements are plaintext letters, and group operations involve letter permutations. In substitution ciphers, elements are bits or bytes, and group operations are substitution operations.
2. Group in Elliptic Curve Cryptography: In elliptic curve cryptography, group operations are based on the addition of points on an elliptic curve. This group structure is utilized in designing public-key cryptosystems such as Elliptic Curve Digital Signature Algorithm (ECDSA) and Elliptic Curve Diffie-Hellman (ECDH).
3. Multiplicative Group in Number Theory: In number theory, the multiplicative group consists of integers modulo a prime number, where the group operation is modular multiplication.

In summary, groups are a crucial concept in cryptography, providing powerful mathematical tools for designing and analyzing cryptographic algorithms.

```
# Closure
def is_closed(a, b):
    result = a + b
    return isinstance(result, int)

# Associativity
def is_associative(a, b, c):
    left_associative = (a + b) + c
    right_associative = a + (b + c)
    return left_associative == right_associative

# Identity Element
def identity_element(a):
    return a + 0 == 0 + a == a

# Inverse Element
def has_inverse(a):
    inverse = -a
    return a + inverse == inverse + a == 0

# Commutativity
def is_commutative(a, b):
    return a + b == b + a

# Example
a = 5
b = -3
c = 2

print("Closure:", is_closed(a, b))
print("Associativity:", is_associative(a, b, c))
print("Identity Element:", identity_element(a))
print("Inverse Element:", has_inverse(a))
print("Commutativity:", is_commutative(a, b))

```

## Ring

In modern algebra, a ring is a more general algebraic structure than a group. A ring has two binary operations, typically represented as addition and multiplication, and satisfies the following properties:

1. Addition Closure: For any two elements in the ring, the result of the addition operation still belongs to the ring.
2. Addition Associativity: For any elements a, b, and c in the ring, (a + b) + c = a + (b + c).
3. Zero Element: There exists a specific element in the ring, usually denoted as 0, such that for any element a in the ring, a + 0 = 0 + a = a.
4. Additive Inverse Element: For each element a in the ring, there exists an additive inverse element b such that a + b = b + a = 0.
5. Multiplication Closure: For any two elements in the ring, the result of the multiplication operation still belongs to the ring.
6. Multiplication Associativity: For any elements a, b, and c in the ring, (a \* b) \* c = a \* (b \* c).
7. Multiplication Distributivity: For any elements a, b, and c in the ring, a \* (b + c) = a \* b + a \* c, and (a + b) \* c = a \* c + b \* c.

A ring does not necessarily require the existence of multiplicative inverse elements. If the multiplication in a ring satisfies the existence of multiplicative inverses, meaning for every non-zero element a, there exists an element b such that a \* b = b \* a = 1, then the ring is called a field.

A classic example of a ring is the set of integers, where addition and multiplication correspond to the usual integer addition and multiplication. Another example is the set of polynomials, where addition and multiplication correspond to polynomial addition and multiplication operations.

In cryptography, the concept of rings is not as common as groups and fields, but it may still be involved in the structure of certain cryptographic algorithms and protocols.

To view related code, please provide the specific code you'd like to explore.

```
class RingElement:
    def __init__(self, value, ring):
        self.value = value
        self.ring = ring

    def __add__(self, other):
        if isinstance(other, RingElement) and self.ring == other.ring:
            return RingElement((self.value + other.value) % self.ring.modulus, self.ring)
        else:
            raise ValueError("Incompatible operands or rings")

    def __mul__(self, other):
        if isinstance(other, RingElement) and self.ring == other.ring:
            return RingElement((self.value * other.value) % self.ring.modulus, self.ring)
        else:
            raise ValueError("Incompatible operands or rings")

    def __eq__(self, other):
        return isinstance(other, RingElement) and self.value == other.value and self.ring == other.ring

class Ring:
    def __init__(self, modulus):
        self.modulus = modulus

    def zero(self):
        return RingElement(0, self)

    def one(self):
        return RingElement(1, self)


# Closure
def is_closure(ring, a, b):
    result = a * b
    return isinstance(result, RingElement) and result.ring == ring

# Associativity
def is_associative(ring, a, b, c):
    left_associative = (a * b) * c
    right_associative = a * (b * c)
    return left_associative == right_associative

# Distributivity
def is_distributive(ring, a, b, c):
    left_distributive = a * (b + c)
    right_distributive = (a * b) + (a * c)
    return left_distributive == right_distributive

# Commutativity
def is_commutative(ring, a, b):
    return a * b == b * a

# Identity Element
def has_identity_element(ring, a):
    identity = ring.one()
    return a * identity == identity * a == a


# Example
modulus = 7
ring = Ring(modulus)

a = RingElement(3, ring)
b = RingElement(5, ring)
c = RingElement(2, ring)

print("Closure:", is_closure(ring, a, b))
print("Associativity:", is_associative(ring, a, b, c))
print("Distributivity:", is_distributive(ring, a, b, c))
print("Commutativity:", is_commutative(ring, a, b))
print("Identity Element:", has_identity_element(ring, a))

```

## Field

In modern algebra, a field is a more generalized algebraic structure than a ring. A field includes two binary operations, typically represented as addition and multiplication, satisfying a set of properties. Unlike a ring, a field requires that the multiplication operation has inverse elements, meaning that every non-zero element has a multiplicative inverse.

A field must satisfy the following properties:

1. Addition Closure: For any two elements in the field, the result of the addition operation still belongs to the field.
2. Addition Associativity: For any elements a, b, and c in the field, (a + b) + c = a + (b + c).
3. Zero Element: There exists a specific element in the field, usually denoted as 0, such that for any element a in the field, a + 0 = 0 + a = a.
4. Additive Inverse Element: For each element a in the field, there exists an additive inverse element b such that a + b = b + a = 0.
5. Multiplication Closure: For any two non-zero elements in the field, the result of the multiplication operation still belongs to the field.
6. Multiplication Associativity: For any elements a, b, and c in the field, (a \* b) \* c = a \* (b \* c).
7. Multiplication Distributivity: For any elements a, b, and c in the field, a \* (b + c) = a \* b + a \* c, and (a + b) \* c = a \* c + b \* c.
8. Multiplicative Inverse Element: For each non-zero element a in the field, there exists a multiplicative inverse element b such that a \* b = b \* a = 1.

The set of integers is not a field because the multiplication of integers does not satisfy the existence of multiplicative inverses. For example, the integer 2 does not have a multiplicative inverse because there is no integer that can be multiplied by 2 to yield 1. However, the sets of rational numbers, real numbers, and complex numbers are all fields.

In cryptography, the structure of fields often involves finite fields, especially in elliptic curve cryptography. Finite fields (also known as Galois fields) are fields with a finite number of elements, typically represented as q, where q is a power of a prime. The characteristics of finite fields make them crucial in cryptography, especially in the implementation of secure elliptic curve encryption algorithms.

```
class FieldElement:
    def __init__(self, value, field):
        self.value = value
        self.field = field

    def __add__(self, other):
        if isinstance(other, FieldElement) and self.field == other.field:
            return FieldElement((self.value + other.value) % self.field.modulus, self.field)
        else:
            raise ValueError("Incompatible operands or fields")

    def __mul__(self, other):
        if isinstance(other, FieldElement) and self.field == other.field:
            return FieldElement((self.value * other.value) % self.field.modulus, self.field)
        else:
            raise ValueError("Incompatible operands or fields")

    def __eq__(self, other):
        return isinstance(other, FieldElement) and self.value == other.value and self.field == other.field

    def __str__(self):
        return str(self.value)

    def inverse(self):
        if self.value == 0:
            raise ValueError("Cannot compute the inverse of 0 in a field")
        return FieldElement(pow(self.value, -1, self.field.modulus), self.field)


class Field:
    def __init__(self, modulus):
        self.modulus = modulus

    def zero(self):
        return FieldElement(0, self)

    def one(self):
        return FieldElement(1, self)


# Closure
def is_closure(field, a, b):
    result_add = a + b
    result_mul = a * b
    return isinstance(result_add, FieldElement) and isinstance(result_mul, FieldElement) \
           and result_add.field == field and result_mul.field == field

# Associativity
def is_associative(field, a, b, c):
    left_associative_add = (a + b) + c
    right_associative_add = a + (b + c)
    left_associative_mul = (a * b) * c
    right_associative_mul = a * (b * c)
    return left_associative_add == right_associative_add and left_associative_mul == right_associative_mul

# Distributivity
def is_distributive(field, a, b, c):
    left_distributive = a * (b + c)
    right_distributive = (a * b) + (a * c)
    return left_distributive == right_distributive

# Inverse Element
def has_inverse(field, a):
    try:
        inverse = a.inverse()
        return a * inverse == inverse * a == field.one()
    except ValueError:
        return False


# Example
modulus = 7
field = Field(modulus)

a = FieldElement(3, field)
b = FieldElement(5, field)
c = FieldElement(2, field)

print("Closure:", is_closure(field, a, b))
print("Associativity:", is_associative(field, a, b, c))
print("Distributivity:", is_distributive(field, a, b, c))
print("Inverse Element:", has_inverse(field, a))

```

## Galois field

Now, let's learn about finite fields.

A finite field, often denoted as GF(p), where p is a prime number, is an important structure in modern algebra. GF(p) is a finite field with p elements, established over a prime field.

The elements in GF(p) are integers from the set {0, 1, 2, ..., p-1}. Addition and multiplication are performed modulo p. Specifically, for a, b belonging to GF(p), the definitions for addition and multiplication are as follows:

1. Addition: (a + b) mod p
2. Multiplication: (a \* b) mod p

In GF(p), both addition and multiplication operations result in another element belonging to GF(p). This ensures that GF(p) is a closed algebraic structure.

GF(p) satisfies the definition of a finite field, including the following properties:

1. Addition Closure: For any two elements a and b in GF(p), a + b belongs to GF(p).
2. Addition Associativity: For any elements a, b, and c in GF(p), (a + b) + c = a + (b + c).
3. Zero Element: There exists a zero element 0 in GF(p) such that for any element a in GF(p), a + 0 = 0 + a = a.
4. Additive Inverse Element: For each element a in GF(p), there exists an additive inverse element b such that a + b = b + a = 0.
5. Multiplication Closure: For any two elements a and b in GF(p) (where both a and b are not zero), a \* b belongs to GF(p).
6. Multiplication Associativity: For any elements a, b, and c in GF(p), (a \* b) \* c = a \* (b \* c).
7. Multiplication Distributivity: For any elements a, b, and c in GF(p), a \* (b + c) = a \* b + a \* c, and (a + b) \* c = a \* c + b \* c.
8. Multiplicative Inverse Element: For each non-zero element a in GF(p), there exists a multiplicative inverse element b such that a \* b = b \* a = 1.

In cryptography, finite fields GF(p) are often used to implement certain encryption algorithms, especially in elliptic curve cryptography based on finite fields. The characteristics of finite fields make them valuable in cryptographic applications because they provide an effective mathematical structure for building secure encryption systems.

```
class FiniteFieldElement:
    def __init__(self, value, field):
        self.value = value % field.modulus
        self.field = field

    def __add__(self, other):
        if isinstance(other, FiniteFieldElement) and self.field == other.field:
            return FiniteFieldElement((self.value + other.value) % self.field.modulus, self.field)
        else:
            raise ValueError("Incompatible operands or fields")

    def __mul__(self, other):
        if isinstance(other, FiniteFieldElement) and self.field == other.field:
            return FiniteFieldElement((self.value * other.value) % self.field.modulus, self.field)
        else:
            raise ValueError("Incompatible operands or fields")

    def __eq__(self, other):
        return isinstance(other, FiniteFieldElement) and self.value == other.value and self.field == other.field

    def __str__(self):
        return str(self.value)

    def inverse(self):
        if self.value == 0:
            raise ValueError("Cannot compute the inverse of 0 in a finite field")
        return FiniteFieldElement(pow(self.value, -1, self.field.modulus), self.field)


class FiniteField:
    def __init__(self, modulus, degree):
        if not self.is_prime(modulus):
            raise ValueError("Modulus must be a prime number")
        self.modulus = modulus
        self.degree = degree

    @staticmethod
    def is_prime(n):
        if n <= 1:
            return False
        for i in range(2, int(n**0.5) + 1):
            if n % i == 0:
                return False
        return True

    def zero(self):
        return FiniteFieldElement(0, self)

    def one(self):
        return FiniteFieldElement(1, self)


# Example
modulus = 7  # Prime modulus for the finite field
degree = 2   # Degree of the finite field extension

finite_field = FiniteField(modulus, degree)

a = FiniteFieldElement(3, finite_field)
b = FiniteFieldElement(5, finite_field)
c = FiniteFieldElement(2, finite_field)

print("Closure:", isinstance(a + b, FiniteFieldElement) and isinstance(a * b, FiniteFieldElement))
print("Associativity:", (a + b) + c == a + (b + c) and (a * b) * c == a * (b * c))
print("Distributivity:", a * (b + c) == (a * b) + (a * c))
print("Inverse Element:", isinstance(a.inverse(), FiniteFieldElement))

```

We are now learning how to compute the multiplicative inverse in it. In the finite field GF(p), the process of finding the multiplicative inverse involves using the Extended Euclidean Algorithm. The goal of the Extended Euclidean Algorithm is to find integers x and y such that, for given integers a and p, it satisfies ax + py = gcd(a, p).

For an element a in the finite field GF(p), its multiplicative inverse b satisfies

<figure><img src=".gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>

This can be written in the form ab - 1 = kp, where k is an integer. This also implies that ab - kp = 1. This aligns with the objective of the Extended Euclidean Algorithm. Therefore, we can use the Extended Euclidean Algorithm to find x and y, and take b = x.

Now, let's look at the corresponding code.

```
def find_inverse(a, p):
    """
    Find the multiplicative inverse of element a in GF(p).
    :param a: Element for which the inverse is to be found.
    :param p: Prime modulus.
    :return: Multiplicative inverse of element a.
    """
    if a == 0:
        raise ValueError("0 has no multiplicative inverse")
    inverse = pow(a, -1, p)
    return inverse


# Example
p = 17  # Prime modulus for the finite field
elements = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]

for a in elements:
    try:
        inverse = find_inverse(a, p)
        print(f"The multiplicative inverse of element {a} in GF({p}) is {inverse}")
    except ValueError as e:
        print(e)

```

Now we are going to learn operations related to polynomials. Basic polynomial operations include addition, subtraction, multiplication, and division.

1. Polynomial Addition and Subtraction:
   * Addition: For two polynomials A(x) and B(x), their addition involves adding corresponding coefficients, i.e., ( (A + B)(x) = A(x) + B(x) ). For example, ( (3x^2 + 2x + 1) + (2x^2 - x + 5) = 5x^2 + x + 6 ).
   * Subtraction: Subtraction is similar to addition, where corresponding coefficients are subtracted. For example, ( (3x^2 + 2x + 1) - (2x^2 - x + 5) = x^2 + 3x - 4 ).
2. Polynomial Multiplication:
   * The result of multiplying polynomial A(x) by polynomial B(x) is ( C(x) = A(x) \cdot B(x) ). This involves expanding using the distributive law, adding the products of each term ( A\_i \cdot B\_j ). For example, ( (2x + 1) \cdot (3x - 4) = 6x^2 - 5x - 4 ).
3. Polynomial Division:
   * Polynomial division involves dividing one polynomial A(x) by another polynomial B(x) to get the quotient Q(x) and remainder R(x). This can be done using a method similar to long division.

It is important to pay attention to the degrees and coefficients of polynomials when performing these operations. Usually, polynomials are arranged in descending order of degree when carrying out polynomial operations. Check the related code.

```
import numpy as np

# Define coefficients for two polynomials
poly1_coefficients = [1, 2, 3]  # Represents 1 + 2x + 3x^2
poly2_coefficients = [4, 5, 6]  # Represents 4 + 5x + 6x^2

# Create polynomial objects using NumPy
poly1 = np.poly1d(poly1_coefficients)
poly2 = np.poly1d(poly2_coefficients)

# Polynomial addition
poly_sum = poly1 + poly2
print("Polynomial Addition:", poly_sum)

# Polynomial multiplication
poly_product = np.polymul(poly1, poly2)
print("Polynomial Multiplication:", poly_product)

# Polynomial division
poly_quotient, poly_remainder = np.polydiv(poly1, poly2)
print("Polynomial Division - Quotient:", poly_quotient)
print("Polynomial Division - Remainder:", poly_remainder)

```

Now, we are learning about polynomial operations with coefficients in GF(p).

In the finite field GF(p) (also known as a finite field or Galois field), operations involving polynomial coefficients must adhere to the rules of the finite field. A finite field is a field that contains a finite number of elements, where p is a prime number representing the count of elements in the field. Below, we elaborate on polynomial addition, subtraction, and multiplication in GF(p).

1. Polynomial Addition and Subtraction:

* In GF(p), each coefficient of a polynomial is chosen from the range 0 to p-1.
* Addition and subtraction operations are performed modulo p, meaning the coefficients are added or subtracted, and the result is then taken modulo p. For example, if p = 7, then (3x^2 + 2x + 1) + (2x^2 - x + 5) is computed following the rules of GF(7), resulting in 5x^2 + x + 6.

2. Polynomial Multiplication:

* Polynomial multiplication is also performed modulo p, with each coefficient in the product being calculated by multiplying the corresponding coefficients and then taking the result modulo p.
* For instance, in GF(7), the product of (2x + 1) and (3x - 4) is 6x^2 - 5x - 4, where each coefficient falls within the range of 0 to 6.

Let's examine the code for illustration.

```
def mod_p(x, p):
    """
    Take the modulo in GF(p) to ensure coefficients are in GF(p).
    """
    return x % p


def poly_add(poly1, poly2, p):
    """
    Polynomial addition.
    """
    # Ensure the lengths of both polynomials are equal
    len_diff = len(poly1) - len(poly2)
    if len_diff > 0:
        poly2 = [0] * len_diff + poly2
    elif len_diff < 0:
        poly1 = [0] * abs(len_diff) + poly1

    result = [(c1 + c2) % p for c1, c2 in zip(poly1, poly2)]
    return result


def poly_multiply(poly1, poly2, p):
    """
    Polynomial multiplication.
    """
    result = [0] * (len(poly1) + len(poly2) - 1)
    for i in range(len(poly1)):
        for j in range(len(poly2)):
            result[i + j] = mod_p(result[i + j] + poly1[i] * poly2[j], p)
    return result


# Example
p = 7  # Prime modulus for GF(p)
poly1 = [1, 2, 3]  # Represents 1 + 2x + 3x^2
poly2 = [4, 5, 6]  # Represents 4 + 5x + 6x^2

# Polynomial addition
poly_sum = poly_add(poly1, poly2, p)
print("Polynomial Addition:", poly_sum)

# Polynomial multiplication
poly_product = poly_multiply(poly1, poly2, p)
print("Polynomial Multiplication:", poly_product)

```

Can we find the greatest common divisor of two polynomials? Yes, we can modify the Euclidean Algorithm to achieve this.

The Euclidean Algorithm is employed in polynomial rings to determine the greatest common divisor of two polynomials. The greatest common divisor of polynomials is the highest-degree common factor, excluding constant factors.

Here are the specific steps of the Euclidean Algorithm in the polynomial ring F\[x]:

1. Initialization: Given two polynomials A(x) and B(x), where the degree of A(x) is greater than or equal to the degree of B(x), initialize R\_0 = A(x) and R\_1 = B(x).
2. Iterative Process: Use polynomial division to calculate Q\_i and R\_{i+1}, where Q\_i is the quotient of R\_i divided by R\_{i-1}, and R\_{i+1} is the remainder of R\_{i-1} divided by R\_i. Iterate this process until the degree of R\_{i+1} becomes less than a predetermined threshold or R\_{i+1} becomes the zero polynomial.
3. Result: The last non-zero polynomial R\_{i+1} is the greatest common divisor of A(x) and B(x).

Now, let's take a look at the corresponding code.

```
def poly_gcd(a, b):
    while b:
        a, b = b, poly_remainder(a, b)
    return a

def poly_remainder(a, b):
    while len(a) >= len(b):
        factor = [0] * (len(a) - len(b)) + b
        factor = [coeff * a[0] for coeff in factor]
        a = [a[i] - factor[i] for i in range(len(a))]
        a = trim_zeros(a)
    return a

def trim_zeros(poly):
    while poly and poly[-1] == 0:
        poly.pop()
    return poly

# Simple example
A = [2, 4, 2]  # 2x^2 + 4x + 2
B = [1, 1]     # x + 1

gcd = poly_gcd(A, B)

print("Polynomial A(x):", A)
print("Polynomial B(x):", B)
print("GCD(x):", gcd)

```

Now, we are going to learn about modular operations on polynomials.

Modular operations on polynomials involve performing modular arithmetic on the coefficients of the polynomials. In the polynomial ring F\[x], the coefficients of polynomials typically belong to a specific field, such as the real number field or a finite field. Modular operations on polynomials primarily involve two aspects: modular division and modular multiplicative inverse.

1. **Modular Division of Polynomials:** Given two polynomials A(x) and B(x), to compute the remainder when dividing A(x) by B(x), polynomial division is used. The specific steps involve gradually reducing the degree of A(x) through long division until the degree of A(x) is less than that of B(x). The resulting remainder is the modular division result of A(x) by B(x). For example, if A(x) = 2x^3 + 3x^2 - 5x + 1 and B(x) = x - 2, then A(x) mod B(x) can be obtained through long division.
2. **Modular Multiplicative Inverse of Polynomials:** If we are working in a finite field GF(p), for a given pair of polynomials A(x) and B(x), finding the multiplicative inverse B^{-1}(x) of B(x) modulo p requires using the extended Euclidean algorithm. The specific steps involve finding integer polynomials X(x) and Y(x) such that

<figure><img src=".gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

3. Here, `mod{p}` represents performing modulo p on the coefficients. For example, if B(x) = x^2 + 1 in the finite field GF(5), finding the multiplicative inverse of B(x) can be achieved using the extended Euclidean algorithm. Check the code for details.

```
import numpy as np

def poly_modulus(poly, mod_poly):
    # Perform polynomial division and return the remainder
    _, remainder = np.polydiv(poly, mod_poly)
    return remainder

# Example polynomial: 3x^3 + 2x^2 + 5x + 1
coefficients = [3, 2, 5, 1]

# Example modulus polynomial: x^2 + 1
modulus_coefficients = [1, 0, 1]

# Create polynomial objects
polynomial = np.poly1d(coefficients)
modulus_polynomial = np.poly1d(modulus_coefficients)

# Calculate polynomial modulus
result = poly_modulus(polynomial, modulus_polynomial)

# Output results
print(f"Original polynomial: {polynomial}")
print(f"Modulus polynomial: {modulus_polynomial}")
print(f"Modulus operation result: {result}")

```

Now we learn how to compute the multiplicative inverse of a polynomial using the Extended Euclidean Algorithm. The Extended Euclidean Algorithm is a method used to calculate the greatest common divisor of integers and express the Bézout coefficients representing the greatest common divisor. In the finite field GF(p), we can employ the Extended Euclidean Algorithm to find the multiplicative inverse of a polynomial. Extended Euclidean Algorithm: Given two polynomials A(x) and B(x), we aim to find polynomials X(x) and Y(x) such that A(x)X(x) + B(x)Y(x) = gcd(A(x), B(x)).

1. Base case: If B(x) is zero, then A(x) is the greatest common divisor. In this case, X(x) is set to \[1], and Y(x) is set to \[0].
2. Recursive case: Otherwise, we recursively call the Extended Euclidean Algorithm with B(x) and A(x) mod B(x) as the new inputs. We obtain X\_1(x), Y\_1(x), and gcd(A(x), B(x)).
3.  Compute the result: Then, we can calculate X(x) and Y(x) based on the recurrence relation:

    X(x) = Y\_1(x) Y(x) = X\_1(x) - (A(x) // B(x)) \* Y\_1(x)

    Here, // denotes the polynomial division operation.

By using the Extended Euclidean Algorithm, we can find the multiplicative inverse of the polynomial B(x) modulo p, ensuring that B(x) \* B^{-1}(x) ≡ 1 (mod p).

Check the related code.

```
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    else:
        d, x, y = extended_gcd(b, a % b)
        return d, y, x - (a // b) * y

def mod_inverse(a, m):
    d, x, y = extended_gcd(a, m)
    if d != 1:
        raise ValueError(f"{a} does not have a multiplicative inverse modulo {m}")
    else:
        return x % m

def poly_mod_inverse(poly, mod):
    # poly represents the coefficients of a polynomial, e.g., [1, 2, 3] represents 1 + 2x + 3x^2
    # mod is the modulus for calculating the polynomial's multiplicative inverse

    n = len(poly)
    result = [0] * n
    result[0] = mod_inverse(poly[0], mod)

    for i in range(1, n):
        # Calculate (result[0] * poly[i]) mod mod
        term = (result[0] * poly[i]) % mod

        # Calculate the multiplicative inverse of -term mod mod
        inverse_term = mod_inverse(term, mod)

        # Update result[i]
        result[i] = inverse_term

    return result

# Example
poly_coefficients = [1, 2, 3]  # 1 + 2x + 3x^2
modulus = 5

inverse_poly = poly_mod_inverse(poly_coefficients, modulus)
print("Multiplicative inverse of the polynomial:", inverse_poly)

```

## Reference

1\. [https://www.britannica.com/science/modern-algebra](https://www.britannica.com/science/modern-algebra)

2\. [https://en.wikipedia.org/wiki/Abstract\_algebra](https://en.wikipedia.org/wiki/Abstract\_algebra)

3\. [https://mathcs.clarku.edu/\~djoyce/ma225/algebra.pdf](https://mathcs.clarku.edu/\~djoyce/ma225/algebra.pdf)

4\. [https://ocw.mit.edu/courses/18-703-modern-algebra-spring-2013/](https://ocw.mit.edu/courses/18-703-modern-algebra-spring-2013/)
