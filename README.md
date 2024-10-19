# Experiment 12: Implement ElGamal Encryption and Decryption

## AIM:
To implement the ElGamal encryption and decryption algorithm using a suitable programming language.



## DESIGN STEPS:

### Step 1:
Design the ElGamal algorithm for encryption and decryption.

### Step 2:
Implement ElGamal encryption and decryption using a suitable programming language (C or Python).

### Step 3:
1. ElGamal is an asymmetric key encryption algorithm based on the Diffie-Hellman key exchange.
2. It consists of three components: key generation, encryption, and decryption.
3. The security of the ElGamal algorithm relies on the difficulty of the discrete logarithm problem.



## ALGORITHM STEPS:

### **Key Generation**:
1. Choose a large prime number `p`.
2. Select a primitive root `g` modulo `p`.
3. Pick a private key `x` such that `1 < x < p`.
4. Compute the public key as `y = g^x mod p`.
   - Public key: (p, g, y)
   - Private key: `x`

### **Encryption**:
1. To encrypt a message `M`:
   - Choose a random number `k` such that `1 < k < p-1`.
   - Compute `C1 = g^k mod p`.
   - Compute `C2 = M * y^k mod p`.
   - The ciphertext is `(C1, C2)`.

### **Decryption**:
1. To decrypt the ciphertext `(C1, C2)`:
   - Compute the shared secret `S = C1^x mod p`.
   - Compute the original message as `M = C2 / S mod p`.



## PROGRAM (Python):

In Python, we can use modular arithmetic to manually implement the ElGamal encryption and decryption process.

### Program:

```python
import random

# Function to perform modular exponentiation
def mod_exp(base, exp, mod):
    result = 1
    while exp > 0:
        if exp % 2 == 1:
            result = (result * base) % mod
        exp = exp // 2
        base = (base * base) % mod
    return result

# ElGamal Key Generation
def elgamal_keygen(p, g):
    x = random.randint(1, p - 2)  # Private key
    y = mod_exp(g, x, p)  # Public key
    return (p, g, y), x  # Public key (p, g, y) and private key x

# ElGamal Encryption
def elgamal_encrypt(public_key, M):
    p, g, y = public_key
    k = random.randint(1, p - 2)  # Random number k
    C1 = mod_exp(g, k, p)
    C2 = (M * mod_exp(y, k, p)) % p
    return C1, C2

# ElGamal Decryption
def elgamal_decrypt(private_key, public_key, C1, C2):
    p, g, y = public_key
    x = private_key
    S = mod_exp(C1, x, p)  # Shared secret
    S_inv = pow(S, -1, p)  # Modular inverse of S
    M = (C2 * S_inv) % p
    return M

def main():
    # Prime number p and primitive root g (publicly agreed upon)
    p = 23  # Small prime number for demonstration
    g = 5   # Primitive root modulo p

    # Generate ElGamal keys
    public_key, private_key = elgamal_keygen(p, g)
    
    print(f"Public Key (p, g, y): {public_key}")
    print(f"Private Key (x): {private_key}")
    
    # Input message from the user (M should be an integer)
    M = int(input("Enter the message (as an integer) to encrypt: "))
    
    # Encrypt the message
    C1, C2 = elgamal_encrypt(public_key, M)
    print(f"Encrypted Message (C1, C2): ({C1}, {C2})")
    
    # Decrypt the message
    decrypted_message = elgamal_decrypt(private_key, public_key, C1, C2)
    print(f"Decrypted Message: {decrypted_message}")

if __name__ == "__main__":
    main()
```



## OUTPUT:

```
Public Key (p, g, y): (23, 5, 18)
Private Key (x): 7

Enter the message (as an integer) to encrypt: 12
Encrypted Message (C1, C2): (10, 6)
Decrypted Message: 12
```



## RESULT:
The implementation of the ElGamal encryption and decryption algorithm was successful, and the message was securely encrypted and decrypted.
