# Classical Cryptography Ⅱ

## Polyalphabetic cipher

Now we are learning about the Polygraphic Substitution Cipher, which is an improved substitution cipher technique that enhances encryption security by employing multiple substitution tables. In contrast to the Single Table Substitution Cipher, the Polygraphic Substitution Cipher utilizes different substitution rules, making attacks such as frequency analysis more challenging.

Some key concepts and features of the Polygraphic Substitution Cipher are:

1. Multiple Substitution Tables: The Polygraphic Substitution Cipher uses multiple substitution tables, with each table corresponding to a letter. These tables can be arbitrary letter arrangements, forming a key table. This method disrupts simple one-to-one substitution relationships, increasing the complexity of the cipher.
2. Encryption Process: During the encryption process, for each letter in the plaintext, the corresponding substitution table from the key is used for substitution. If the key is shorter than the plaintext, it can be repeated to match the length of the plaintext. Specific operations can be implemented using algorithms such as the Vigenère Table.
3. Decryption Process: The decryption process is the reverse of encryption. For each letter in the ciphertext, the corresponding substitution table from the key is used for reverse substitution. Similarly, if the key is shorter than the ciphertext, it needs to be repeated.
4. Key Cruciality: The security of the Polygraphic Substitution Cipher depends on the choice of the key. If the key can be easily guessed or inferred, the security of the entire cipher system will be compromised.

The Polygraphic Substitution Cipher increases the complexity of substitution ciphers but is not absolutely secure. In modern cryptography, more complex encryption algorithms such as symmetric key algorithms and asymmetric key algorithms are widely used to provide higher levels of security.

## Playfair Cipher

Let's take a look at a typical representative—Playfair Cipher.

The Playfair Cipher is a type of Polygraphic Substitution Cipher that uses a 5x5 matrix (Playfair Square) to encrypt and decrypt information. This cipher technique was independently invented in the late 19th century by Charles Wheatstone and Giovan Battista Bellaso in the United Kingdom.

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>



The main features and operational steps of the Playfair Cipher are as follows:

1.  Constructing the Playfair Square: Firstly, a 5x5 square is constructed based on the key. This square is typically composed of the unique letters from the key, and the remaining cells are filled accordingly. Usually, the letters I and J are treated as the same letter within the square. The construction rules are as follows:

    * Fill the square with letters from the key without repetition.
    * Disregard duplicate letters in the key.
    * If the square contains the letter I and the key has the letter J, treat I and J as the same letter. For example, using the key "KEYWORD," the constructed Playfair Square might look like:

    ```
    K E Y W O
    R D A B C
    F G H I/J L
    M N P Q S
    T U V X Z
    ```
2. Encryption Process: During encryption, each pair of letters (called a Bigram) in the plaintext is located in the Playfair Square. The following cases may arise:
   * If the two letters are in the same row, replace them with the letters to their right, looping back to the beginning of the row.
   * If the two letters are in the same column, replace them with the letters below, looping back to the top of the column.
   * If the two letters are neither in the same row nor the same column, replace them with the other two corner letters of the formed rectangle. For example, for the plaintext "HELLO," the encrypted result might be "DPRYV."
3. Decryption Process: During decryption, the reverse of the encryption process is applied. Following the rules of the Playfair Square, each pair of letters in the ciphertext is restored to the plaintext using Bigrams. For example, for the ciphertext "DPRYV," the decrypted result might be "HELLO."

The Playfair Cipher was designed to provide a certain level of protection against traditional attacks such as frequency analysis, as it disrupts simple letter substitution relationships. However, for modern cryptography, the Playfair Cipher is not considered sufficiently secure, as it is still vulnerable to some attacks. In practical applications, more complex encryption algorithms are widely used to ensure a higher level of security.



```
def toLowerCase(text):
    return text.lower()

def removeSpaces(text):
    return text.replace(" ", "")

def Diagraph(text):
    # Group the text into bigrams
    # If consecutive letters are the same, insert 'x' between them
    pairs = [text[i:i + 2] if text[i] != text[i + 1] else text[i] + 'x' for i in range(0, len(text), 2)]
    return pairs

def FillerLetter(text):
    # Insert 'x' between consecutive same letters
    return ''.join([text[i] + ('x' if text[i] == text[i + 1] else '') for i in range(0, len(text) - 1, 2)]) + text[-1]

def generateKeyTable(word, list1):
    # Generate the Playfair key table
    keyTable = [[0] * 5 for _ in range(5)]
    key = word + list1
    key = ''.join(sorted(set(key), key=key.index))
    
    for i in range(5):
        for j in range(5):
            keyTable[i][j] = key[i * 5 + j]

    return keyTable

def search(mat, element):
    # Search for the position of an element in the Playfair key table
    for i in range(5):
        for j in range(5):
            if mat[i][j] == element:
                return i, j

def encrypt_RowRule(matr, e1r, e1c, e2r, e2c):
    # Apply encryption rule for letters in the same row
    return matr[e1r][(e1c + 1) % 5] + matr[e2r][(e2c + 1) % 5]

def encrypt_ColumnRule(matr, e1r, e1c, e2r, e2c):
    # Apply encryption rule for letters in the same column
    return matr[(e1r + 1) % 5][e1c] + matr[(e2r + 1) % 5][e2c]

def encrypt_RectangleRule(matr, e1r, e1c, e2r, e2c):
    # Apply encryption rule for letters not in the same row or column
    return matr[e1r][e2c] + matr[e2r][e1c]

def encryptByPlayfairCipher(Matrix, plainList):
    # Encrypt the plaintext using Playfair cipher
    cipherList = []

    for pair in plainList:
        e1r, e1c = search(Matrix, pair[0])
        e2r, e2c = search(Matrix, pair[1])

        if e1r == e2r:
            cipherList.append(encrypt_RowRule(Matrix, e1r, e1c, e2r, e2c))
        elif e1c == e2c:
            cipherList.append(encrypt_ColumnRule(Matrix, e1r, e1c, e2r, e2c))
        else:
            cipherList.append(encrypt_RectangleRule(Matrix, e1r, e1c, e2r, e2c))

    return cipherList

# Main Program
key = "Monarchy"
plaintext = "instruments"

# Preprocessing
plaintext = toLowerCase(plaintext)
plaintext = removeSpaces(plaintext)

# If the length of plaintext is odd, append 'z' at the end
if len(plaintext) % 2 != 0:
    plaintext += 'z'

# Generate Playfair key table
alphabet = 'abcdefghiklmnopqrstuvwxyz'
keyTable = generateKeyTable(key, alphabet)

# Display key and processed plaintext
print("Playfair Key Table:")
for row in keyTable:
    print(' '.join(row))
print("\nProcessed Plaintext:", plaintext)

# Generate bigrams
plaintextBigrams = Diagraph(FillerLetter(plaintext))

# Encrypt the plaintext using Playfair cipher
ciphertext = encryptByPlayfairCipher(keyTable, plaintextBigrams)

# Display encrypted ciphertext
print("Encrypted Ciphertext:", ''.join(ciphertext))

```

This code implements the Playfair Cipher algorithm. Below is a detailed analysis of each part:

1. Preprocessing Functions:
   * `toLowerCase(text)`: Converts the input string to lowercase.
   * `removeSpaces(text)`: Removes all spaces from the input string.
2. Generating Bigrams:
   * `Diagraph(text)`: Groups the text into bigrams.
3. Handling Adjacent Identical Letters:
   * `FillerLetter(text)`: Handles consecutive identical letters by inserting 'x' between them to comply with Playfair rules.
4. Generating the Key Table:
   * `generateKeyTable(word, list1)`: Generates a 5x5 Playfair key table based on the given key and alphabet.
5. Search Function:
   * `search(mat, element)`: Searches for the position of a specific element in the Playfair key table.
6. Encryption Rules:
   * `encrypt_RowRule(matr, e1r, e1c, e2r, e2c)`: Applies encryption rules when two letters are in the same row.
   * `encrypt_ColumnRule(matr, e1r, e1c, e2r, e2c)`: Applies encryption rules when two letters are in the same column.
   * `encrypt_RectangleRule(matr, e1r, e1c, e2r, e2c)`: Applies encryption rules when two letters are neither in the same row nor column.
7. Playfair Encryption:
   * `encryptByPlayfairCipher(Matrix, plainList)`: Encrypts the plaintext using the above rules, returning a list of ciphertext.

Main Program Section:

* Preprocesses the plaintext.
* If the length of the plaintext is odd, appends 'z' at the end.
* Generates the Playfair key table.
* Outputs the key and processed plaintext.
* Encrypts the plaintext using the Playfair Cipher.
* Outputs the encrypted ciphertext.

The purpose of this code is to demonstrate the encryption process of the Playfair Cipher with a key of "Monarchy" and plaintext of "instruments."

## Hill cipher

Another typical polygraphic substitution cipher is the Hill cipher.

The Hill cipher is a block cipher algorithm based on linear algebra, capable of encrypting and decrypting plaintext. The main idea behind the Hill cipher is to use matrix multiplication for encryption and decryption operations. Here is a detailed explanation of the Hill cipher:

1.  Matrix Representation and Key Selection:

    * The Hill cipher employs a matrix as its key. The size of the key matrix determines the block size. Typically, 2x2 and 3x3 matrices are common. For example, for a 2x2 key matrix:

    <figure><img src=".gitbook/assets/image (1) (1) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

    For a 3x3 key matrix:

<figure><img src=".gitbook/assets/image (2) (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

* The key matrix must be invertible. In the case of modulo 26, the determinant of the matrix must be coprime.

2. Encryption Operation:
   * Divide the plaintext into blocks, each block having the same size as the key matrix.
   * Represent each plaintext block as a column vector and multiply it with the key matrix. Take the modulo 26 to obtain the ciphertext block.

* Combine the resulting ciphertext blocks to form the complete ciphertext.

3.  Decryption Operation:

    * Use the inverse matrix of the key matrix to multiply it with the ciphertext block, obtaining the plaintext block.


4. Considerations:
   * Due to the necessity of using the matrix inverse, the size of the key matrix should not be too large to avoid complex inverse matrix calculations.
   * If the block size does not match the matrix size, padding may be required in the plaintext.

The Hill cipher is a robust encryption algorithm based on mathematical principles. However, in practical applications, it is important to choose an appropriate key matrix and handle blocks properly.

To illustrate this abstract explanation, let's consider a specific example.

The Hill cipher is a polygraphic substitution cipher based on linear algebra. Each letter is represented by a number modulo 26. A simple scheme is often used, where A = 0, B = 1, ..., Z = 25, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters (considered as an n-dimensional vector) is multiplied by a reversible n × n matrix, followed by modulo 26. To decrypt the message, each block is multiplied by the inverse matrix of the one used for encryption.

The matrix used for encryption serves as the cryptographic key and should be randomly chosen from the set of reversible n × n matrices (modulo 26).

Let's assume we need to encrypt the message 'ACT' (n=3). The key is 'GYBNQKURP', which can be represented as an n × n matrix:

<figure><img src=".gitbook/assets/image (3) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The message 'ACT' can be represented as the following vector:

<figure><img src=".gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

The encrypted vector is given as:

<figure><img src=".gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>

The corresponding ciphertext is 'POH'. To decrypt the message, we convert the ciphertext back into a vector and then simply multiply it by the inverse matrix of the key matrix (IFKVIVVMI corresponds to the letters). The inverse matrix of the matrix used in the previous example is:

<figure><img src=".gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

For the previous ciphertext 'POH':

<figure><img src=".gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>

This will allow us to recover 'ACT'.

Assuming all letters are in uppercase, let's take a look at the specific code implementation.

```
# Python3 code to implement Hill Cipher

keyMatrix = [[0] * 3 for i in range(3)]

# Generate vector for the message
messageVector = [[0] for i in range(3)]

# Generate vector for the cipher
cipherMatrix = [[0] for i in range(3)]

# Following function generates the
# key matrix for the key string
def getKeyMatrix(key):
	k = 0
	for i in range(3):
		for j in range(3):
			keyMatrix[i][j] = ord(key[k]) % 65
			k += 1

# Following function encrypts the message
def encrypt(messageVector):
	for i in range(3):
		for j in range(1):
			cipherMatrix[i][j] = 0
			for x in range(3):
				cipherMatrix[i][j] += (keyMatrix[i][x] *
									messageVector[x][j])
			cipherMatrix[i][j] = cipherMatrix[i][j] % 26

def HillCipher(message, key):

	# Get key matrix from the key string
	getKeyMatrix(key)

	# Generate vector for the message
	for i in range(3):
		messageVector[i][0] = ord(message[i]) % 65

	# Following function generates
	# the encrypted vector
	encrypt(messageVector)

	# Generate the encrypted text 
	# from the encrypted vector
	CipherText = []
	for i in range(3):
		CipherText.append(chr(cipherMatrix[i][0] + 65))

	# Finally print the ciphertext
	print("Ciphertext: ", "".join(CipherText))

# Driver Code
def main():

	# Get the message to 
	# be encrypted
	message = "ACT"

	# Get the key
	key = "GYBNQKURP"

	HillCipher(message, key)

if __name__ == "__main__":
	main()

```

This code implements the Hill Cipher algorithm. Here is a detailed analysis of the code:

1. `keyMatrix`, `messageVector`, and `cipherMatrix` are data structures representing matrices and vectors, used to store the key matrix, message vector, and the matrix after encryption.
2. The `getKeyMatrix(key)` function is used to generate the key matrix from the key string. It iterates through the characters of the key string, storing the result of each character's ASCII code modulo 65 in the `keyMatrix`.
3. The `encrypt(messageVector)` function encrypts the message vector by multiplying the key matrix with the message vector. The encrypted result is taken modulo 26 to ensure the result is within the range of the alphabet.
4. The `HillCipher(message, key)` function is the main functionality of the Hill Cipher. It calls the `getKeyMatrix` function to generate the key matrix and converts the message string into a message vector. Then, it calls the `encrypt` function to encrypt the message vector, and finally, prints the result as ciphertext.
5. In the `main` function, the message to be encrypted ("ACT") and the key for encryption ("GYBNQKURP") are defined. The `HillCipher` function is then called to execute the encryption process.

In summary, the Hill Cipher is a polygraphic substitution cipher based on linear algebra. It utilizes matrix operations for encrypting and decrypting messages. In this instance, the key matrix is a 3x3 matrix, and the message vector is a column vector with 3 elements.

## Vigenère Cipher

Now, let's learn about the Vigenère Cipher.

The Vigenère Cipher is a classic polygraphic substitution cipher proposed by the French cryptographer Blaise de Vigenère in the 16th century. It encrypts plaintext by using a keyword to guide the encryption process, employing different Caesar cipher tables to achieve a polyalphabetic substitution effect.

<figure><img src=".gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

The basic principles and steps of the Vigenère Cipher are as follows:

1. Key: Choose a keyword (key), for example, "KEY." This keyword will be used to guide the encryption. The keyword can be repeated until the entire plaintext is encrypted.
2. Repeat Key: Expand the keyword to the same length as the plaintext. For example, if the plaintext is "HELLO," then the keyword is expanded to "KEYKE."
3. Encryption: For each plaintext character, use the corresponding letter in the keyword to select a Caesar cipher table. For instance, if the plaintext character is "H" and the corresponding character in the keyword is "K," use the row in the Caesar cipher table that starts with "K" to encrypt "H." Repeat this process until the entire plaintext is encrypted.
4. Decryption: The decryption process is similar to encryption. For each ciphertext character, use the corresponding letter in the keyword to select a Caesar cipher table and find the corresponding plaintext character. Repeat this process until the entire ciphertext is decrypted.

Here is a simple example of the Vigenère Cipher: Plaintext: HELLO Key: KEY Repeated Key: KEYKE Encryption Process:

* H is encrypted to "K" (using the Caesar cipher table with the row starting with "K")
* E is encrypted to "R" (using the Caesar cipher table with the row starting with "R")
* L is encrypted to "I" (using the Caesar cipher table with the row starting with "I")
* L is encrypted to "I" (using the Caesar cipher table with the row starting with "I")
* O is encrypted to "V" (using the Caesar cipher table with the row starting with "V") Ciphertext: KRIVI

The strength of the Vigenère Cipher lies in its use of multiple Caesar cipher tables, making it more challenging to break. However, for longer keywords, there are still some attack methods. In modern cryptography, more complex encryption algorithms are typically used. Nevertheless, as one of the historical classic ciphers, the Vigenère Cipher retains educational and historical value.

Now, let's take a look at the corresponding code.

```
# Vigenere Cipher

# This function generates the 
# key in a cyclic manner until 
# it's length isn't equal to 
# the length of original text
def generateKey(string, key):
	key = list(key)
	if len(string) == len(key):
		return(key)
	else:
		for i in range(len(string) -
					len(key)):
			key.append(key[i % len(key)])
	return("" . join(key))
	
# This function returns the 
# encrypted text generated 
# with the help of the key
def cipherText(string, key):
	cipher_text = []
	for i in range(len(string)):
		x = (ord(string[i]) +
			ord(key[i])) % 26
		x += ord('A')
		cipher_text.append(chr(x))
	return("" . join(cipher_text))
	
# This function decrypts the 
# encrypted text and returns 
# the original text
def originalText(cipher_text, key):
	orig_text = []
	for i in range(len(cipher_text)):
		x = (ord(cipher_text[i]) -
			ord(key[i]) + 26) % 26
		x += ord('A')
		orig_text.append(chr(x))
	return("" . join(orig_text))
	
# Driver code
if __name__ == "__main__":
	string = "HELLOWORLD"
	keyword = "AYUSH"
	key = generateKey(string, keyword)
	cipher_text = cipherText(string,key)
	print("Ciphertext :", cipher_text)
	print("Original/Decrypted Text :", 
		originalText(cipher_text, key))

```

This Python code implements the Vigenere Cipher. Here's a detailed explanation of the code:

1. The `generateKey` function generates a key, ensuring its length is the same as the original text. If the key's length is less than the original text, it cyclically uses characters from the key until reaching the same length.
2. The `cipherText` function encrypts the text. It iterates over each character in the original text and adds it to the corresponding position in the key. Then, it takes the result modulo 26 concerning the ASCII value of the character 'A' and converts the obtained value back to an ASCII character.
3. The `originalText` function decrypts the encrypted text. It performs the opposite operation of encryption, subtracting each ciphertext character from the corresponding position in the key. The result is taken modulo 26 concerning the ASCII value of the character 'A', and the obtained value is converted back to an ASCII character.
4. In the main program, the original text and key are specified, and the above functions are called for encryption and decryption. Finally, the encrypted text (cipher) and decrypted text (original) are output.

This cryptographic technique uses a keyword (key) and applies different keys during the encryption and decryption processes. This makes the Vigenere Cipher more secure than simple substitution ciphers.

After execution, the output demonstrates successful implementation of Vigenere Cipher encryption and decryption.

## Reference

1\. [https://www.geeksforgeeks.org/playfair-cipher-with-examples/](https://www.geeksforgeeks.org/playfair-cipher-with-examples/)

2\. [https://www.geeksforgeeks.org/hill-cipher/](https://www.geeksforgeeks.org/hill-cipher/)

3\. [https://www.britannica.com/topic/cryptology/Vigenere-ciphers](https://www.britannica.com/topic/cryptology/Vigenere-ciphers)

4\. https://www.geeksforgeeks.org/vigenere-cipher/
