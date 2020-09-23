[![CircleCI](https://circleci.com/gh/open-quantum-safe/openssl/tree/OQS-OpenSSL_1_1_1-stable.svg?style=svg)](https://circleci.com/gh/open-quantum-safe/openssl/tree/OQS-OpenSSL_1_1_1-stable)

openssl_ccac
==================================

[OpenSSL](https://openssl.org/) is an open-source implementation of the TLS protocol and various cryptographic algorithms ([View the original README](https://github.com/open-quantum-safe/openssl/blob/OQS-OpenSSL_1_1_1-stable/README).)

OQS-OpenSSL\_1\_1\_1 is a fork of OpenSSL 1.1.1 that adds quantum-safe key exchange and authentication algorithms using [liboqs](https://github.com/open-quantum-safe/liboqs) for prototyping and evaluation purposes. This fork is not endorsed by the OpenSSL project. This version of openssl add mulan and aigis signature algorithms, base on OQS_OPENSSL_1_1_1-satble.



### Supported Algorithms

If an algorithm is provided by liboqs but is not listed below, it might still be possible to use it in the fork through [either one of two ways](https://github.com/open-quantum-safe/openssl/wiki/Using-liboqs-algorithms-that-are-not-in-the-forks).

#### Key Exchange

The following quantum-safe algorithms from liboqs are supported (assuming they have been enabled in liboqs):

- `oqs_kem_default` (see [here](https://github.com/open-quantum-safe/openssl/wiki/Using-liboqs-algorithms-that-are-not-in-the-forks#oqsdefault) for what this denotes)
- **BIKE**: `bike1l1cpa`, `bike1l3cpa`, `bike1l1fo`, `bike1l3fo`
- **FrodoKEM**: `frodo640aes`, `frodo640shake`, `frodo976aes`, `frodo976shake`, `frodo1344aes`, `frodo1344shake`
- **HQC**: `hqc128_1_cca2`, `hqc192_1_cca2`, `hqc192_2_cca2`, `hqc256_1_cca2`† , `hqc256_2_cca2`†, `hqc256_3_cca2`†
- **Kyber**: `kyber512`, `kyber768`, `kyber1024`, `kyber90s512`, `kyber90s768`, `kyber90s1024`
- **NTRU**: `ntru_hps2048509`, `ntru_hps2048677`, `ntru_hps4096821`, `ntru_hrss701`
- **Saber**: `lightsaber`, `saber`, `firesaber`
- **SIDH** and **SIKE**: `sidhp434`, `sidhp503`, `sidhp610`, `sidhp751`, `sikep434`, `sikep503`, `sikep610`, `sikep751`

If ``<KEX>`` is any of the algorithms listed above, the following hybrid algorithms are supported:

- if `<KEX>` has L1 security, the fork provides the method `p256_<KEX>`, which combine `<KEX>` with ECDH using the P256 curve.
- if `<KEX>` has L3 security, the fork provides the method `p384_<KEX>`, which combines `<KEX>` with ECDH using the P384 curve.
- if `<KEX>` has L5 security, the fork provides the method `p521_<KEX>`, which combines `<KEX>` with ECDH using the P521 curve.

For example, since `kyber768` claims L3 security, the hybrid `p384_kyber768` is available.

Note that algorithms marked with a dagger (†) have large stack usage and may cause failures when run on threads or in constrained environments.

#### Authentication

The following digital signature algorithms from liboqs are supported by the fork. **Note that not all variants of all algorithms are enabled by default; algorithms that are enabled by default are marked with an asterisk, and should you wish to enable additional variants, consult [the "Code Generation" section of the documentation in the wiki](https://github.com/open-quantum-safe/openssl/wiki/Using-liboqs-algorithms-not-in-the-fork#code-generation)**.

- `oqs_sig_default*` (see [here](https://github.com/open-quantum-safe/openssl/wiki/Using-liboqs-algorithms-that-are-not-in-the-forks#oqsdefault) for what this denotes)
- **CRYSTALS-DILITHIUM**: `dilithium2*`, `dilithium3*`, `dilithium4*`
- **Falcon**: `falcon512*`, `falcon1024*`
- **Picnic**: `picnicl1fs`, `picnicl1ur`, `picnicl1full*`, `picnic3l1*`, `picnic3l3`, `picnic3l5`
- **Rainbow**: `rainbowIaclassic*`, `rainbowIacyclic`, `rainbowIacycliccompressed`, `rainbowIIIcclassic`, `rainbowIIIccyclic`, `rainbowIIIccycliccompressed`, `rainbowVcclassic*`, `rainbowVccyclic`, `rainbowVccycliccompressed`
- **SPHINCS-Haraka**: `sphincsharaka128frobust*`, `sphincsharaka128fsimple`, `sphincsharaka128srobust`, `sphincsharaka128ssimple`, `sphincsharaka192frobust`, `sphincsharaka192fsimple`, `sphincsharaka192srobust`, `sphincsharaka192ssimple`, `sphincsharaka256frobust`, `sphincsharaka256fsimple`, `sphincsharaka256srobust`, `sphincsharaka256ssimple`
- **SPHINCS-SHA256**: `sphincssha256128frobust`, `sphincssha256128fsimple`, `sphincssha256128srobust`, `sphincssha256128ssimple`, `sphincssha256192frobust`, `sphincssha256192fsimple`, `sphincssha256192srobust`, `sphincssha256192ssimple`, `sphincssha256256frobust`, `sphincssha256256fsimple`, `sphincssha256256srobust`, `sphincssha256256ssimple`
- **SPHINCS-SHAKE256**: `sphincsshake256128frobust`, `sphincsshake256128fsimple`, `sphincsshake256128srobust`, `sphincsshake256128ssimple`, `sphincsshake256192frobust`, `sphincsshake256192fsimple`, `sphincsshake256192srobust`, `sphincsshake256192ssimple`, `sphincsshake256256frobust`, `sphincsshake256256fsimple`, `sphincsshake256256srobust`, `sphincsshake256256ssimple`
- **mulan**: `mulan`, `p256_mulan`
- **aigis**: `aigis`, `p256_aigis`
The following hybrid algorithms are supported; they combine a quantum-safe algorithm listed above with a traditional digital signature algorithm (`<SIG>` is any one of the algorithms listed above):

- if `<SIG>` has L1 security, then the fork provides the methods `rsa3072_<SIG>` and `p256_<SIG>`, which combine `<SIG>` with RSA3072 and with ECDSA using NIST's P256 curve respectively.
- if `<SIG>` has L3 security, the fork provides the method `p384_<SIG>`, which combines `<SIG>` with ECDSA using NIST's P384 curve.
- if `<SIG>` has L5 security, the fork provides the method `p521_<SIG>`, which combines `<SIG>` with ECDSA using NIST's P521 curve.

For example, since `dilithium2` claims L1 security, the hybrids `rsa3072_dilithium2` and `p256_dilithium2` are available.

## Quickstart

The steps below have been confirmed to work on macOS 10.14 (with clang 10.0.0), Ubuntu 18.04.1 (with gcc-7) and should work on more recent versions of these operating systems/compilers. They have also been confirmed to work on Windows 10 with Visual Studio 2019.

### Building

#### Linux and macOS

#### Step 0: Get pre-requisites

On **Ubuntu**, you need to install the following packages:

	sudo apt install cmake gcc libtool libssl-dev make ninja-build git

Then, get source code of this fork (`<OPENSSL_DIR>` is a directory of your choosing):

	git clone https://github.com/gudengxia/openssl_ccac.git <OPENSSL_DIR>

#### Step 1: Build and install liboqs

The following instructions will download and build liboqs, then install it into a subdirectory inside the OpenSSL folder.

	git clone --branch master https://github.com/gudengxia/liboqs_ccac.git <OPENSS_DIR>
	cd liboqs
	mkdir build && cd build
	cmake .. -GNinja -DCMAKE_INSTALL_PREFIX=<OPENSSL_DIR>/oqs -DUSE_OQS_OPENSSL=OFF -DBUILD_SHARED_LIBS=OFF -DOQS_USE_AVX2_INSTRUCTIONS=OFF
	ninja
	ninja install

Building liboqs requires your system to have (a standard) OpenSSL already installed. `configure` will detect it if it is located in a standard location, such as `/usr` or `/usr/local/opt/openssl` (for brew on macOS).  Otherwise, you may need to specify it with `-DOPENSSL_ROOT_DIR=<path-to-system-openssl-dir>` added to the `cmake` command.

#### Step 2: Build the fork

Now we follow the standard instructions for building OpenSSL. Navigate to `<OPENSSL_DIR>`, and:

on **Ubuntu**, run:

	./Configure no-shared linux-x86_64 -lm --prefix=<path-to-openssl_ccac>
	make -j


	

### Running

#### TLS demo

OpenSSL contains a basic TLS server (`s_server`) and TLS client (`s_client`) which can be used to demonstrate and test TLS connections.

To run a server, you first need to generate an X.509 certificate, using either a classical (`rsa`), quantum-safe (any quantum-safe authentication algorithm in the [Supported Algorithms](#supported-algorithms) section above), or hybrid (any hybrid authentication algorithm in the [Supported Algorithms](#supported-algorithms) section above) algorithm. The server certificate can either be self-signed or part of a chain. In either case, you need to generate a self-signed root CA certificate using the following command, replacing `<SIG>` with an algorithm mentioned above:

	apps/openssl req -x509 -new -newkey <SIG> -keyout <SIG>_CA.key -out <SIG>_CA.crt -nodes -subj "/CN=oqstest CA" -days 365 -config apps/openssl.cnf

If you want an ECDSA certificate (`<SIG>` = `ecdsa`), you instead need to run:

	apps/openssl req -x509 -new -newkey ec:<(apps/openssl ecparam -name secp384r1) -keyout <SIG>_CA.key -out <SIG>_CA.crt -nodes -subj "/CN=oqstest" -days 365 -config apps/openssl.cnf

The root CA certificate can be used directly to start the server (see below), or can be used to issue a server certificate, using the usual OpenSSL process (note that for simplicity, we use the same algorithm for the server and CA certificates; in practice the CA is likely to use a stronger one):

1. The server generates its key pair:

		apps/openssl genpkey -algorithm <SIG> -out <SIG>_srv.key

2. The server generates a certificate request and sends it the to CA:

		apps/openssl req -new -newkey <SIG> -keyout <SIG>_srv.key -out <SIG>_srv.csr -nodes -subj "/CN=oqstest server" -config apps/openssl.cnf

3. The CA generates the signed server certificate:

		apps/openssl x509 -req -in <SIG>_srv.csr -out <SIG>_srv.crt -CA <SIG>_CA.crt -CAkey <SIG>_CA.key -CAcreateserial -days 365

To run a basic TLS server with all possible key-exchange algorithms enabled, run the following command, replacing `<SERVER>` with either `<SIG>_CA` or `<SIG>_srv`:

	apps/openssl s_server -cert <SERVER>.crt -key <SERVER>.key -www -tls1_3

In another terminal window, you can run a TLS client requesting one of the supported key-exchanges (`<KEX>` = one of the quantum-safe or hybrid key exchange algorithms listed in the [Supported Algorithms section above](#key-exchange):

	apps/openssl s_client -groups <KEX> -CAfile <SIG>_CA.crt

#### CMS demo

OpenSSL has facilities to perform signing operations pursuant to [RFC 5652](https://datatracker.ietf.org/doc/rfc5652). This fork can be used to perform such operations with quantum-safe algorithms.

Building on the artifacts created in the TLS setup above (CA and server certificate creation using a specific (quantum-safe) `<SIG>` algorithm), the following command can be used to generate a (quantum-safe) signed file from some input file:

	apps/openssl cms -in inputfile -sign -signer <SIG>_srv.crt -inkey <SIG>_srv.key -nodetach -outform pem -binary -out signedfile.cms 

This command can be used to verify (and extract the contents) of the CMS file resultant from the command above:

	apps/openssl cms -verify -CAfile <SIG>_CA.crt -inform pem -in signedfile.cms -crlfeol -out signeddatafile

The contents of `inputfile` and the resultant `signeddatafile` should be the same.

#### Performance testing

##### TLS end-to-end testing

"Empty" TLS handshakes can be performance tested via the standard `openssl s_time` command. In order to suitably trigger this with an OQS KEM/SIG pair of choice, first follow all steps outlined in the [TLS demo](#tls-demo) section to obtain an OQS-algorithm-signed server certificate. You can then run the performance test in one of two ways:

- Start the server with the desired certificate and key exchange algorithm as follows (`<SERVER>` and `<KEX>` are defined in the [TLS demo](#tls-demo) section above):

```
apps/openssl s_server -cert <SERVER>.crt -key <SERVER>.key -www -tls1_3 -groups <KEX>
```

Then run `apps/openssl s_time`

- Start the server with the desired certificate:

```
apps/openssl s_server -cert <SERVER>.crt -key <SERVER>.key -www -tls1_3
```

and specify the key-exchange algorithm through `s_time` using `apps/openssl s_time -curves <KEX>`.

##### Algorithm speed testing

OpenSSL also has facilities to perform pure speed tests of the cryptographic algorithms supported. This can be used to compare the relative performance of OQS algorithms.

To measure the speed of all KEM algorithms supported by the underlying `liboqs`:

	apps/openssl speed oqskem

Similarly, to measure the speed of all OQS signature algorithms:

	apps/openssl speed oqssig

As with standard OpenSSL, one can also pass a particular algorithm name to be tested, e.g., `apps/openssl speed dilithium2`.

