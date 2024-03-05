# Secret Sharing

## Secret Sharing

Now we are learning about secret sharing schemes.

Secret sharing is a technique in the fields of cryptography and security, used to split a secret message into multiple parts, called shares or shards, and distribute them to multiple participants. The original secret message can only be recovered when a sufficient number of shares are combined. This method helps enhance security because no single entity can independently obtain the original secret.

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

Secret sharing schemes are used to distribute a secret value among multiple participants (shareholders) by splitting the secret into fragments (shares). The splitting operation is designed in such a way that no single shareholder can obtain useful information about the original secret. The only way to retrieve the original secret is by combining the shares allocated to the participants. Thus, control over the secret is distributed. These schemes are examples of threshold cryptosystems, involving the partitioning of the secret among multiple parties, such that multiple parties (exceeding a certain threshold) must cooperate to reconstruct the secret.

Typically, a secret can be divided into n shares (held by n shareholders), where at least t (t less than n) shares are required for successful reconstruction. Such schemes are referred to as (t, n) sharing schemes. From among n participants, any subset of shareholders containing at least t shares can reconstruct the secret. Importantly, even with k (k less than t) shares, no new information about the original secret can be obtained.

Shamir's Secret Sharing (SSS) is one of the most popular secret sharing schemes, created by the renowned Israeli cryptographer Adi Shamir, who also contributed to the invention of the RSA algorithm. SSS allows for the secret to be split into any number of shares and permits the use of any threshold (as long as it is less than the total number of participants). SSS is based on the mathematical concept of polynomial interpolation, which states that a polynomial of degree t-1 can be reconstructed from knowing t or more points lying on the curve.

For example, to reconstruct a linear curve (a straight line), we need at least 2 points lying on that line. Conversely, if the number of available distinct points is less than (the degree of the curve + 1), then mathematically the curve cannot be reconstructed. It can be imagined that with only one point available in a two-dimensional space, an infinite number of possible lines could be formed.

By embedding the secret into a polynomial, the concept of polynomial interpolation can be applied to generate secret sharing schemes. A general polynomial of degree p can be represented as:

In the expression for P(x), the values a\_{1}, a\_{2}, a\_{3}, ..., a\_{n} represent the coefficients of the polynomial. Thus, constructing the polynomial requires selecting these coefficients. Note that no actual substitution of x with any values is made; each term in the polynomial acts as a "placeholder" to store coefficient values. Once the polynomial is generated, we actually only need to represent p+1 points on the curve. This is based on the principle of polynomial interpolation. For instance, if we can access at least 5 unique points lying on the curve, we can reconstruct a curve of degree 4. To achieve this, we can run Lagrange interpolation or any other similar interpolation mechanism.

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

The above figure illustrates an example of reconstructing a curve using polynomial interpolation.

Therefore, if we conceal the secret value within such a polynomial and use various points on the curve as shares, we obtain a secret sharing scheme. To be more precise, to establish a (t, n) secret sharing scheme, we can construct a polynomial of degree t-1 and select n points on the curve as shares, so that the polynomial can only be reconstructed when t or more shares are collected. The secret value (s) is hidden in the constant term of the polynomial (the coefficient of the zeroth-degree term or the y-intercept of the curve), which can only be obtained after successfully reconstructing the curve.

Shamir's secret sharing utilizes the principle of polynomial interpolation to execute the threshold sharing in the following two phases:

Phase One: Share Generation This phase involves system setup and share generation.

1. Determine the values of the number of participants (n) and the threshold (t) to protect a certain secret value (s).
2. Construct a random polynomial P(x) of degree t-1 by selecting random coefficients of the polynomial. Set the constant term of the polynomial (the coefficient of the zeroth-degree term) equal to the secret value s.
3. To generate n shares, randomly select n points that fall on the polynomial P(x).
4. Distribute the coordinates chosen in the previous step among the participants. These coordinates serve as shares in the system.

Phase Two: Secret Reconstruction To reconstruct the secret, at least t participants need to combine their shares.

1. Collect t or more shares.
2. Reconstruct the polynomial P'(x) from these shares using an interpolation algorithm. Lagrange interpolation is an example of such an algorithm.
3. Determine the value of the reconstructed polynomial for x = 0, i.e., compute P'(0). This value reveals the constant term of the polynomial, which happens to be the original secret. Thus, the secret is reconstructed.

```
import random
from math import ceil
from decimal import Decimal

FIELD_SIZE = 10**5


def reconstruct_secret(shares):
	"""
	Combines individual shares (points on graph)
	using Lagranges interpolation.

	`shares` is a list of points (x, y) belonging to a
	polynomial with a constant of our key.
	"""
	sums = 0
	prod_arr = []

	for j, share_j in enumerate(shares):
		xj, yj = share_j
		prod = Decimal(1)

		for i, share_i in enumerate(shares):
			xi, _ = share_i
			if i != j:
				prod *= Decimal(Decimal(xi)/(xi-xj))

		prod *= yj
		sums += Decimal(prod)

	return int(round(Decimal(sums), 0))


def polynom(x, coefficients):
	"""
	This generates a single point on the graph of given polynomial
	in `x`. The polynomial is given by the list of `coefficients`.
	"""
	point = 0
	# Loop through reversed list, so that indices from enumerate match the
	# actual coefficient indices
	for coefficient_index, coefficient_value in enumerate(coefficients[::-1]):
		point += x ** coefficient_index * coefficient_value
	return point


def coeff(t, secret):
	"""
	Randomly generate a list of coefficients for a polynomial with
	degree of `t` - 1, whose constant is `secret`.

	For example with a 3rd degree coefficient like this:
		3x^3 + 4x^2 + 18x + 554

		554 is the secret, and the polynomial degree + 1 is 
		how many points are needed to recover this secret. 
		(in this case it's 4 points).
	"""
	coeff = [random.randrange(0, FIELD_SIZE) for _ in range(t - 1)]
	coeff.append(secret)
	return coeff


def generate_shares(n, m, secret):
	"""
	Split given `secret` into `n` shares with minimum threshold
	of `m` shares to recover this `secret`, using SSS algorithm.
	"""
	coefficients = coeff(m, secret)
	shares = []

	for i in range(1, n+1):
		x = random.randrange(1, FIELD_SIZE)
		shares.append((x, polynom(x, coefficients)))

	return shares


# Driver code
if __name__ == '__main__':

	# (3,5) sharing scheme
	t, n = 3, 5
	secret = 1234
	print(f'Original Secret: {secret}')

	# Phase I: Generation of shares
	shares = generate_shares(n, t, secret)
	print(f'Shares: {", ".join(str(share) for share in shares)}')

	# Phase II: Secret Reconstruction
	# Picking t shares randomly for
	# reconstruction
	pool = random.sample(shares, t)
	print(f'Combining shares: {", ".join(str(share) for share in pool)}')
	print(f'Reconstructed secret: {reconstruct_secret(pool)}')

```

This Python code implements Shamir's Secret Sharing (SSS) scheme, which allows splitting a secret into multiple shares such that a minimum number of shares are required to reconstruct the original secret.

Here's a breakdown of the code:

1. **Reconstruct Secret Function (`reconstruct_secret`)**: This function takes a list of shares (points on a graph) as input and combines them using Lagrange's interpolation to reconstruct the original secret. It iterates through each share, calculates the product of the fractional terms for interpolation, and accumulates the sum to reconstruct the secret.
2. **Polynomial Function (`polynom`)**: This function generates a single point on the graph of a given polynomial for a given value of x. It iterates through the list of coefficients in reverse order (to match actual coefficient indices) and computes the value of the polynomial at the given x.
3. **Coefficient Function (`coeff`)**: This function randomly generates a list of coefficients for a polynomial with a specified degree (t - 1) such that the constant term of the polynomial is the secret. It returns the list of coefficients.
4. **Generate Shares Function (`generate_shares`)**: This function splits the given secret into n shares with a minimum threshold of m shares required to recover the secret. It generates random coefficients for a polynomial using the `coeff` function and then evaluates the polynomial at randomly chosen x-values to generate shares.
5. **Driver Code**: In the driver code section, it sets up a (3,5) sharing scheme with a secret value of 1234. It generates shares using the `generate_shares` function and then randomly selects t shares for secret reconstruction. Finally, it prints out the original secret, generated shares, selected shares for reconstruction, and the reconstructed secret.

Overall, this code demonstrates how to split a secret into shares and then reconstruct the original secret using Shamir's Secret Sharing scheme.



## Brickell secret sharing scheme

Now we are learning about the Brickell secret sharing scheme.

The Brickell secret sharing scheme is an extension of the Shamir secret sharing scheme designed to support the sharing of multi-dimensional vectors. Similar to the Shamir scheme, the Brickell scheme also utilizes the principle of polynomial interpolation, but it introduces handling for multi-dimensional data, allowing each participant to store a vector instead of just a scalar.

The basic steps of the Brickell secret sharing scheme are as follows:

1. Polynomial Generation: Choose a secret value and generate a polynomial where the constant term is the secret value, and the other terms consist of randomly selected coefficients. These coefficients form a multi-dimensional vector.
2. Share Generation: Generate a share for each participant, where the share is the value of the polynomial on randomly chosen points in the multi-dimensional vector. Each participant thus obtains a share containing multi-dimensional data.
3. Secret Reconstruction: When a sufficient number of shares are collected to reach the threshold, interpolation algorithms (such as Lagrange interpolation) can be used to reconstruct the polynomial, thereby obtaining the multi-dimensional vector of the secret. Finally, the original secret value can be extracted from this vector.

The advantage of the Brickell secret sharing scheme lies in its flexibility, suitable for scenarios requiring sharing and reconstruction of multi-dimensional data. This could be particularly useful for applications involving complex data structures or requiring higher dimensions.

The core idea of the Brickell secret sharing scheme is to use the principle of polynomial interpolation to generate shares of the polynomial, ensuring that at least t shares are available to reconstruct the secret. This scheme serves as a fundamental building block for applications in secure multiparty computation and distributed storage, among others.

````
```python
import numpy as np
import random

def brickell_secret_reconstruct(shares, t):
    # Extract x and y values from the shares
    x_values, y_values = zip(*shares)

    # Fit a polynomial to the points
    coefficients = np.polyfit(x_values, y_values, t - 1)

    # Reconstruct the secret using the constant term of the polynomial
    reconstructed_secret = int(round(coefficients[-1]))

    return reconstructed_secret

def brickell_generate_shares(secret, n, t):
    # Generate coefficients for a polynomial with degree t - 1
    coefficients = [random.randint(1, 100) for _ in range(t - 1)]
    coefficients.append(secret)

    shares = []

    for i in range(1, n + 1):
        x = random.randint(1, 100)
        # Evaluate the polynomial at x to generate shares
        y = sum(c * (x ** (t - 1 - j)) for j, c in enumerate(coefficients))
        shares.append((x, y))

    return shares

# Example Usage
if __name__ == '__main__':
    # Set parameters
    t, n = 3, 5
    secret = 1234

    print(f'Original Secret: {secret}')

    # Generate shares
    shares = brickell_generate_shares(secret, n, t)
    print(f'Shares: {", ".join(str(share) for share in shares)}')

    # Reconstruct the secret
    # Randomly select t shares for reconstruction
    selected_shares = random.sample(shares, t)
    print(f'Combining shares: {", ".join(str(share) for share in selected_shares)}')
    reconstructed_secret = brickell_secret_reconstruct(selected_shares, t)
    print(f'Reconstructed secret: {reconstructed_secret}')
```
````

This Python code implements the Brickell secret sharing scheme. Here's an analysis of the code:

1. **`brickell_secret_reconstruct` Function**: This function takes a list of shares and the threshold t as input. It extracts the x and y values from the shares, fits a polynomial of degree t - 1 to these points using NumPy's `polyfit` function, and then reconstructs the secret using the constant term of the polynomial.
2. **`brickell_generate_shares` Function**: This function generates shares for a given secret, number of participants (n), and threshold (t). It first generates coefficients for a polynomial of degree t - 1, with the constant term set to the secret. Then, it evaluates this polynomial at randomly chosen x values to generate shares.
3. **Example Usage**: In the driver code section, it sets the parameters for the sharing scheme (t and n) and the secret value. It generates shares using the `brickell_generate_shares` function and prints them. Then, it randomly selects t shares for secret reconstruction, combines them, and prints the reconstructed secret using the `brickell_secret_reconstruct` function.

Overall, this code demonstrates the generation and reconstruction of shares using the Brickell secret sharing scheme.



Then we will study the Blakley secret sharing scheme.

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Blakley's secret sharing scheme is a secret sharing scheme that utilizes geometric principles, which differs from the Shamir scheme. The basic idea of the Blakley scheme is to embed the secret value into a geometric object in multi-dimensional space, such as a hyperplane. Only when a sufficient number of points (shares) lie on this geometric object can the original secret value be reconstructed.

The main steps of the Blakley secret sharing scheme are as follows:

1. Choose a Geometric Object: Select a geometric object, such as a hyperplane. This hyperplane can be defined by choosing a vector and a point.
2. Choose the Secret Value: Select a secret value and embed it into the chosen geometric object, for example, by computing the intersection point of the vector and the hyperplane.
3. Generate Shares: Generate a share for each participant, which is a point on the geometric object that can be obtained by selecting different parameters. Thus, each participant will receive a point on the geometric object as their share.
4. Reconstruct the Secret: When a sufficient number of shares are collected to reach the threshold, geometric principles can be used to reconstruct the geometric object, thereby obtaining the original secret value.

The advantage of the Blakley secret sharing scheme lies in its reliance on geometric principles, making it easier to apply to certain geometric and spatial analysis problems. However, compared to the Shamir scheme, the Blakley scheme may have higher computational complexity. The choice of which scheme to use typically depends on the specific application scenario and requirements.

```
import numpy as np

def generate_random_vector(n):
    """
    Generate a random vector of size n with elements ranging from 0 to 255.
    """
    return np.random.randint(0, 256, n, dtype=np.uint8)

def blakley_scheme(secret, k, n):
    """
    Generate shares using Blakley's secret sharing scheme.

    Parameters:
    - secret: The secret to be shared.
    - k: The threshold number of shares required to reconstruct the secret.
    - n: The total number of participants.

    Returns:
    - shares: A list of shares generated using Blakley's scheme.
    """
    if k > n:
        raise ValueError("The number of shares (k) must be less than or equal to the total number of participants (n).")

    # Generate random vectors
    vectors = [generate_random_vector(n) for _ in range(k - 1)]

    # Calculate the last vector to satisfy the equation
    last_vector = secret - np.sum(vectors, axis=0)

    # Create shares
    shares = vectors + [last_vector]

    return shares

def reconstruct_secret(shares):
    """
    Reconstruct the secret from shares.

    Parameters:
    - shares: A list of shares.

    Returns:
    - secret: The reconstructed secret.
    """
    if len(shares) < 2:
        raise ValueError("At least 2 shares are required to reconstruct the secret.")

    # Reconstruct the secret
    secret = np.sum(shares, axis=0)

    return secret

# Set the secret and number of participants
original_secret = 42
participants = 5

# Share the secret using Blakley's Scheme
shares = blakley_scheme(original_secret, 3, participants)

# Output shares
print("Shares:")
for i, share in enumerate(shares):
    print(f"Share {i + 1}: {share}")

# Reconstruct the secret
reconstructed_secret = reconstruct_secret(shares)
print("\nReconstructed Secret:", reconstructed_secret)

```

This Python code demonstrates the implementation of Blakley's secret sharing scheme. Here's an analysis:

1. **`generate_random_vector` Function**: This function generates a random vector of a specified size with elements ranging from 0 to 255.
2. **`blakley_scheme` Function**: This function generates shares using Blakley's secret sharing scheme. It takes the secret, threshold k, and total number of participants n as inputs. It generates k - 1 random vectors and calculates the last vector to satisfy the equation. Then, it combines all vectors to create shares.
3. **`reconstruct_secret` Function**: This function reconstructs the secret from shares. It takes a list of shares as input, sums them up, and returns the reconstructed secret.
4. **Example Usage**: In the main code section, it sets the original secret and the number of participants, shares the secret using Blakley's scheme, prints out the shares, and reconstructs the secret.

Now we will learn about some key distribution schemes.

Key distribution schemes are techniques in the fields of cryptography and security used to securely distribute keys to various entities within a system. Keys play a crucial role in encryption and decryption processes, making secure key distribution essential for ensuring the confidentiality of communication and data.

The main objective of key distribution schemes is to ensure that keys are not obtained by unauthorized individuals or malicious entities during transmission. This involves designing and employing various cryptographic techniques and protocols to ensure the security of keys during transmission and storage.

The choice of key distribution scheme typically depends on the requirements of the communication environment and security needs. Different schemes adopt different technologies and protocols to ensure the confidentiality, integrity, and availability of keys.

One key distribution scheme we previously learned about is the Diffie-Hellman (DH) key exchange protocol. The Diffie-Hellman protocol is an algorithm used to securely negotiate shared keys between communication participants. It allows two remote parties to negotiate a shared secret key without directly transmitting the key.

The basic principle of the Diffie-Hellman protocol involves the discrete logarithm problem. By selecting large prime numbers and corresponding primitive roots, communication parties can generate public parameters. Each communication party has its private key and calculates a shared secret value using the public parameters. Due to the complexity of the discrete logarithm problem, unauthorized third parties have difficulty calculating this shared secret value even when they know the public parameters.

Therefore, the Diffie-Hellman protocol provides a secure way for two communication parties to jointly generate a shared key through a public channel without directly transmitting the key. This makes the Diffie-Hellman protocol a simple and effective key distribution mechanism while ensuring the security of the keys.

We will now learn about the Blom key predistribution scheme.

The Blom Key Predistribution Scheme is a key management solution designed to pre-distribute keys in a multi-user environment. Its objective is to achieve effective key management in multi-user environments to facilitate secure access to keys when needed.

Here are the main points of the Blom Key Predistribution Scheme:

1. System Initialization (Setup):

* Choose two large prime numbers, p and q, where p is a multiple of q - 1.
* Generate a generator g in the finite field F\_p.
* Select n different k-tuples (a\_1, a\_2, ..., a\_k), where each a\_i is an element in F\_p.

2. Key Extraction:

* The private key sk\_i for user i is sk\_i = (a\_{i1}, a\_{i2}, ..., a\_{ik}).
* The public key pk\_i for user i is pk\_i = g^{sk\_i}.

3. Key Synthesis:

* Users i and j can synthesize a shared key K\_{ij} for a k-tuple.
* The calculation of K\_{ij} is K\_{ij} = g^{(sk\_i \cdot sk\_j)}.

4. Key Update:

* If it's proven that some k-tuples are no longer secure, the private key for user i can be updated.

The Blom Key Predistribution Scheme allows secure communication between users without directly transmitting keys. Key synthesis between users depends on their corresponding k-tuples, providing flexibility and scalability to the scheme. Additionally, due to the use of the discrete logarithm problem, it offers a certain level of security.

```
import numpy as np

# Define a function to generate a random binary matrix
def generate_random_matrix(rows, cols):
    """
    Generates a random binary matrix of specified dimensions.

    Parameters:
        rows (int): Number of rows in the matrix.
        cols (int): Number of columns in the matrix.

    Returns:
        numpy.ndarray: Random binary matrix of shape (rows, cols).
    """
    return np.random.randint(0, 2, size=(rows, cols), dtype=np.uint8)

# Define Blom's key pre-distribution function
def blom_key_pre_distribution(nodes, keys_per_node):
    """
    Generates a key matrix for Blom's key pre-distribution scheme.

    Parameters:
        nodes (int): Number of nodes in the network.
        keys_per_node (int): Number of keys per node.

    Returns:
        numpy.ndarray: Key matrix for pre-distribution.
    """
    # Generate a random matrix for key pre-distribution
    key_matrix = generate_random_matrix(nodes, keys_per_node)
    return key_matrix

# Define a function to extract shared keys between nodes
def extract_shared_keys(key_matrix, node_indices):
    """
    Extracts shared keys between specified nodes from the key matrix.

    Parameters:
        key_matrix (numpy.ndarray): Key matrix generated for pre-distribution.
        node_indices (list): List of node indices for which shared keys are to be extracted.

    Returns:
        numpy.ndarray: Shared keys between specified nodes.
    """
    # Extract shared keys for a set of nodes
    shared_keys = key_matrix[node_indices, :].sum(axis=0) % 2
    return shared_keys

# Set the number of nodes in the network and keys per node
nodes = 10
keys_per_node = 5

# Use Blom's key pre-distribution scheme
key_matrix = blom_key_pre_distribution(nodes, keys_per_node)

# Randomly choose two nodes
node1 = np.random.randint(0, nodes)
node2 = np.random.randint(0, nodes)

# Extract shared keys between the two nodes
shared_keys = extract_shared_keys(key_matrix, [node1, node2])

# Output the results
print("Key Matrix:")
print(key_matrix)

print(f"\nNode {node1} Keys: {key_matrix[node1]}")
print(f"Node {node2} Keys: {key_matrix[node2]}")
print(f"\nShared Keys between Node {node1} and Node {node2}: {shared_keys}")

```

The provided code implements Blom's key pre-distribution scheme in Python using NumPy. Here's a breakdown of the code:

1. **Import Libraries**: The code starts by importing the NumPy library as `np`.
2. **Generate Random Matrix Function**: The `generate_random_matrix()` function is defined to create a random binary matrix of specified dimensions. It uses NumPy's `randint()` function to generate random integers (0 or 1) and returns the resulting matrix.
3. **Blom's Key Pre-distribution Function**: The `blom_key_pre_distribution()` function is defined to generate a key matrix for Blom's key pre-distribution scheme. It takes the number of nodes in the network (`nodes`) and the number of keys per node (`keys_per_node`) as input parameters. Inside the function, it calls the `generate_random_matrix()` function to create a random matrix representing the pre-distributed keys.
4. **Extract Shared Keys Function**: The `extract_shared_keys()` function extracts shared keys between specified nodes from the key matrix. It takes the key matrix (`key_matrix`) and a list of node indices (`node_indices`) as input parameters. It calculates the sum of keys for the specified nodes and returns the result as the shared keys between those nodes.
5. **Main Code**:
   * It sets the number of nodes (`nodes`) and keys per node (`keys_per_node`).
   * It calls the `blom_key_pre_distribution()` function to generate the key matrix.
   * It randomly selects two nodes (`node1` and `node2`) from the network.
   * It calls the `extract_shared_keys()` function to extract shared keys between the selected nodes.
   * Finally, it prints out the key matrix, keys for the selected nodes, and the shared keys between them.

Overall, the code provides a basic implementation of Blom's key pre-distribution scheme, demonstrating the generation of a key matrix and extraction of shared keys between nodes in a network.



## Reference

1\. [https://www.geeksforgeeks.org/implementing-shamirs-secret-sharing-scheme-in-python/](https://www.geeksforgeeks.org/implementing-shamirs-secret-sharing-scheme-in-python/)

2\. [https://www.cs.umd.edu/\~gasarch/TOPICS/secretsharing/brickIDEAL.pdf](https://www.cs.umd.edu/\~gasarch/TOPICS/secretsharing/brickIDEAL.pdf)

3\. [https://link.springer.com/article/10.1007/BF00196772](https://link.springer.com/article/10.1007/BF00196772)

4\. [https://arxiv.org/abs/1901.02802](https://arxiv.org/abs/1901.02802)

5\. [https://medium.com/asecuritysite-when-bob-met-alice/blakley-secret-sharing-2b776bf2eead](https://medium.com/asecuritysite-when-bob-met-alice/blakley-secret-sharing-2b776bf2eead)

6\. [http://course.sdu.edu.cn/Download/20150305104836003.pdf](http://course.sdu.edu.cn/Download/20150305104836003.pdf)
