# Random Sequence Ⅳ

## Maximum Length Sequence

Before, we studied LFSR, and now let's take a look at the m-sequence of LFSR.

The m-sequence (Maximum Length Sequence) of LFSR is a pseudo-random sequence with the longest possible period. This sequence is generated in the LFSR by appropriately selecting the initial state and feedback polynomial, allowing the LFSR to achieve its maximum possible period. Such m-sequences are highly useful in various applications, especially in the fields of communication, cryptography, and test pattern generation.

Some basic concepts and characteristics of the m-sequence of LFSR are as follows:

1. Maximum Periodicity: The m-sequence of LFSR has the longest possible period, equal to 2^n - 1, where n is the number of bits in the LFSR. This is because the m-sequence will go through all 2^n - 1 different states before reaching its initial state.
2. Initial State: Selecting an appropriate initial state is crucial for generating an m-sequence with the maximum period. Generally, the initial state cannot be all zeros.
3. Feedback Polynomial: Choosing an appropriate feedback polynomial is also key. The feedback polynomial determines which bits participate in the feedback operation. A common feedback polynomial is a primitive polynomial, such as x^n + 1, where n is the number of bits in the LFSR.
4. Period Detection: To verify whether the generated sequence is an m-sequence, period detection methods can be used. A simple method is to observe whether the sequence returns to the initial state after reaching 2^n - 1 bits.
5. Applications: M-sequences have important applications in many areas, such as in pseudo-random number generation, pseudo-random test pattern generation, and sequence spreading in spread spectrum communication systems (e.g., CDMA).

```
class LFSR:
    def __init__(self, initial_state, feedback_mask):
        self.state = initial_state
        self.feedback_mask = feedback_mask

    def shift(self):
        # Calculate feedback bit using XOR of masked state bits
        feedback_bit = sum((self.state >> i) & 1 for i in range(len(self.feedback_mask)) if self.feedback_mask[i]) % 2
        # Shift state to the right and insert feedback bit at the leftmost position
        self.state = (self.state >> 1) | (feedback_bit << (len(self.feedback_mask) - 1))
        return feedback_bit

    def generate_m_sequence(self, num_bits):
        m_sequence = []
        # Generate m-sequence by repeatedly shifting and collecting the rightmost bit
        for _ in range(num_bits):
            m_sequence.append(self.state & 1)
            self.shift()
        return m_sequence

# 4-bit LFSR, initial state is 0b1011, feedback polynomial is x^4 + x^3 + 1
initial_state = 0b1011
feedback_mask = [1, 0, 0, 1]  # Corresponds to x^4 + x^3 + 1
lfsr = LFSR(initial_state, feedback_mask)

# Generate 15-bit m-sequence
m_sequence = lfsr.generate_m_sequence(15)

# Print the result
print("Generated M Sequence:", m_sequence)

```

This code implements a Linear Feedback Shift Register (LFSR) class for generating m-sequences.

* Initialization Method: The constructor takes two parameters - `initial_state` represents the initial state of the LFSR, and `feedback_mask` is the mask for the feedback polynomial. `feedback_mask` is a list where 1s and 0s indicate whether the corresponding bit participates in the feedback.
* Shift Method: This method simulates the shift operation of the LFSR. First, it calculates the feedback bit `feedback_bit` using list comprehension and bitwise operations. Then, it shifts the state to the right by one position and updates the leftmost bit using bitwise operations, simulating the LFSR shift process.
* `generate_m_sequence` Method: This method generates the m-sequence. In each iteration, it appends the rightmost bit of the current state to `m_sequence` and then calls the `shift` method for state shifting.
* An instance of a 4-bit LFSR is created, with an initial state of `0b1011` and a feedback polynomial corresponding to the mask `[1, 0, 0, 1]`, representing `x^4 + x^3 + 1`. Then, a list containing a 15-bit m-sequence is generated, and the result is printed.

This example demonstrates how an LFSR generates m-sequences based on the feedback polynomial and initial state. In practical applications, different LFSR bit lengths and feedback polynomials can be chosen to generate m-sequences with varying periodicity and properties as needed.

Using m-sequences directly generated by an LFSR may not be secure, primarily due to the inherent properties of LFSRs and the fixed initial state, leading to some security weaknesses that make the generated sequences vulnerable to attacks. Specifically, these weaknesses include:

1. Predictability: If an attacker can deduce or guess the LFSR's initial state and feedback polynomial, they can predict the generated m-sequence. For shorter LFSRs, brute force might be an effective attack method since the state space of an LFSR is finite.
2. Periodicity: The period of an LFSR is finite and depends on its bit length and the choice of feedback polynomial. After a certain number of steps, the generated sequence will repeat. If an attacker knows the LFSR's period, they can predict subsequent outputs by observing part of the sequence.
3. Fixed Initial State: If the initial state is fixed and known to an attacker, they can repeatedly generate the same sequence. This can be insecure in cryptographic applications where passwords should exhibit unpredictability.
4. Linearity: Sequences generated by LFSRs exhibit linearity, meaning each bit in the sequence can be expressed as a linear combination of previous bits. This linearity can make the generated sequences more susceptible to statistical attacks.

Due to these security vulnerabilities, using m-sequences directly from an LFSR as cryptographic keys or in security applications is not recommended. In practical cryptography applications, more complex Pseudo-Random Number Generators (PRNGs) or stream cipher algorithms are widely used to enhance security and prevent potential attacks. These algorithms are typically designed with greater complexity, employing non-linear operations and larger state spaces to enhance cryptographic security.

## Filter

We study three typical methods, with the first one related to filter generators.

In pseudo-random sequence generators based on Linear Feedback Shift Registers (LFSRs), filter generators are an improved structure designed to enhance the statistical properties and security of the generated pseudo-random sequences. Filter generators combine the concepts of Linear Feedback Shift Registers (LFSRs) and linear complexity filters by processing the output of the LFSR through non-linear filtering operations.

Basic principles of filter generators include:

1. LFSR: Like traditional LFSRs, filter generators use a register to store states and generate pseudo-random sequences through shifting and linear feedback operations.
2. Linear Complexity Filter: Filter generators introduce a linear complexity filter, which is a non-linear operation typically composed of one or more non-linear components, such as Boolean functions. This filter enhances the complexity of the sequence by applying non-linear transformations to the output of the LFSR.
3. Feedback Path: The feedback path in filter generators includes not only feedback from the LFSR but also feedback from the linear complexity filter. This integrated feedback path increases the non-linearity of the generated sequence, improving its statistical properties.
4. Selection of Appropriate Boolean Functions: The security and performance of filter generators largely depend on the choice of Boolean functions for the non-linear filter. These Boolean functions should possess cryptographic properties, such as uniformity and resistance to analysis.
5. Initial State: The selection of the initial state is equally crucial for the generated sequence. Avoiding the use of an all-zero or all-one state is a good practice.

```


class LFSR:
    def __init__(self, initial_state, feedback_mask):
        # Initialize LFSR with given initial state and feedback mask
        self.state = initial_state
        self.feedback_mask = feedback_mask

    def shift(self):
        # Perform LFSR shift operation and return the feedback bit
        feedback_bit = sum((self.state >> i) & 1 for i in range(len(self.feedback_mask)) if self.feedback_mask[i]) % 2
        self.state = (self.state >> 1) | (feedback_bit << (len(self.feedback_mask) - 1))
        return feedback_bit

class FilterFunctionGenerator:
    def __init__(self, lfsr_length, filter_length, feedback_mask):
        # Initialize FilterFunctionGenerator with LFSR parameters and filter feedback mask
        self.lfsr_length = lfsr_length
        self.filter_length = filter_length
        self.lfsr = LFSR(1, feedback_mask[:lfsr_length])
        self.filter_function = feedback_mask[lfsr_length:]

    def generate_key_stream(self, num_bits):
        key_stream = []
        # Generate key stream by combining LFSR and non-linear filter function
        for _ in range(num_bits):
            lfsr_output = self.lfsr.shift()
            # Calculate output of the non-linear filter function
            filter_output = sum((self.filter_function[i] and self.lfsr.state >> i & 1) for i in range(self.filter_length)) % 2
            # XOR the LFSR and filter outputs to get the key bit
            key_bit = lfsr_output ^ filter_output
            key_stream.append(key_bit)
        return key_stream

# Example: A 4-stage LFSR and a 3-element non-linear filter function
lfsr_length = 4
filter_length = 3
feedback_mask = [True, False, True, True, False, True, False]  # Feedback polynomial for x^3 + x^2 + 1

filter_generator = FilterFunctionGenerator(lfsr_length, filter_length, feedback_mask)

# Generate and print 10 key stream bits
key_stream = filter_generator.generate_key_stream(10)
print("Generated Key Stream:", key_stream)

```

This code implements a filter generator based on both an LFSR and a non-linear filter function.

* **LFSR class:** Represents a Linear Feedback Shift Register. The constructor takes the initial state (`initial_state`) and the feedback mask (`feedback_mask`). The `shift` method simulates the LFSR shift operation, calculates the feedback bit, and updates the state accordingly.
* **FilterFunctionGenerator class:** Represents the filter generator. The constructor takes the length of the LFSR (`lfsr_length`), the length of the filter function (`filter_length`), and the feedback polynomial (`feedback_mask`). It initializes the LFSR and the filter function.
* The `generate_key_stream` method generates a key stream, producing one bit at a time. At each step, it generates a bit (`lfsr_output`) using the LFSR and calculates the output of the non-linear filter function (`filter_output`). The final key bit is obtained by XORing these two outputs.
* An example is created using a 4-bit LFSR and a 3-element non-linear filter function. The `feedback_mask` defines the feedback polynomial as x^3 + x^2 + 1. The filter generator is then used to generate 10 key stream bits, and the result is printed.

In summary, this filter generator combines an LFSR with a non-linear filter function, enhancing the complexity and statistical properties of the generated key stream by introducing non-linear operations. Such a design contributes to improving the security of the generated key. In practical applications, the choice of feedback polynomial and non-linear filter function should be adjusted based on specific security requirements.

## Combining generator

A combiner generator is an extended form of a pseudo-random sequence generator based on LFSR, and it generates more complex key streams by combining multiple LFSRs and non-linear filter functions. Such a design helps enhance the non-linearity, complexity, and security of the generated sequences.

Basic principles of a combiner generator:

1. LFSR Combination: The combiner generator includes multiple LFSRs, each with its own initial state and feedback polynomial. These LFSRs can run in parallel, generating their respective portions of the key stream.
2. Non-linear Filter Functions: Each LFSR is followed by a non-linear filter function, and these functions can be different. These functions perform non-linear transformations on the sequences generated by their respective LFSRs, introducing additional complexity.
3. Combination Operation: The generator combines the individual key stream portions in some way, such as through XOR operations or other combination operations. This combination operation can further increase the overall complexity of the generated sequence.
4. Initial States: The initial states of each LFSR should be different to ensure diversity among the various portions of the sequence.

```
# 

class LFSR:
    def __init__(self, initial_state, feedback_mask):
        # Initialize LFSR with given initial state and feedback mask
        self.state = initial_state
        self.feedback_mask = feedback_mask

    def shift(self):
        # Perform LFSR shift operation and return the feedback bit
        feedback_bit = sum((self.state >> i) & 1 for i in range(len(self.feedback_mask)) if self.feedback_mask[i]) % 2
        self.state = (self.state >> 1) | (feedback_bit << (len(self.feedback_mask) - 1))
        return feedback_bit

class CombinatorialGenerator:
    def __init__(self, lfsr_configs, combiner_function):
        # Initialize CombinatorialGenerator with LFSR configurations and combiner function
        self.lfsrs = [LFSR(config["initial_state"], config["feedback_mask"]) for config in lfsr_configs]
        self.combiner_function = combiner_function

    def generate_key_stream(self, num_bits):
        key_stream = []
        for _ in range(num_bits):
            # Generate outputs from each LFSR
            lfsr_outputs = [lfsr.shift() for lfsr in self.lfsrs]
            # Combine LFSR outputs using the specified combiner function
            combined_output = self.combiner_function(lfsr_outputs)
            key_stream.append(combined_output)
        return key_stream

# Example: A combinatorial generator with two 4-bit LFSRs and a simple XOR combiner function
lfsr_configs = [
    {"initial_state": 0b1101, "feedback_mask": [True, False, True, True]},
    {"initial_state": 0b1010, "feedback_mask": [True, False, False, True]}
]

def xor_combiner(lfsr_outputs):
    # XOR the LFSR outputs and return the result
    return sum(lfsr_outputs) % 2

combinatorial_generator = CombinatorialGenerator(lfsr_configs, xor_combiner)

# Generate and print 10 key stream bits
key_stream = combinatorial_generator.generate_key_stream(10)
print("Generated Key Stream:", key_stream)

```

Let's analyze the provided Python code:

#### `LFSR` Class:

1. **Initialization (`__init__` method):**
   * Takes `initial_state` and `feedback_mask` as parameters.
   * Initializes the LFSR instance with the provided initial state and feedback mask.
2. **Shift (`shift` method):**
   * Simulates the LFSR shift operation.
   * Computes the feedback bit based on the feedback mask.
   * Updates the LFSR state by shifting and incorporating the feedback bit.
   * Returns the computed feedback bit.

#### `CombinatorialGenerator` Class:

1. **Initialization (`__init__` method):**
   * Takes `lfsr_configs` and `combiner_function` as parameters.
   * Initializes multiple LFSR instances based on the provided configurations.
   * Stores the combiner function that will be used to combine the outputs of the LFSRs.
2. **Generate Key Stream (`generate_key_stream` method):**
   * Takes `num_bits` as a parameter.
   * Iterates for the specified number of bits.
   * Shifts each LFSR to generate outputs.
   * Uses the specified combiner function to combine the LFSR outputs.
   * Appends the combined output to the key stream.
   * Returns the generated key stream.

#### Example Usage:

1. Initializes an instance of `CombinatorialGenerator` with two 4-bit LFSRs and an XOR combiner function.
2. Defines configurations for each LFSR, specifying initial state and feedback mask.
3. Defines an XOR combiner function (`xor_combiner`) that XORs the outputs of the LFSRs.
4. Creates an instance of `CombinatorialGenerator` with the specified configurations and combiner function.
5. Generates and prints a key stream of 10 bits using the combinatorial generator.

#### Notes:

* The `LFSR` class represents a basic Linear Feedback Shift Register.
* The `CombinatorialGenerator` class demonstrates the concept of combining multiple LFSRs with a non-linear combiner function.
* The example uses a simple XOR combiner, but more complex combiner functions could be implemented.

Overall, this code illustrates the construction of a combinatorial generator using LFSRs, providing a foundation for generating complex key streams. The specific configurations and combiner functions can be customized based on the desired cryptographic requirements.

## Clock-Controlled Generator

Clock-Controlled Generator (CCG) is a type of pseudo-random sequence generator based on LFSR (Linear Feedback Shift Register), and its operation relies on a clock signal. The clock signal controls the shifting operations of the LFSR during the sequence generation process. This generator is commonly used in synchronous systems where the periodic variation of the clock signal is utilized to influence the generation of the pseudo-random sequence.

The basic principles of the Clock-Controlled Generator are as follows:

1. LFSR Module: The CCG consists of one or more LFSR modules, each of which is an independent LFSR. These LFSRs may have different initial states and feedback polynomials.
2. Clock Signal: The clock signal is a periodically changing signal that controls the shifting operations of the LFSR. During each clock cycle, the LFSR undergoes a shifting operation, generating a bit.
3. Clock Control: Changes in the clock signal directly impact the state update of the LFSR. When the clock signal changes, the LFSR performs shifting operations based on its feedback polynomial.
4. Output Sequence: The output sequence of the generator is a combination of outputs from all LFSR modules. Typically, a combination function (such as XOR operation) is used to combine the outputs of individual LFSRs to form the final pseudo-random sequence.

Now, let's take a look at the code.

```
# Linear Feedback Shift Register (LFSR) class
class LFSR:
    def __init__(self, initial_state, feedback_mask):
        self.state = initial_state
        self.feedback_mask = feedback_mask

    def shift(self):
        # Calculate feedback bit based on the feedback mask
        feedback_bit = sum((self.state >> i) & 1 for i in range(len(self.feedback_mask)) if self.feedback_mask[i]) % 2
        # Perform shift operation and update the state
        self.state = (self.state >> 1) | (feedback_bit << (len(self.feedback_mask) - 1))
        return feedback_bit

# Clock-Controlled Generator class
class ClockControlledGenerator:
    def __init__(self, primary_lfsr_config, control_lfsr_config):
        # Initialize primary and control LFSRs
        self.primary_lfsr = LFSR(primary_lfsr_config["initial_state"], primary_lfsr_config["feedback_mask"])
        self.control_lfsr = LFSR(control_lfsr_config["initial_state"], control_lfsr_config["feedback_mask"])

    def generate_key_stream(self, num_bits):
        key_stream = []
        for _ in range(num_bits):
            # Shift the control LFSR to determine whether the primary LFSR should operate
            control_bit = self.control_lfsr.shift()
            if control_bit:  # If the control bit is 1, the primary LFSR operates
                key_bit = self.primary_lfsr.shift()
            else:  # If the control bit is 0, the primary LFSR is inactive
                key_bit = 0
            key_stream.append(key_bit)
        return key_stream

# Example: Clock-Controlled Generator with a 4-bit primary LFSR and a 3-bit control LFSR
primary_lfsr_config = {"initial_state": 0b1101, "feedback_mask": [True, False, True, True]}
control_lfsr_config = {"initial_state": 0b101, "feedback_mask": [True, False, True]}

# Create a Clock-Controlled Generator instance
clock_controlled_generator = ClockControlledGenerator(primary_lfsr_config, control_lfsr_config)

# Generate and print 10 bits of the Clock-Controlled Sequence
clock_controlled_sequence = clock_controlled_generator.generate_key_stream(10)
print("Generated Clock-Controlled Sequence:", clock_controlled_sequence)

```

## Legendre sequence

In addition to leveraging the superior structure of Linear Feedback Shift Registers (LFSR) for designing pseudo-random sequence generators, there are also other generators that rely on knowledge of number theory or finite fields. These generators utilize mathematical tools, making theoretical analysis, including periodicity, random statistical properties, and linear complexity, more accessible. We will explore two common types of pseudo-random sequence generators, namely Legendre sequences and elliptic curve sequences.

Let's begin by studying Legendre sequences.

<figure><img src=".gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Legendre sequence is a pseudo-random sequence based on number theory, commonly employed in constructing pseudo-random number generators. The construction of such sequences is based on the properties of the Legendre symbol, which involves some important concepts in number theory, such as quadratic residues and the law of quadratic reciprocity.

Here are the basic steps for constructing a Legendre sequence:

1. Choose a modulus ( p ): The construction of Legendre sequences depends on a prime number ( p ). Choosing a sufficiently large prime number helps ensure the randomness and uniformity of the sequence.
2. Compute the Legendre symbol: The Legendre symbol is a mathematical notation in number theory, typically represented as ( \left(\frac{a}{p}\right) ), where ( a ) is an integer and ( p ) is a prime number. The computation of the Legendre symbol involves determining whether ( a ) is a quadratic residue modulo ( p ). The Legendre symbol is often used in conjunction with the law of quadratic reciprocity.

onstructing a Legendre sequence, we focus on quadratic residues and non-residues modulo p. The computation of Legendre symbols involves the properties of quadratic residues modulo p.

3. Constructing the sequence: By continuously selecting specific numbers and computing their Legendre symbols, a sequence can be obtained. Each term in the Legendre sequence is derived from a specific set of numbers, usually chosen based on their Legendre symbol properties.
4. Applying appropriate rules: Based on the values of Legendre symbols, rules can be defined. For example, if the symbol is 1, the corresponding position in the sequence takes the value 1; if the symbol is -1, it takes 0. This results in the final pseudo-random sequence.

The construction of Legendre sequences exhibits good mathematical properties, especially when the choice of modulo p is appropriate. These sequences find applications in cryptography and random number generation, among other fields, but they are not as widely used as LFSRs. In practical applications, analyzing the periodicity and statistical properties of Legendre sequences is crucial.

Now, let's look at the code.