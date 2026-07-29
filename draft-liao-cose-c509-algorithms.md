---
v: 3
title: "Additional Algorithms for C509 Certificates"
docname: draft-liao-cose-c509-algorithms-00
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
    org: NIO
    email: lijun.liao@nio.io

normative:
  RFC5280:
  RFC7748:
  RFC8032:
  RFC8391:
  RFC8554:
  RFC8949:
  RFC8610:
  RFC9053:
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

This document registers additional algorithms in the IANA registries defined by {{I-D.ietf-cose-cbor-encoded-cert}}.  It extends the base C509 certificate specification with post-quantum cryptography (PQC) signature and key-encapsulation algorithms including ML-DSA, ML-KEM, stateful hash-based algorithms (HSS/LMS, XMSS, XMSS^MT), and composite algorithms.

--- middle

# Introduction {#intro}

{{I-D.ietf-cose-cbor-encoded-cert}} defines C509, a compact CBOR encoding of X.509 certificates and certification requests, and establishes IANA registries for algorithms, extensions, RDN attribute types, and CR attribute types.  The base specification populates these registries with a set of commonly used values.

This document adds additional entries to the algorithm registries, focusing on post-quantum cryptography (PQC) algorithms for signatures and key encapsulation, including ML-DSA, ML-KEM, stateful hash-based algorithms (HSS/LMS, XMSS, XMSS^MT), and composite algorithms.

All new values are registered under the "CBOR Encoded X.509 (C509)" registry group.  This document follows the registration procedures defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

# Conventions and Definitions {#conventions}

{::boilerplate bcp14-tagged}

This document uses the terminology and registry structure defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

# Additional Signature Algorithms {#sigalg}

This section defines additional entries for the C509 Signature Algorithms Registry established in {{I-D.ietf-cose-cbor-encoded-cert}}.

## ML-DSA (Module-Lattice-Based Digital Signature Algorithm)

ML-DSA is a lattice-based digital signature algorithm standardized by NIST as {{FIPS204}} and specified for use in X.509 PKI by {{RFC9881}}.  In C509, the `signature` field is a `bytes` value containing the raw ML-DSA signature bytes.

~~~
+-------+----------------------------------------------------------+
| Value | Signature Algorithm                                      |
+=======+==========================================================+
| TBD1  | Name:        ML-DSA-44                                   |
|       | Identifiers: id-ml-dsa-44                                |
|       | OID:         2.16.840.1.101.3.4.3.17                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 11            |
|       | Comments:    FIPS 204; RFC 9881;                         |
|       |              signature: bytes of size 2420               |
+-------+----------------------------------------------------------+
| TBD2  | Name:        ML-DSA-65                                   |
|       | Identifiers: id-ml-dsa-65                                |
|       | OID:         2.16.840.1.101.3.4.3.18                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 12            |
|       | Comments:    FIPS 204; RFC 9881;                         |
|       |              signature: bytes of size 3309               |
+-------+----------------------------------------------------------+
| TBD3  | Name:        ML-DSA-87                                   |
|       | Identifiers: id-ml-dsa-87                                |
|       | OID:         2.16.840.1.101.3.4.3.19                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 13            |
|       | Comments:    FIPS 204; RFC 9881;                         |
|       |              signature: bytes of size 4627               |
+-------+----------------------------------------------------------+
~~~

## Stateful Hash-Based Signature Algorithms (HSS/LMS, XMSS, XMSS^MT)

HSS and XMSS are stateful hash-based signature algorithms.  The Hierarchical Signature System (HSS) over the Leighton-Micali Signature (LMS) scheme is defined in {{RFC8554}} and its use in X.509 PKI is specified by {{RFC9802}} and {{RFC9708}}.  The eXtended Merkle Signature Scheme (XMSS) and its multi-tree variant (XMSS^MT) are defined in {{RFC8391}} and their use in X.509 PKI is specified by {{RFC9802}}.  In C509, the `signature` field is a `bytes` value containing the raw signature bytes for all three schemes.

Note: These are stateful signature schemes.  Signature and public key sizes depend on the chosen parameter set.

~~~
+-------+----------------------------------------------------------+
| Value | Signature Algorithm                                      |
+=======+==========================================================+
| TBD4  | Name:        HSS/LMS Hash-based Signature                |
|       | Identifiers: id-alg-hss-lms-hashsig,                     |
|       |              id-alg-mts-hashsig                          |
|       | OID:         1.2.840.113549.1.9.16.3.17                  |
|       | Parameters:  absent                                      |
|       | DER:         06 0B 2A 86 48 86 F7 0D 01 09 10 03 11      |
|       | Comments:    RFC 8554; RFC 9708; RFC 9802;               |
|       |              signature: bytes of variable size           |
+-------+----------------------------------------------------------+
| TBD5  | Name:        XMSS Hash-based Signature                   |
|       | Identifiers: id-alg-xmss-hashsig                         |
|       | OID:         1.3.6.1.5.5.7.6.34                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 22               |
|       | Comments:    RFC 8391; RFC 9802;                         |
|       |              signature: bytes of variable size           |
+-------+----------------------------------------------------------+
| TBD6  | Name:        XMSS^MT Hash-based Signature                |
|       | Identifiers: id-alg-xmssmt-hashsig                       |
|       | OID:         1.3.6.1.5.5.7.6.35                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 23               |
|       | Comments:    RFC 8391; RFC 9802;                         |
|       |              signature: bytes of variable size           |
+-------+----------------------------------------------------------+
~~~

## Composite ML-DSA Signature Algorithms

Composite ML-DSA combines a Module-Lattice-Based Digital Signature Algorithm (ML-DSA) component with a traditional signature algorithm (RSA, ECDSA, Ed25519, or Ed448) to form a single composite signature algorithm suitable for use in X.509 PKI structures.  These algorithms are defined in {{I-D.ietf-lamps-pq-composite-sigs}} and assigned OIDs under the PKIX algorithms arc (1.3.6.1.5.5.7.6).

In C509, composite signature values are encoded as a two-element CBOR array `CompositeSignatureValue` rather than as raw concatenated bytes.  The first element holds the raw ML-DSA signature bytes; the second element holds the traditional signature encoded as specified in {{Section 3.2.2 of I-D.ietf-cose-cbor-encoded-cert}}.

~~~
CompositeSignatureValue = [
  pqc  : any,  ; PQC component signature
  trad : any   ; traditional component signature
]
~~~

Parameters are absent.

~~~
+-------+----------------------------------------------------------+
| Value | Signature Algorithm                                      |
+=======+==========================================================+
| TBD7  | Name:        sa-MLDSA44-RSA2048-PSS-SHA256               |
|       | Identifiers: id-MLDSA44-RSA2048-PSS-SHA256               |
|       | OID:         1.3.6.1.5.5.7.6.37                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 25               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 2420,                   |
|       |              trad: bytes of size 256                     |
+-------+----------------------------------------------------------+
| TBD8  | Name:        sa-MLDSA44-RSA2048-PKCS15-SHA256            |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256            |
|       | OID:         1.3.6.1.5.5.7.6.38                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 26               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 2420,                   |
|       |              trad: bytes of size 256                     |
+-------+----------------------------------------------------------+
| TBD9  | Name:        sa-MLDSA44-Ed25519-SHA512                   |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                   |
|       | OID:         1.3.6.1.5.5.7.6.39                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 27               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 2420,                   |
|       |              trad: bytes of size 64                      |
+-------+----------------------------------------------------------+
| TBD10 | Name:        sa-MLDSA44-ECDSA-P256-SHA256                |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                |
|       | OID:         1.3.6.1.5.5.7.6.40                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 28               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 2420,                   |
|       |              trad: bytes of size 64                      |
+-------+----------------------------------------------------------+
| TBD11 | Name:        sa-MLDSA65-RSA3072-PSS-SHA512               |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.41                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 29               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 384                     |
+-------+----------------------------------------------------------+
| TBD12 | Name:        sa-MLDSA65-RSA3072-PKCS15-SHA512            |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512            |
|       | OID:         1.3.6.1.5.5.7.6.42                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2A               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 384                     |
+-------+----------------------------------------------------------+
| TBD13 | Name:        sa-MLDSA65-RSA4096-PSS-SHA512               |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.43                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2B               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 512                     |
+-------+----------------------------------------------------------+
| TBD14 | Name:        sa-MLDSA65-RSA4096-PKCS15-SHA512            |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512            |
|       | OID:         1.3.6.1.5.5.7.6.44                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2C               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 512                     |
+-------+----------------------------------------------------------+
| TBD15 | Name:        sa-MLDSA65-ECDSA-P256-SHA512                |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.45                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2D               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 64                      |
+-------+----------------------------------------------------------+
| TBD16 | Name:        sa-MLDSA65-ECDSA-P384-SHA512                |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.46                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2E               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 96                      |
+-------+----------------------------------------------------------+
| TBD17 | Name:        sa-MLDSA65-ECDSA-brainpoolP256r1-SHA512     |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512     |
|       | OID:         1.3.6.1.5.5.7.6.47                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2F               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 64                      |
+-------+----------------------------------------------------------+
| TBD18 | Name:        sa-MLDSA65-Ed25519-SHA512                   |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                   |
|       | OID:         1.3.6.1.5.5.7.6.48                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 30               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 3309,                   |
|       |              trad: bytes of size 64                      |
+-------+----------------------------------------------------------+
| TBD19 | Name:        sa-MLDSA87-ECDSA-P384-SHA512                |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.49                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 31               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 96                      |
+-------+----------------------------------------------------------+
| TBD20 | Name:        sa-MLDSA87-ECDSA-brainpoolP384r1-SHA512     |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512     |
|       | OID:         1.3.6.1.5.5.7.6.50                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 32               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 96                      |
+-------+----------------------------------------------------------+
| TBD21 | Name:        sa-MLDSA87-Ed448-SHAKE256                   |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                   |
|       | OID:         1.3.6.1.5.5.7.6.51                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 33               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 114                       |
+-------+----------------------------------------------------------+
| TBD22 | Name:        sa-MLDSA87-RSA3072-PSS-SHA512               |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.52                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 34               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 384                     |
+-------+----------------------------------------------------------+
| TBD23 | Name:        sa-MLDSA87-RSA4096-PSS-SHA512               |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.53                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 35               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 512                     |
+-------+----------------------------------------------------------+
| TBD24 | Name:        sa-MLDSA87-ECDSA-P521-SHA512                |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.54                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 36               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              signature: CompositeSignatureValue;         |
|       |              pqc:  bytes of size 4627,                   |
|       |              trad: bytes of size 132                       |
+-------+----------------------------------------------------------+
~~~

# Additional Public Key Algorithms {#pkalg}

This section defines additional entries for the C509 Public Key Algorithms Registry established in {{I-D.ietf-cose-cbor-encoded-cert}}.

When an algorithm OID appears in both the C509 Signature Algorithms Registry and the C509 Public Key Algorithms Registry, the same integer value is assigned in both registries.  This shared numbering allows decoders to use a single OID lookup table.  Algorithm OIDs that exist only as public key algorithms (e.g., key encapsulation mechanisms such as ML-KEM) receive unique integer values not used in the signature registry.

## ML-DSA Public Keys

ML-DSA public keys in X.509 certificates are encoded as per {{RFC9881}}. In C509, the `subjectPublicKey` field is a `bytes` value containing the raw ML-DSA public key bytes.  The integer values TBD1, TBD2, and TBD3 are shared with the C509 Signature Algorithms Registry.

~~~
+-------+----------------------------------------------------------+
| Value | Public Key Algorithm                                     |
+=======+==========================================================+
| TBD1  | Name:        ML-DSA-44                                   |
|       | Identifiers: id-ml-dsa-44                                |
|       | OID:         2.16.840.1.101.3.4.3.17                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 11            |
|       | Comments:    FIPS 204; RFC 9881;                      |
|       |              subjectPublicKey: bytes of size 1312        |
+-------+----------------------------------------------------------+
| TBD2  | Name:        ML-DSA-65                                   |
|       | Identifiers: id-ml-dsa-65                                |
|       | OID:         2.16.840.1.101.3.4.3.18                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 12            |
|       | Comments:    FIPS 204; RFC 9881;                      |
|       |              subjectPublicKey: bytes of size 1952        |
+-------+----------------------------------------------------------+
| TBD3  | Name:        ML-DSA-87                                   |
|       | Identifiers: id-ml-dsa-87                                |
|       | OID:         2.16.840.1.101.3.4.3.19                     |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 03 13            |
|       | Comments:    FIPS 204; RFC 9881;                      |
|       |              subjectPublicKey: bytes of size 2592        |
+-------+----------------------------------------------------------+
~~~

## ML-KEM Public Keys (Key Encapsulation Mechanism)

ML-KEM is a lattice-based key encapsulation mechanism standardized by NIST as {{FIPS203}} and specified for use in X.509 PKI by {{RFC9935}}.  ML-KEM OIDs do not appear in the signature registry, so TBD25, TBD26, and TBD27 are assigned exclusively to this registry.

~~~
+-------+----------------------------------------------------------+
| Value | Public Key Algorithm                                     |
+=======+==========================================================+
| TBD25 | Name:        ML-KEM-512                                  |
|       | Identifiers: id-ml-kem-512                               |
|       | OID:         2.16.840.1.101.3.4.4.1                      |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 04 01            |
|       | Comments:    FIPS 203; RFC 9935;                         |
|       |              subjectPublicKey: bytes of size 800         |
+-------+----------------------------------------------------------+
| TBD26 | Name:        ML-KEM-768                                  |
|       | Identifiers: id-ml-kem-768                               |
|       | OID:         2.16.840.1.101.3.4.4.2                      |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 04 02            |
|       | Comments:    FIPS 203; RFC 9935;                         |
|       |              subjectPublicKey: bytes of size 1184        |
+-------+----------------------------------------------------------+
| TBD27 | Name:        ML-KEM-1024                                 |
|       | Identifiers: id-ml-kem-1024                              |
|       | OID:         2.16.840.1.101.3.4.4.3                      |
|       | Parameters:  absent                                      |
|       | DER:         06 09 60 86 48 01 65 03 04 04 03            |
|       | Comments:    FIPS 203; RFC 9935;                         |
|       |              subjectPublicKey: bytes of size 1568        |
+-------+----------------------------------------------------------+
~~~

## Stateful Hash-Based Public Keys

HSS/LMS, XMSS, and XMSS^MT public keys in X.509 certificates are encoded as per {{RFC9802}}.  In C509, the `subjectPublicKey` field is a `bytes` value containing the raw public key bytes.  Public key size depends on the chosen parameter set.

~~~
+-------+----------------------------------------------------------+
| Value | Public Key Algorithm                                     |
+=======+==========================================================+
| TBD4  | Name:        HSS/LMS Hash-based Signature                |
|       | Identifiers: id-alg-hss-lms-hashsig,                     |
|       |              id-alg-mts-hashsig                          |
|       | OID:         1.2.840.113549.1.9.16.3.17                  |
|       | Parameters:  absent                                      |
|       | DER:         06 0B 2A 86 48 86 F7 0D 01 09 10 03 11      |
|       | Comments:    RFC 8554; RFC 9708; RFC 9802;               |
|       |              subjectPublicKey: bytes of variable size    |
+-------+----------------------------------------------------------+
| TBD5  | Name:        XMSS Hash-based Signature                   |
|       | Identifiers: id-alg-xmss-hashsig                         |
|       | OID:         1.3.6.1.5.5.7.6.34                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 22               |
|       | Comments:    RFC 8391; RFC 9802;                         |
|       |              subjectPublicKey: bytes of variable size    |
+-------+----------------------------------------------------------+
| TBD6  | Name:        XMSS^MT Hash-based Signature                |
|       | Identifiers: id-alg-xmssmt-hashsig                       |
|       | OID:         1.3.6.1.5.5.7.6.35                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 23               |
|       | Comments:    RFC 8391; RFC 9802;                         |
|       |              subjectPublicKey: bytes of variable size    |
+-------+----------------------------------------------------------+
~~~

## Composite ML-DSA Public Keys {#sec-composite-mldsa-pkeys}

In C509, composite public keys are encoded as a two-element CBOR array `CompositePublicKey` rather than as raw concatenated bytes.  Each element uses the C509 encoding of the corresponding standalone algorithm per {{I-D.ietf-cose-cbor-encoded-cert}}: ML-DSA public keys as raw bytes per {{RFC9881}}; ECDSA public keys as the compressed or uncompressed EC point; EdDSA public keys as raw bytes per {{RFC8032}}; and RSA public keys as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}, i.e., the modulus encoded as an unwrapped CBOR unsigned bignum (~biguint) with the exponent omitted when 65537.

~~~
CompositePublicKey = [
  pqc  : any,  ; PQC component public key
  trad : any   ; traditional component public key
]
~~~

The OIDs are the same as for the corresponding composite signature algorithms in {{I-D.ietf-lamps-pq-composite-sigs}}.

~~~
+-------+----------------------------------------------------------+
| Value | Public Key Algorithm                                     |
+=======+==========================================================+
| TBD7  | Name:        pk-MLDSA44-RSA2048-PSS-SHA256               |
|       | Identifiers: id-MLDSA44-RSA2048-PSS-SHA256               |
|       | OID:         1.3.6.1.5.5.7.6.37                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 25               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1312,                   |
|       |              trad: bytes of size 256, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD8  | Name:        pk-MLDSA44-RSA2048-PKCS15-SHA256            |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256            |
|       | OID:         1.3.6.1.5.5.7.6.38                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 26               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1312,                   |
|       |              trad: bytes of size 256, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD9  | Name:        pk-MLDSA44-Ed25519-SHA512                   |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                   |
|       | OID:         1.3.6.1.5.5.7.6.39                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 27               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1312,                   |
|       |              trad: bytes of size 32                      |
+-------+----------------------------------------------------------+
| TBD10 | Name:        pk-MLDSA44-ECDSA-P256-SHA256                |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                |
|       | OID:         1.3.6.1.5.5.7.6.40                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 28               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1312,                   |
|       |              trad: bytes of size 33 or 65                |
+-------+----------------------------------------------------------+
| TBD11 | Name:        pk-MLDSA65-RSA3072-PSS-SHA512               |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.41                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 29               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 384, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD12 | Name:        pk-MLDSA65-RSA3072-PKCS15-SHA512            |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512            |
|       | OID:         1.3.6.1.5.5.7.6.42                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2A               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 384, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD13 | Name:        pk-MLDSA65-RSA4096-PSS-SHA512               |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.43                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2B               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 512, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD14 | Name:        pk-MLDSA65-RSA4096-PKCS15-SHA512            |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512            |
|       | OID:         1.3.6.1.5.5.7.6.44                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2C               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 512, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD15 | Name:        pk-MLDSA65-ECDSA-P256-SHA512                |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.45                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2D               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 33 or 65                |
+-------+----------------------------------------------------------+
| TBD16 | Name:        pk-MLDSA65-ECDSA-P384-SHA512                |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.46                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2E               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 49 or 97                |
+-------+----------------------------------------------------------+
| TBD17 | Name:        pk-MLDSA65-ECDSA-brainpoolP256r1-SHA512     |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512     |
|       | OID:         1.3.6.1.5.5.7.6.47                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 2F               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 33 or 65                |
+-------+----------------------------------------------------------+
| TBD18 | Name:        pk-MLDSA65-Ed25519-SHA512                   |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                   |
|       | OID:         1.3.6.1.5.5.7.6.48                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 30               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1952,                   |
|       |              trad: bytes of size 32                      |
+-------+----------------------------------------------------------+
| TBD19 | Name:        pk-MLDSA87-ECDSA-P384-SHA512                |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.49                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 31               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 2592,                   |
|       |              trad: bytes of size 49 or 97               |
+-------+----------------------------------------------------------+
| TBD20 | Name:        pk-MLDSA87-ECDSA-brainpoolP384r1-SHA512     |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512     |
|       | OID:         1.3.6.1.5.5.7.6.50                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 32               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 2592,                   |
|       |              trad: bytes of size 49 or 97                |
+-------+----------------------------------------------------------+
| TBD21 | Name:        pk-MLDSA87-Ed448-SHAKE256                   |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                   |
|       | OID:         1.3.6.1.5.5.7.6.51                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 33               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 2592,                   |
|       |              trad: bytes of size 57                      |
+-------+----------------------------------------------------------+
| TBD22 | Name:        pk-MLDSA87-RSA3072-PSS-SHA512               |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.52                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 34               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc: bytes of size 2592,                    |
|       |              trad: bytes of size 384, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD23 | Name:        pk-MLDSA87-RSA4096-PSS-SHA512               |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512               |
|       | OID:         1.3.6.1.5.5.7.6.53                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 35               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc: bytes of size 2592,                    |
|       |              trad: bytes of size 512, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD24 | Name:        pk-MLDSA87-ECDSA-P521-SHA512                |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.54                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 36               |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;         |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 2592,                   |
|       |              trad: bytes of size 67 or 133               |
+-------+----------------------------------------------------------+
~~~

## Composite ML-KEM Public Keys

Composite ML-KEM combines an ML-KEM component with a traditional key encapsulation mechanism (RSA-OAEP, ECDH, or X25519/X448) to form a composite KEM for use in X.509 PKI structures.  These algorithms are defined in {{I-D.ietf-lamps-pq-composite-kem}} and assigned OIDs under the PKIX algorithms arc (1.3.6.1.5.5.7.6).  Composite ML-KEM OIDs do not appear in the signature registry, so TBD28 through TBD39 are assigned exclusively to this registry.

In C509, composite KEM public keys are encoded using the same `CompositePublicKey` CDDL structure defined in {{sec-composite-mldsa-pkeys}}, where `pqc` holds the raw ML-KEM public key bytes and `trad` holds the C509 encoding of the traditional public key per {{I-D.ietf-cose-cbor-encoded-cert}}.  ECDH public keys use the compressed or uncompressed ECDH point; X25519 and X448 keys use the raw 32- or 56-byte key per {{RFC7748}}; RSA public keys are encoded as specified in {{Section 3.2.1 of I-D.ietf-cose-cbor-encoded-cert}}.

~~~
+-------+----------------------------------------------------------+
| Value | Public Key Algorithm                                     |
+=======+==========================================================+
| TBD28 | Name:        pk-MLKEM768-RSA2048-SHA3-256                |
|       | Identifiers: id-MLKEM768-RSA2048-SHA3-256                |
|       | OID:         1.3.6.1.5.5.7.6.55                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 37               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 256, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD29 | Name:        pk-MLKEM768-RSA3072-SHA3-256                |
|       | Identifiers: id-MLKEM768-RSA3072-SHA3-256                |
|       | OID:         1.3.6.1.5.5.7.6.56                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 38               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 384, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD30 | Name:        pk-MLKEM768-RSA4096-SHA3-256                |
|       | Identifiers: id-MLKEM768-RSA4096-SHA3-256                |
|       | OID:         1.3.6.1.5.5.7.6.57                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 39               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 512, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD31 | Name:        pk-MLKEM768-X25519-SHA3-256                 |
|       | Identifiers: id-MLKEM768-X25519-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.58                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3A               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 32                      |
+-------+----------------------------------------------------------+
| TBD32 | Name:        pk-MLKEM768-ECDH-P256-SHA3-256              |
|       | Identifiers: id-MLKEM768-ECDH-P256-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.59                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3B               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 33 or 65                |
+-------+----------------------------------------------------------+
| TBD33 | Name:        pk-MLKEM768-ECDH-P384-SHA3-256              |
|       | Identifiers: id-MLKEM768-ECDH-P384-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.60                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3C               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 49 or 97                |
+-------+----------------------------------------------------------+
| TBD34 | Name:        pk-MLKEM768-ECDH-brainpoolP256r1-SHA3-256   |
|       | Identifiers: id-MLKEM768-ECDH-brainpoolP256r1-SHA3-256   |
|       | OID:         1.3.6.1.5.5.7.6.61                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3D               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1184,                   |
|       |              trad: bytes of size 33 or 65                |
+-------+----------------------------------------------------------+
| TBD35 | Name:        pk-MLKEM1024-RSA3072-SHA3-256               |
|       | Identifiers: id-MLKEM1024-RSA3072-SHA3-256               |
|       | OID:         1.3.6.1.5.5.7.6.62                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3E               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1568,                   |
|       |              trad: bytes of size 384, or                 |
|       |                    array of 2 elements                   |
+-------+----------------------------------------------------------+
| TBD36 | Name:        pk-MLKEM1024-ECDH-P384-SHA3-256             |
|       | Identifiers: id-MLKEM1024-ECDH-P384-SHA3-256             |
|       | OID:         1.3.6.1.5.5.7.6.63                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 3F               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1568,                   |
|       |              trad: bytes of size 49 or 97                |
+-------+----------------------------------------------------------+
| TBD37 | Name:        pk-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256  |
|       | Identifiers: id-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256  |
|       | OID:         1.3.6.1.5.5.7.6.64                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 40               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1568,                   |
|       |              trad: bytes of size 49 or 97                |
+-------+----------------------------------------------------------+
| TBD38 | Name:        pk-MLKEM1024-X448-SHA3-256                  |
|       | Identifiers: id-MLKEM1024-X448-SHA3-256                  |
|       | OID:         1.3.6.1.5.5.7.6.65                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 41               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1568,                   |
|       |              trad: bytes of size 56                      |
+-------+----------------------------------------------------------+
| TBD39 | Name:        pk-MLKEM1024-ECDH-P521-SHA3-256             |
|       | Identifiers: id-MLKEM1024-ECDH-P521-SHA3-256             |
|       | OID:         1.3.6.1.5.5.7.6.66                          |
|       | Parameters:  absent                                      |
|       | DER:         06 08 2B 06 01 05 05 07 06 42               |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;          |
|       |              subjectPublicKey: CompositePublicKey;       |
|       |              pqc:  bytes of size 1568,                   |
|       |              trad: bytes of size 67 or 133               |
+-------+----------------------------------------------------------+
~~~

# Security Considerations {#security}

The security considerations of {{I-D.ietf-cose-cbor-encoded-cert}} apply to all entries registered in this document.

For post-quantum algorithms, implementers should consult the respective NIST standards ({{FIPS203}}, {{FIPS204}}) for security level and parameter guidance.

When selecting algorithm value assignments, values for public key algorithms whose OID is the same as the corresponding signature algorithm MUST be assigned the same integer value.  This shared-integer policy is defined in this document to reduce decoder complexity.

# IANA Considerations {#iana}

## C509 Signature Algorithms Registry

IANA is requested to assign values in the "C509 Signature Algorithms" registry under the registry group "CBOR Encoded X.509 (C509) Parameters" for the entries defined in {{sigalg}}.  The registration policy follows the policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

## C509 Public Key Algorithms Registry

IANA is requested to assign values in the "C509 Public Key Algorithms" registry under the registry group "CBOR Encoded X.509 (C509) Parameters" for the entries defined in {{pkalg}}.  The registration policy follows the policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

--- back

# Examples {#examples}

TBD: add examples

# Acknowledgements {#acknowledgements}
{:numbered="false"}

The authors want to thank xxx for reviewing and commenting on intermediate versions of the draft.
