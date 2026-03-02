# Portable Secret Hardening Roadmap

## Goal
Raise cryptographic assurance and operational safety while preserving portability and offline usability.

## Threat Model (v1)
- Opportunistic interception of ciphertext files in email/cloud/USB.
- Offline password guessing against stolen encrypted blobs.
- Tampering with encrypted container / delivery channel.
- Compromised browser endpoint at decrypt time.
- User error: weak passphrases, reuse, insecure sharing workflows.

## Priority Workstreams

### 1) Cryptography Baseline (Critical)
- [ ] Verify and document current primitives (KDF, cipher mode, nonce generation, tag handling).
- [ ] Add envelope versioning (`v2`) for future migrations.
- [ ] Move to memory-hard KDF (Argon2id preferred; PBKDF2 fallback only for compatibility).
- [ ] Enforce AEAD-only encryption (AES-256-GCM or XChaCha20-Poly1305).

### 2) Integrity & Authenticity (High)
- [ ] Add signed metadata block (optional Ed25519 signatures).
- [ ] Add hash commitment for plaintext length/type metadata.
- [ ] Strict verification failures with no partial decrypt output.

### 3) Replay / Policy Controls (High)
- [ ] Add optional expiry timestamp in signed metadata.
- [ ] Add policy flags (no-copy hint, watermark hint, audit hint).
- [ ] Add anti-tamper metadata checks before decryption attempt.

### 4) Password UX & Safety (High)
- [ ] Passphrase strength estimator + minimum entropy threshold.
- [ ] Block known weak/common passwords.
- [ ] Encourage diceware-style passphrases with inline generator guidance.

### 5) Build & Supply-Chain Hardening (Medium)
- [ ] Remove remote dependencies from generated artifacts.
- [ ] Add strict CSP for generated HTML where possible.
- [ ] Deterministic build notes + integrity checksum outputs.

### 6) Documentation & SOP (Medium)
- [ ] Security model doc: what this tool protects and does not protect.
- [ ] Operational playbook for safe usage (separate password channel, rotation, storage tiers).
- [ ] Incident response notes for suspected key/password compromise.

## Acceptance Criteria
- New `v2` files pass crypto test vectors and tamper tests.
- Backward compatibility maintained for legacy decrypt (read-only path).
- Security guide includes explicit non-goals and threat boundaries.
- Reproducible tests for KDF/cipher/signature verification.

## Proposed Milestones
- **M1 (2–3 days):** Audit + `v2` envelope + KDF/cipher upgrade
- **M2 (3–5 days):** Signature + metadata integrity + policy fields
- **M3 (3–4 days):** UX hardening + docs + test suite expansion

