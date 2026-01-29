# 👻 GhostPass Node v8.1

**The Hybrid Mnemonic Licensing Standard for Humans.**  
GP-Node-V8.1 is the core cryptographic node and documentation hub for the GhostPass system. It translates complex licensing data into human-readable 12-word phrases using the BIP-39 standard.

---

## 🚀 Key Features
- **Headless API Hub**: Securely encode and decode license data via REST API.
- **12-Word Mapping**: Global standard for embedding Role, Version, Dates, and SKUs into mnemonics.
- **Security Checksum**: Built-in integrity verification (Word #12).
- **MkDocs Integration**: Comprehensive, searchable documentation for users and developers.

---

## 📂 Repository Structure
- `/app`: Next.js 15 application (API Endpoints & Status UI).
- `/data`: Core mnemonic engine and BIP-39 wordlist.
- `/docs`: Markdown documentation source files.
- `mkdocs.yml`: Configuration for the documentation site.

---

## 🌐 API Quick Start
### Encode (Generate Phrase)
`POST /api/encode`
```json
{
  "role": 1,
  "type": 0,
  "name": 102,
  "version": 1,
  ...
}
```

### Decode (Verify Phrase)
`POST /api/decode`
```json
{
  "phrase": "word1 word2 ... word12"
}
```

---

## 📖 Documentation
To view the full documentation locally:
1. Install MkDocs Material: `pip install mkdocs-material`
2. Run server: `mkdocs serve`

---
© 2026 **GhostMicro Cryptographic Network**. All reserved slots are standing by.
