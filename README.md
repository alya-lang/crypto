# crypto

[![CI](https://github.com/alya-lang/crypto/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/crypto/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/crypto?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fcrypto%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2Fcrypto%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

Comprehensive cryptography and hashing library for Alya.

---

## 🌟 Features

- ⚡ **Lightweight & High Performance**: 180k+ SHA-256 ops/sec, 500k+ Base64 ops/sec in pure Alya
- 🔒 **Cryptographic Hash Functions**: SHA-256, SHA-224, SHA-1, MD5
- 🔑 **Message Authentication Codes**: HMAC (HMAC-SHA256, HMAC-SHA1, HMAC-MD5)
- 🛡️ **Key Derivation (KDF)**: RFC 2898 PBKDF2-HMAC-SHA256 for secure password storage
- 🌐 **Encodings**: Hex, Base64 (RFC 4648), Base64URL (RFC 7515 / JWT-ready)
- ⏱️ **Timing Attack Protection**: Constant-time string and byte equality (`constant_time_eq`)
- 🎲 **Entropy & Randomness**: Cryptographic UUID v4 and random byte generation
- 🧪 **Well Tested**: 100% test coverage against NIST and RFC official test vectors

---

## 📁 Project Architecture

```
crypto/
├── alya.toml               # Package manifest
├── src/
│   ├── lib.alya            # Central public API export facade
│   ├── types.alya          # Configuration and data structures
│   ├── core/
│   │   ├── bits.alya       # 32-bit rotation primitives (rotl32, rotr32)
│   │   └── security.alya   # Timing-safe constant-time comparisons
│   ├── encodings/
│   │   ├── hex.alya        # Hex encoder, decoder, validator
│   │   ├── base64.alya     # Standard Base64 encoder/decoder
│   │   └── base64url.alya  # URL-safe Base64 without padding (RFC 7515)
│   ├── hashes/
│   │   ├── sha256.alya     # SHA-256 & SHA-224 (FIPS 180-4)
│   │   ├── sha1.alya       # SHA-1 (RFC 3174)
│   │   └── md5.alya        # MD5 (RFC 1321)
│   ├── mac/
│   │   └── hmac.alya       # HMAC-SHA256, HMAC-SHA1, HMAC-MD5 (RFC 2104)
│   ├── kdf/
│   │   └── pbkdf2.alya     # PBKDF2-HMAC-SHA256 (RFC 2898)
│   └── random/
│       └── entropy.alya    # UUID v4 and random byte generator
├── examples/
│   └── demo.alya           # Comprehensive runnable demo
├── tests/
│   └── test_basic.alya     # NIST/RFC test vectors suite
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
alyac add crypto --git https://github.com/alya-lang/crypto --branch main
alyac install
```

---

## 🚀 Quick Start

```alya
import "crypto" as crypto

function main()
    # 1. Hashing
    let hash = crypto::sha256("Hello, Alya!")
    say "SHA-256: " + hash

    # 2. HMAC Signatures
    let sig = crypto::hmac_sha256("my-secret-key", "data-to-sign")
    say "HMAC: " + sig

    # 3. Base64 & Base64URL
    let encoded = crypto::base64url_encode("token payload")
    say "Base64URL: " + encoded

    # 4. Constant-Time Verification
    let valid = crypto::constant_time_eq(sig, "expected_signature")
    if valid
        say "Signature verified!"
    end

    # 5. UUID v4 Generation
    let id = crypto::random_uuid()
    say "UUID: " + id
end

main()
```

---

## 📖 API Reference

### Hash Functions
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `sha256(msg)` | `msg: string` | `string` | Computes SHA-256 digest as 64-char hex string. |
| `sha256_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-256 digest returning 32 byte array. |
| `sha224(msg)` | `msg: string` | `string` | Computes SHA-224 digest as 56-char hex string. |
| `sha224_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-224 digest returning 28 byte array. |
| `sha1(msg)` | `msg: string` | `string` | Computes SHA-1 digest as 40-char hex string. |
| `sha1_bytes(bytes)` | `bytes: list` | `list` | Computes SHA-1 digest returning 20 byte array. |
| `md5(msg)` | `msg: string` | `string` | Computes MD5 digest as 32-char hex string. |
| `md5_bytes(bytes)` | `bytes: list` | `list` | Computes MD5 digest returning 16 byte array. |

### Message Authentication (HMAC)
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `hmac_sha256(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA256 digest as hex string. |
| `hmac_sha256_bytes(k, m)` | `k: list, m: list` | `list` | Computes HMAC-SHA256 returning 32 byte array. |
| `hmac_sha1(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-SHA1 digest as hex string. |
| `hmac_md5(key, msg)` | `key: string, msg: string` | `string` | Computes HMAC-MD5 digest as hex string. |

### Encodings & Conversions
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `hex_encode(s)` | `s: string` | `string` | Encodes string to lowercase hexadecimal. |
| `hex_decode(s)` | `s: string` | `string` | Decodes hexadecimal string to original string. |
| `bytes_to_hex(bytes)` | `bytes: list` | `string` | Converts byte array to hex string. |
| `hex_to_bytes(s)` | `s: string` | `list` | Converts hex string to byte array. |
| `is_hex(s)` | `s: string` | `int` | Returns `1` if valid hex string, else `0`. |
| `base64_encode(s)` | `s: string` | `string` | Encodes string to standard Base64 with padding. |
| `base64_decode(s)` | `s: string` | `string` | Decodes standard Base64 string. |
| `base64url_encode(s)` | `s: string` | `string` | URL-safe Base64 without `=` padding (RFC 7515). |
| `base64url_decode(s)` | `s: string` | `string` | Decodes URL-safe Base64 string. |

### Key Derivation (KDF)
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `pbkdf2_hmac_sha256(pwd, salt, iters, len)` | `pwd: str, salt: str, iters: int, len: int` | `string` | Derives key via PBKDF2 returning hex string. |
| `pbkdf2_hmac_sha256_bytes(p, s, iters, len)` | `p: list, s: list, iters: int, len: int` | `list` | Derives key via PBKDF2 returning byte array. |

### Security & Randomness
| Function | Arguments | Returns | Description |
|---|---|---|---|
| `constant_time_eq(a, b)` | `a: string, b: string` | `int` | Constant-time string comparison (timing attack safe). |
| `constant_time_eq_bytes(a, b)` | `a: list, b: list` | `int` | Constant-time byte array comparison. |
| `random_bytes(count)` | `count: int` | `list` | Generates `count` random bytes. |
| `random_hex(count)` | `count: int` | `string` | Generates random hex string of length `count * 2`. |
| `random_uuid()` | none | `string` | Generates RFC 4122 version 4 random UUID. |

---

## 🧪 Running Tests & Benchmarks

```bash
# Run test suite
alyac run tests/test_basic.alya

# Run benchmarks
alyac run benches/bench_basic.alya

# Run demo
alyac run examples/demo.alya
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alyac install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alyac test
   alyac fmt . --check
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
