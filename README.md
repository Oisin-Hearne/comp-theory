# Computational Theory

This repository's tasks.ipynb file contains the implementation of several different algorithms, from bitwise operations to bubble sort. These are built in Python with additional markdown cells explaining their implementations.

Several of these tasks involve the Secure Hash Algorithm, and cover several different parts of its implementation such as bitwise operations, prime roots, and message padding.

More detail on the specific implementation of these tasks can be found in tasks.ipynb, where documentation cells are woven throughout the code for each task, describing how the code functions.


## Usage
- Ensure that the requirements in requirements.txt are present.
- Ensure that a words.txt file containing an english dictionary of your choice is present.
- Ensure that a sha256.txt file containing a message you would like to hash is present.
- Run the notebook via Jupyter or VSCode.


## Task 1 - Bitwise Operations
This section contains several functions for modifying binary numbers. These functions are used in the [Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf) and are used as part of the Secure Hash Algorithm.

These functions include:
- rotl(): Rotate the bits of a binary input to the right.
- rotr(): Rotate the bits of a binary input to the left. 
- ch(): Choose the bits from Y where X has bits set to 1, and bits from Z where X has bits set to 0. This is effectively just an AND on X & Y, and a NAND on Z & X.
- maj(): Takes a "vote" for each bit given three inputs. Where two of them have a 1, the output is 1. Where two of them have 0, the output is 0.

## Task 2 - Hashing
This is a simple python implementation of the hashing function featured in [The C Programming Language](https://seriouscomputerist.atariverse.com/media/pdf/book/C%20Programming%20Language%20-%202nd%20Edition%20(OCR).pdf). It takes in a string, and returns a number (that is, the hash value of that string). 

This uses the constants 31 and 101, which interestingly enough have no proven reason behind why they function best in this implementation - They are primes close to the values 32 and 100, but there are no proven explanations for why they are the best.

Hashing functions such as these are used for password encryption (Though this implementation is quite simple and not safe for password storage, of course). When input into this function, "hello" results in an output of 17. Reverse engineering this output to get "hello" is difficult, but verifying that "hello" hashes to 17 is easy.

## Task 3 - SHA256 Padding
This section features a python implementation of the padding for a SHA256 message, described in section 5.1.1 of the [Secure Hash Standard](https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.180-4.pdf).

In short, a 1 is appended to the last block of the message (the message being split into blocks of 512), followed by a number of zeroes and a 64 bit number representing the length of the message.

This padding is necessary in order to describe how long the message is, clearly mark where the message ends and ensure that each block is the same length of 512.

**Note**: In order to run this task, a "sha256.txt" file should be present with a simple message. An input file of "hello world" resulted in the following hex (hidden for brevity).
<details> 
<summary>Hex String</summary>

10 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 05 07
</details>

## Task 4 - Prime Numbers
This section features several different methods of finding the first 100 prime numbers. Finding prime numbers is computationally difficult and runtimes can quickly get out of hand.

The implementations examined are the brute force method (simply, counting up and checking if every single number is a prime) as well as the sieves of Eratosthenes and Atkin.

The latter two implementations are much more robust, marking off numbers that cannot be prime numbers as they go (such as every multiple of two). This involves a number of conditions to check every iteration and is typically still computationally expensive.

While the Sieve of Eratosthenes is faster for the first 100 primes, at a larger scale the Sieve of Atkin would likely end up completing faster. Both algortihms can still be rather slow, though.


## Task 5 - Prime Roots
With the first 100 prime numbers gathered from the previous task, this task involves a specific calculation used in the SHS in order to create the initial hash value for SHA-256.

This section calculates the first thirty two bits of the fractional parts of the square roots of the first 100 prime numbers.

In order, this means:
- Getting the square roots of the first 100 prime numbers.
- Getting only the fractional part. That is,everything after the decimal point.
- Calculating the first thirty-two bits of this fractional part.

Variations on this formula involving different prime numbers (and occasionally cube roots instead of prime roots) are used to form the initial hashes for several different variations of the SHA.

## Task 6 - Proof of Work
This section demonstrates Proof of Work by finding the word in a given dictionary with the highest number of leading zeroes in it's SHA256 digest.

The purpose of this is to create a task that is difficult to perform computationally, as proof that some work has been done. This is used for emails (see HashCash, [11]) in order to prevent email servers from sending millions of emails simultaneously, reducing spam.

It's most famous use is probably in cryptocurrency, though, where proof of work is required in order to mine a coin. An example of this could be generating a string with a certain number of leading zeroes - this would take quite some time to compute, but would be easy to verify by other machines once done.

**Note**: This task requires a words.txt in the root of the repository to function. I used the one from last semseter's Emerging Technology, to the following results, hidden for brevity.

<details>

<summary>Results of Task 6</summary>

The word with the most leading zeroes was APPLICANT, with sixteen. The hash to get this result was as follows:

```
0000000000000000110010100000000110101101110010010111001111000010101001011010100011100110101000110000000100110100111100000111001110010110110100001000100010110110110101100101000000100101001010010010111101101111111110010111101100100011011111001010101101001101
```

</details>

## Task 7 - Turing Machines
Turing Machines are the simplest and most abstract form of a computer, reading in 1s and 0s from a tape and manipulating the tape and the position of the tape head based on the current state.

This specific turing machine adds 1 to a binary number. That is, 011101 should become 011110. The state table for this turing machine has been included below.
```python
state_table = {
    ("U", "0"): ("U", "0", "R"),
    ("U", "1"): ("U", "1", "R"),
    ("U", "_"): ("V", "_", "L"),
    
    ("V", "0"): ("W", "1", "L"),
    ("V", "1"): ("V", "0", "L"),
    ("V", "_"): ("A", "1", "R"),
    
    ("W", "0"): ("W", "0", "L"),
    ("W", "1"): ("W", "1", "L"),
    ("W", "_"): ("A", "_", "R")
}
```

This implementation includes a simulation of all of the functionality of a turing machine - the state table, moving the tape head left and right, and so on.


## Task 8 - Computational Complexity
This section features an analysis of BubbleSort, an inefficient but interesting sorting algorithm that swaps the position of items in a list in an attempt to sort it.

Bubblesort was performed on all permutations of the given list [5,4,3,2,1]. It was found that at permutations closely resembling [1,2,3,4,5], the algorithm required fewer swaps and was therefore more efficient, though it quickly becomes incredibly inefficient at less sorted lists.


## References
The below references were gathered through research for completing the above tasks. They are cited throughout the jupyter notebook.
- [1] Appending bits to the start of a binary number: https://stackoverflow.com/a/51678298
- [2] ord() in Python: https://docs.python.org/3.4/library/functions.html?highlight=ord#ord
- [3] Open file with encoding: https://stackoverflow.com/a/49375134
- [4] Splitting an array into even-sized chunks: https://www.geeksforgeeks.org/break-list-chunks-size-n-python/
- [5] Sieve of Eratosthenes: https://cp-algorithms.com/algebra/sieve-of-eratosthenes.html
- [6] Cube Roots Notes: https://github.com/ianmcloughlin/computational_theory/blob/main/materials/cube_roots.ipynb
- [7] Bubble Sort Explanation: https://www.w3schools.com/dsa/dsa_algo_bubblesort.php
- [8] Bubble Sort Time Complexity: https://www.w3schools.com/dsa/dsa_timecomplexity_bblsort.php
- [9] Sieve of Atkin: https://www.geeksforgeeks.org/sieve-of-atkin/
- [10] Using the HashLib library: https://github.com/ianmcloughlin/computational_theory/blob/main/materials/hash_functions.ipynb
- [11] HashCash Paper "A Denial of Service Counter-Measure": http://www.hashcash.org/hashcash.pdf
- [12] Permutations using Itertools: https://stackoverflow.com/a/104436