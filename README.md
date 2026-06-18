# iartifact-verify

Client-side verifier for iArtifact sealed works. Drop an artifact folder into the page and verify locally that the seal is intact, the chain is unbroken, and every file matches its recorded hash. Everything runs in your browser. No server. No account. No upload.

---

## What v1.0 actually verifies

- Every file hash matches what was recorded at creation (SHA-384, verified locally)
- The provenance chain is intact and unbroken (entry hashes re-computed locally)
- The RFC 3161 timestamp token is present and shows granted status
- The seal chain entry is present and the recorded manifest hash matches

Nothing leaves your machine during verification. If you use the optional registry lookup (user-initiated), only the iA-ID is sent.

---

## What v1.0 does NOT yet check

### Known gaps — closing in v2

**GAP 1 — TSA certificate chain validation**
The TSR file's PKIStatus byte is checked, but the TSA's signature and certificate chain are not validated against a trusted root. A forged TSR with a granted status byte would pass v1. v2 will embed the DigiCert QTSP root and perform full PKI.js-based chain validation.

**GAP 2 — Creator identity binding**
v1 does not verify a creator-controlled cryptographic signature over the manifest. The seal proves the manifest hasn't been altered post-sealing, but not who sealed it. v2 will add a detached Ed25519 signature over the canonical manifest, with the creator's public key published in a stable location.

**GAP 3 — Robust media-substrate watermarking**
v1's publish pipeline includes an experimental embedded identity layer that is best-effort only. It is not robust against deliberate removal or aggressive recompression. v2 will replace the LSB image embedding with a DCT-domain, key-dependent scheme with documented test results against JPEG, resize, and crop transformations. Audio will gain echo-hiding embedding. Video will gain per-keyframe watermarking.

**GAP 4 — Full C2PA compliance**
The current video sidecar is C2PA-structured JSON, not a signed CBOR-encoded C2PA manifest as defined by the specification. v2 will produce fully spec-compliant signed C2PA via CBOR + Ed25519.

---

## Version history

| Version | Date | Notes |
|---|---|---|
| v1.0 | 2026-06-17 | Initial public release. File hash + chain + seal verification. TSR grant status check. Registry lookup tab. Commit eb9928b. |
| v1.1 | 2026-06-18 | GAP 5: dual-algorithm chain support (SHA-256 + SHA-384 per entry, auto-detected). GAP 6: stamped-event handling (STAMPED verdict for RFC 3161 timestamped but not yet sealed). GAP 7: three-file seal coverage (manifest + artifact + lineage, each shown individually). |

---

## Roadmap

| Target | Scope |
|---|---|
| v1.1 | Verifier UI polish, sample sealed artifact for self-test, accessibility audit, optional language localization |
| v2.0 | Close GAP 1 (full TSA chain validation via PKI.js + DigiCert QTSP root), close GAP 2 (Ed25519 detached manifest signature), close GAP 4 (signed C2PA CBOR for video) |
| v2.1 | Close GAP 3 image substrate (DCT-domain, key-dependent watermark, tested against JPEG q70–100, resize 50–200%, and center crop) |
| v2.2 | Close GAP 3 audio substrate (echo-hiding, tested against MP3 128 kbps and 320 kbps re-encode) |
| v2.3 | Close GAP 3 video substrate (per-keyframe DCT watermark, tested against H.264 re-encode) |

---

## How verification works

Five steps run locally in your browser — no server contact:

1. **Locate artifact files** — Finds and parses `manifest.json`, `artifact.json`, `chain.json`, and (optionally) `timestamps.json` from the dropped folder.

2. **File integrity — SHA-384** — For each file listed in the manifest with a recorded hash, the verifier reads the file bytes and recomputes SHA-384 via the Web Crypto API. Any mismatch fails immediately.

3. **Provenance chain integrity** — For each entry in `chain.json`, the verifier recomputes `ALGO(prev_hash | event | compact_json(data) | timestamp)` and checks that it matches the stored `entry_hash`, and that each entry's `prev_hash` equals the previous entry's `entry_hash`. The hash algorithm is auto-detected per entry from the `entry_hash` length: 64 hex characters = SHA-256, 96 = SHA-384. This means artifacts created before the SHA-384 migration verify correctly alongside newer entries in the same chain.

4. **Seal integrity — three-file coverage** — Handles two seal-phase events distinctly. If a `stamped` event is present but no `sealed` event, the verifier reports STAMPED (RFC 3161 timestamped, not yet finally sealed — a valid intermediate state). If a `sealed` event is found, the verifier reads its recorded `manifest_sha384`, `artifact_json_sha384`, and `lineage_sha384` and computes SHA-384 of each actual file. All three must match for the seal to verify. Each check is displayed individually in the results.

5. **RFC 3161 timestamp status** — For each record in `timestamps.json`, loads the corresponding `.tsr` file and checks the DER PKIStatus byte for granted (`0x00`) or grantedWithMods (`0x01`). This confirms the TSR file's grant status only — full TSA certificate chain validation is GAP 1 above, closing in v2.

---

## License and provenance

MIT License — see [LICENSE](LICENSE) if present.

Copyright © 2024–2026 Ground Zero Studios. All rights reserved.  
Author: Terry Whitaker, Hamilton, Ohio  
U.S. Copyright Registration 1-15177775421 (Literary Work, filed 2026-06-04)  
Built within the SDEA framework.
