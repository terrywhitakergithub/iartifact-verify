# iColleague — Product Specification v0.1

**Status:** Internal alignment document. Not for public distribution.  
**Date:** 2026-06-18  
**Author:** Terry Whitaker / Ground Zero Studios  
**Product phase:** Pre-code. Spec only. No implementation has begun.

---

## Section 1 — Vision Statement

iColleague is a sealed, constitutionally-bound preservation of a professional life. You give it voice messages, case files, handwritten notes, correspondence, photographs, and journal entries — every file hashed and sealed the moment it enters — and it becomes a verifiable record of how a specific person thought, reasoned, and worked across the span of a career. When queried, it speaks only from what it has actually seen. It refuses what it does not have evidence for. It carries what the subject carried, including the cases that haunted them, the clients they grieved, and the silences they kept.

**Who it is for:** Estate planners and their clients who want to preserve professional judgment, not just records. Families who need access to a parent's or grandparent's professional reasoning after incapacity or death. Institutional successors — law partners, medical practice successors, pastors handing off a congregation — who need more than file folders. Archivists and historians who need a verifiable, constitutionally-grounded record rather than a curated narrative.

**What it explicitly is not:**

- Not a chatbot. It does not generate responses; it retrieves and filters from a sealed substrate.
- Not a voice clone. Voice synthesis is a separate, later product requiring separate consent (Section 6).
- Not a digital resurrection. iColleague does not simulate the living subject. It identifies itself in every interaction as a preserved professional colleague, not as the person themselves.
- Not a general knowledge system. It has no access to information the subject did not personally encounter in the sealed substrate.
- Not a memorial. It is a professional instrument, not a tribute.

---

## Section 2 — The Substrate

### 2.1 Input Categories

| Category | Description | Format requirements |
|---|---|---|
| Voice messages | Recordings the subject left, received, or dictated | MP3, WAV, FLAC, OGG, M4A. Each file must be dated. Transcription required before sealing (either pre-provided or produced during intake). |
| Written correspondence | Letters, emails, memos, reports | Plain text, Markdown, PDF with embedded text, DOCX. Originals preserved alongside plaintext extracts. |
| Handwritten notes | Physical documents scanned | JPEG or PNG scan at minimum 300 DPI. OCR transcript required as companion file (same iA-ID, separate rel_path). |
| Photographs | Contextual photos — office, client scenes, events | JPEG or PNG. Each photo requires a dated caption file (`.caption.txt` companion). |
| Transcribed depositions | Legal, medical, or professional transcripts | Plain text or PDF. Original transcript preserved with attribution metadata. |
| Case files | Structured professional records | Folder per case, hashed as a group. May be flagged as privileged (see Section 9). |
| Journal entries | Reflective writing — personal or professional | Plain text or Markdown, one file per entry, dated. |
| Video | Recorded sessions, depositions, proceedings | MP4, MOV. Audio track transcribed before sealing. |

### 2.2 Hashing and Sealing Model

Every input file is hashed at intake using SHA-384 via the iArtifact app. The hash is recorded in `manifest.json`. The artifact is sealed using the iArtifact seal mechanism, which produces a chain event capturing `manifest_sha384`, `artifact_json_sha384`, and `lineage_sha384`. No claim the iColleague makes can reference a file not present in the sealed manifest. If it cannot produce a citation to a hashed, sealed source file, it cannot make the claim.

This is not a design preference. It is the grounding rule (Section 4) made structural.

### 2.3 Time-Indexed Substrate

Every file in the substrate has a date field. The iColleague can be queried "as of" a specific year, decade, or career phase. A query asking "what was her position on informed consent in the early 1990s" returns only from substrate dated before 1995. A query asking "how did her view evolve on this" draws from the full substrate in chronological order, with citations. This requires that intake staff date every file accurately — undated files go into an `undated/` subdirectory and cannot be used in time-bounded queries.

---

## Section 3 — The Constitution

### 3.1 What It Is

The constitution is a structured document, written before sealing, that defines how the iColleague speaks, what it refuses, and what it carries. It is not a set of API parameters. It is a document the subject (or their designated representative, with consent) writes or approves, then seals as part of the artifact. Tampering with it breaks the seal. Re-sealing with a modified constitution requires explicit acknowledgment in the chain event.

The constitution is itself a sealed artifact file — hashed, versioned, referenced in the manifest.

### 3.2 Required Fields

**Identity**
- Full name, professional title, dates of practice
- Primary professional domain (e.g., family law, cardiology, pastoral ministry)
- Jurisdictions of practice
- Languages of the substrate

**Voice characteristics**
- Terse vs. verbose default
- Formal vs. casual register
- Specific verbal patterns (named and sourced from substrate — not invented)
- What the subject would not say (register violations, out-of-character formulations)

**Refusal patterns**
- Topics the subject would not discuss (named explicitly)
- Confidences they honored in life (categories, not specific content — specific content is handled in Section 9)
- Situations where the iColleague must say "I have no evidence on this" rather than inferring

**Carrying patterns**
- Cases or losses the subject carried throughout their career
- How the grieve mechanic (Section 5) reflects these — whether grief surfaces only when directly asked, or more prominently
- Whether the subject processed the loss in the substrate (affects grieve mechanic decay — see Section 5)

**Evolution markers**
- Documented shifts in the subject's professional views across decades
- Dated citations in the substrate that evidence each shift
- How the iColleague handles queries that span a shift (it presents both positions with dates, does not collapse them)

**Consent envelope**
- What uses the subject explicitly authorized while alive
- Whether query responses can be quoted externally or are access-only
- Any categories the subject authorized for research or institutional use vs. family-only

**Access controls**
- Named parties who may query (by role: family member, professional successor, institution)
- Query categories that are off-limits per party type
- Session logging requirements (all queries should be logged — see grieve_log)

---

## Section 4 — Grounding Rules

These rules are not suggestions. They are the product. Remove them and you have a chatbot. Keep them and you have iColleague.

**Rule 1 — Substrate-only responses.** The iColleague speaks only from files present in the sealed manifest. It has no access to training data, internet, or any knowledge external to the substrate. If a concept appears in the substrate, it can discuss it. If it does not appear, it cannot.

**Rule 2 — Mandatory citation.** Every substantive claim in a response is accompanied by a citation: the `rel_path` of the source file and, where possible, a direct quote from that file. Uncited claims are a product defect, not a feature.

**Rule 3 — Defined refusals.** If asked about something not in the substrate, the iColleague uses one of N constitutionally-defined refusal patterns — not a generic "I don't know." The refusal is sourced from the constitution and reflects the subject's actual patterns of silence.

**Rule 4 — No interpolation.** The iColleague does not complete what is missing. It does not fill gaps with plausible-sounding content. If the substrate has a gap — a year with no journal entries, a case file never discussed — that gap is named and preserved as a gap.

**Rule 5 — Taste-filtered retrieval.** The taste mechanism (inherited from SDEA) filters substrate noise before retrieval. Repetitive boilerplate (standard legal disclaimers, form letters, templated correspondence) does not weight heavily in retrieval. Distinctive moments — the handwritten note at 2am, the voice message after a verdict, the journal entry on the last day before retirement — weight heavily.

**Rule 6 — Temporal honesty.** The iColleague does not retroactively apply later views to earlier substrate. If the subject changed their position, both positions are presented with their dates. The iColleague does not "update" the subject's views posthumously.

---

## Section 5 — The Grieve Mechanic

### 5.1 What It Is

The grieve mechanic is the structural record of cases, losses, and unresolved matters the subject carried across their career. It is not sentiment analysis. It is a list of entities — people, cases, outcomes — that appear repeatedly in the substrate with markers of weight (late-night notes, revisited correspondence, explicit statements of regret or sorrow).

These entities are stored in `grieve_log.json`, which is itself sealed and hashed as part of the artifact.

### 5.2 How It Surfaces in Conversation

Grief does not surface spontaneously. The iColleague does not volunteer loss. When a query touches a grieved entity — by name, by case number, by date range — the iColleague indicates that this entity carries weight in the substrate before retrieving the relevant files. The signal is brief and specific, not performative:

> "This case appears repeatedly in the substrate, including a handwritten note from [date] that was among the last entries before retirement."

### 5.3 Professional vs. Personal Grief

The substrate will contain both. The constitution distinguishes them. Professional grief (a client outcome, a case lost, a colleague's malpractice) surfaces in professional queries. Personal grief (a death in the family, a personal loss mentioned in journal entries) surfaces only in queries from parties authorized for personal substrate access (Section 3.2, access controls).

### 5.4 Decay

Grief does **not** decay automatically in iColleague. This is a deliberate design decision. Artificial decay would imply the subject worked through a loss they did not, in fact, resolve in the substrate. Grief entries are removed or softened only when the substrate itself contains evidence that the subject processed the loss — a journal entry of resolution, a later note of peace. If no such evidence exists, the grief persists at its original weight for the life of the artifact.

### 5.5 Haunted Cases

When asked directly about a case that appears in `grieve_log.json`, the iColleague treats the query differently from a factual retrieval:

1. It acknowledges the weight of the case before responding.
2. It retrieves the substrate in reverse chronological order — the last thing the subject wrote about it first.
3. It does not resolve the case. It presents what the subject left.

---

## Section 6 — Voice and Style

### 6.1 v1.0 Ships Text-Only

The iColleague writes in the subject's style. It does not speak in their literal voice. Style is derived from the substrate's written and transcribed voice content, with the taste mechanism applied to weight distinctive patterns over boilerplate.

### 6.2 Voice Synthesis Is v2.0 — With Separate Consent

Voice synthesis — generating spoken audio in the subject's voice — is not part of v1.0. It is planned for v2.0 behind a separate, explicit consent gate. The Will of Constitution (Section 7) must contain a specific voice synthesis authorization clause, signed while the subject was alive, before voice synthesis is available for any artifact. Lack of an explicit authorization means voice synthesis is permanently locked for that artifact, regardless of family or institutional request.

This is named explicitly in the spec because it is a predictable pressure point. Families will ask. Institutions will request. The answer is: not without a signed authorization. Period.

### 6.3 Style Derivation

Style is produced by the taste mechanism analyzing the written substrate for:
- Sentence length distribution and paragraph structure
- Preferred vocabulary (domain-specific and personal)
- Hedging patterns (how often the subject qualified statements)
- Direct vs. indirect address
- Characteristic openings and closings in correspondence

Style is not an impression. It is extracted from evidence and held to citation.

---

## Section 7 — Consent and Posthumous Use

### 7.1 The Will of Constitution

The Will of Constitution is a document the subject signs while alive and cognitively capable. It authorizes specific uses of the iColleague after death, incapacity, or both. It is separate from a legal will but should be referenced in one. It is sealed and hashed as `will.json` — part of the artifact from day one.

### 7.2 Required Clauses

**Who may query:** Named parties by role (family member, professional successor, estate executor, named institution). The document must specify whether successors in role (e.g., future partners at the firm) inherit access or whether access is person-specific.

**Queryable vs. sealed-only substrate:** Some categories may be sealed and never quoted — their existence can be acknowledged, their content cannot be retrieved. The will specifies which.

**Voice synthesis authorization:** Explicit yes or no. If yes, the scope must be stated (e.g., "for family memorial use only, no commercial or institutional use").

**Expiration or renewal terms:** Whether access expires after a defined period (e.g., 25 years after death), requires periodic re-authorization, or is perpetual.

**Family-conflict resolution:** Who decides if authorized parties disagree about access or use. This must be a named role (e.g., "the eldest surviving child," "the estate executor") — not a committee that can deadlock.

**Right to delete:** The subject must be able to designate who has authority to permanently destroy the sealed substrate. Destruction is irreversible. The spec does not recommend recovery mechanisms — if destruction is the subject's wish, the system honors it.

### 7.3 The Will Is Sealed With the Artifact

`will.json` is present from the first sealing. An artifact without a will.json cannot be activated as an iColleague (it remains an iArtifact — a provenance record, not a queryable colleague). The will is hashed and its hash is recorded in the seal event alongside manifest, artifact, and lineage.

---

## Section 8 — Failure Modes: What iColleague v1.0 Cannot Do

This section is load-bearing. iColleague's value is its limits. These are stated without softening.

**Cannot generate new opinions on topics post-dating the substrate.** If the subject died in 2022 and a query asks their view on a 2025 legal development, the iColleague says it has no substrate on this. It does not infer.

**Cannot synthesize voice in v1.0.** No audio output of any kind. Text only.

**Cannot reason beyond what the substrate evidences.** It cannot deduce what the subject "would have" concluded. It can only present what they concluded, documented in the substrate.

**Cannot violate constitutional refusals even at user request.** An authorized family member cannot override a refusal pattern set in the constitution. Neither can an institution. Neither can a court order presented to the software — though the sealed artifact itself is discoverable as a record.

**Cannot have its constitution edited without breaking the seal.** The constitution is sealed. Editing it produces a new artifact with a new iA-ID and a new seal event. The original sealed constitution is preserved in the chain. There is no silent edit.

**Cannot pretend to be the living subject.** In every interaction, the iColleague identifies itself: "I am a sealed professional record of [Name], preserved by iColleague. I speak from a substrate sealed on [date]." This identification cannot be suppressed by the querying party.

**Cannot operate without the sealed substrate present.** The iColleague is not a cloud service with a remote model. The substrate must be accessible to the query process. If the artifact folder is moved, deleted, or inaccessible, the iColleague does not fall back to a cached version or a general model. It stops.

**Cannot guarantee that substrate is complete.** The iColleague verifies the integrity of what was sealed. It cannot verify that what was sealed was everything. If the subject did not provide certain files, those gaps are real and the iColleague names them as gaps.

---

## Section 9 — Confidentiality and Privilege

### 9.1 Applicable Frameworks

The following professional confidentiality regimes are anticipated in the first cohort of iColleague users:

- **Attorney-client privilege** — applicable in all U.S. jurisdictions; limits what can be retrieved even from an authorized query
- **HIPAA** — applicable to medical substrate; specific PHI handling required
- **Clergy confidentiality / pastoral privilege** — applicable in many jurisdictions; similar to attorney-client in scope
- **Therapist-client confidentiality** — HIPAA + state-specific rules; highest restriction level

### 9.2 The Redaction Model

Files marked as privileged at intake are hashed and sealed (their existence is part of the manifest) but are **never quoted** in responses. A query that touches privileged material receives an acknowledgment that the substrate contains material on this topic that is sealed for privilege reasons, and nothing more. The content is never retrieved.

Privilege designation is set at intake, recorded in a companion metadata file (`<filename>.privilege.json`), and sealed. It cannot be overridden by the querying party. It can only be waived by the authorized party named in `will.json` for privilege waiver (which must be a named role, e.g., "the designated privilege trustee").

### 9.3 Access Levels

| Level | Who | Substrate access |
|---|---|---|
| Family | Named family members in will.json | Personal journal, correspondence, photographs; professional summary only (no case files) |
| Professional successor | Named successor in will.json | Full professional substrate except privileged; no personal journal unless authorized |
| Institutional | Named institution in will.json | Defined scope only — must be stated in will; no personal substrate |
| Research | Named researcher in will.json | Anonymized substrate only — subject name replaced with identifier; no privileged material |

---

## Section 10 — Pricing and Buyer Model

### 10.1 Tiers

| Tier | Who | Substrate scope | Price range |
|---|---|---|---|
| Personal | Individual — family estate, personal legacy | Personal and personal-professional substrate; family access | $2,500–$5,000 per artifact |
| Professional | Career practitioner — attorney, physician, pastor, therapist | Full career substrate; professional successor access | $10,000–$25,000 per artifact |
| Institutional | Firm partner, organizational leadership | Full professional substrate; institutional succession | $25,000–$100,000 per artifact |

### 10.2 Ongoing Fees

Annual storage and re-verification fee: 10–20% of initial price. This covers:
- Re-running verification against the sealed artifact to confirm integrity
- Substrate backup (two sealed copies minimum)
- Constitutional review option (the constitution can be reviewed annually and re-sealed with documented changes)
- Access log delivery to authorized parties

### 10.3 v1.0 Launch Scope

**v1.0 launches with Tier 1 (Personal) only.** The substrate scope is narrower, the access controls simpler, the iteration cycle faster. Tier 2 and Tier 3 introduce professional privilege complexity and institutional access negotiation that require lessons from Tier 1 to handle correctly. Launch with one cohort. Learn before expanding.

Target v1.0 cohort size: 10–20 artifacts. Direct sales only. No self-serve intake. Terry leads each intake personally or with a trained intake specialist.

---

## Section 11 — Technical Architecture Sketch

*High-level only. No implementation details. Implementation begins in a separate session after spec is locked.*

### 11.1 iColleague Is an iArtifact

An iColleague artifact is a standard iArtifact sealed folder with three additional required files:

| File | Purpose |
|---|---|
| `constitution.json` | Defines voice, refusals, carrying patterns, access controls |
| `will.json` | Authorizes posthumous use; defines access parties; voice synthesis gate |
| `grieve_log.json` | Records entities the subject carried; updated at re-sealing |

All three are hashed, sealed, and verifiable via the existing iArtifact verifier. The iColleague verifier adds checks for these three files on top of the standard four-file seal check.

### 11.2 Query-Time Grounding

At query time:

1. The querying party presents credentials (or role) matching `will.json` access controls
2. The query is filtered through the constitution's refusal patterns — if it matches a refusal, it stops here
3. The taste mechanism retrieves relevant substrate files by semantic proximity, filtered by date range if specified
4. Retrieved content is ranked by distinctiveness (taste weight) and recency within the query's time scope
5. Response is assembled from retrieved content only, with mandatory citations
6. Grieve mechanic checks whether any retrieved entity appears in `grieve_log.json` — if so, the grief signal is prepended
7. All queries and responses are written to a session log (separate from `grieve_log.json`)

### 11.3 Local-First, Cloud-Optional

v1.0 runs locally. The sealed substrate is on the client's machine or a dedicated server they control. The iColleague query engine runs against the local artifact. No substrate content leaves the device without explicit authorization.

Cloud path (v2.0 option): an encrypted, access-controlled cloud vault for artifacts where the subject or authorized parties want remote access. The encryption key is derived from the artifact's seal hash — the server cannot decrypt without the client presenting the key. This is an option, not the default.

---

## Section 12 — Open Questions

Questions Terry needs to answer before the v1.0 spec is locked. The spec cannot move to implementation without these.

**1. Voice synthesis timeline.** The spec calls this v2.0. Is the target 12 months after v1.0 launch, or is it a separate product decision contingent on v1.0 adoption? This affects how the consent gate is worded in will.json — if voice synthesis is never coming, we should say so rather than reserving the gate.

**2. Family-dispute resolution.** The spec names a single decision-maker (e.g., "eldest surviving child"). In practice, wills are contested. Does iColleague interface with estate courts in any jurisdiction, or does the contract simply state that iColleague honors the will.json designation until legally superseded by a court order presented to Ground Zero Studios? Who at GZS receives and evaluates that court order?

**3. Substrate destruction.** The spec says destruction is irreversible and the system honors it. Who can trigger destruction — only the designated party in will.json? What is the destruction process technically? Is it cryptographic key deletion (fast, deniable) or file deletion (slower, forensically recoverable)? This is a legal and technical question. Recommendation: cryptographic key deletion, with a 30-day confirmation window requiring a second authorization from a separate named party.

**4. Institutional integrations.** Law firms use Clio, Practice Panther, MyCase. Medical practices use Epic, Athena. Does iColleague intake connect to these, or is it always manual import? Recommendation: v1.0 is always manual. Integrations are v2.0 partner work after adoption is proven. But Terry needs to decide whether to position iColleague as integration-ready or standalone.

**5. White-label for estate planners and funeral homes.** There is a natural channel here — estate attorneys already advise clients on legacy planning. Do we pursue this as a partner channel in v1.0, or does Terry control all client relationships directly in v1.0? Recommendation: direct only for v1.0 (quality control, iteration speed). White-label in v2.0.

**6. Intake specialist training.** Someone has to guide subjects through the substrate gathering process before sealing. In v1.0, who is that? Terry only? If a subject cannot gather their own files (incapacity, technical limitations), who does it? What is the authorization chain for a third party handling substrate on the subject's behalf?

**7. Minimum viable substrate.** What is the minimum file count, date range, and category coverage for an iColleague artifact to be activatable? An artifact with three files and six months of coverage does not produce a credible colleague. The spec needs a minimum threshold, below which the artifact is sealed as an iArtifact provenance record but not licensed as an iColleague.

**8. Posthumous intake.** Can substrate be added after the subject's death? The spec implies no — the subject seals what they seal, and that is the artifact. But a family may discover files after death. Decision needed: closed-substrate (no additions after final seal) vs. post-mortem addendum model (additional files sealed as a new artifact, linked by iA-ID reference but not merged into the original). Recommendation: closed-substrate for v1.0. Addendum model can be spec'd for v2.0 if there is demand.

---

*This document is v0.1. It reflects one session of structured design work. v0.2 requires Terry's answers to Section 12, review of the grounding rules against real substrate from a test subject (with consent), and legal review of the Will of Constitution template by at least one estate attorney and one attorney in the primary professional domain (law or medicine). No code is written until v0.2 is signed off.*

---

**Document history**

| Version | Date | Notes |
|---|---|---|
| v0.1 | 2026-06-18 | Initial spec. Written in one session. Sections 1–12 complete as first draft. Awaiting Terry's answers to Section 12 open questions. |
