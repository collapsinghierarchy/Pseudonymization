# Pseudonymization

## Introduction

The following reasoning is derived from current technical reports by ENISA, the European Data Protection Board (EDPB), the International Organization for Standardization (ISO), and our extensive experience in designing cryptographic protocols for both industrial and academic use cases.

### Relevant Resources

- [Pseudonymisation Techniques and Best Practices](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices)
- [Data Pseudonymisation: Advanced Techniques and Use Cases](https://www.enisa.europa.eu/publications/data-pseudonymisation-advanced-techniques-and-use-cases)
- [Recommendations on shaping technology according to GDPR provisions](https://op.europa.eu/en/publication-detail/-/publication/0e1ca64f-29c7-11e9-8d04-01aa75ed71a1/language-en)
- [Guidelines 01/2025 on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en)
- [Deploying Pseudonymisation Techniques](https://www.enisa.europa.eu/publications/deploying-pseudonymisation-techniques)
- [ISO 25237:2017 Health informatics — Pseudonymization](https://www.iso.org/obp/ui/#iso\:std\:iso:25237)
- [ISO/IEC 20889:2018 Privacy enhancing data de-identification terminology and classification of techniques](https://www.iso.org/obp/ui/#iso\:std\:iso-iec:20889)

### Disclaimer

**Disclaimer:** Our solutions assume that the company wishing to implementing pseudonymization already has a secure and properly functioning authorization system in place for its employees. This is a prerequisite security measure and is usually achieved by some kind of Single-Sign-On (SSO).Ideally, this system should include enhancements such as two-factor authentication (2FA) that our solutions can rely on. Lastly, pseudonymization is not a substitute for lawful data processing; Pseudonymization does not make data processing lawful if it was otherwise unlawful. It is a tool to enhance data protection, not a justification for processing personal data without a valid legal basis.

## What is Pseudonymization?

Pseudonymization is a data protection technique that replaces identifiable information within a dataset with pseudonyms. This process reduces the risk of re-identification while allowing the data to remain useful for analysis and processing. According to the GDPR (Article 4(5)), pseudonymization is defined as:

> *"The processing of personal data in such a manner that the personal data can no longer be attributed to a specific data subject without the use of additional information, provided that such additional information is kept separately and is subject to technical and organizational measures to ensure that the personal data are not attributed to an identified or identifiable natural person."*

There are also other definitions available from ISO/IEC 20889 and ISO 25237 standards. However, all definitions share the essence of protecting personal data while keeping it usable. These definitions, while valuable, are not formal in the sense of provable security; they require proper risk analysis and knowledge of state-of-the-art pseudonymization techniques for effective implementation.
The following should help you in the orientation, what the current state of the art of the available tools is.
## Difference Between Pseudonymization and Anonymization

While both pseudonymization and anonymization aim to protect personal data, they differ in their approach and outcomes:

- **Pseudonymization:** Replaces identifiable information with pseudonyms, allowing the data to be re-identified with additional information. It is reversible under specific conditions.
- **Anonymization:** Irreversibly removes all identifiable information, making it impossible to re-identify the data subject. Anonymized data falls outside the scope of the GDPR.

The current [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en) from the EDPB add some clarifications on the distinction of both concepts. 

Paragraph 22 states: 

> *Pseudonymised data, which could be attributed to a natural person by the use of additional
information, is to be considered information on an identifiable natural person,7 and is therefore
personal. This statement also holds true if pseudonymised data and additional information are
not in the hands of the same person. If pseudonymised data and additional information could be
combined having regard to the means reasonably likely to be used by the controller or by another
person, then the pseudonymised data is personal. Even if all additional information retained by
the pseudonymising controller has been erased, the pseudonymised data becomes anonymous
only if the conditions for anonymity are met*

One possible interpretation is that "pseudonymous" data, even if it meets "the contitions for anonymity" is still personal if there is the possibility for reversibility.
Therefore, it seems that the reversibility is the only condition that distinctly separates both concepts. 

## Pseudonymization Architectures

The pseudonymization techniques described in the next section are, first and foremost, tools without inherent context. Their deployment can vary significantly based on the environment and the specific requirements they are designed to address. Proper implementation should consider factors such as security and the intended data flow within the system.

The current [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en) describe two important application-agnostic architectures for deploying these techniques:

- **Proxy Pseudonymization:** Data is securely transmitted to a designated organizational unit within the controller’s domain responsible for pseudonymization. This unit transforms the data accordingly and configures a proper and secure de-pseudonymization mechanism (if required). The transformed data is then sent to its intended endpoint, where it is processed in its pseudonymized form. If necessary, de-pseudonymization is performed under strict access control policies, ensuring minimal exposure of sensitive information.
  
- **Source Pseudonymization:** Data is pseudonymized at the point of origin before transmission. This method enhances security by preventing the transmission of raw, identifiable information. In many cases, **delegated pseudonymization** techniques are used, where a trusted intermediary or cryptographic mechanism ensures that only authorized parties can access the original data. 

Both architectures require robust key management, secure transmission channels, and strict access controls to ensure compliance with privacy regulations such as the GDPR. Therefore, choosing the right tool is a minor challenge compared to designing a secure and well-structured architecture around it.

## Overview of Pseudonymization Techniques

In [Technical Guidance on Pseudonymisation](https://op.europa.eu/en/publication-detail/-/publication/0e1ca64f-29c7-11e9-8d04-01aa75ed71a1/language-en), it is stated:

> *"[...] not all pseudonymisation techniques are equally effective and possible practices vary from simple scrambling of identifiers to sophisticated techniques based on advanced cryptographic mechanisms. Although many of these techniques would fall under the broad definition of pseudonymisation, they would not offer the same level of protection for personal data. In fact, in certain cases, poor pseudonymisation techniques might even increase the risks for the rights and freedoms of data subjects, giving a false sense of protection. It is essential therefore, to explore the availability of existing solutions, together with their strengths, as well as their limitations."*

We have explored the availability of such tools during our many years of scientific work in cryptology and applied them within industrial scenarios. The application of these tools requires a proper risk-driven reasoning; merely knowing the tools is not enough. The combination of risk analysis methodology with pseudonymization techniques is what makes them effective. Eventually, you should not forget to properly document the deployed pseudonymization measures, which serves as your proof that you applied the data minimization and privacy-by-default principles to their full extent. If you are feeling versed enough in the deployment and design of cryptographic schemes, then you might use the following overview for yourself. Otherwise, it is highly recommended that you consult someone that is versed enough, like us, to help you with your problem. 

### Categories of Pseudonymization Techniques

Overall, pseudonymization techniques can be broadly categorized into two types:

- **Regular Pseudonymization:** Involves replacing identifiable data with pseudonyms using symmetric cryptographic techniques. 
- **Delegated Pseudonymization:** Involves outsourcing delegation of the pseudonymation process to someone else. Typically, the data subjects themselves or a third party holding the data. These methods involve the use asymmetric cryptographic techniques. Usually, these techniques are part of the Privacy Enhancing Technology (PET) or the Privacy Enhancing Cryptography (PEC) [NIST](https://csrc.nist.gov/projects/pec) family.

### Regular Pseudonymization
The following describes the use of different methods from the symmetric cryptography as being argued in [Pseudonymisation Techniques and Best Practices](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices)

#### **Hashing Without Key**

Hashing is a cryptographic technique that transforms an input (e.g., an identifier) into a fixed-size output (hash value). The same input always produces the same hash value. Typically, cryptographic hash functions are designed to be collision-resistant, ensuring that no two different identifiers hash to the same pseudonym. However, for pseudonymization, collision resistance is often less critical than one-wayness (irreversibility).
  
The one-way property means that, given a hash, it should be computationally infeasible to retrieve the original identifier. However, if the original identifiers are easily guessable (e.g., small sets like email addresses or sequential user IDs), an attacker can precompute hashes (e.g., using rainbow tables) or brute-force them.

  **Example Hash Functions**: SHA-256, SHA-3, BLAKE2

- **Strengths**:
  - Ensures data integrity (the hash will always produce the same output for a given input).

- **Weaknesses**:
  - **Vulnerability to Rainbow Tables & Brute-Force Attacks**: Attackers can precompute hashes for common values and look them up in a table to reverse the hash.
  - **Lack of Unlinkability**: Since the same identifier always produces the same hash, different systems using the same hashing technique will generate identical pseudonyms, making it easy to link data across domains.

- **Use Case**:
  - Can be used in low-risk scenarios where identifiers are not easily guessable or when uniqueness verification is required.

#### **Hashing with Salt**

A salt is a randomly generated value added to the identifier before hashing. This ensures that the same identifier will not always produce the same hash, thereby preventing rainbow table attacks. Unlike keys, salts are not secret but should be unique per identifier or at least per system.

  **Example Hash Functions**: PBKDF2, bcrypt, Argon2

- **Strengths**:
  - **Defends Against Rainbow Tables**: Because each identifier gets a unique salt, precomputed hash tables (rainbow tables) are ineffective.
  - **Improved Unlinkability**: Even if two systems hash the same identifier, their outputs will differ if they use different salts.

- **Weaknesses**:
  - **Storage Overhead**: Each pseudonym must be stored along with its corresponding salt to allow future verification.
  - **Security Depends on Salt Implementation**: If the salt is too short or not properly random, it may not provide sufficient protection.

- **Use Case**:
  - Recommended when unlinkability is desirable but reversibility has to be managed in form of a lookup table.

#### **HMAC (Hashing with a Key)**

A Hash-based Message Authentication Code (HMAC) uses a secret key in conjunction with a cryptographic hash function. Instead of simply hashing an identifier, HMAC ensures that the output is tied to a specific key. Without knowing this key, it is computationally infeasible to regenerate the same pseudonym.

  **Example Hash Functions**: HMAC-SHA256, HMAC-SHA512, HMAC-BLAKE2

- **Strengths**:
  - **Prevents Unauthorized Reversibility**: Unlike simple hashing, an attacker cannot regenerate hashes without access to the secret key.
  - **Controlled Reversibility**: The system owner can re-identify data by keeping the key secure, but unauthorized entities cannot.

- **Weaknesses**:
  - **Key Management Complexity**: The secret key must be securely stored and periodically rotated to prevent compromise.
  - **Potential for Insider Attacks**: If an insider gains access to the key, they can reverse the pseudonymization.

- **Use Case**:
  - Suitable for scenarios where controlled reversibility is required, such as when tracking users internally without exposing their identifiers externally. We deploy these solutions usually in combination with an HSM.

#### **Symmetric Encryption**
Unlike cryptographic hash functions, encryption techniques provide a fundamentally different type of security. While hashing ensures one-way irreversibility, encryption allows for bidirectional transformation—ciphertext can be decrypted back to plaintext using the correct key. This distinction affects both the cryptographic foundations and the pseudonymization use cases.

##### **Deterministic Encryption**

Deterministic encryption is among the weakest forms of symmetric encryption in terms of confidentiality because it always produces the same ciphertext for a given plaintext and key. This consistency enables efficient lookups but introduces significant security risks, including **pattern leakage** and **linkability** across datasets. If used without proper safeguards, deterministic encryption can severely weaken data protection, making it susceptible to statistical and frequency analysis attacks.

- **Example Algorithms**:
  - AES in ECB mode (not recommended due to lack of diffusion)
  - AES in deterministic CTR mode (provides better security but still prone to pattern analysis)

- **Strengths**:
  - **Reversibility**: The original identifier can be retrieved using the correct decryption key.
  - **Consistency**: Enables deterministic lookups without requiring additional metadata, making it useful for applications requiring exact matching.

- **Weaknesses**:
  - **Linkability & Pattern Leakage**: Since the same plaintext always encrypts to the same ciphertext, attackers can identify recurring patterns and infer relationships across datasets.
  - **Vulnerability to Frequency Analysis**: If the plaintext has a predictable distribution (e.g., common names, email domains), attackers can analyze encrypted values to make educated guesses about their meaning.
  - **Key Management Complexity**: The encryption key must be securely stored, rotated, and access-controlled to prevent unauthorized decryption.

- **Use Case**:
  - **Searchable Encryption**: When deterministic consistency is required to enable encrypted lookups while preserving confidentiality.
  - **Regulated Environments with Strict Lookup Needs**: Certain compliance frameworks (e.g., some financial and healthcare regulations) may allow deterministic encryption under strict access controls.
  - **Tokenization & Data Masking**: When data must be consistently pseudonymized while maintaining usability for indexing or analytics.

**Security Considerations:**
Deterministic encryption should only be used in scenarios where confidentiality risks are explicitly mitigated, such as when access to the encrypted data is tightly controlled or combined with additional cryptographic safeguards (e.g., format-preserving encryption with key diversification). For most privacy-sensitive applications, probabilistic or searchable encryption schemes provide stronger security guarantees.


#### **Format-Preserving Encryption (FPE)**

Format-preserving encryption (FPE) retains the structure of the original identifier (e.g., encrypting a credit card number into another valid-looking number). Unlike conventional encryption schemes designed for securing data-in-transit (e.g., in the [TLS standard](https://datatracker.ietf.org/doc/html/rfc8446)), FPE modifies the mode of operation of a block cipher, such as AES, to ensure that encrypted values conform to predefined formats.

While FPE provides format consistency, it is a relatively recent cryptographic development compared to symmetric encryption primitives that have undergone extensive cryptanalysis over multiple decades. The **FF1 and FF3** schemes in **NIST Special Publication 800-38G** represent the first formalized standards for FPE. However, they have not yet undergone the same level of cryptanalytic scrutiny as standard AES modes. This raises concerns regarding their long-term security and the risk of unforeseen vulnerabilities.

The evolution of symmetric encryption history—from the **breaks of DES (1978) and 3DES (1996)** to the adoption of **AES (2001)** as the state-of-the-art standard—suggests that caution is warranted when deploying FPE. **Improper parameterization can lead to trivial identifier recovery attacks**, making cryptographic expertise essential for secure implementation. **FPE is not a direct replacement for traditional encryption methods**, and its use should be carefully considered based on the threat model and application context.

- **Example Algorithms**: [FF1 and FF3 (NIST-approved FPE modes of AES)](https://csrc.nist.gov/pubs/sp/800/38/g/upd1/final)

- **Strengths**:
  - **Preserves Data Format**: Eliminates the need to modify database schemas or data validation processes. No changes of applications working with the modified database are required.
  - **Reversibility**: Allows recovery of the original identifier when the correct decryption key is used.

- **Weaknesses**:
  - **Error-Prone Parameterization**: Securely deploying FPE requires cryptographic expertise, as improper configurations can lead to trivial plaintext recovery attacks.
  - **Linkability**: If the same encryption key and tweak value are reused across multiple systems, identical plaintexts will yield identical ciphertexts, making them linkable.

- **Use Case**:
  - Suitable for applications requiring pseudonymization while maintaining data format, such as tokenization in payment processing. Particularly beneficial when security enhancements are needed without modifying application logic, allowing encryption to be applied at the database level without affecting existing business logic.


#### **CCA-Secure Symmetric Encryption (Authenticated Encryption)**

Probabilistic encryption methods represent the current state-of-the-art in encryption. In most cases, **Chosen Ciphertext Attack (CCA)-secure encryption** is the most secure and **fully sufficient choice** for protecting sensitive data.

However, the enhanced security guarantees come with **three functional drawbacks**:
1. **Loss of Determinism**: Due to the probabilistic nature of these encryption methods, encrypting the same identifier twice with the same key results in **unlinkable pseudonyms/ciphertexts**. This means that deterministic encryption’s functional advantage—enabling exact lookups—is lost.
2. **Loss of Format Preservation**: Because of semantic security guarantees, no structural patterns of the encrypted identifier are leaked. While this is a security benefit, it eliminates the functional advantage of **format-preserving encryption (FPE)**, which is sometimes needed in constrained database systems.
3. **Loss of Malleability**: The ciphertext is **unmalleable**—a significant security advantage—but at the cost of eliminating **homomorphic properties**. Homomorphic encryption allows computation on encrypted data without decryption, a feature that is **not** available with CCA-secure encryption.

Due to its **strong security guarantees**, CCA-secure encryption should **always be the default choice**. Alternative encryption methods, such as **deterministic encryption, format-preserving encryption, or homomorphic encryption**, should only be considered when justified by strong technical or functional requirements.

- **Example Algorithms**: AES-GCM, AES-CCM, ChaCha20-Poly1305

- **Strengths**:
  - **Integrity Protection**: Ensures that any tampering with encrypted data is detected.
  - **Unlinkability**: Different encryptions of the same plaintext yield different ciphertexts, preventing correlation attacks.
  - **Semantic Security**: No meaningful information about the plaintext is leaked.
  - **Reversibility**: The original plaintext can be decrypted using the key.
  - **Well-Established Security**: These methods have undergone decades of cryptanalysis and scrutiny, with widespread implementation in **developer-friendly standardized APIs**.
  - **Performance Optimization & Hardware Support**: Many authenticated encryption algorithms are optimized for efficiency and benefit from **hardware acceleration** in modern CPUs.

- **Weaknesses**:
  - **Key Management Complexity**: Encryption keys must be securely stored, rotated periodically, and properly managed.
  - **Lack of Post-Encryption Processing**: Ciphertexts are **dead-end pseudonyms**, meaning further cryptographic processing (e.g., homomorphic computations) is not possible.

- **Use Case**:
  - **Recommended for securing sensitive data** in storage and transit. It is best practice to **encrypt persistently stored data** using these methods and to **decrypt or depseudonymize it only on a need-to-know basis** for an authorized set of recipients.
  - **Key Management Best Practices**: Secure key management can be effectively handled with a **Hardware Security Module (HSM)** combined with **two-factor authentication (2FA)** to strengthen existing access control mechanisms within an organization.

#### **Searchable Encryption**
Compared to the previously discussed encryption methods, **searchable encryption introduces a significant increase in algorithmic complexity**. While the prior methods function as **cryptographic primitives**—describing basic encryption/decryption operations—**searchable encryption incorporates multiple mechanisms** beyond simple transformation functions.

This increased complexity results in **security definitions that are not directly comparable to traditional symmetric encryption methods**. Searchable encryption enables **querying over encrypted data** without requiring full decryption, allowing **keyword searches or structured queries while maintaining data confidentiality up to a certain degree**, as defined by the scheme’s **leakage function**.

There are variations, such as **Order-Preserving Encryption (OPE)** and **Oblivious RAM (ORAM)**, that can be used for similar purposes. However, these techniques currently **exist only in academic research or in prototype implementations**, and detailing their distinctions is beyond the scope of this section.

Additionally, **searchable encryption is much newer than Format-Preserving Encryption or Homomorphic Encryption** and is currently **primarily used in academic research or limited proof-of-concept implementations within companies**. There are **no standardized protocols or security standards** available yet, nor are there **any formal draft proposals for standardization**. 

**We strongly advise against using searchable encryption for pseudonymization without cryptographic expertise, thorough prototyping, and rigorous security evaluations.** If applied incorrectly, security guarantees **may not hold**, and the **resources spent on implementation may be wasted** due to vulnerabilities or inefficiencies. 

That being said, this cautionary stance is **not** due to any inherent flaw in the concepts or potential of searchable encryption. On the contrary, its **strengths and use cases** are highly promising for enhancing security in various applications. However, in our assessment, **the technology is not yet mature enough for deployment in production systems**. 

- **Example Algorithms**: Symmetric Searchable Encryption (SSE), Order-Preserving Encryption (OPE), Oblivious RAM (ORAM)

- **Strengths**:
  - **Controlled Searchability**: Search access can be restricted based on cryptographic query mechanisms.
  - **Reduced Decryption Exposure**: Only the retrieved search results need to be decrypted, minimizing data exposure.

- **Weaknesses**:
  - **Leakage Risks**: Many searchable encryption schemes leak some information, such as **query patterns, access patterns, or partial order information**. This makes them susceptible to **inference attacks**, where an adversary can deduce relationships between queries and ciphertexts.
  - **Performance Overhead**: Searching over encrypted data requires **specialized cryptographic operations**, which introduce computational and storage overhead compared to plaintext search.
  - **Limited Query Expressiveness**: Most practical searchable encryption schemes support **only exact keyword matches or limited boolean queries**, making complex searches less efficient or infeasible.

- **Use Case**:
  - **Encrypted Databases**: Suitable for **searching over encrypted records** in cloud storage or privacy-sensitive environments where full decryption is not feasible.
  - **Confidential Cloud Storage**: Used in settings where sensitive data is stored remotely and must be queried securely.
  - **Legal and Compliance-Driven Applications**: Useful when regulatory frameworks require **confidentiality-preserving search capabilities** while maintaining access controls.

**Security Considerations:**
While **searchable encryption provides enhanced privacy**, its **leakage risks must be carefully evaluated**. Stronger techniques, such as **Oblivious RAM (ORAM) or differential privacy-enhanced search**, may be required for **high-security applications where adversaries are assumed to have significant query monitoring capabilities**. **Organizations must carefully balance efficiency, security, and usability when implementing searchable encryption solutions.**


### **Delegated Pseudonymization**
---
This section reflects a **controversial stance by ENISA**, as it includes several **Privacy-Enhancing Cryptography (PEC) mechanisms** within the scope of **pseudonymization**. This categorization has led to **misinterpretations**, with some viewing it as a redefinition of privacy-enhancing cryptographic methods as **delegated pseudonymization techniques**—placing them alongside traditional, rather weak, methods like **Format-Preserving Encryption (FPE), Deterministic Encryption, Hashing, Masking, and Tokenization**.

From a **scientific cryptographic perspective**, this controversy is understandable. However, from a **practical security standpoint**, the classification does not alter the **essential role of these methods**. **Fully Homomorphic Encryption (FHE) and Multi-Party Computation (MPC)** remain among the **most advanced and secure techniques for protecting personal data while enabling computational operations**.

This terminology shift may **create confusion for practitioners**, making it harder to identify **appropriate security measures for specific use cases**. The term **"pseudonymization"** itself is broad and lacks precise cryptographic definition, further contributing to potential misunderstandings. However, this debate falls beyond the scope of this section. **For fellow cryptographers reading this, consider this an acknowledgment of the controversy rather than an endorsement of the classification.**

---

The main difference in this category is that the transformation of the original data/identifier occurs with the help of publicly available information, which has been provided to the party responsible for performing the pseudonymization, making it a delegated process. Meanwhile, the secret information required for the reverse transformation may be securely stored elsewhere. In the section about FHE, we will see that this is not necessarily always the case.

#### **Asymmetric Encryption (CCA2)**

The fundamental difference between asymmetric encryption and symmetric encryption lies in **key separation**: instead of using a **single secret key** for both encryption and decryption, asymmetric encryption utilizes a **key pair**—a **public key** (which can be freely shared) and a **private key** (which must remain secret). This simple yet powerful change **solved the key distribution problem** that had long plagued symmetric encryption systems. 

Asymmetric encryption forms the backbone of **secure communication protocols** on the internet, including **TLS/HTTPS**, **email encryption (PGP, S/MIME)**, and **cryptographic authentication mechanisms**. 

Although asymmetric encryption provides strong security guarantees, it has an inherent **efficiency drawback**: it is computationally **much slower than symmetric encryption** and is impractical for encrypting large data payloads. Instead, it is typically used to **securely transmit symmetric encryption keys**, a process known as **hybrid encryption**. In hybrid encryption, an asymmetric algorithm encrypts a small **session key**, which is then used for efficient symmetric encryption of the actual data.

Due to its **strong security guarantees**, **CCA2-secure (Chosen Ciphertext Attack-secure) asymmetric encryption should always be the default choice** when using public-key encryption. Alternative asymmetric encryption methods should only be considered when justified by strong technical or functional requirements.

- **Example Algorithms**:
  - **Traditional Public-Key Encryption**: RSA-OAEP and the Elliptic Curve Cryptography variants. These mechanism currently undergo a world-wide phase-out due to being vulnerable to quantum computer cryptoanalysis. It is strongly advised, if possible, not to use them anymore.
  - **Modern Lattice-Based PQC Algorithms**:ML-KEM, ML-DSA an others.
  
- **Strengths**:
  - **Public-Key Distribution**: Eliminates the need for a pre-shared secret key, solving the key distribution problem of symmetric encryption.
  - **Strong Security Guarantees**: When implemented properly, CCA2-secure asymmetric encryption ensures confidentiality, integrity, and semantic security.
  - **Hybrid Encryption Support**: Efficiently integrates with symmetric encryption to provide secure key exchange mechanisms.
  
- **Weaknesses**:
  - **Key Management Complexity**: Private keys must be **securely stored, rotated periodically**, and protected from exposure.
  - **Computational Overhead**: Requires significant processing power for key generation and encryption/decryption compared to symmetric encryption.
  - **Lack of Post-Encryption Processing**: Ciphertexts are **dead-end pseudonyms**, meaning further cryptographic processing (e.g., homomorphic computations) is not possible.
  
- **Use Case**:
  - **Key Exchange & Secure Communication**: Used in TLS (HTTPS) for securely exchanging symmetric keys over untrusted networks.
  - **Digital Signatures & Authentication**: Provides verifiable message integrity and authenticity in cryptographic protocols.
  - It is best used to encrypt the personal data directly at it's source and transport and store the data in encrypted form, while the decryption can happen on-demand on the need-to-know basis.


**Key Management Best Practices**:
- **Private key protection is critical**—use **Hardware Security Modules (HSMs)** or **secure enclaves**.
- Consider using **hybrid encryption** (asymmetric for key exchange, symmetric for bulk data encryption) to balance **security and efficiency**.
- Stay updated with **PQC standardization efforts** (NIST PQC project) and prepare for a post-quantum transition.

#### **(Fully) Homomorphic Encryption (FHE)**
Homomorphic Encryption (HE) schemes fall under **asymmetric cryptography**, although they often introduce additional cryptographic components such as **evaluation keys, relinearization keys, or Galois keys** to enable computations on ciphertexts. Unlike traditional **CCA2-secure asymmetric encryption**, where ciphertexts are immutable, homomorphic encryption schemes produce **malleable ciphertexts**, meaning encrypted values can be manipulated without decryption. While this might initially appear to be a **security weakness**, it is actually a functional enhancement that enables **encrypted computations**, meaning ciphertexts are **not dead-end pseudonyms**.

This reduction in traditional security guarantees can be mitigated through **organizational security measures**, such as **limiting access to ciphertexts**, enforcing **strong authentication mechanisms**, and defining **strict authorization policies** that regulate **who can decrypt data, when decryption is allowed, and what results can be communicated back**. When these measures are applied, **HE remains secure in an honest-but-curious model**, ensuring that only trusted (but curious) parties can interact with encrypted data.

With basic homomorphic encryption, **a wide range of data processing tasks can be performed on encrypted data** without modifying existing processing pipelines, while maintaining end-to-end encryption.

**Fully Homomorphic Encryption (FHE)** extends this functionality to allow **arbitrary computations** on encrypted data, producing an encrypted result that, when decrypted, matches the outcome of the same computation performed on plaintext. However, **FHE comes with a significant computational overhead**, making optimizations essential for usability. 

For practical applications, it is advisable to **optimize encrypted computations** to ensure efficiency and **avoid unnecessary performance penalties**.

We plan to release a dedicated chapter in the future detailing the various **homomorphic encryption schemes** and their **potential applications** as a (delegated) pseudonymization mechanism.

- **Example Algorithms**: BGV Scheme, BFV Scheme, CKKS Scheme, TFHE, Hybrid Homomorphic Encryption (HHE), Multi-Key Homomorphic Encryption

- **Strengths**:
  - **Semantic Security**: While homomorphic encryption lacks the **CCA2** security guarantees of standard asymmetric encryption due to its malleability, it still provides **CPA-level security**. This makes it a significant security improvement over **deterministic encryption, format-preserving encryption, or searchable encryption**, as it does not leak patterns of encrypted identifiers.
  - **Encrypted Processing**: Data can be processed **without de-pseudonymization**, allowing computations on encrypted values. This provides **the same advantage as searchable encryption**, but **without its leakage vulnerabilities**.

- **Weaknesses**:
  - **Malleability**: The very feature that makes homomorphic encryption useful also poses a security risk. If an **unauthorized party** gains access to ciphertexts, they may manipulate them in a **malicious manner**, potentially **revealing the original plaintext** in the worst-case scenario. However, **deleting the secret key renders any modified ciphertext useless**, mitigating potential breaches.
  - **Computational Overhead**: HE operations are **several orders of magnitude slower** than standard computations. However, this inefficiency can be **optimized through SIMD encoding techniques** (e.g., batching multiple identifiers into a single encryption). Additionally, **naive applications of FHE** may lead to unnecessarily complex computations—**with proper technical expertise, complexity can be reduced**, making encrypted processing feasible.
  - **Parameterization Complexity**: Implementing HE for practical use **requires careful parameter selection** to balance **security, efficiency, and correctness**. **Incorrect parameterization can result in insecure encryption or excessive performance penalties**, necessitating **cryptographic expertise** for proper tuning.
  
- **Use Case**: A dedicated chapter will be published in the near future, fully exploring **use cases of homomorphic encryption**, along with **architectural guidelines** on how to enhance personal data processing through these techniques.


### **Miscellaneous Pseudonymization**

#### **Secure Multiparty Computation (MPC)**
Secure Multiparty Computation (MPC) enables multiple parties to collaboratively compute a function over their inputs **without revealing any individual input**. Unlike traditional cryptographic techniques that rely on a **trusted third party**, MPC ensures that **no single participant has access to the full dataset**, thereby maintaining privacy while still allowing joint computations.

MPC is particularly useful for **privacy-preserving data analytics**, enabling organizations to derive insights from sensitive datasets **without exposing the underlying raw data**. It forms the foundation for **distributed privacy-preserving computations** in sectors such as **healthcare, finance, and confidential AI training**.

Similar to **Fully Homomorphic Encryption (FHE)**, MPC holds significant **theoretical promise** but is still **limited in practical applications** due to computational and network overheads. However, **unlike FHE**, MPC does not suffer from **malleability weaknesses**, as traditional MPC protocols are designed to prevent **malicious modification of intermediate values**. Academic research has developed **many secure models and protocols**, some of which offer security against **strong adversaries, including active attacks**.

- **Example Algorithms**: Yao’s Garbled Circuits, Goldreich-Micali-Wigderson (GMW) Protocol, SPDZ

- **Strengths**:
  - **Encrypted Processing**: Ensures sensitive data is never exposed during processing, even against **adversaries with strong computational power**.
  - **No Malleability**: Traditional MPC protocols incorporate mechanisms that **prevent malicious modification of encrypted values**, unlike **FHE-only approaches**, where **malleability** can be a security weakness.
  - **Versatile Use Cases**: Applicable to **secure auctions, privacy-preserving AI, confidential business analytics, collaborative data processing**, and more.

- **Weaknesses**:
  - **Computational Overhead**: Requires significant processing power, especially for complex computations over large datasets.
  - **Communication Complexity**: Multi-party interactions result in a heavy increase of network traffic and latency.
  - **Implementation Complexity**: Similar to **Searchable Encryption**, MPC protocols are **far more complex than standard asymmetric encryption** and involve **multiple security-critical subroutines**. Current implementations may **not yet be fully mature for widespread production use**.

- **Use Case**: Similar to **FHE**, we plan to release a **dedicated chapter** on MPC in the near future, detailing its applications and practical deployment considerations.


#### **Secret Sharing Schemes**
Secret Sharing splits a secret (e.g., a cryptographic key or sensitive data) into multiple **shares**, which are distributed among participants. The secret can only be reconstructed when a predefined threshold of shares is combined, ensuring **data resilience and confidentiality**.

According to ENISA in [Data Pseudonymisation: Advanced Techniques and Use Cases](https://www.enisa.europa.eu/publications/data-pseudonymisation-advanced-techniques-and-use-cases), **secret sharing can be interpreted as a form of pseudonymization**, as it can be considered a specific instance of **Secure Multiparty Computation (MPC)**.

This technique is particularly useful for **distributed key management, threshold encryption, and secure authentication systems**, where no single entity should have full control over sensitive information. **Secret Sharing enhances other pseudonymization mechanisms** by allowing **the de-pseudonymization secret (e.g., an encryption key) to be split into shares** and stored on independent systems, making it significantly harder for attackers to compromise the entire key. Similarly, a **single personal identifier** can be **decomposed into different shares**, providing a comparable layer of security and access control.

- **Example Algorithms**: Shamir’s Secret Sharing, Verifiable Secret Sharing (VSS)

- **Strengths**:
  - **Threshold-Based Security**: Ensures that no single party can reconstruct the secret without the required number of shares.
  - **Resilient Against Single-Point Failures**: Provides strong protection against data loss and unauthorized access.
  - **Enhances Secure Key Management**: Useful for cryptographic key recovery, decentralized identity verification, and high-assurance access control systems.

- **Weaknesses**:
  - **Reconstruction Complexity**: Requires precise coordination among authorized parties to retrieve the original secret, which can introduce operational challenges.


#### **Other Pseudonymization Techniques**
There are various additional techniques used for pseudonymization, including **Masking**, **Tokenization**, **Scrambling**, **Blurring** et al. These techniques aim to protect personal data in a **reversible** manner while ensuring security within a defined adversary model that aligns with a risk-based approach.

Additionally, we have not yet covered **anonymous authentication and authorization mechanisms**, nor cryptographic primitives such as **Ring Signatures**, which **ENISA classifies as advanced pseudonymization methods** in [Data Pseudonymisation: Advanced Techniques and Use Cases](https://www.enisa.europa.eu/publications/data-pseudonymisation-advanced-techniques-and-use-cases). For now, we emphasize that these methods are **at least as crucial** as the previously discussed cryptographic techniques. We plan to release a **dedicated chapter** in the near future, detailing these mechanisms and their practical applications.

The definition of pseudonymization remains broad, requiring that **personal data be transformed in a way that prevents direct identification while allowing controlled re-identification under strict conditions**. Since adversary models can vary significantly depending on the **organizational context, regulatory requirements, and threat landscape**, the selection of an appropriate pseudonymization technique must be guided by a **well-defined risk assessment**.

Due to this **heterogeneity in adversary models**, the set of potential pseudonymization mechanisms is extensive, and it is beyond the scope of this document to list every possible approach. However, for **most practical use cases**, one of the techniques described earlier—such as **deterministic encryption, format-preserving encryption, CCA2 secure encryption and FHE/MPC**—should be **sufficient when properly configured**.

If a **corner case arises** where standard techniques do not meet your specific requirements, we **strongly advise consulting a cryptographic expert** before implementing a custom solution. **Designing ad hoc cryptographic mechanisms without expert validation often leads to flawed implementations, rendering the protection ineffective.**  

### **Quasi-Identifiers and Discrimination/Attribute-Linkage**
The [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en) from the EDPB emphasize in several paragraphs that **pseudonymization is a complex process that requires additional protective measures**, regardless of the security guarantees offered by the various methods discussed earlier.

This is because traditional pseudonymization techniques primarily focus on **hiding identifying information in its explicit form**. However, cryptographic protections are only effective **within the defined adversary model**, which assumes a controlled set of attack scenarios. If the adversary operates **outside of this model**, then the cryptographic guarantees **may no longer hold**.

Consider the example of a highly secure **MPC protocol** that enables parties to sign a message without revealing their identities. While such a protocol can be **provably secure** in an abstract model where **no metadata exists**, its **real-world execution** introduces additional risks. When a user interacts with the protocol via a **standard web browser**, various metadata—such as **User Agents, IP addresses, browser fingerprints**—can be collected and potentially used for **re-identification**. 

In this case, metadata can act as **quasi-identifiers**—pieces of information that, when **combined**, can significantly increase the risk of re-identification. **Quasi-identifiers do not reveal an individual’s identity on their own**, but when accumulated, they may allow a precise linkage back to a person. Prominent examples of **quasi-identifiers** include:
- ZIP codes
- Dates of birth
- Gender
- Language preferences
- Employment-related data (job role, working hours, tenure, income)

In **Paragraph 101** of the [Guidelines on Pseudonymization](https://www.edpb.europa.eu/our-work-tools/documents/public-consultations/2025/guidelines-012025-pseudonymisation_en), the EDPB states:

> *One way to attribute data to a natural person is by looking at several attributes contained in the data that reveal information about the physical, physiological, genetic, mental, economic, cultural, or social identity of the data subject. If a combination of those attributes is sufficient to attribute at least part of the pseudonymized data to data subjects, then they are called quasi-identifiers. Demographic data are prime examples of such attributes: age, gender, languages spoken, marital or family status, profession, income. If the data concern employees, then other relevant data may be structural role, number of working hours, length of service. Persons handling pseudonymized data may well know the values of those attributes of some of the individuals to whom the pseudonymized data relate. This would enable them to attribute the data to those persons without the use of the pseudonymization secrets, i.e., without the need to reverse the pseudonymization transformation.*

#### **Handling Quasi-Identifiers**
There is **no clear boundary** between what constitutes a **quasi-identifier** and what does not. **By default, any information about a person should be treated as a potential quasi-identifier**. The key factor in determining its relevance is the **background knowledge** of individuals who may have access to the pseudonymized data.

Evaluating the risks associated with quasi-identifiers is a **highly technical task**, requiring expertise in **re-identification risks and data privacy methodologies**. Current research in this area is ongoing, and it is possible that **automated tools or standardized methodologies** will emerge in the future to facilitate this process, making it accessible even to professionals with **less experience in data re-identification risks**.

If **multiple quasi-identifiers** are identified in a dataset, **additional pseudonymization techniques must be applied**. Various methods exist to address this challenge, which will be discussed in the following sections.

The application of these methods should carefully balance the **need-to-know principle** with the **risk of re-identification**. This process requires **technical expertise**, as any failure in implementation may lead to **potential full disclosure of sensitive information**.


#### **Record Linkage (Re-Identification)**
k-anonymity was introduced by **Latanya Sweeney in 2002** in her seminal paper *"k-anonymity: A Model for Protecting Privacy"* published in the *International Journal of Uncertainty, Fuzziness and Knowledge-Based Systems*. The author observed that a **published medical dataset**, even without direct identifiers, could be **linked with census data** using **quasi-identifiers**, enabling **re-identification** and the **attribution of sensitive medical information** to specific individuals.

k-anonymity describes a **generalization procedure** where multiple quasi-identifiers are grouped into a **single generalized quasi-identifier** within an **equivalence class**, ensuring that each record is **indistinguishable among at least k others**. This technique effectively **protects against record linkage attacks**, where an adversary attempts to associate a **specific record** with an **identifiable individual**. However, it does **not fully prevent** linking an individual to a **set of k records**, which can still pose privacy risks.

#### **Attribute Linkage (Discrimination)**
Several years later, researchers recognized that linking an individual to **a set of k records** could still pose a privacy risk if all k records share the **same sensitive attribute** (e.g., a disease diagnosis). If an attacker already knows that their target **must** be present in the dataset, they can **confidently infer** that the target has the sensitive attribute. This vulnerability is commonly referred to as an **attribute linkage attack**.

In [Pseudonymisation Techniques and Best Practices](https://www.enisa.europa.eu/publications/pseudonymisation-techniques-and-best-practices), **ENISA describes attribute linkage as a form of discrimination**:

> *The goal of the discrimination attack is to identify properties of a pseudonym holder (at least one). These properties may not directly lead to uncovering the identity of the pseudonym holder, but may suffice to discriminate him or her in some way.*

To mitigate **attribute linkage**, an **enhanced approach to k-anonymity** was proposed, known as **l-diversity**. This concept ensures that no **single equivalence class** (group of k records) contains **only one sensitive attribute value**. The parameter **l** controls the **minimum level of diversity** required among unchanged sensitive attributes within each equivalence class.

However, **applying k-anonymity and l-diversity together while maintaining data utility is challenging**. If applied **naïvely**, the equivalence classes may become **too broad**, significantly reducing the usefulness of quasi-identifiers to a point where simply **removing them** would yield the same level of utility.

#### **Probabilitic Inference (Discrimination with High Probability)**
Recognizing that **k-anonymity and l-diversity** were still insufficient in some cases, a more refined method, **t-closeness**, was introduced. This approach may protect against **worst-case attribute linkage attacks** by ensuring that the **distribution of sensitive attributes within an equivalence class does not deviate significantly from the overall dataset distribution**. This means that even if an adversary gains access to a particular equivalence class, they cannot **infer more information than what is already present in the global dataset**.

This notion introduces a **probabilistic inference attack model**, which we may call **fuzzy discrimination**. Instead of directly linking an individual to a sensitive attribute, **an attacker can still infer probabilistic insights about individuals**, leading to **belief changes about their attributes**, which can indirectly cause discrimination.

### **Expanding Beyond k-Anonymity, l-Diversity, and t-Closeness**
k-anonymity, l-diversity, and t-closeness are the **most well-known techniques** for protecting against **record linkage and attribute linkage**. However, additional attack vectors exist that can lead to discrimination, such as **table linkage**, where an attacker determines whether **a specific individual is present in a dataset**. This can result in discrimination if the dataset **reveals sensitive population-wide characteristics**. To mitigate these risks, **additional measures** such as **suppression** or **randomization** are required to strengthen privacy protection against such attacks.

Moreover, many of these **deterministic pseudonymization methods** suffer from a critical vulnerability known as **intersection attacks**. Even if individual datasets are properly pseudonymized, the risk arises when multiple **pseudonymized datasets are published separately** but contain overlapping individuals. When these datasets are **combined**, an adversary may be able to **infer additional details**, ultimately compromising the original protections. Addressing such risks **requires careful risk analysis** to anticipate potential **dataset linkability** and its implications.

At some point, the distinction between **properly pseudonymized data and anonymized data** may become unclear. In some cases, they may effectively be **indistinguishable**, with the key difference being that **anonymized data is irreversibly transformed**, whereas **pseudonymized data retains the possibility of reversal under controlled conditions**.

We plan to release a **dedicated chapter** on these **privacy-preserving techniques**, covering around **50 different formal notions** of privacy protection, their relationships, and their practical implications. **Stay tuned.**

