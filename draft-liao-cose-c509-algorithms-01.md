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
  RFC8391:
  RFC8554:
  RFC9708:
  RFC9802:
  RFC9881:
  RFC9935:
  RFC8949:
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
  RFC5958:
  RFC9964:
  I-D.ietf-jose-pq-composite-sigs:

--- abstract

This document registers additional algorithms in the IANA registries defined by {{I-D.ietf-cose-cbor-encoded-cert}}.  It extends the C509 certificate specification with post-quantum cryptography (PQC) signature and key encapsulation algorithms: ML-DSA, ML-KEM, the stateful hash-based signature algorithms HSS/LMS, XMSS, and XMSS^MT, and composite ML-DSA and ML-KEM algorithms.

--- middle

# Introduction {#intro}

{{I-D.ietf-cose-cbor-encoded-cert}} defines C509, a compact CBOR encoding of X.509 certificates and certification requests, and establishes a set of IANA registries, including the "C509 Signature Algorithms" and "C509 Public Key Algorithms" registries.  That specification populates these registries with commonly used values and anticipates that new algorithms, including post-quantum algorithms, are registered as they are specified.

This document adds entries to the C509 Signature Algorithms and C509 Public Key Algorithms registries for PQC signature and key encapsulation algorithms: ML-DSA, ML-KEM, the stateful hash-based signature algorithms HSS/LMS, XMSS, and XMSS^MT, and composite ML-DSA and ML-KEM algorithms.

All new values are registered in the "CBOR Encoded X.509 (C509)" registry group, following the registration procedures defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

# Conventions and Definitions {#conventions}

{::boilerplate bcp14-tagged}

This document uses the terminology and registry structure defined in {{I-D.ietf-cose-cbor-encoded-cert}}.  The CBOR terminology of {{RFC8949}} applies.

# Additional Signature Algorithms {#sigalg}

This section defines additional entries for the C509 Signature Algorithms registry established in {{I-D.ietf-cose-cbor-encoded-cert}}.

## ML-DSA (Module-Lattice-Based Digital Signature Algorithm)

ML-DSA is a lattice-based digital signature algorithm standardized by NIST as {{FIPS204}} and specified for use in X.509 PKI by {{RFC9881}}.  In C509, the signature value (`issuerSignatureValue` in a C509 certificate, `subjectSignatureValue` in a C509 certification request) is a byte string containing the raw ML-DSA signature.

For natively signed C509 certificates and certification requests, the ML-DSA signature is computed over the CBOR-encoded TBSCertificate or TBSCertificationRequest, respectively, with the context string (ctx) set to the empty string, as specified in {{RFC9881}} for the DER-encoded structures.

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

All three are stateful signature schemes.  Their signature sizes depend on the chosen parameter set, while their public key sizes depend only on the hash output length; see {{sec-hbs-pkeys}}.

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

In C509, the signature value is a byte string containing the composite signature value as serialized by SerializeSignatureValue in {{I-D.ietf-lamps-pq-composite-sigs}}, i.e., the ML-DSA signature followed by the traditional signature, exactly as in X.509.  The traditional component is not re-encoded.  In particular, ECDSA signatures remain DER-encoded Ecdsa-Sig-Value structures, so the size of composite signatures with an ECDSA component varies slightly.  This is the same encoding as used for composite ML-DSA signatures in JOSE and COSE {{I-D.ietf-jose-pq-composite-sigs}}, so composite signature values need no conversion between X.509, C509, and COSE.

For natively signed C509 certificates and certification requests, the composite signature is computed with the Composite-ML-DSA.Sign function of {{I-D.ietf-lamps-pq-composite-sigs}}, with the message M set to the CBOR-encoded TBSCertificate or TBSCertificationRequest, respectively, and the application context string (ctx) set to the empty string.

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
|       |              signature: bytes of size 2676                |
+-------+-----------------------------------------------------------+
| TBD8  | Name:        sa-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | OID:         1.3.6.1.5.5.7.6.38                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 26          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 2676                |
+-------+-----------------------------------------------------------+
| TBD9  | Name:        sa-MLDSA44-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.39                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 27          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 2484                |
+-------+-----------------------------------------------------------+
| TBD10 | Name:        sa-MLDSA44-ECDSA-P256-SHA256                 |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                 |
|       | OID:         1.3.6.1.5.5.7.6.40                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 28          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 2492        |
+-------+-----------------------------------------------------------+
| TBD11 | Name:        sa-MLDSA65-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.41                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 29          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 3693                |
+-------+-----------------------------------------------------------+
| TBD12 | Name:        sa-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.42                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2A          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 3693                |
+-------+-----------------------------------------------------------+
| TBD13 | Name:        sa-MLDSA65-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.43                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2B          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 3821                |
+-------+-----------------------------------------------------------+
| TBD14 | Name:        sa-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.44                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2C          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 3821                |
+-------+-----------------------------------------------------------+
| TBD15 | Name:        sa-MLDSA65-ECDSA-P256-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.45                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2D          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 3381        |
+-------+-----------------------------------------------------------+
| TBD16 | Name:        sa-MLDSA65-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.46                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2E          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 3413        |
+-------+-----------------------------------------------------------+
| TBD17 | Name:        sa-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.47                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2F          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 3381        |
+-------+-----------------------------------------------------------+
| TBD18 | Name:        sa-MLDSA65-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.48                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 30          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 3373                |
+-------+-----------------------------------------------------------+
| TBD19 | Name:        sa-MLDSA87-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.49                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 31          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 4731        |
+-------+-----------------------------------------------------------+
| TBD20 | Name:        sa-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.50                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 32          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 4731        |
+-------+-----------------------------------------------------------+
| TBD21 | Name:        sa-MLDSA87-Ed448-SHAKE256                    |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                    |
|       | OID:         1.3.6.1.5.5.7.6.51                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 33          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 4741                |
+-------+-----------------------------------------------------------+
| TBD22 | Name:        sa-MLDSA87-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.52                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 34          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 5011                |
+-------+-----------------------------------------------------------+
| TBD23 | Name:        sa-MLDSA87-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.53                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 35          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size 5139                |
+-------+-----------------------------------------------------------+
| TBD24 | Name:        sa-MLDSA87-ECDSA-P521-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.54                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 36          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              signature: bytes of size at most 4766        |
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

## Stateful Hash-Based Public Keys {#sec-hbs-pkeys}

HSS/LMS, XMSS, and XMSS^MT public keys in X.509 certificates are encoded as specified in {{RFC9802}}.  In C509, the `subjectPublicKey` field is a byte string containing the raw public key, i.e., the `hss_public_key` structure of {{RFC8554}} or the `xmss_public_key` or `xmssmt_public_key` structure of {{RFC8391}}.  Its size depends only on the hash output length n of the parameter set:

* HSS/LMS: 4 + 24 + n bytes, i.e., 52 bytes for n = 24 and 60 bytes for n = 32.
* XMSS and XMSS^MT: 4 + 2n bytes, i.e., 52 bytes for n = 24, 68 bytes for n = 32, and 132 bytes for n = 64.  The values TBD4, TBD5, and TBD6 are the same as in the C509 Signature Algorithms registry.

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
|       |              subjectPublicKey: bytes of size 52 or 60     |
+-------+-----------------------------------------------------------+
| TBD5  | Name:        XMSS Hash-based Signature                    |
|       | Identifiers: id-alg-xmss-hashsig                          |
|       | OID:         1.3.6.1.5.5.7.6.34                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 22          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              subjectPublicKey: bytes of size 52, 68, or   |
|       |                                132                        |
+-------+-----------------------------------------------------------+
| TBD6  | Name:        XMSS^MT Hash-based Signature                 |
|       | Identifiers: id-alg-xmssmt-hashsig                        |
|       | OID:         1.3.6.1.5.5.7.6.35                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 23          |
|       | Comments:    RFC 8391; RFC 9802;                          |
|       |              subjectPublicKey: bytes of size 52, 68, or   |
|       |                                132                        |
+-------+-----------------------------------------------------------+
~~~

## Composite ML-DSA Public Keys {#sec-composite-mldsa-pkeys}

In C509, the `subjectPublicKey` field is a byte string containing the composite public key as serialized by SerializePublicKey in {{I-D.ietf-lamps-pq-composite-sigs}}, i.e., the ML-DSA public key followed by the traditional public key, exactly as in X.509.  The traditional component is not re-encoded: RSA public keys remain DER-encoded RSAPublicKey structures, and ECDSA public keys remain uncompressed points.  This is the same encoding as used for the `pub` parameter of composite ML-DSA keys in JOSE and COSE {{I-D.ietf-jose-pq-composite-sigs}}.  The sizes given below for RSA-based composites assume the public exponent 65537.

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
|       |              subjectPublicKey: bytes of size 1582         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD8  | Name:        pk-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | Identifiers: id-MLDSA44-RSA2048-PKCS15-SHA256             |
|       | OID:         1.3.6.1.5.5.7.6.38                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 26          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 1582         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD9  | Name:        pk-MLDSA44-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA44-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.39                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 27          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 1344         |
+-------+-----------------------------------------------------------+
| TBD10 | Name:        pk-MLDSA44-ECDSA-P256-SHA256                 |
|       | Identifiers: id-MLDSA44-ECDSA-P256-SHA256                 |
|       | OID:         1.3.6.1.5.5.7.6.40                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 28          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 1377         |
+-------+-----------------------------------------------------------+
| TBD11 | Name:        pk-MLDSA65-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.41                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 29          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2350         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD12 | Name:        pk-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA3072-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.42                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2A          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2350         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD13 | Name:        pk-MLDSA65-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA65-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.43                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2B          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2478         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD14 | Name:        pk-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | Identifiers: id-MLDSA65-RSA4096-PKCS15-SHA512             |
|       | OID:         1.3.6.1.5.5.7.6.44                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2C          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2478         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD15 | Name:        pk-MLDSA65-ECDSA-P256-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P256-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.45                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2D          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2017         |
+-------+-----------------------------------------------------------+
| TBD16 | Name:        pk-MLDSA65-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA65-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.46                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2E          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2049         |
+-------+-----------------------------------------------------------+
| TBD17 | Name:        pk-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | Identifiers: id-MLDSA65-ECDSA-brainpoolP256r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.47                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 2F          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2017         |
+-------+-----------------------------------------------------------+
| TBD18 | Name:        pk-MLDSA65-Ed25519-SHA512                    |
|       | Identifiers: id-MLDSA65-Ed25519-SHA512                    |
|       | OID:         1.3.6.1.5.5.7.6.48                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 30          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 1984         |
+-------+-----------------------------------------------------------+
| TBD19 | Name:        pk-MLDSA87-ECDSA-P384-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P384-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.49                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 31          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2689         |
+-------+-----------------------------------------------------------+
| TBD20 | Name:        pk-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | Identifiers: id-MLDSA87-ECDSA-brainpoolP384r1-SHA512      |
|       | OID:         1.3.6.1.5.5.7.6.50                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 32          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2689         |
+-------+-----------------------------------------------------------+
| TBD21 | Name:        pk-MLDSA87-Ed448-SHAKE256                    |
|       | Identifiers: id-MLDSA87-Ed448-SHAKE256                    |
|       | OID:         1.3.6.1.5.5.7.6.51                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 33          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2649         |
+-------+-----------------------------------------------------------+
| TBD22 | Name:        pk-MLDSA87-RSA3072-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA3072-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.52                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 34          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2990         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD23 | Name:        pk-MLDSA87-RSA4096-PSS-SHA512                |
|       | Identifiers: id-MLDSA87-RSA4096-PSS-SHA512                |
|       | OID:         1.3.6.1.5.5.7.6.53                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 35          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 3118         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD24 | Name:        pk-MLDSA87-ECDSA-P521-SHA512                 |
|       | Identifiers: id-MLDSA87-ECDSA-P521-SHA512                 |
|       | OID:         1.3.6.1.5.5.7.6.54                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 36          |
|       | Comments:    draft-ietf-lamps-pq-composite-sigs;          |
|       |              subjectPublicKey: bytes of size 2725         |
+-------+-----------------------------------------------------------+
~~~

## Composite ML-KEM Public Keys

Composite ML-KEM combines an ML-KEM component with a traditional key establishment algorithm (RSA-OAEP, ECDH, X25519, or X448) into a single KEM for use in X.509 PKI.  These algorithms are defined in {{I-D.ietf-lamps-pq-composite-kem}}, which assigns their OIDs under the PKIX algorithms arc (1.3.6.1.5.5.7.6).  These entries have no counterpart in the C509 Signature Algorithms registry.

In C509, the `subjectPublicKey` field is a byte string containing the composite public key as serialized by SerializePublicKey in {{I-D.ietf-lamps-pq-composite-kem}}, i.e., the ML-KEM public key followed by the traditional public key, exactly as in X.509.  As for composite ML-DSA ({{sec-composite-mldsa-pkeys}}), the traditional component is not re-encoded, and the sizes given below for RSA-based composites assume the public exponent 65537.

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
|       |              subjectPublicKey: bytes of size 1454         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD29 | Name:        pk-MLKEM768-RSA3072-SHA3-256                 |
|       | Identifiers: id-MLKEM768-RSA3072-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.56                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 38          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1582         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD30 | Name:        pk-MLKEM768-RSA4096-SHA3-256                 |
|       | Identifiers: id-MLKEM768-RSA4096-SHA3-256                 |
|       | OID:         1.3.6.1.5.5.7.6.57                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 39          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1710         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD31 | Name:        pk-MLKEM768-X25519-SHA3-256                  |
|       | Identifiers: id-MLKEM768-X25519-SHA3-256                  |
|       | OID:         1.3.6.1.5.5.7.6.58                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3A          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1216         |
+-------+-----------------------------------------------------------+
| TBD32 | Name:        pk-MLKEM768-ECDH-P256-SHA3-256               |
|       | Identifiers: id-MLKEM768-ECDH-P256-SHA3-256               |
|       | OID:         1.3.6.1.5.5.7.6.59                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3B          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1249         |
+-------+-----------------------------------------------------------+
| TBD33 | Name:        pk-MLKEM768-ECDH-P384-SHA3-256               |
|       | Identifiers: id-MLKEM768-ECDH-P384-SHA3-256               |
|       | OID:         1.3.6.1.5.5.7.6.60                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3C          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1281         |
+-------+-----------------------------------------------------------+
| TBD34 | Name:        pk-MLKEM768-ECDH-brainpoolP256r1-SHA3-256    |
|       | Identifiers: id-MLKEM768-ECDH-brainpoolP256r1-SHA3-256    |
|       | OID:         1.3.6.1.5.5.7.6.61                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3D          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1249         |
+-------+-----------------------------------------------------------+
| TBD35 | Name:        pk-MLKEM1024-RSA3072-SHA3-256                |
|       | Identifiers: id-MLKEM1024-RSA3072-SHA3-256                |
|       | OID:         1.3.6.1.5.5.7.6.62                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3E          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1966         |
|       |              (with RSA public exponent 65537)             |
+-------+-----------------------------------------------------------+
| TBD36 | Name:        pk-MLKEM1024-ECDH-P384-SHA3-256              |
|       | Identifiers: id-MLKEM1024-ECDH-P384-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.63                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 3F          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1665         |
+-------+-----------------------------------------------------------+
| TBD37 | Name:        pk-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256   |
|       | Identifiers: id-MLKEM1024-ECDH-brainpoolP384r1-SHA3-256   |
|       | OID:         1.3.6.1.5.5.7.6.64                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 40          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1665         |
+-------+-----------------------------------------------------------+
| TBD38 | Name:        pk-MLKEM1024-X448-SHA3-256                   |
|       | Identifiers: id-MLKEM1024-X448-SHA3-256                   |
|       | OID:         1.3.6.1.5.5.7.6.65                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 41          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1624         |
+-------+-----------------------------------------------------------+
| TBD39 | Name:        pk-MLKEM1024-ECDH-P521-SHA3-256              |
|       | Identifiers: id-MLKEM1024-ECDH-P521-SHA3-256              |
|       | OID:         1.3.6.1.5.5.7.6.66                           |
|       | Parameters:  Absent                                       |
|       | DER:         30 0A 06 08 2B 06 01 05 05 07 06 42          |
|       | Comments:    draft-ietf-lamps-pq-composite-kem;           |
|       |              subjectPublicKey: bytes of size 1701         |
+-------+-----------------------------------------------------------+
~~~

# Use in Other C509 Structures {#other}

## Private Keys {#privkeys}

{{Section 3.6 of I-D.ietf-cose-cbor-encoded-cert}} defines the `C509PrivateKey` structure, whose `subjectPrivateKeyAlgorithm` field takes values from the C509 Public Key Algorithms registry.  The entries registered in {{pkalg}} can therefore be used in `C509PrivateKey` with any C509 private key type, without further definitions in this document.

For example, with C509PrivateKeyType 0, `subjectPrivateKey` is the PrivateKey OCTET STRING value field of the OneAsymmetricKey structure {{RFC5958}}, encoded as a CBOR byte string.  For the algorithms in this document, the PrivateKey content is defined in the following specifications:

* ML-DSA: the `ML-DSA-44-PrivateKey`, `ML-DSA-65-PrivateKey`, or `ML-DSA-87-PrivateKey` defined in {{RFC9881}}, i.e., the 32-byte seed, the expanded private key, or both.
* ML-KEM: the `ML-KEM-512-PrivateKey`, `ML-KEM-768-PrivateKey`, or `ML-KEM-1024-PrivateKey` defined in {{RFC9935}}, i.e., the 64-byte seed, the expanded private key, or both.
* Composite ML-DSA and composite ML-KEM: the composite private key defined in {{I-D.ietf-lamps-pq-composite-sigs}} and {{I-D.ietf-lamps-pq-composite-kem}}, respectively.

With C509PrivateKeyType 1, `subjectPrivateKey` is a COSE_Key.  For example, ML-DSA private keys can be represented as COSE_Key using the Algorithm Key Pair (AKP) key type defined in {{RFC9964}}, and composite ML-DSA private keys using the AKP key type as specified in {{I-D.ietf-jose-pq-composite-sigs}}.

## Certification Requests {#csr}

A C509 certification request is signed by the subject using the algorithm in `subjectSignatureAlgorithm` ({{Section 4.2 of I-D.ietf-cose-cbor-encoded-cert}}).  ML-KEM and composite ML-KEM keys cannot produce signatures, so a C509 certification request for such a key cannot prove possession of the private key with its own signature.  Another proof-of-possession mechanism is required, which this document does not define.

# Security Considerations {#security}

The security considerations of {{I-D.ietf-cose-cbor-encoded-cert}} apply to all entries registered in this document.

For post-quantum algorithms, implementers should consult the respective NIST standards ({{FIPS203}}, {{FIPS204}}) for guidance on security levels and parameter sets.

HSS/LMS, XMSS, and XMSS^MT are stateful signature schemes: each one-time key MUST be used to sign at most one message, and the signer has to update its private key state reliably after each signature.  Reusing a one-time key undermines the security of the scheme; see the security considerations of {{RFC8554}}, {{RFC8391}}, and {{RFC9802}}.  Copying or restoring the private key state, for example by exporting it in a private key structure, can lead to such reuse.

# IANA Considerations {#iana}

## C509 Signature Algorithms Registry

IANA is requested to register the entries defined in {{sigalg}} in the "C509 Signature Algorithms" registry within the "CBOR Encoded X.509 (C509)" registry group, following the registration policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

## C509 Public Key Algorithms Registry

IANA is requested to register the entries defined in {{pkalg}} in the "C509 Public Key Algorithms" registry within the "CBOR Encoded X.509 (C509)" registry group, following the registration policy defined in {{I-D.ietf-cose-cbor-encoded-cert}}.

Per {{Section 8.15 of I-D.ietf-cose-cbor-encoded-cert}}, the public key algorithm entries TBD1 through TBD24 MUST be assigned the same values as the corresponding entries in the "C509 Signature Algorithms" registry.

## Suggested Values

*RFC Editor's Note:* Please remove this section before publication.

Values in the interval \[0, 23\] are encoded in one byte, and values in the interval \[24, 255\] in two bytes ({{Section 8.1 of I-D.ietf-cose-cbor-encoded-cert}}).  Negative values are used in {{I-D.ietf-cose-cbor-encoded-cert}} mainly for deprecated algorithms, such as those based on SHA-1, and are not suggested for the algorithms in this document.  The public keys and signatures of these algorithms are hundreds to thousands of bytes long, so saving one byte per algorithm identifier has little effect on certificate size, whereas the one-byte interval is small.  The following assignments are therefore suggested:

* ML-DSA (TBD1 through TBD3): values from the interval \[0, 23\] that are unassigned in both registries, as ML-DSA is expected to be widely used.
* ML-KEM (TBD25 through TBD27): values from the interval \[0, 23\] that are unassigned in the "C509 Public Key Algorithms" registry.  These values need not be unassigned in the "C509 Signature Algorithms" registry.
* HSS/LMS, XMSS, and XMSS^MT (TBD4 through TBD6): values from the interval \[24, 255\].
* Composite ML-DSA (TBD7 through TBD24): a contiguous block of values from the interval \[24, 255\] that are unassigned in both registries.
* Composite ML-KEM (TBD28 through TBD39): a contiguous block of values from the interval \[24, 255\].

--- back

# Examples {#examples}

TBD: add examples

# Change Log {#changelog}
{:numbered="false"}

*RFC Editor's Note:* Please remove this section before publication.

## Changes from -00 to -01
{:numbered="false"}

* Aligned with draft-ietf-cose-cbor-encoded-cert-21:
  * Used the registry group name "CBOR Encoded X.509 (C509)".
  * Gave the DER column as the DER encoding of the complete AlgorithmIdentifier and wrote "Absent" for absent parameters.
  * Referred to the signature value as `issuerSignatureValue` or `subjectSignatureValue`.
  * Referenced Section 8.15 of that document for assigning the same value in the signature and public key algorithm registries, and moved this requirement from Security Considerations to IANA Considerations.
* Changed the encoding of composite public keys and signatures from a two-element CBOR array to the serialization defined in {{I-D.ietf-lamps-pq-composite-sigs}} and {{I-D.ietf-lamps-pq-composite-kem}}, i.e., the same byte string as in X.509.
* Specified the context string for natively signed ML-DSA and composite ML-DSA signatures.
* Specified the public key sizes of HSS/LMS, XMSS, and XMSS^MT.
* Added suggested value ranges to the IANA considerations.
* Added a section on the use of the registered algorithms in C509 private keys and certification requests.
* Added security considerations for stateful hash-based signature algorithms.
* Removed unused references.
* Editorial improvements.

# Acknowledgements {#acknowledgements}
{:numbered="false"}

The authors would like to thank xxx for reviewing and commenting on earlier versions of this document.
