# Team G-JJ's Private Set Intersection Algorithm
In this project, we designed and implemented several algorithms for secure and private set intersection.



## Contents
- [Motivation](#motivation)
- [Use of Code](#use-of-code)
  - [v1.0 - The basic algorithm](#v10---the-basic-algorithm)
    - [Change phone number count](#change-phone-number-count)
    - [Change key size](#change-key-size)
  - [v1.1 - Prevent man-in-the-middle attack with symmetric key encryption!](#v11---prevent-man-in-the-middle-attack-with-symmetric-key-encryption)
  - [v1.2 - Prevent set size reveal with padding!](#v12---prevent-set-size-reveal-with-padding)
    - [Change set size](#change-set-size)
  - [v2.0 - Detect shuffling of encrypted set with commit mechanism!](#v20---detect-shuffling-of-encrypted-set-with-commit-mechanism)
    - [Try shuffling the encrypted set](#try-shuffling-the-encrypted-set)
- [Algorithm Design](#algorithm-design)
  - [v1.0: Basic DH Algorithm](#v10-basic-dh-algorithm)
  - [v1.1: DH Algorithm with Symmetric Encryption](#v11-dh-algorithm-with-symmetric-encryption)
  - [v1.2: DH Algorithm with Padding](#v12-dh-algorithm-with-padding)
  - [v2.0: DH Algorithm with Result Verification](#v20-dh-algorithm-with-result-verification)
- [Design Considerations](#design-considerations)
  - [Secret from Each Party](#secret-from-each-party)
  - [No Collisions](#no-collisions)
- [Limitations](#limitations)
  - [Computational Time Trade-Off](#computational-time-trade-off)
  - [Guessing Phone Numbers](#guessing-phone-numbers)
  - [Sending Wrong Encryption Values](#sending-wrong-encryption-values)
- [External Libraries](#external-libraries)
  - [Cryptographic Libraries](#cryptographic-libraries)
    - [cryptography](#cryptography)
    - [hashlib](#hashlib)
    - [sympy](#sympy)
    - [secrets](#secrets)
  - [Other Libraries](#other-libraries)
    - [random](#random)
    - [pandas](#pandas)
  - [Installation](#installation)
- [Other Development Tools](#other-development-tools)
- [References](#references)



## Motivation 
[(go to top)]

Grab and Gojek want to identify the drivers who are double-dipping on both platforms while using the same hired vehicle and the same phone number for receiving orders (assuming this behaviour is against the individual contracts).
- Parties should be able to compute the intersection of two sets (phone numbers) without disclosing the sets.
- No party should gain any knowledge about sets of other parties (prevent poaching of drivers in the example).
- Every party should gain the knowledge about final intersection set (the common phone numbers are revealed).



## Use of Code

[(go to top)]  

Note: ensure all libraries mentioned in [External Libraries](#external-libraries) have been installed

### Explanation of development choice
We developed the code in an incremental manner, where each version is an improvement of the previous. This style of development
allowed us start simple and slowly improve on the algorithm. It also complements the time restriction where we can keep improving 
as long as the time permitted, while not being at risk of having no deliverables.
### Explanation of notebook format
There are six main sections in v1.0, v1.1 and v1.2. They are:
1. About
   * Explains the features in the version
2. Generate phone numbers and store them in "phone_numbers.csv"
3. Define classes necessary for the algorithm
   * These include `NumberGenerator`, `NumberInspector`, `StorageManager` and `Client`
   * Detailed comments explaining the purpose of each class is provided in the notebooks
4. Initialize context
   * Initialize variables required for psi algorithm
     * These include `key_size`, prime modulus `p`, `factors_pminus1` (factors of p-1), and the two clients `grab` and `gojek`
5. Get intersection
   * The main algorithm that consists of 4 steps 
   * Detailed explanations on each step is provided in the notebook
   * For all steps, there are 3 subsections
     1. The actual code for the step
     2. Clients' status after the step 
        * Displays the value of selected variables in the client class after the step
     3. Files' status after the step
        * Displays the content of each client's file after the step
     * Note: Subsections `b` and `c` shows a snapshot of the current state. 
     Subsection `c` is sometimes omitted if there are no changes to the file after that step
6. Check results
   * Verify that the intersection found is correct  

In v2.0, there is one additional section - `Verify no shuffling of encrypted set`  
* This section is meant to verify that no shuffling of the encrypted set was done by either client
* In this section, there 6 steps
* Detailed explanations on each step are provided in the notebook
### v1.0 - The basic algorithm
[(go to top)]  

Notebook: [v1.0 notebook](v1.0.ipynb)  
Algorithm explanation: [v1.0 explanation](#v10-basic-dh-algorithm)  

To run the notebook, simply click `Kernel -> Restart & Run All`  

#### Change phone number count
Edit the following in the notebook
```python
# set phone number count
gojek_phone_number_count = 61 # inclusive of phone numbers in common with grab
grab_phone_number_count = 91 # inclusive of phone numbers in common with gojek
common_phone_number_count = 10
```

#### Change key size
Edit the following cell in the notebook
```python
# assign pre-determined variables for psi
key_size = 1024 # both private keys and large prime
```
Note: Possible key sizes are 512&leq;`key_size`&leq;1024, where `key_size` is a multiple of 8. For minimum security, 1024 should be used.

### v1.1 - Prevent man-in-the-middle attack with symmetric key encryption!
[(go to top)]  

Notebook: [v1.1 notebook](v1.1.ipynb)   
Algorithm explanation: [v1.1 explanation](#v11-dh-algorithm-with-symmetric-encryption)  

To run the notebook, simply click `Kernel -> Restart & Run All`  
Everything mentioned in [v1.0](#v10---the-basic-algorithm) is applicable



### v1.2 - Prevent set size reveal with padding!
[(go to top)]

Notebook: [v1.2 notebook](v1.2.ipynb)   
Algorithm explanation: [v1.2 explanation](#v12-dh-algorithm-with-padding)  

To run the notebook, simply click `Kernel -> Restart & Run All`  
Everything mentioned in [v1.1](#v11---prevent-man-in-the-middle-attack-with-symmetric-key-encryption) is applicable

#### Change set size
Edit the following cell in the notebook
```python
# set set-size
set_size = 100 # both parties are to exchange sets of a pre-determined size

# set phone number count
gojek_phone_number_count = 61 # inclusive of phone numbers in common with grab
grab_phone_number_count = 91 # inclusive of phone numbers in common with gojek
common_phone_number_count = 10
```
Note: `set_size`&GreaterEqual;max{`gojek_phone_number_count`, `grab_phone_number_count`}


### v2.0 - Detect shuffling of encrypted set with commit mechanism!
[(go to top)]

Notebook: [v2.0 notebook](v2.0.ipynb)  
Algorithm explanation: [v2.0 explanation](#v20-dh-algorithm-with-result-verification)  

To run the notebook, simply click `Kernel -> Restart & Run All`  
Everything mentioned in [v1.2](#v12---prevent-set-size-reveal-with-padding) is applicable
#### Try shuffling the encrypted set
Edit the following cell to make one party shuffle the encrypted set and send the wrong order to the other party
```python
# clients encrypt other party's self encrypted set
grab.encrypt_set(True) # set is_other_party to true
# grab.encrypt_set(True, True) # set shuffle to true to make grab shuffle the encrypted set
gojek.encrypt_set(True)
# gojek.encrypt_set(True, True)
```
* In the example given, uncomment `grab.encrypt_set(True, True)` and comment out `grab.encrypt_set(True)` 
to make Grab shuffle the encrypted set i.e.:
```python
# clients encrypt other party's self encrypted set
# grab.encrypt_set(True) # set is_other_party to true
grab.encrypt_set(True, True) # set shuffle to true to make grab shuffle the encrypted set
gojek.encrypt_set(True)
# gojek.encrypt_set(True, True)
```
* Gojek ends up finding the wrong intersection while Grab still arrives at the correct intersection
![v2.0-1](img/v2.0-1.png)
Note: The NaN values is due to the intersection Gojek arrived by containing the invalid phone numbers used for padding
* Gojek is able to find out that Grab cheated
![v2.0-2](img/v2.0-2.png)

  

## Algorithm Design 
[(go to top)]

We have chosen to use an adapted version of the Diffie-Hellman key exchange algorithm for our PSI design. We have listed several versions of our algorithm below, each with varying levels of security and complexity.

<ins>Notation</ins>
| Notation | Definition |
| --- | --- |
| ![p] | common safe prime used in the Diffie-Hellman exchange to perform modular calculation|
| ![q] | Sophie Germain prime used to generate ![p]
| ![hash function 1] | function that hashes a value into a primitive root modulo ![p] |
| ![hash function 2] | standard hash function |
| ![symmetric encrpytion] | value m being encrypted by symmetric encryption
| ![key] | private Diffie-Hellman key of party ![party] |
| ![common size] | size of phone number list that parties agree on
| ![plaintext] | ![i]-th phone number of party ![party] in plaintext |
| ![hashed text] | ![i]-th phone number of party ![party] after it has been hashed using ![hash function 1] |
| ![ciphertext] | ![i]-th phone number of party ![party] in ciphertext, after it has been encrypted by party ![party]'s private key |
| ![shared ciphertext] | ![i]-th phone number of party ![party] in ciphertext, after it has been encrypted by both parties' private keys
| ![common ciphertext] | ![i]-th element in the common ciphertext set intersection computed by party ![party] |
| ![common plaintext] | ![i]-th element in the common phone number set intersection computed by party ![party] |
| ![sum of hashes] | sum of all ![common plaintext] values hashed with ![hash function 2] |

[p]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20p
[//]: # "p"
[q]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20q
[//]: # "q"
[party]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20A
[//]: # "A"
[i]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20i
[//]: # "i"
[common size]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20l
[//]: # "l"
[hash function 1]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20H_%7B1%7D
[//]: # "H_{1}"
[hash function 2]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20H_%7B2%7D
[//]: # "H_{2}"
[symmetric encrpytion]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20E(m)
[//]: # "E(m)"
[key]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20k_%7BA%7D
[//]: # "k_{A}"
[plaintext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20m_%7BA:i%7D
[//]: # "m_{A:i}"
[hashed text]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20h_%7BA:i%7D
[//]: # "h_{A:i}"
[ciphertext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20c_%7BA:i%7D
[//]: # "c_{A:i}"
[shared ciphertext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20s_%7BA:i%7D
[//]: # "s_{A:i}"
[common ciphertext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20x_%7BA:i%7D
[//]: # "x_{A:i}"
[common plaintext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20y_%7BA:i%7D
[//]: # "y_{A:i}"
[sum of hashes]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20z_%7BA%7D
[//]: # "z_{A}"



### v1.0: Basic DH Algorithm
[(go to top)]

In version 1.0 and its sub-versions, we assume that the parties are semi-honest. This means they will not deviate from the protocol. However, the parties will try to find out as much as possible about the other party's set thorough other means, such as performing dictionary attacks.

We have based this algorithm off of Clark's algorithm [[1]](#references). We use 1024-bit safe prime for ![p] and 1024-bit private keys for both parties.

The algorithm is as follows:

![v1.0](img/v1.0.png)

The security of Diffie-Hellman is based on the assumed difficulty of the discrete logarithm problem. For a Diffie-Hellman protocol to leverage this, it must use generators that are primitive roots modulo ![p]. In our algorithm, the "generators" being used are the phone numbers. Hence, for our algorithm to be secure, the phone numbers must be first hashed to primitive roots modulo ![p]. Additionally, hashing also helps to further obscure the phone numbers, making our algorithm more secure.

In our hash function ![hash function], we repeatedly hash a phone number using SHA3-256 until we get a value that is a primitive root modulo ![p]. The number of primitive roots modulo ![p] is ![toitent p-1] [[2]](#references). In our algorithm, where ![safe prime equation], the number of primitive roots modulo ![p] will be ![primitive root no]. This means that for numbers ranging from 1 to ![p-1], approximately half of the numbers will be primitive roots modulo ![p]. Consequently, the expected number of rounds our hash function ![hash function 1] will run to produces a primitive root modulo ![p] is 2.



[toitent p-1]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20%5Cphi(p-1)%20
[//]: # "\phi(p-1)"
[primitive root no]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20q-1
[//]: # "q-1"
[safe prime equation]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20p=2q&plus;1
[//]: # "p=2q+1"

<ins>Prevent Refusal to Send Phone Numbers</ins>

Consider a scenario where Grab chooses to withhold some of its phone numbers and not include them in the input set. However, in order for Grab to know whether those phone numbers are common phone numbers, it would need Gojek to encrypt the numbers with its private key. Hence, Grab has no choice but to include all its phone numbers in the algorithm. The same also applies for Gojek.

<ins>Prevent Dictionary Attack</ins>

For a party to run a dictionary attack, it would first have to find the private key of the other party. To do so, the party would have solve the discrete logarithm problem. However, the current best record for doing so is only for a 795-bit prime [[3]](#references). Hence, trying to find the private key for a 1024-bit prime would be computationally infeasible for either party or an external attacker to perform a dictionary attack. 



### v1.1: DH Algorithm with Symmetric Encryption
[(go to top)]

This is an improvement of the version 1.0 algorithm. On top of the base algorithm, we add symmetric encryption.

The algorithm is as follows (additions are in red):

![v1.1](img/v1.1.png)

The security of this algorithm would depend on the strength of the symmetric encryption scheme used. We have chosen to use Fernet encryption for our algorithm.
  
<ins>Prevent Man-in-the-Middle (MITM) Attacks </ins>

In the previous version, as with all Diffie-Hellman protocols, the algorithm is vulnerable to MITM attacks. However, in this version, without the shared symmetric key that the parties are using, there would be no way for an external attacker to pretend to be either party. Hence, this algorithm is safe from MITM attacks.



### v1.2: DH Algorithm with Padding
[(go to top)]

This is an improvement of the version 1.1 algorithm. On top of the base algorithm and symmetric encryption, we add padding of phone numbers.

The algorithm is as follows (additions are in red):

![v1.1](img/v1.2.png)

In this version of the algorithm, the list is padded with random dummy values until the size of the list matches ![common size], which is a common size that the two parties agree on. Each dummy value is a number ranging from 1000 0000 to 7999 9999. Since Singaporean mobile phone numbers start with either 8 or 9, these dummy values cannot not be valid Singaporean phone numbers. The dummy values do not affect the final set intersection, as any dummy values that appear in the computed set intersection are filtered out.

<ins>Prevent Set Size Leak </ins>

In the previous versions, Grab would be able to find out Gojek's set size from the number of elements in the set Gojek sends over, and vice versa. However, in this version of the algorithm, both sets are padded to size ![common size] with random dummy numbers. Since parties would not be able to distinguish between the encrypted ciphertexts of actual phone numbers and the dummy numbers, the set size of both parties is successfully hidden.



### v2.0: DH Algorithm with Result Verification
[(go to top)]

In this version, we assume that the parties are malicious. This means that, in addition to trying to gain information on the other party's set, they will also try to mess up the other party's intersection results if possible. They may do so by not following the protocol. For this algorithm, we assume that the only malicious actions the parties will take is to shuffle the ciphertexts. 

Consider a scenario where Grab wants to mess up Gojek's intersection results. After Grab has computed the  ![gojek shared ciphertext] values, it shuffles the values. As a result, when Gojek tries to find the common phone numbers by matching the values back to its original list, it will get back the wrong values since the order has been changed. 

Hence, we have added a verification portion to prevent this. In the verification phase, parties will confirm that no cheating has occurred and that they both got the same intersection values. This will discourage parties from cheating, as cheating can be uncovered during the verification phase.

The algorithm is as follows (additions are in red):

![v1.1](img/v2.0.png)

<ins>Step 1: Sum of hashes</ins>

First, the parties will compute the sum of hashes for its intersection sets. The parties will find all the ![hashed text] values that correspond to the phone numbers in the computed intersection. Then, they sum all these hashed values together. This condenses all of the intersection values into a single value for easier comparison and computation. If the intersection sets are the same, both parties will have the same sum of hashes value.

<ins>Step 2: Exchange sum of hashes using Diffie-Hellman exchange</ins>

However, the sum of hashes values are vulnerable to dictionary attack. For example, Gojek could try different combinations of phone numbers until it gets a combination that produces Grab's sum of hashes value. From this, Gojek would be able to find out that Grab has this specific combination of phone numbers in its set. Hence, the parties cannot exchange the sum of hashes values by simply sending them to each other.

We solve this by using a Diffie-Hellman exchange protocol to exchange the values. The exchange will perform the following steps:

1. Parties encrypt their sum of hashes with their Diffie-Hellman private keys.
2. Parties send their encrypted values to the other party.
3. Parties encrypt the other party's value with their Diffie-Hellman private keys. This new value is known as the comparison value.

If the parties used the same intersection set to compute the sum of hashes, the comparison values will be the same.

<ins>Step 3: Commit mechanism</ins>

Now the parties need to exchange the comparison values so that they can compare them. Consider a scenario where the parties do a simple exchange for the comparison values. Grab sends its comparison value first, followed by Gojek. Now let's say that Gojek cheated during the PSI algorithm phase and reordered Grab's encrypted values. To cover up its cheating, Gojek needs to send a comparison value that matches Grab's. Since Grab sends its comparison value to Gojek first, Gojek can simply send back Grab's value. As a result, Grab would wrongly think that no cheating has occurred and Gojek would have successfully gotten away with cheating.

We solve this problem using a commit mechanism. The commit mechanism has the following steps:

1. Grab and Gojek compute the hash of their comparison values using ![hash function 2], which is one round of SHA3-256.
2. Grab commits to a value by sending its hashed comparison value.
3. Gojek commits to a value by sending its hashed comparison value. Gojek also sends its actual comparison value.
4. Grab sends its actual comparison value.

With the commit mechanism, Gojek will no longer be able to hide its cheating during the verification phase. In step 2, Gojek can still send back Grab's hashed comparison value. However, it will be unable to fabricate a comparison value that matches Grab's due to two reasons.

Firstly, Gojek is unable to derive Grab's comparison value form Grab's hashed comparison value. This is because ![hash function 2] uses SHA3, which has very strong pre-image resistance. Hence, it would be infeasible for Gojek to try to derive Grab's comparison value through this method.

Secondly, Gojek will not know what phone numbers are in Grab's computed intersection.  After Gojek shuffles Grab's encrypted values, the common ciphertext will map back to values in Grab's set that Gojek is not aware of, since Gojek does not know Grab's set of phone numbers. Since Gojek is unable to know what values are in Grab's computed intersection, it is not able to produce Grab's comparison value. Therefore, Gojek would not able to cover up its cheating.


[gojek shared ciphertext]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20s_%7BGojek:i%7D
[//]: # "s_{Gojek:i}"



## Design Considerations
[(go to top)]



### Secret from Each Party
[(go to top)]

A secure PSI protocol needs to use a "secret" from each party [[4]](#references). This prevents the sets of either party from being leaked. Consider a scenario where Grab wants to find out what the phone numbers in Gojek's set are. In this scenario, Grab would require Gojek's "secret" to find out the phone numbers. However, since Gojek's "secret" is private and accessible by Gojek only, Grab would hae no way of doing so.

Hence, we have chosen to base our algorithm off the Diffie-Hellman key exchange protocol. In our algorithms, the "secrets" used from each party are the Diffie-Hellman private keys.



### No Collisions
[(go to top)]

In order for the parties to correctly find the common intersection, each phone number must map to a unique cipher text. There are 2 possible areas in our algorithm where collisions could occur &mdash; hashing and encryption.

<ins>Hashing</ins>

Singaporean phone numbers have the format +65 XXXX YYYY and mobile phone numbers start with 8 or 9. There are 19,999,999 possible phone numbers in Singapore. Using the taylor series approximation to the birthday problem, the probability of a collision is approximately ![taylor series approximation] [[5]](#references), where ![n] is the number of unique phone numbers and ![d] is the number of unique hash values. For SHA3-256, there are ![2^255] hash values. In the worst case scenario, using ![hashing d] and maximum ![max n], the probability of collision will be approximately ![hashing collision prob].

[taylor series approximation]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%201-e%5E%7B-%5Cfrac%7Bn%5E%7B2%7D%7D%7B2d%7D%7D
[//]: # "1-e^{-\frac{n^{2}}{2d}}"
[n]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20n 
[//]: # "n"
[d]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20d
[//]: # "d"
[2^255]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%202%5E%7B255%7D
[//]: # "2^{255}"
[hashing d]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20d=2%5E%7B255%7D
[//]: # "d=2^{255}"
[max n]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20n=19,999,999 
[//]: # "n=19,999,999"
[hashing collision prob]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%203.45%20%5Ctimes%2010%5E%7B-63%7D
[//]: # "3.45 \times 10^{-63}"

<ins>Encryption</ins>

Since all the hashed values are primitive roots modulo ![p], all phone numbers can generate up to ![p-1] values when encrypted. In the worst case scenario, using minimum ![encryption d] and maximum ![max n], the probability of collision will be approximately ![encryption collision prob].

[p-1]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20p-1
[//]: # "p-1"
[encryption d]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20d=2%5E%7B1023%7D-1 
[//]: # "d=2^{1023}-1"
[collision probability]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20%5Cleft%20(%202.23%20%5Ctimes%2010%5E%7B-294%7D%20%5Cright%20) 
[//]: # "\left ( 2.23 \times 10^{-294} \right )"
[encryption collision prob]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%202.23%20%5Ctimes%2010%5E%7B-294%7D
[//]: # "2.23 \times 10^{-294}"

<ins>Overall</ins>

For each phone number, the hashing occurs once and the encryption occurs twice, hence:

![collision prob formula]

Therefore, the probability of collision in our algorithm is approximately ![collision prob], which is negligible.

[collision prob formula]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%20Pr(no%5C%20collision)=Pr(no%5C%20collision%5C%20in%5C%20hashing)%20%5Ctimes%20Pr(no%5C%20collision%5C%20in%5C%20encryption)%5E%7B2%7D
[//]: # "3.45 \times 10^{-63}"
[collision prob]: https://latex.codecogs.com/png.image?%5Cdpi%7B100%7D%203.45%20%5Ctimes%2010%5E%7B-63%7D
[//]: # "3.45 \times 10^{-63}"



## Limitations
[(go to top)]

### Computational Time Trade-Off
[(go to top)]

For our algorithms, the higher the version number, the more secure the algorithm. However, this comes at the cost of computation time. The more secure algorithms require more computations and steps, and hence will require more time. 



### Guessing Phone Numbers
[(go to top)]

Consider a scenario where Grab suspects that Gojek has the phone number 9999 9999. Grab could add this phone number to its input set. If the number appears in the intersection set, Grab would know definitely that Gojek has the phone number 9999 9999. Grab would be able to this for any phone number it suspects is in Gojek's set. Similarly, Gojek would also be able to do the same. 

Unfortunately, none of our algorithms would be able to stop the parties from trying to guess phone numbers from the other party's set. In order for this problem to be solved, a credible and trusted third party organisation would have to be involved [[6]](#references). The third party organisation would have to verify that both parties only include their own phone numbers in their input sets.



### Sending Wrong Encryption Values
[(go to top)]

Consider a scenario where Grab wants to hide the set intersection values from Gojek. When Grab is supposed to send the values of ![gojek shared ciphertext], it instead sends randomly generated nonsense. As a result, Gojek does not find any common ciphertext and wrongly concludes that there is no intersection between the sets. In such a scenario, Gojek would have no way of finding out this occurred, as the values encrypted by Grab would appear pseudo-random to Gojek.

Unfortunately, none of our algorithms would be able to stop the parties from doing this or even detecting that the parties have done it. However, in our research we have found an algorithm by Meadows [[7]](#references) which addresses this problem. To briefly summarise the algorithm, each phone number is hashed to a special value with a unique property, and this special value is used in the Diffie-Hellman exchange. Additionally, the exchange also uses tags. With these two factors combined, it becomes computationally infeasible to find false values and corresponding tags that satisfy the unique property. Hence, this algorithm could be used to prevent parties from sending nonsense values.


## External Libraries
[(go to top)]

### Cryptographic libraries

#### cryptography
`cryptography.hazmat.primitives.asymmetric.dh.generate_parameters(generator,key_size)` 
used to generate random safe prime for prime modulus
* [documentation](https://cryptography.io/en/latest/hazmat/primitives/asymmetric/dh/#group-parameters)

`cryptography.fernet.Fernet` used for symmetric key encryption  
* [documentation](https://cryptography.io/en/latest/fernet/)
* `Fernet` is a high level symmetric encryption recipe provided by `cryptography`
* `Fernet` uses:
  * `AES` in `CBC` mode with a `128-bit key` for encryption; using `PKCS7` padding
  * `HMAC` using `SHA256` for authentication
  * `os.urandom()` for generation of IV
  * [source](https://cryptography.io/en/latest/fernet/#implementation)  

#### hashlib
`hashlib.sha256` used for hashing values
* [documentation](https://docs.python.org/3/library/hashlib.html#hash-algorithms)
* `hashlib` uses `SHA3` from `OpenSSL 1.1.1` and newer
  * [source](https://docs.python.org/3/library/hashlib.html#hash-algorithms)  


#### sympy
`sympy.ntheory.primetest.isprime(n)` used to check if n is prime
* [documentation](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.primetest.isprime)
* For n larger than 2<sup>64</sup>, a strong BPSW test is performed. 
  * [source](https://docs.sympy.org/latest/modules/ntheory.html#sympy.ntheory.primetest.isprime)  

#### secrets
`secrets.randbits(size)` used to generate random numbers of bit-size 1024 for clients' private secret
* [documentation](https://docs.python.org/3/library/secrets.html#random-numbers)
* `secrets.randbits(size)` is a cryptographically secure random number generator


### Other libraries
[(go to top)]

#### random
`random.sample()` used to generate phone numbers and invalid phone numbers
* This usage does not require the generator to be cryptographically secure  

`random.shuffle()` used to shuffle the phone numbers
* This usage does not require the generator to be cryptographically secure

#### pandas
`pandas` used for displaying data as well as reading and writing to `.csv` files

### Installation
cryptography
```
pip install cryptography
```
hashlib
```
pip install hashlib
```
sympy
```
pip install sympy
```
secrets
```
pip install secrets
```
pandas
```
pip install pandas
```

## Other Development Tools
Programming Language: python  
IDE: Jupyter notebook  
Other softwares: Github


## References
[(go to top)]

[1] W. Clark, "Private set intersection with Diffie-Hellman," *OpenMined Blog*, July. 9, 2020. [Online]. Available: https://blog.openmined.org/private-set-intersection-with-diffie-hellman/. [Accessed: Nov. 27, 2021].

[2] "Primitive Elements," 2008. [Word document]. Available: http://homepages.math.uic.edu/~leon/mcs425-s08/handouts/PrimitiveElements.pdf. [Accessed: Nov. 27, 2021].

[3] K. B. Tay, "PKC & Diffie-Hellman Key Exchange", presented to CZ4010 Applied Cryptography, Nanyang Technological University, Singapore, Oct. 8, 2021. [PowerPoint slides]. [Accessed: Nov. 27, 2021].

[4] "Special Topics on Privacy and Public Auditability &mdash; Event 2: A Brief Overview of Private Set Intersection," *CSRC | NIST*, Apr. 19, 2021. [Online]. Available: https://csrc.nist.gov/presentations/2021/a-brief-overview-of-private-set-intersection. [Accessed: Nov. 27, 2021].

[5] "Approximations," *Wikipedia*. [Online]. Available: https://en.wikipedia.org/wiki/Birthday_problem#Approximations. [Accessed: Nov. 27, 2021].

[6] J. Camenisch and G. M. Zaverucha, ???Private Intersection of Certified Sets,??? in *Financial Cryptography and Data Security*, Berlin, Heidelberg: Springer Berlin Heidelberg, pp. 108???127. doi: 10.1007/978-3-642-03549-4_7.

[7] C. Meadows, "A More Efficient Cryptographic Matchmaking Protocol for Use in the Absence of a Continuously Available Third Party," in *1986 IEEE Symposium on Security and Privacy*, 1986, pp. 134???134. doi: 10.1109/SP.1986.10022.

[(go to top)]: #team-g-jjs-private-set-intersection-algorithm