# Developer Guide: GhostPass Engine

Technical implementation details for developers integrating with the GhostPass v8.1 standard.

## 📐 Data Architecture
GhostPass v8.1 uses a **Position-Based Mapping** approach rather than raw bit-stream serialization. 

- **Dictionary**: 2048 words (BIP-39 English).
- **Indexing**: Each word index is an 11-bit integer (0 to 2047).
- **Phrase Length**: Exactly 12 words.

### Word Mapping Logic
The engine takes an object containing 11 numeric fields and maps them to word indices. **A `masterSecret` (Salt) is required for both encoding and decoding.**

```typescript
// Example Encoding with Secret Salt
const phrase = encodeGhostPass(data, "your_custom_salt");
```

```typescript
const indices = [
  data.role,        // Word 1
  data.type,        // Word 2
  data.name,        // Word 3
  data.reserved1,   // Word 4 (Future)
  data.version,     // Word 5
  data.model,       // Word 6
  data.prodDate,    // Word 7
  data.actDate,     // Word 8
  data.expiryDate,  // Word 9
  data.sku,         // Word 10
  data.reserved2    // Word 11 (Future)
];
```

## 🛡️ Security Checksum (The "Guard")
Word 12 is the security signature. It is calculated by hashing the indices combined with the `masterSecret`. This ensures that even if an attacker knows the first 11 words, they cannot generate the correct 12th word without the secret salt.

## 🚀 How to Integrate
The core logic is located in `@/data/mnemonic.ts`. 

### Encoding
```typescript
import { encodeGhostPass } from './data/mnemonic';

const myData = { ... };
const phrase = encodeGhostPass(myData, "my_secret_salt"); 
// Returns: ["abandon", "ability", ...]
```

### Decoding
```typescript
import { decodeGhostPass } from './data/mnemonic';

const phrase = ["...", "..."];
const { data, valid } = decodeGhostPass(phrase, "my_secret_salt");

if (valid) {
  console.log("Valid license for role:", data.role);
}
```
