# Wangyi Protocol — Changelog

## v1.0-draft — 2026-10-15 (initial independent release)

First standalone packaging of the audit-adjudication & truth-grading system
developed in the SiliconLifeOS research tree.

### Added

- Zenodo DOI registered: **10.5281/zenodo.23376296** (README citation
  section, EN + CN)
- `LICENSE` — CC BY-NC-ND 4.0 (copyright 2026 Yi-Ting-zheng) +
  protocol-specific scope clarification (license governs protocol text;
  independent implementations are separate works)
- `README.md` §How to use — no-code-ships-here usage loop: feed the spec to
  an agent → instruct it to write the conforming auditor → validate via
  WP-L3 dual controls → archive verdicts hash-locked (中英双语)
- `README.md` — bilingual (EN first, then 中文): positioning, three-layer
  model, flagship evidence, maturity & limitations
- `spec/wangyi_spec.md` — normative English specification:
  reverse burden of proof gate (§1.3), truth grading + no-recursive-upgrade
  (§2), WP-L1/L2/L3 validity model (§3), MA gates (§4), axiom whitelist
  (§5), R01–R13 meta-rules (§6), verdicts (§7), conformance levels (§8)
- `spec/ma_000.md` — human delivery gate (build ≠ archived; delivery status
  ⊥ truth grade)
- `spec/ma_007.md` — dual-condition Mathlib verification gate, incl.
  empirical anchor (OAI 409-entry survey: 0/409 complete) and auditor
  dual-control validation record
- `spec/terms.md` — bilingual glossary
- `schema/verdict_schema.yaml` — machine-readable verdict schema
  (verdict enum, condition checks, record fields, whitelist baseline)
- `examples/oai_409_entry_audit/` — flagship case: 409-entry audit snapshot
  (SHA-256 `f40ab455…` + SHA256SUMS), case notes, R-series origin mapping

### Maturity statement

`v1.0-draft`: single reference implementation (EMF Profile in the
SiliconLifeOS tree); portability to a second independent codebase
UNVERIFIED. Specification text is `DEFINITION`-level; published audit
results are `EMPIRICAL`-level. Not a "standard" until a second independent
implementation conforms.

---

## v1.0-draft — 2026-10-18 (amendment: MA-008 provenance gate)

### Added

- `spec/ma_008.md` — **MA-008 per-declaration provenance & model-signature
  registration gate**: every declaration needs its own production record
  (`model_family`, `model_version`, `model_signature`, `contribution_type`);
  project-level blanket "AI-assisted" statements are a BLOCKING finding;
  missing provenance ledger on a multi-declaration project defaults to
  `INCONCLUSIVE` (reverse-burden-of-proof gate applied to provenance).
- `schema/provenance_schema.yaml` — machine-readable provenance schema:
  three closed enumerations (`artifact_type`, `contribution_type`, `stage`),
  the 8 MA-008 checks (P1–P8), the closed 8+1 verdict set, and the manifest
  self-describing hash convention.
- `spec/wangyi_spec.md` §4.3 — MA-008 registered as a normative gate, plus
  the core principle that **provenance ≠ truth**: source annotation carries
  no credibility weight and does not relax L1/L2/L3.
- `spec/terms.md` — new glossary entries: provenance ledger, contribution
  type, model signature, knowledge-provenance chain.
- `README.md` — provenance ledger described in both the English and Chinese
  core-components lists.
- **§9 of MA-008 — relation to the compute-chain route**: the two ledgers
  ledger different objects for different consensus goals; the compute chain
  schedules production, the provenance chain records output provenance and
  audit storage, and together they close the loop with WP-L1/L2/L3 truth
  adjudication. MA-008 is a knowledge-provenance chain, not a mining chain.

### Maturity statement

Unchanged: `v1.0-draft`, single implementation, portability unverified.
MA-008 is a new gate; the EMF Profile reference implementation
(`ma008_provenance.py`) is the single conforming tool.

---

## v1.0-draft — 2026-10-18 (schema v1.1 amendment: execution environment & cost ledger pointer)

### Added

- `schema/provenance_schema.yaml` → **v1.1-draft** (additive, non-breaking
  for v1.0 manifests): (1) `execution_environment` closed enum
  (`internal_cluster` / `external_api` / `local_runtime`) + per-stage field —
  the chain-of-thought is a surface narrative after coordinate
  transformation; the same model family generates under **unequal
  conditions** across an API boundary (server-side truncation, sampling
  caps, opaque metering — the internal side can even bypass external token
  metering), so the generating regime is itself provenance. (2)
  `cost_ledger_ref` — token/cache/quota figures are self-attested
  quantities; they live ONLY in the compute-chain billing ledger, the
  manifest carries a SHA-256 pointer, never the numbers.
- `spec/ma_008.md` — §5.4 enum (mixed environment must be split into
  separate stages), §4.1 field rows, §9.1 "cost ledger pointer & the
  asymmetric-environment problem" (compute chain ↔ provenance chain loop
  now closes with cost included), §10 four-enumeration statement.
- **P9 (execution-environment gate)** — registered as `type: warn`,
  `suspended_promotion: true`: per-item disclosure only, produces no
  verdict and no new closed-set value until promoted. Promotion is gated by
  `roadmap/ROADMAP-MA-008.md` TP-MA008-1 (four-axis T01-style test group on
  P9 + ≥2 real-manifest exposure rounds + second-implementation
  readability + closed-verdict-set review).
- `spec/terms.md` — new entries: execution environment, cost ledger
  reference.
- `spec/wangyi_spec.md` §4.3, `README.md` (EN + 中文) — environment & cost
  pointer described in the MA-008 premises.
- `roadmap/ROADMAP-MA-008.md` — new roadmap registry; TP-MA008-1 is the
  first registered topic (P9 verdict-form stabilization), explicitly
  NOT merged into blocking semantics until its promotion gate closes.

### Discipline notes

- Provenance ≠ truth is preserved unchanged: `execution_environment` and
  `cost_ledger_ref` carry no credibility weight; they condition
  reproducibility claims only.
- The closed verdict set is unchanged by v1.1 (P9 emits warnings, not
  verdicts).

### Maturity statement

Unchanged: `v1.0-draft` protocol, schema now `v1.1-draft`. The
execution-environment fields are unimplemented in the reference tools until
the EMF Profile aligns; P1–P8 semantics are untouched.

---

## v1.0-draft — 2026-10-18 (pre-v2 defect sweep: schema v1.2-draft)

Full-repository defect audit before the second release. Text-level fixes are
non-semantic; schema changes are additive/clarifying.

### Fixed (text-level defects)

- `spec/ma_008.md` §8.3 verdict table was **corrupted** (the
  `PROVENANCE_VERIFIED` row and the `P1` row had merged with `||`, shifting
  every subsequent row) — restored.
- `spec/ma_008.md`: `WP-L1/L2/L2` → `WP-L1/L2/L3`; "a artifact" → "an
  artifact"; stray space in the `local_runtime` description.
- `spec/wangyi_spec.md` §4 intro now names MA-008 (§4.3 existed but the
  intro sentence above it still listed only MA-000/MA-007); footer dates
  aligned to 2026-10-18 (`wangyi_spec.md` was 2026-10-15, README was
  2026-10-14 while both carried 10-18 edits).
- `spec/ma_008.md` §9.1: the cost-ledger **pointer/scope/conflict triple**
  is now defined normatively *inside this repository* (the previous text
  pointed to an external, unresolvable reference). `spec/ma_008.md` §3.2's
  internal cross-reference made self-contained.
- `roadmap/ROADMAP-MA-008.md`: the external reference-implementation
  pointer reworded descriptively ("SiliconLifeOS EMF Profile — an
  independent research tree, external to this repository; this repository
  ships no code").

### Changed (schema v1.1-draft → **v1.2-draft**)

- **P6 made self-consistent:** `modification_log` gains an optional
  `entry_kind` (`edit` default | `zero_edit_attestation`). `AI_COMPLETE`
  now requires exactly one auditor-signed `zero_edit_attestation` entry
  whose `proof_hash_sha256` equals the record's own `proof_hash_sha256`
  (draft = final — the hash equality *is* the binding evidence). The old
  wording demanded "≥1 entry showing zero human edits" from a log whose
  entries by definition record edits — self-contradictory.
- **P3 HUMAN_FULL exemption, explicit:** the three model-identity fields
  MUST be the empty string on `HUMAN_FULL` stages; emptiness is valid
  *only* there (uniform schema shape preserved; previously the required
  "non-empty" rule made every pure-human stage non-conformant on first
  contact).
- **Closed verdict set repaired:** `INCONCLUSIVE` added to
  `verdict_enum.canonical` (9 values; alias
  `INCONCLUSIVE_PROVENANCE_UNADJUDICATABLE`) — it was already emitted by
  `verdict_logic` and by §8.3 but absent from the closed set.
- **Hash normalization pinned normatively** (§4.2 annex): statement =
  whitespace-collapsed + stripped, from the depth-0 `:` through the
  declaration head; proof = byte-exact raw span (no collapsing — tactic
  indentation is signal); `CONJECTURE` = SHA-256 of the empty string.
  Mirrors the reference implementation exactly.

### Added

- `examples/ma008_hash_vectors/` — 5 conformance vectors
  (statement ×2, proof ×3), parsed-back and re-hashed 5/5 OK. Test vectors
  are data, not code; the "no executable scripts" discipline is unchanged.
  This is the first portability precondition for a second independent
  implementation.
- `roadmap/ROADMAP-MA-008.md` **TP-MA008-2** — the remaining 9 audit
  findings (stale-log reconciliation threshold, stale-manifest P1 window,
  `schema_version` field in manifest meta, `audit_verdict` enum, MA-008
  verdict-record schema, origin-forgery escape class, GAP-ledger schema,
  single-declaration asymmetry, P2 operationalization) with concrete
  falsification conditions.

### Maturity statement

Protocol remains `v1.0-draft` for that amendment line; schema advances to
`v1.2-draft`. The reference implementation is unchanged (v1.2 is
definition-aligned with its current behavior — the normalizer was
documented, not redesigned).

---

## v2.0-draft — 2026-10-18 (major version bump from v1.0-draft)

### Why a major version

The delta from the archived v1.0-draft release is **structural, not
editorial**: (1) a full new gate (MA-008) with a normative provenance
schema now at v1.2, (2) a pinned hash-normalization annex §4.2 with
conformance vectors — the precondition any second independent
implementation must match, (3) the execution-environment axis and the
cost-ledger pointer design (schema v1.1 additions), (4) the closed
provenance verdict set (9 values) and the two-axis paired-verdict
reporting (spec §7.1). A reader who cites "the protocol" is now citing a
different object than the 2026-10-15 archive.

### Changed

- `spec/wangyi_spec.md` — protocol version `v1.0-draft` → `v2.0-draft`
  throughout; §7.1 gains the **paired-verdict reporting rule** (MA-007
  axis / provenance axis, each inside its own closed set, neither axis
  inferable from the other); §9 records the version lineage.
- `spec/ma_000.md` / `spec/ma_007.md` / `spec/ma_008.md` — protocol
  label updated (gate contents unchanged in this bump; dates keep their
  authoring record).
- `spec/terms.md` — maturity-tag entry updated.
- `README.md` — maturity statements (EN + 中文), citation section now
  distinguishes the archived v1 DOI (10.5281/zenodo.23376296, retained)
  from the pending v2.0-draft DOI.
- `CHANGELOG.md` — this section; footer updated.

### Registered ambition (level-honest)

- **Long-term ambition, `DEFINITION`-level (a plan, not a result):** the
  per-declaration provenance ledger is designed to be the accountability
  substrate for *decentralized mathematical verification* — independent
  auditors converging on the same hash-locked, append-only record of
  *who proved what with which model, under which audit, with which gaps*.
  No decentralization claim is made in v2.0-draft; TP-MA008-1/2 carry the
  work that must close first (multi-auditor reconciliation, GAP-ledger
  schema, second implementation).

### Maturity statement

`v2.0-draft`: single reference implementation, portability now partially
supported (hash conformance vectors shipped); not a "standard" until a
second independent implementation conforms. Specification text is
`DEFINITION`-level; published audit results are `EMPIRICAL`-level.

---

`Wangyi Protocol | changelog | v2.0-draft | schema v1.2-draft | 2026-10-18`
