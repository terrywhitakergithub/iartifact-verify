# Sample Artifact — EPE 3.9.1 FRAMEWORK

A real sealed iArtifact created with the iArtifact desktop app by Terry Whitaker (U.S. Copyright Registration 1-15177775421). The work is the **EPE 3.9.1 FRAMEWORK** — a self-contained HTML5/JS application driving the Epistemic Preservation Engine, a structured reasoning and knowledge-graph system built within the SDEA framework.

iA-ID: `cb9c3f78868d`  
Sealed: 2026-06-18  
Creator: Terry Whitaker / Ground Zero Studios

---

## How to use this sample

1. Download this folder — click **Code → Download ZIP** at the top of the GitHub repo page, then extract the `sample-artifact` subfolder from the ZIP
2. Drop the extracted `sample-artifact` folder onto the verifier at:  
   **https://terrywhitakergithub.github.io/iartifact-verify/**

**Expected result:** VERIFIED. All 5 verification steps green.

| Step | What it checks | Expected |
|---|---|---|
| 1 | Locate artifact files | 1 file in manifest · 7 chain entries · 1 timestamp record |
| 2 | File integrity — SHA-384 | EPE_Generator_3_9_1.html ✓ |
| 3 | Provenance chain integrity | All 7 entries verified ✓ (entries 0–3 SHA-256, 4–6 SHA-384) |
| 4 | Seal integrity — three-file coverage | Manifest ✓ · Artifact metadata ✓ · Lineage ✓ |
| 5 | RFC 3161 timestamp status | 1 granted (FreeTSA) |
| Final | Verdict | **VERIFIED** |

---

## What this artifact contains

| File | Purpose |
|---|---|
| `EPE_Generator_3_9_1.html` | The sealed work — EPE 3.9.1 FRAMEWORK application |
| `manifest.json` | SHA-384 hash of every sealed file |
| `artifact.json` | Artifact metadata (iA-ID, type, creator, seal status) |
| `chain.json` | Provenance chain — 7 events from creation to sealing |
| `lineage.json` | Lineage record of the work's development history |
| `context.md` | Human-readable context document |
| `timestamps.json` | RFC 3161 timestamp records |
| `manifest.json.20260617.tsr` | The actual RFC 3161 timestamp token (DER format) from FreeTSA |
