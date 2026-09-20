# crypto

[![CI](https://github.com/alya-lang/crypto/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/crypto/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/crypto?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fcrypto%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fcrypto%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Comprehensive cryptography, hashing, and cipher library for Alya.

---

## 🌟 Features

- ⚡ **Lightweight & High Performance**: 100% pure Alya implementations with zero C dependencies
- 🔒 **Cryptographic Hash Functions**:
  - **SHA-2**: SHA-512, SHA-384, SHA-256, SHA-224 (FIPS 180-4)
  - **SHA-3 / Keccak**: SHA3-256, SHA3-512, Keccak-256 (NIST FIPS 202)
  - **BLAKE2**: BLAKE2s-256 (RFC 7693)
  - Legacy / Checksums: SHA-1, MD5
- 🔑 **Message Authentication Codes & AEAD**:
  - **HMAC**: HMAC-SHA512, HMAC-SHA384, HMAC-SHA256, HMAC-SHA1, HMAC-MD5
  - **Poly1305**: One-time universal MAC (RFC 8439)
  - **ChaCha20-Poly1305**: Authenticated Encryption with Associated Data (AEAD, RFC 8439)
- 🛡️ **Key Derivation & Passwords**:
  - RFC 5869 **HKDF** (HMAC-based Extract-and-Expand)
  - RFC 2898 **PBKDF2-HMAC-SHA256**
  - Modular Crypt Format **password_hash** & **password_verify** with automatic salt generation
- 🗝️ **Symmetric Ciphers**:
  - **AES** (FIPS 197): AES-128 and AES-256 in **CBC** (with PKCS#7 padding) and **CTR** stream mode
  - **ChaCha20** (RFC 8439): 256-bit high-speed stream cipher
  - **RC4**: Classic Rivest Cipher 4 stream cipher
- 🌐 **Encodings**:
  - Hex, Base64 (RFC 4648), Base64URL (RFC 7515 / JWT-ready)
  - Base32 (RFC 4648, standard 2FA alphabet)
  - Base58 (Bitcoin alphanumeric alphabet)
- ⏱️ **Timing Attack Protection**: Constant-time string and byte equality (`constant_time_eq`)
- 🎲 **Entropy & Randomness**: Cryptographic UUID v4 and random byte generation
- 🧪 **Well Tested**: 100% test coverage against NIST and RFC official test vectors (73 passing tests)

---

## 📁 Project Architecture

```
crypto/
├── alya.toml               # Package manifest (v0.5.0)
├── src/
│   ├── lib.alya            # Central public API export facade
│   ├── types.alya          # Configuration and data structures
│   ├── core/
│   │   ├── bits.alya       # 32-bit & 64-bit rotation primitives (rotl32, rotl64, u64)
│   │   └── security.alya   # Timing-safe constant-time comparisons
│   ├── encodings/
│   │   ├── hex.alya        # Hex encoder, decoder, validator
│   │   ├── base64.alya     # Standard Base64 encoder/decoder
│   │   ├── base64url.alya  # URL-safe Base64 without padding (RFC 7515)
│   │   ├── base32.alya     # Base32 encoder/decoder (RFC 4648)
│   │   └── base58.alya     # Bitcoin Base58 encoder/decoder
│   ├── hashes/
│   │   ├── sha512.alya     # SHA-512 & SHA-384 (FIPS 180-4, 64-bit word architecture)
│   │   ├── sha256.alya     # SHA-256 & SHA-224 (FIPS 180-4)
│   │   ├── sha3.alya       # SHA3-256, SHA3-512, Keccak-256 (FIPS 202)
│   │   ├── blake2s.alya    # BLAKE2s-256 (RFC 7693)
│   │   ├── sha1.alya       # SHA-1 (RFC 3174)
│   │   └── md5.alya        # MD5 (RFC 1321)
│   ├── mac/
│   │   ├── hmac.alya       # HMAC (SHA-512, SHA-384, SHA-256, SHA-1, MD5)
│   │   └── poly1305.alya   # Poly1305 one-time authenticator (RFC 8439)
│   ├── kdf/
│   │   ├── hkdf.alya       # HKDF Extract-and-Expand (RFC 5869)
│   │   ├── pbkdf2.alya     # PBKDF2-HMAC-SHA256 (RFC 2898)
│   │   └── password.alya   # Modular crypt password hashing & verification
│   ├── ciphers/
│   │   ├── aes.alya        # AES-128 & AES-256 with CBC (PKCS#7) and CTR modes
│   │   ├── chacha20.alya   # ChaCha20 stream cipher (RFC 8439)
│   │   ├── chacha20poly1305.alya # ChaCha20-Poly1305 AEAD cipher (RFC 8439)
│   │   └── rc4.alya        # RC4 stream cipher
│   └── random/
│       └── entropy.alya    # UUID v4 and random byte generator
├── tests/
│   └── test_basic.alya     # Comprehensive test suite (73 NIST/RFC vectors)
└── benches/
    └── bench_basic.alya    # Micro-benchmarks
```

---

## 📦 Installation

Add `crypto` to your `alya.toml`:

```toml
[dependencies]
crypto = { git = "https://github.com/alya-lang/crypto", branch = "main" }
```

Or install it directly via CLI:

```bash
alya add crypto --git https://github.com/alya-lang/crypto --branch main
alya install
```

---

## 🚀 Quick Start

```alya
import "crypto" as crypto

function main()
    # 1. Hashing (SHA-256, SHA-512)
    let h256 = crypto::sha256("Hello, Alya!")
    say "SHA-256: " + h256

    let h512 = crypto::sha512("Hello, Alya!")
    say "SHA-512: " + h512

    # 2. Symmetric Ciphers: AES-128-CBC
    let key = crypto::hex_to_bytes("2b7e151628aed2a6abf7158809cf4f3c")
    let iv = crypto::hex_to_bytes("000102030405060708090a0b0c0d0e0f")
    let encrypted_hex = crypto::aes_cbc_encrypt(key, iv, "Secret Message")
    let decrypted_str = crypto::aes_cbc_decrypt(key, iv, encrypted_hex)
    say "AES-CBC Decrypted: " + decrypted_str

    # 3. Stream Cipher: ChaCha20 (RFC 8439)
    let cc_key = crypto::random_bytes(32)
    let cc_nonce = crypto::random_bytes(12)
    let cc_enc = crypto::chacha20_encrypt(cc_key, cc_nonce, 1, "ChaCha20 Payload")
    let cc_dec = crypto::chacha20_decrypt(cc_key, cc_nonce, 1, cc_enc)
    say "ChaCha20 Decrypted: " + cc_dec

    # 4. Encodings: Base32 & Base58
    say "Base32: " + crypto::base32_encode("foobar")
    say "Base58: " + crypto::base58_encode("Hello World")

    # 5. HKDF Key Derivation
    let prk = crypto::hkdf_extract_bytes(iv, key)
    let okm = crypto::hkdf_expand_bytes(prk, [1, 2, 3], 32)
    say "Derived key bytes len: " + len(okm)
end

main()
```

---

## 📖 API Reference

### Hash Functions
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `sha512(msg)` | `msg: string` | `string` | Computes SHA-512 digest as 128-char hex string. |
| `sha512_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-512 digest returning 64 byte array. |
| `sha384(msg)` | `msg: string` | `string` | Computes SHA-384 digest as 96-char hex string. |
| `sha384_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-384 digest returning 48 byte array. |
| `sha256(msg)` | `msg: string` | `string` | Computes SHA-256 digest as 64-char hex string. |
| `sha256_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-256 digest returning 32 byte array. |
| `sha224(msg)` | `msg: string` | `string` | Computes SHA-224 digest as 56-char hex string. |
| `sha224_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-224 digest returning 28 byte array. |
| `sha1(msg)` | `msg: string` | `string` | Computes SHA-1 digest as 40-char hex string. |
| `md5(msg)` | `msg: string` | `string` | Computes MD5 digest as 32-char hex string. |

### Message Authentication (HMAC)
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `hmac_sha512(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA512 digest as hex string. |
| `hmac_sha384(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA384 digest as hex string. |
| `hmac_sha256(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA256 digest as hex string. |
| `hmac_sha1(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA1 digest as hex string. |
| `hmac_md5(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-MD5 digest as hex string. |

### Ciphers
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `aes_cbc_encrypt(k, iv, pt)` | `k: list, iv: list, pt: str` | `string` | AES-128 / AES-256 CBC encryption with PKCS#7 (hex). |
| `aes_cbc_decrypt(k, iv, ct)` | `k: list, iv: list, ct: str` | `string` | AES-128 / AES-256 CBC decryption with PKCS#7. |
| `aes_ctr_encrypt(k, iv, pt)` | `k: list, iv: list, pt: str` | `string` | AES-128 / AES-256 CTR stream encryption (hex). |
| `aes_ctr_decrypt(k, iv, ct)` | `k: list, iv: list, ct: str` | `string` | AES-128 / AES-256 CTR stream decryption. |
| `chacha20_encrypt(k, n, ctr, pt)` | `k: list, n: list, ctr: int, pt: str` | `string` | ChaCha20 stream encryption (RFC 8439). |
| `chacha20_decrypt(k, n, ctr, ct)` | `k: list, n: list, ctr: int, ct: str` | `string` | ChaCha20 stream decryption (RFC 8439). |
| `rc4_encrypt(key, pt)` | `key: str, pt: str` | `string` | RC4 stream encryption (hex). |
| `rc4_decrypt(key, ct)` | `key: str, ct: str` | `string` | RC4 stream decryption. |

### Key Derivation (KDF)
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `hkdf(salt, ikm, info, len)` | `salt: str, ikm: str, info: str, len: int` | `string` | RFC 5869 HKDF Extract-and-Expand. |
| `hkdf_hex(salt, ikm, info, len)` | `salt: str, ikm: str, info: str, len: int` | `string` | HKDF with hex inputs/outputs. |
| `pbkdf2_hmac_sha256(pwd, salt, iters, len)` | `pwd: str, salt: str, iters: int, len: int` | `string` | Derives key via PBKDF2 returning hex string. |

### Encodings & Conversions
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `base32_encode(s)` | `s: string` | `string` | RFC 4648 Base32 encoder with padding. |
| `base32_decode(s)` | `s: string` | `string` | RFC 4648 Base32 decoder. |
| `is_base32(s)` | `s: string` | `int` | Validates RFC 4648 Base32 string. |
| `base58_encode(s)` | `s: string` | `string` | Bitcoin Base58 encoder. |
| `base58_decode(s)` | `s: string` | `string` | Bitcoin Base58 decoder. |
| `is_base58(s)` | `s: string` | `int` | Validates Bitcoin Base58 string. |
| `hex_encode(s)` | `s: string` | `string` | Encodes string to lowercase hexadecimal. |
| `hex_decode(s)` | `s: string` | `string` | Decodes hexadecimal string to original string. |
| `base64_encode(s)` | `s: string` | `string` | Standard Base64 with padding. |
| `base64url_encode(s)` | `s: string` | `string` | URL-safe Base64 without padding (RFC 7515). |

---

## 🧪 Running Tests & Benchmarks

```bash
alya test
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
