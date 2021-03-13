Updates: rfc4210 (CMP), rfc4211 (CRMF), rfc5652 (CMS), 

# Abstract
This draft defines a content encryption process following the hybrid model as described in the NIST Post-Quantum Crypto FAQ. This draft defines id-composite-encryption which encrypts for a recipient with a composite public key by generating a shared secret for each of the recipient's component public keys and combining them into a single content encryption key using NIST SP 800-56Cr2.



# Composite Encryption and Decryption

   The composite content-encryption algorithm supports encryption and decryption
   operations.  The encryption operation maps an octet string (the
   plaintext) to another octet string (the ciphertext) under control of
   a content-encryption key formed by combining multiple shared secrets each encrypted for a different recipient component public key.
   The decryption operation is the inverse of the encryption operation.  Context determines which operation is
   intended.

## Composite Encryption Process

Write this out as a proper process with `for i` loops:

Input: A1, A2, .., An ::= A list of recipient public key algorithms
       P1, P2, .., Pn ::= A list of recipient public keys
       ContentEncryptionAlg ::= a content encryption algorithm selected by the protocol.


1. Generate a shared secret for each recipient component key (EDNOTE: length?)
2. Encrypt each shared secret according to the algorithm specification.
3. Derive an encryption key according to NIST SP 800-5Cr2 with Z' = SS1 || SS2 || .. || SSn. Or alteranitively, Z = SS1 and T = SS2 || .. || SSn.
4. Encrypt the content using the key derived in step 3 and the content encryption algorithm selected by the protocol.


EDNOTE: what bit length for each component secret? I suppose it's easy to make each secret the same size as the final key.


## Composite-OR Encryption Process
You may omit a cipher text by placing a null in the SEQUENCE

Modify the generation process to indicate what you do with a null shared secret.

## Composite Decryption Process

Copy the process from above and run it backwards.

## Composite-OR Decryption Process
 
Copy the process from above and run it backwards.