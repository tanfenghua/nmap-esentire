### Weak ephemeral Diffie-Hellman parameter detection for SSL/TLS services.

This script simulates SSL/TLS handshakes using ciphersuites that have ephemeral
Diffie-Hellman as the key exchange algorithm.

Diffie-Hellman MODP group parameters are extracted and analyzed for vulnerability
to Logjam (CVE 2015-4000) and other weaknesses.

Opportunistic STARTTLS sessions are established on services that support them.

### Usage:

`nmap --script ssl-dh-params <target>`

### Sample output:

#### Anonymous Diffie-Hellman Key Exchange MitM Vulnerability
```
| ssl-dh-params: 
|   VULNERABLE:
|   Anonymous Diffie-Hellman Key Exchange MitM Vulnerability
|     State: VULNERABLE
|       Transport Layer Security (TLS) services that use anonymous Diffie-Hellman
|       key exchange only provide protection against passive eavesdropping, and
|       are vulnerable to active man-in-the-middle attacks which could completely
|       compromise the confidentiality and integrity of any data exchanged over
|       the resulting session.
|     Check results:
|       ANONYMOUS DH GROUP 1
|         Cipher Suite: TLS_DH_anon_WITH_AES_256_CBC_SHA
|         Modulus Type: Safe prime
|         Modulus Source: Unknown/Custom-generated
|         Modulus Length: 512 bits
|         Generator Length: 8 bits
|         Public Key Length: 512 bits
|     References:
|       https://www.ietf.org/rfc/rfc2246.txt
```

#### Logjam MitM Vulnerability (CVE 2015-4000)
```
| ssl-dh-params: 
|   VULNERABLE:
|   Transport Layer Security (TLS) Protocol DHE_EXPORT Ciphers Downgrade MitM (Logjam)
|     State: VULNERABLE
|     IDs:  OSVDB:122331  CVE:CVE-2015-4000
|       The Transport Layer Security (TLS) protocol contains a flaw that is triggered
|       when handling Diffie-Hellman key exchanges defined with the DHE_EXPORT cipher.
|       This may allow a man-in-the-middle attacker to downgrade the security of a TLS
|       session to 512-bit export-grade cryptography, which is significantly weaker,
|       allowing the attacker to more easily break the encryption and monitor or tamper
|       with the encrypted stream.
|     Disclosure date: 2015-5-19
|     Check results:
|       EXPORT-GRADE DH GROUP 1
|         Ciphersuite: TLS_DHE_RSA_EXPORT_WITH_DES40_CBC_SHA
|         Modulus Type: Non-safe prime
|         Modulus Source: sun.security.provider/512-bit DSA group with 160-bit prime order subgroup
|         Modulus Length: 512 bits
|         Generator Length: 512 bits
|         Public Key Length: 512 bits
|     References:
|       https://weakdh.org
|       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-4000
|       http://osvdb.org/122331
```

#### Insufficient Diffie-Hellman Group Strength
```
|   Diffie-Hellman Key Exchange Insufficient Group Strength
|     State: VULNERABLE
|       Transport Layer Security (TLS) services that use Diffie-Hellman groups of
|       insufficient strength, especially those using one of a few commonly shared
|       groups, may be susceptible to passive eavesdropping attacks.
|     Check results:
|       WEAK DH GROUP 1
|         Cipher Suite: TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA
|         Modulus Type: Safe prime
|         Modulus Source: Unknown/Custom-generated
|         Modulus Length: 512 bits
|         Generator Length: 8 bits
|         Public Key Length: 512 bits
|     References:
|       https://weakdh.org
```

#### Incorrectly Generated Diffie-Hellman Group Parameters
```
|   Diffie-Hellman Key Exchange Incorrectly Generated Group Parameters
|     State: LIKELY VULNERABLE
|       This TLS service appears to be using non-safe group parameters that do not
|       correspond to any well-known DSA group for Diffie-Hellman key exchange. If the
|       parameters were not generated according to the procedure described in FIPS 186
|       (for example, if the group parameters were randomly generated without checking
|       for the additional properties required for security), this configuration could
|       be exploited by an attacker to recover the encryption keys for any session.
|     Check results:
|       NON-SAFE DH GROUP 1
|         Cipher Suite: TLS_DHE_RSA_WITH_3DES_EDE_CBC_SHA
|         Modulus Type: Non-safe prime
|         Modulus Source: Unknown/Custom-generated
|         Modulus Length: 1024 bits
|         Generator Length: 1024 bits
|         Public Key Length: 1024 bits
|     References:
|       https://weakdh.org
```

Depends on NSE library code currently available only in the development version
of Nmap, which can be obtained by cloning and compiling the Subversion repository
at https://svn.nmap.org/nmap.

