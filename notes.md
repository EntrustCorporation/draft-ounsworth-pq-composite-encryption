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

Needs to mention keyUsage of dataEncryption.




EncryptedContentInfo ::= SEQUENCE {
        contentType ContentType,
        contentEncryptionAlgorithm ContentEncryptionAlgorithmIdentifier,
        encryptedContent [0] IMPLICIT EncryptedContent OPTIONAL }

      EncryptedContent ::= OCTET STRING


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

The Composite-OR encryption mode allows an encryptor (EDNOTE: what is the standard antonym of recipient in CMS lingo?) to encrypt a message using a subset of the recipient's component public keys.

EDNOTE: should the Composite-OR encryption mode only be allowed when the recipient's public key is of type Composite-OR, or should it be allowed for either type of composite public keys? I'm inclined to do this because an EE with a Composite pub key would not be happy if sensitive data was sent to it encrypted with a weak subset of its public keys.

You may omit a cipher text by placing a null in the SEQUENCE

Modify the generation process to indicate what you do with a null shared secret.

A Composite-OR signature MUST NOT be entirely null; it must contain at least one valid signature.

The design intent of this mode is to support migration scenarios where the encryptor does not support all component algorithms in the recipient's composite public key. Note that the encryptor must select  unlike Composite-OR signatures [I-D.draft-ounsworth-pq-composite-sigs], the recipient 


 an end entity has been issued keys on algorithms that either itself or the peer with which it is communicating do not (yet) support. This design allows for both the mode where the signer omits signatures that it knows its peer cannot process in order to save bandwidth and performance, and the mode where it includes all component signatures and allows the verifier to choose how many to verify. The latter is RECOMMENDED for signatures that need both sort-term backwards compatibility as well as long-term security.


## Key encryption

Copy from CMS (RFC 5652):

6.4.  Key-encryption Process

   The input to the key-encryption process -- the value supplied to the
   recipient's key-encryption algorithm -- is just the "value" of the
   content-encryption key.

   Any of the aforementioned key management techniques can be used for
   each recipient of the same encrypted content.

## Composite Decryption Process

Copy the process from above and run it backwards.

## Composite-OR Decryption Process
 
Copy the process from above and run it backwards.