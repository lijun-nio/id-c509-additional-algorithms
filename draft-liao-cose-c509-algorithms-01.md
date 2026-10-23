---
v: 3
title: "Additional Algorithms for C509 Certificates"
docname: draft-liao-cose-c509-algorithms-01
abbrev: C509 Algorithms
cat: std
submissiontype: IETF
coding: utf-8
pi:
  toc: yes
  sortrefs: yes
  symrefs: yes
  tocdepth: 4
venue:
  group: "CBOR Object Signing and Encryption"
  type: "Working Group"
  mail: "cose@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/cose/"
  github: "cose-wg/xxx"

author:
  - ins: L. Liao
    name: Lijun Liao
    org: NIO Inc.
    email: lijun.liao@nio.io

normative:
  RFC7748:
  RFC8032:
  RFC8391:
  RFC8554:
  RFC9708:
  RFC9802:
  RFC9881:
  RFC9935:
  I-D.ietf-cose-cbor-encoded-cert:
  I-D.ietf-lamps-pq-composite-sigs:
  I-D.ietf-lamps-pq-composite-kem:
  FIPS203:
    title: "Module-Lattice-Based Key-Encapsulation Mechanism Standard (FIPS 203)"
    target: https://doi.org/10.6028/NIST.FIPS.203
    author:
      -
        org: "National Institute of Standards and Technology (NIST)"
    date: 2024
    seriesinfo:
      DOI: 10.6028/NIST.FIPS.203
  FIPS204:
    title: "Module-Lattice-Based Digital Signature Standard (FIPS 204)"
    target: https://doi.org/10.6028/NIST.FIPS.204
    author:
      -
        org: "National Institute of Standards and Technology (NIST)"
    date: 2024
    seriesinfo:
      DOI: 10.6028/NIST.FIPS.204


informative:

--- abstract

This document registers additional algorithms in the IANA registries defined by {{I-D.ietf-cose-cbor-encoded-cert}}.  It extends the C509 certificate specification with post-quantum cryptography (PQC) signature and key encapsulation algorithms: ML-DSA, ML-KEM, the stateful hash-based signature algorithms HSS/LMS, XMSS, and XMSS^MT, and composite ML-DSA and ML-KEM algorithms.

--- middle

# Introduction {#intro}

{{I-D.ietf-cose-cbor-encoded-cert}} defines C509, a compact CBOR encoding of X.509 certificates and certification requests, and establishes a set of IANA registries, including the "C509 Signature Algorithms" and "C509 Public Key Algorithms" registries.  That specification populates these registries with commonly used values and anticipates that new algorithms, including post-quantum algorithms, are registered as they are specified.

This document adds entries to the C509 Signature Algorithms and C509 Public Key Algorithms registries for PQC signature and key encapsulation algorithms: ML-DSA, ML-KEM, the stateful hash-based signature algorithms HSS/LMS, XMSS, and XMSS^MT, and composite ML-DSA and ML-KEM algorithms.

All new values are registered in the "CBOR Encoded X.509 (C509)" registry group, following the registration procedures defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

# Conventions and Definitions {#conventions}

{::boilerplate bcp14-tagged}

This document uses the terminology and registry structure defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

# Additional Signature Algorithms {#sigalg}

This section defines additional entries for the C509 Signature Algorithms registry established in {{I-D.ietf-cose-cbor-encoded-cert}}.

## ML-DSA (Module-Lattice-Based Digital Signature Algorithm)

ML-DSA is a lattice-based digital signature algorithm standardized by NIST as {{FIPS204}} and specified for use in X.509 PKI by {{RFC9881}}.  In C509, the signature value (`issuerSignatureValue` in a C509 certificate, `subjectSignatureValue` in a C509 certification request) is a byte string containing the raw ML-DSA signature.

~~~
+-------+-----------------------------------------------------------+
| Value | Signature Algorithm                                       |
+=======+===========================================================+
| TBD1  | Name:        ML-DSA-44                                    |
|       | Identifiers: id-ml-dsa-44                                 |
|       | OID:         2.16.840.1.101.3.4.3.17                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 11       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              signature: bytes of size 2420                |
+-------+-----------------------------------------------------------+
| TBD2  | Name:        ML-DSA-65                                    |
|       | Identifiers: id-ml-dsa-65                                 |
|       | OID:         2.16.840.1.101.3.4.3.18                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 12       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              signature: bytes of size 3309                |
+-------+-----------------------------------------------------------+
| TBD3  | Name:        ML-DSA-87                                    |
|       | Identifiers: id-ml-dsa-87                                 |
|       | OID:         2.16.840.1.101.3.4.3.19                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 13       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              signature: bytes of size 4627                |
+-------+-----------------------------------------------------------+
~~~

## Stateful Hash-Based Signature Algorithms (HSS/LMS, XMSS, XMSS^MT)

The Hierarchical Signature System (HSS) built on the Leighton-Micali Signature (LMS) scheme is defined in {{RFC8554}}; its use in X.509 PKI is specified in {{RFC9708}} and {{RFC9802}}.  The eXtended Merkle Signature Scheme (XMSS) and its multi-tree variant (XMSS^MT) are defined in {{RFC8391}}; their use in X.509 PKI is specified in {{RFC9802}}.  For all three schemes, the C509 signature value is a byte string containing the raw signature.

All three are stateful signature schemes.  Their signature and public key sizes depend on the chosen parameter set.

~~~
+-------+-----------------------------------------------------------+
| Value | Signature Algorithm                                       |
+=======+===========================================================+
| TBD4  | Name:        HSS/LMS Hash-based Signature                 |
|       | Identifiers: id-alg-hss-lms-hashsig,                      |
|       |              id-alg-mts-hashsig                           |
|       | OID:         1.2.840.113549.1.9.16.3.17                   |
|       | Parameters:  Absent                                       |
|       | DER:         30 0D 06 0B 2A 86 48 86 F7 0D 01 09 10 03 11 |
|       | Comments:    RFC 8554; RFC 9708; RFC 9802;                |
|       |              signature: bytes of variable size            |
+-------+-----------------------------------------------------------+
| TBD5  | Name:        XMSS Hash-based Signature                    |
|       | Identifiers: id-alg-xmss-hashsig                          |
|       | OID:         1.3.6.1.5.5.7.6.34                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 22          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              signature: bytes of variable size            |
+-------+-----------------------------------------------------------+
| TBD6  | Name:        XMSS^MT Hash-based Signature                 |
|       | Identifiers: id-alg-xmssmt-hashsig                        |
|       | OID:         1.3.6.1.5.5.7.6.35                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 23          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              signature: bytes of variable size            |
+-------+-----------------------------------------------------------+
~~~

## Composite ML-DSA Signature Algorithms

Composite ML-DSA combines an ML-DSA component with a traditional signature algorithm (RSA, ECDSA, Ed25519, or Ed448) into a single signature algorithm for use in X.509 PKI.  These algorithms are defined in {{I-D.ietf-lamps-pq-composite-sigs}}, which assigns their OIDs under the PKIX algorithms arc (1.3.6.1.5.5.7.6).

In C509, a composite signature value is encoded as the two-element CBOR array `CompositeSignatureValue` rather than as concatenated bytes.  The first element is the raw ML-DSA signature.  The second element is the traditional signature, encoded as the C509 signature value of the corresponding standalone algorithm: a byte string containing the raw RSA or EdDSA signature, or, for ECDSA, the fixed-length encoding specified in {{Section 3.2.2 of I-D.ietf-cose-cbor-encoded-cert}}.

~~~
CompositeSignatureValue = [
  pqc  : any,  ; PQC component signature
  trad : any   ; traditional component signature
]
~~~

The AlgorithmIdentifier parameters are absent for all composite signature algorithms.

~~~
+-------+-----------------------------------------------------------+
| Value | Signature Algorithm                                       |
+=======+===========================================================+
| TBD7  | Name:        sa-MLDSA44-RSA2048-PSS-SHA256                |
|       | Identifiers: id-MLDSA44-RSA2048-PSS-SHA256                |
|       | OID:         1.3.6.1.5.5.7.6.37                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 25          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 2420,                    |
|       |              trad: bytes of size 256                      |
+-------+-----------------------------------------------------------+
| TBD8  | Name:        sa-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | OID:         1.3.6.1.5.5.7.6.38                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 26          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 2420,                    |
|       |              trad: bytes of size 256                      |
+-------+-----------------------------------------------------------+
| TBD9  | Name:        sa-MLDSA44-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.39                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 27          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 2420,                    |
|       |              trad: bytes of size 64                       |
+-------+-----------------------------------------------------------+
| TBD10 | Name:        sa-MLDSA44-ECDSA-P256-SHA256                 |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                 |
|       | OID:         1.3.6.1.5.5.7.6.40                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 28          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 2420,                    |
|       |              trad: bytes of size 64                       |
+-------+-----------------------------------------------------------+
| TBD11 | Name:        sa-MLDSA65-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.41                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 29          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 384                      |
+-------+-----------------------------------------------------------+
| TBD12 | Name:        sa-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.42                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2A          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 384                      |
+-------+-----------------------------------------------------------+
| TBD13 | Name:        sa-MLDSA65-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.43                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2B          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 512                      |
+-------+-----------------------------------------------------------+
| TBD14 | Name:        sa-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.44                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2C          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 512                      |
+-------+-----------------------------------------------------------+
| TBD15 | Name:        sa-MLDSA65-ECDSA-P256-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.45                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2D          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 64                       |
+-------+-----------------------------------------------------------+
| TBD16 | Name:        sa-MLDSA65-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.46                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2E          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 96                       |
+-------+-----------------------------------------------------------+
| TBD17 | Name:        sa-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.47                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2F          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 64                       |
+-------+-----------------------------------------------------------+
| TBD18 | Name:        sa-MLDSA65-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.48                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 30          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 3309,                    |
|       |              trad: bytes of size 64                       |
+-------+-----------------------------------------------------------+
| TBD19 | Name:        sa-MLDSA87-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.49                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 31          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 96                       |
+-------+-----------------------------------------------------------+
| TBD20 | Name:        sa-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.50                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 32          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 96                       |
+-------+-----------------------------------------------------------+
| TBD21 | Name:        sa-MLDSA87-Ed448-SHAKE256                    |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                    |
|       | OID:         1.3.6.1.5.5.7.6.51                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 33          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 114                      |
+-------+-----------------------------------------------------------+
| TBD22 | Name:        sa-MLDSA87-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.52                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 34          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 384                      |
+-------+-----------------------------------------------------------+
| TBD23 | Name:        sa-MLDSA87-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.53                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 35          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 512                      |
+-------+-----------------------------------------------------------+
| TBD24 | Name:        sa-MLDSA87-ECDSA-P521-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.54                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 36          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: CompositeSignatureValue;          |
|       |              pqc:  bytes of size 4627,                    |
|       |              trad: bytes of size 132                      |
+-------+-----------------------------------------------------------+
~~~

# Additional Public Key Algorithms {#pkalg}

This section defines additional entries for the C509 Public Key Algorithms registry established in {{I-D.ietf-cose-cbor-encoded-cert}}.

As required by {{Section 8.15 of I-D.ietf-cose-cbor-encoded-cert}}, a public key algorithm that can only be used with one signature algorithm and that has the same OID as that signature algorithm is assigned the same integer value in both registries.  This applies to the ML-DSA, HSS/LMS, XMSS, XMSS^MT, and composite ML-DSA entries below.  The ML-KEM and composite ML-KEM entries have no counterpart in the C509 Signature Algorithms registry.

## ML-DSA Public Keys

ML-DSA public keys in X.509 certificates are encoded as specified in {{RFC9881}}.  In C509, the `subjectPublicKey` field is a byte string containing the raw ML-DSA public key.  The values TBD1, TBD2, and TBD3 are the same as in the C509 Signature Algorithms registry.

~~~
+-------+-----------------------------------------------------------+
| Value | Public Key Algorithm                                      |
+=======+===========================================================+
| TBD1  | Name:        ML-DSA-44                                    |
|       | Identifiers: id-ml-dsa-44                                 |
|       | OID:         2.16.840.1.101.3.4.3.17                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 11       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              subjectPublicKey: bytes of size 1312         |
+-------+-----------------------------------------------------------+
| TBD2  | Name:        ML-DSA-65                                    |
|       | Identifiers: id-ml-dsa-65                                 |
|       | OID:         2.16.840.1.101.3.4.3.18                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 12       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              subjectPublicKey: bytes of size 1952         |
+-------+-----------------------------------------------------------+
| TBD3  | Name:        ML-DSA-87                                    |
|       | Identifiers: id-ml-dsa-87                                 |
|       | OID:         2.16.840.1.101.3.4.3.19                      |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 03 13       |
|       | Comments:    FIPS 204; RFC 9881;                          |
|       |              subjectPublicKey: bytes of size 2592         |
+-------+-----------------------------------------------------------+
~~~

## ML-KEM Public Keys (Key Encapsulation Mechanism)

ML-KEM is a lattice-based key encapsulation mechanism standardized by NIST as {{FIPS203}} and specified for use in X.509 PKI by {{RFC9935}}.  In C509, the `subjectPublicKey` field is a byte string containing the raw ML-KEM public key.  ML-KEM is not a signature algorithm, so these entries have no counterpart in the C509 Signature Algorithms registry.

~~~
+-------+-----------------------------------------------------------+
| Value | Public Key Algorithm                                      |
+=======+===========================================================+
| TBD25 | Name:        ML-KEM-512                                   |
|       | Identifiers: id-ml-kem-512                                |
|       | OID:         2.16.840.1.101.3.4.4.1                       |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 04 01       |
|       | Comments:    FIPS 203; RFC 9935;                          |
|       |              subjectPublicKey: bytes of size 800          |
+-------+-----------------------------------------------------------+
| TBD26 | Name:        ML-KEM-768                                   |
|       | Identifiers: id-ml-kem-768                                |
|       | OID:         2.16.840.1.101.3.4.4.2                       |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 04 02       |
|       | Comments:    FIPS 203; RFC 9935;                          |
|       |              subjectPublicKey: bytes of size 1184         |
+-------+-----------------------------------------------------------+
| TBD27 | Name:        ML-KEM-1024                                  |
|       | Identifiers: id-ml-kem-1024                               |
|       | OID:         2.16.840.1.101.3.4.4.3                       |
|       | Parameters:  Absent                                       |
|       | DER:         30 0B 06 09 60 86 48 01 65 03 04 04 03       |
|       | Comments:    FIPS 203; RFC 9935;                          |
|       |              subjectPublicKey: bytes of size 1568         |
+-------+-----------------------------------------------------------+
~~~

## Stateful Hash-Based Public Keys

HSS/LMS, XMSS, and XMSS^MT public keys in X.509 certificates are encoded as specified in {{RFC9802}}.  In C509, the `subjectPublicKey` field is a byte string containing the raw public key, whose size depends on the chosen parameter set.  The values TBD4, TBD5, and TBD6 are the same as in the C509 Signature Algorithms registry.

~~~
+-------+-----------------------------------------------------------+
| Value | Public Key Algorithm                                      |
+=======+===========================================================+
| TBD4  | Name:        HSS/LMS Hash-based Signature                 |
|       | Identifiers: id-alg-hss-lms-hashsig,                      |
|       |              id-alg-mts-hashsig                           |
|       | OID:         1.2.840.113549.1.9.16.3.17                   |
|       | Parameters:  Absent                                       |
|       | DER:         30 0D 06 0B 2A 86 48 86 F7 0D 01 09 10 03 11 |
|       | Comments:    RFC 8554; RFC 9708; RFC 9802;                |
|       |              subjectPublicKey: bytes of variable size     |
+-------+-----------------------------------------------------------+
| TBD5  | Name:        XMSS Hash-based Signature                    |
|       | Identifiers: id-alg-xmss-hashsig                          |
|       | OID:         1.3.6.1.5.5.7.6.34                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 22          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              subjectPublicKey: bytes of variable size     |
+-------+-----------------------------------------------------------+
| TBD6  | Name:        XMSS^MT Hash-based Signature                 |
|       | Identifiers: id-alg-xmssmt-hashsig                        |
|       | OID:         1.3.6.1.5.5.7.6.35                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 23          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              subjectPublicKey: bytes of variable size     |
+-------+-----------------------------------------------------------+
~~~

## Composite ML-DSA Public Keys {#sec-composite-mldsa-pkeys}

In C509, a composite public key is encoded as the two-element CBOR array `CompositePublicKey` rather than as concatenated bytes.  Each element uses the C509 encoding of the corresponding standalone algorithm as defined in {{I-D.ietf-cose-cbor-encoded-cert}}:

* ML-DSA public keys are the raw public key as specified in {{RFC9881}}.
* ECDSA public keys are the EC point, optionally point-compressed, as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}.
* EdDSA public keys are the raw public key as specified in {{RFC8032}}.
* RSA public keys are encoded as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}: the modulus as an unwrapped CBOR unsigned bignum (~biguint) when the exponent is 65537, and otherwise a two-element array of modulus and exponent.

~~~
CompositePublicKey = [
  pqc  : any,  ; PQC component public key
  trad : any   ; traditional component public key
]
~~~

The OIDs are those of the corresponding composite signature algorithms in {{I-D.ietf-lamps-pq-composite-sigs}}, and the values TBD7 through TBD24 are the same as in the C509 Signature Algorithms registry.

~~~
+-------+-----------------------------------------------------------+
| Value | Public Key Algorithm                                      |
+=======+===========================================================+
| TBD7  | Name:        pk-MLDSA44-RSA2048-PSS-SHA256                |
|       | Identifiers: id-MLDSA44-RSA2048-PSS-SHA256                |
|       | OID:         1.3.6.1.5.5.7.6.37                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 25          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1312,                    |
|       |              trad: bytes of size 256, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD8  | Name:        pk-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | OID:         1.3.6.1.5.5.7.6.38                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 26          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1312,                    |
|       |              trad: bytes of size 256, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD9  | Name:        pk-MLDSA44-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.39                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 27          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1312,                    |
|       |              trad: bytes of size 32                       |
+-------+-----------------------------------------------------------+
| TBD10 | Name:        pk-MLDSA44-ECDSA-P256-SHA256                 |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                 |
|       | OID:         1.3.6.1.5.5.7.6.40                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 28          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1312,                    |
|       |              trad: bytes of size 33 or 65                 |
+-------+-----------------------------------------------------------+
| TBD11 | Name:        pk-MLDSA65-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.41                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 29          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 384, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD12 | Name:        pk-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.42                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2A          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 384, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD13 | Name:        pk-MLDSA65-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.43                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2B          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 512, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD14 | Name:        pk-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.44                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2C          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 512, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD15 | Name:        pk-MLDSA65-ECDSA-P256-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.45                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2D          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 33 or 65                 |
+-------+-----------------------------------------------------------+
| TBD16 | Name:        pk-MLDSA65-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.46                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2E          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD17 | Name:        pk-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.47                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2F          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 33 or 65                 |
+-------+-----------------------------------------------------------+
| TBD18 | Name:        pk-MLDSA65-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.48                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 30          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1952,                    |
|       |              trad: bytes of size 32                       |
+-------+-----------------------------------------------------------+
| TBD19 | Name:        pk-MLDSA87-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.49                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 31          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD20 | Name:        pk-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.50                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 32          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD21 | Name:        pk-MLDSA87-Ed448-SHAKE256                    |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                    |
|       | OID:         1.3.6.1.5.5.7.6.51                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 33          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 57                       |
+-------+-----------------------------------------------------------+
| TBD22 | Name:        pk-MLDSA87-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.52                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 34          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 384, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD23 | Name:        pk-MLDSA87-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.53                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 35          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 512, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD24 | Name:        pk-MLDSA87-ECDSA-P521-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.54                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 36          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 2592,                    |
|       |              trad: bytes of size 67 or 133                |
+-------+-----------------------------------------------------------+
~~~

## Composite ML-KEM Public Keys

Composite ML-KEM combines an ML-KEM component with a traditional key establishment algorithm (RSA-OAEP, ECDH, X25519, or X448) into a single KEM for use in X.509 PKI.  These algorithms are defined in {{I-D.ietf-lamps-pq-composite-kem}}, which assigns their OIDs under the PKIX algorithms arc (1.3.6.1.5.5.7.6).  These entries have no counterpart in the C509 Signature Algorithms registry.

In C509, composite KEM public keys use the `CompositePublicKey` structure defined in {{sec-composite-mldsa-pkeys}}.  The `pqc` element is the raw ML-KEM public key, and the `trad` element is the C509 encoding of the traditional public key as defined in {{I-D.ietf-cose-cbor-encoded-cert}}:

* ECDH public keys are the EC point, optionally point-compressed, as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}.
* X25519 and X448 public keys are the raw 32- or 56-byte key as specified in {{RFC7748}}.
* RSA public keys are encoded as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}.

~~~
+-------+-----------------------------------------------------------+
| Value | Public Key Algorithm                                      |
+=======+===========================================================+
| TBD28 | Name:        pk-MLKEM768-RSA2048-SHA3-256                 |
|       | Identifiers: id-MLKEM768-RSA2048-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.55                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 37          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 256, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD29 | Name:        pk-MLKEM768-RSA3072-SHA3-256                 |
|       | Identifiers: id-MLKEM768-RSA3072-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.56                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 38          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 384, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD30 | Name:        pk-MLKEM768-RSA4096-SHA3-256                 |
|       | Identifiers: id-MLKEM768-RSA4096-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.57                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 39          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 512, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD31 | Name:        pk-MLKEM768-X25519-SHA3-256                  |
|       | Identifiers: id-MLKEM768-X25519-SHA3-256                  |
|       | OID:         1.3.6.1.5.5.7.6.58                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3A          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 32                       |
+-------+-----------------------------------------------------------+
| TBD32 | Name:        pk-MLKEM768-ECDH-P256-SHA3-256               |
|       | Identifiers: id-MLKEM768-ECDH-P256-SHA3-256               |
|       | OID:         1.3.6.1.5.5.7.6.59                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3B          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 33 or 65                 |
+-------+-----------------------------------------------------------+
| TBD33 | Name:        pk-MLKEM768-ECDH-P384-SHA3-256               |
|       | Identifiers: id-MLKEM768-ECDH-P384-SHA3-256               |
|       | OID:         1.3.6.1.5.5.7.6.60                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3C          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD34 | Name:        pk-MLKEM768-ECDH-brainpoolP256r1-SHA3-256    |
|       | Identifiers: id-MLKEM768-ECDH-brainpoolP256r1-SHA3-256    |
|       | OID:         1.3.6.1.5.5.7.6.61                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3D          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1184,                    |
|       |              trad: bytes of size 33 or 65                 |
+-------+-----------------------------------------------------------+
| TBD35 | Name:        pk-MLKEM1024-RSA3072-SHA3-256                |
|       | Identifiers: id-MLKEM1024-RSA3072-SHA3-256                |
|       | OID:         1.3.6.1.5.5.7.6.62                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3E          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1568,                    |
|       |              trad: bytes of size 384, or                  |
|       |                    array of 2 elements                    |
+-------+-----------------------------------------------------------+
| TBD36 | Name:        pk-MLKEM1024-ECDH-P384-SHA3-256              |
|       | Identifiers: id-MLKEM1024-ECDH-P384-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.63                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3F          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1568,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD37 | Name:        pk-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256   |
|       | Identifiers: id-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256   |
|       | OID:         1.3.6.1.5.5.7.6.64                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 40          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1568,                    |
|       |              trad: bytes of size 49 or 97                 |
+-------+-----------------------------------------------------------+
| TBD38 | Name:        pk-MLKEM1024-X448-SHA3-256                   |
|       | Identifiers: id-MLKEM1024-X448-SHA3-256                   |
|       | OID:         1.3.6.1.5.5.7.6.65                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 41          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1568,                    |
|       |              trad: bytes of size 56                       |
+-------+-----------------------------------------------------------+
| TBD39 | Name:        pk-MLKEM1024-ECDH-P521-SHA3-256              |
|       | Identifiers: id-MLKEM1024-ECDH-P521-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.66                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 42          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: CompositePublicKey;        |
|       |              pqc:  bytes of size 1568,                    |
|       |              trad: bytes of size 67 or 133                |
+-------+-----------------------------------------------------------+
~~~

# Security Considerations {#security}

The security considerations of {{I-D.ietf-cose-cbor-encoded-cert}} apply to all entries registered in this document.

For post-quantum algorithms, implementers should consult the respective NIST standards ({{FIPS203}}, {{FIPS204}}) for guidance on security levels and parameter sets.

# IANA Considerations {#iana}

## C509 Signature Algorithms Registry

IANA is requested to register the entries defined in {{sigalg}} in the "C509 Signature Algorithms" registry within the "CBOR Encoded X.509 (C509)" registry group, following the registration policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

## C509 Public Key Algorithms Registry

IANA is requested to register the entries defined in {{pkalg}} in the "C509 Public Key Algorithms" registry within the "CBOR Encoded X.509 (C509)" registry group, following the registration policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

Per {{Section 8.15 of I-D.ietf-cose-cbor-encoded-cert}}, the public key algorithm entries TBD1 through TBD24 MUST be assigned the same values as the corresponding entries in the "C509 Signature Algorithms" registry.

--- back

# Examples {#examples}

TBD: add examples

# Change Log {#changelog}
{:numbered="false"}

> RFC Editor: Please remove this section before publication.

## Changes from -00 to -01
{:numbered="false"}

* Aligned with draft-ietf-cose-cbor-encoded-cert-21:
  * Used the registry group name "CBOR Encoded X.509 (C509)".
  * Gave the DER column as the DER encoding of the complete AlgorithmIdentifier and wrote "Absent" for absent parameters.
  * Referred to the signature value as `issuerSignatureValue` or `subjectSignatureValue`.
  * Referenced Section 8.15 of that document for assigning the same value in the signature and public key algorithm registries, and moved this requirement from Security Considerations to IANA Considerations.
  * Referenced Sections 3.2.1 and 3.2.2 of that document for the EC public key and ECDSA signature encodings within composite structures.
* Clarified the RSA public key encoding within `CompositePublicKey`.
* Removed unused references.
* Editorial improvements.

# Acknowledgements {#acknowledgements}
{:numbered="false"}

The authors would like to thank xxx for reviewing and commenting on earlier versions of this document.
