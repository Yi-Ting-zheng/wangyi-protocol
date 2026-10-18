# Wangyi Protocol — Normative Specification

> **Version:** v2.0-draft　**Status:** Draft Specification
> **Normative source language:** English (this document)
> **Truth level of this text:** `DEFINITION` — a convention, not a theorem.
> **Conformance keywords:** MUST / MUST NOT / SHOULD / MAY are used per RFC 2119.

---

## 1. Scope & Positioning

### 1.1 What this protocol governs

Wangyi Protocol (WP) governs the **audit adjudication, truth-grading, and
evidence archival** of formal artifacts — Lean/Coq/Isabelle proof scripts,
mechanized scientific claims, and their supporting ledgers — when those
artifacts are produced by AI or multi-agent pipelines.

### 1.2 What this protocol explicitly does NOT govern

WP does **not** define message transport, context negotiation, or byte-level
exchange between models or agents. Transport/communication protocols (MCP,
A2A, and successors) are orthogonal and composable with WP. WP audits the
*artifacts* those protocols carry.

### 1.3 The reverse burden of proof gate (P0 meta-rule)

Any audit verdict produced under this protocol MUST satisfy:

> **"Undetected" is a statement about the scanner, never a statement about
> the mathematics.**

An auditor that cannot complete verification (missing context, unresolvable
imports, uncompilable corpus, toolchain failure) MUST return `INCONCLUSIVE`.
It MUST NOT return `PASS` on the basis of failed or partial verification.
Error output MUST NOT be interpreted as success (see MA-007 §4).

---

## 2. Truth Grading

Every declarative claim in an audited artifact OR in audit output MUST carry
exactly one truth grade from the following lattice. Grades are strictly
ordered; **level jumping is forbidden**.

```
DEFINITION < HYPOTHESIS < EMPIRICAL < THEOREM
```

| Grade | Meaning | Upgrade path |
|-------|---------|--------------|
| `DEFINITION` | A notation/encoding convention or protocol rule. No truth risk. | n/a |
| `HYPOTHESIS` | A registered conditional assumption. MUST be falsifiable-marked and MUST state its applicability domain. | to `EMPIRICAL` via pre-registered experiment |
| `EMPIRICAL` | An observational/experimental result, INCLUDING ported-and-recompiled lemma checks (`EMPIRICAL (ported check)`). | to `THEOREM` ONLY via complete analytic proof chain |
| `THEOREM` | A claim with a complete, gap-free proof chain. | n/a |
| `GAP-OPEN` | A known, registered gap awaiting resolution. | resolves to a grade when the gap closes |
| `FALSIFIED` | A tentatively rejected claim. NOT deletion: `FALSIFIED` records the applicability domain and tier at rejection time; conditional revival is possible after domain expansion review. | revival requires re-audit |

### 2.1 The no-recursive-upgrade rule

Multiple independent `EMPIRICAL` confirmations NEVER constitute a sufficient
condition for `THEOREM`. Accumulated evidence raises confidence, not grade.
Strict premise-binding + strict proof chain are the only two upgrade channels.

### 2.2 Honesty clause

Empirical support ≠ mathematical proof. Any document that uses "strongly
suggests" or "preliminary evidence" wording to route around a grade boundary
MUST be returned to the archival layer by the chief auditor.

---

## 3. The Three-Layer Validity Model (WP-L1 / L2 / L3)

Conformance is graded per level. A project MUST only claim levels it has
actually passed; partial claims MUST name the passed level.

### 3.1 WP-L1 — Syntax layer

An artifact PASSES WP-L1 iff ALL hold:

1. It compiles in a **version-locked** toolchain (toolchain version recorded
   in the artifact manifest).
2. No executable `sorry` occurs at declaration position (comment-stripped,
   nested-block-comment-aware scan; literal text matches in comments MUST NOT
   count either way — they are false positives).
3. All imports resolve.

WP-L1 alone carries NO trust guarantee. A WP-L1 pass MUST NOT be described as
"mathematically verified" in any deliverable.

### 3.2 WP-L2 — Semantic layer

An artifact PASSES WP-L2 iff it passes WP-L1 AND ALL hold:

1. **Axiom cleanliness.** `#print axioms <main-decl>` over the full
   dependency chain contains only whitelisted axioms (§5) and ZERO `sorryAx`.
2. **Ontology binding.** Core mathematical objects (polynomials, finite
   fields, hypergraphs, matrices, modules, ideals, …) MUST be bound to the
   native library (e.g. Mathlib). Derived aliases over imported native types
   (`abbrev P := MvPolynomial (Fin n) ℂ`) are permitted (LOW risk);
   parallel redefinitions standing in for a native object without importing
   its providing module are HIGH-risk BLOCKING findings (MA-007).
3. **Gap registration.** Every unfinished proof obligation or registered
   hypothesis appears in the GAP ledger, one-to-one. Silent skips are BLOCK.
4. **Dependency hygiene.** The main-chain import closure is recorded; bloat
   beyond 200 internal modules MUST be flagged `CLOSURE_BLOAT_SUSPECT` and
   explicitly registered in the audit report (warning, not standalone block).

### 3.3 WP-L3 — Meta-verification layer

WP-L3 certifies the **auditor**, not the artifact. An audit pipeline PASSES
WP-L3 iff ALL four rigid standards hold:

1. **Tool audibility.** Every audit script is traceable, reproducible,
   version-locked, and hash-frozen. The exact invocation that produced a
   verdict MUST be reconstructible.
2. **Full-dimensional control-test closure.** The tool has been exercised on
   ALL four sample classes:
   - *positive control* — a clean artifact; tool MUST pass it;
   - *negative control* — an artifact with an implanted defect (e.g.
     `sorryAx` dependency); tool MUST flag it;
   - *escape counterexample* — a known AI-forgery pattern (facade import,
     padding, project-internal redefinition); tool MUST catch it or the
     miss MUST be registered as a known blind spot;
   - *boundary case* — toolchain failure, unknown identifier, missing
     olean; tool MUST return `INCONCLUSIVE`, never `PASS`.
3. **Engineering constraints.** Namespace alignment, clear module boundaries,
   no redundant main-chain dependencies.
4. **Honest self-protection.** On context loss / parse failure / untraversable
   axiom chain, the tool MUST return `INCONCLUSIVE` (§1.3).

A tool that was caught producing a false PASS, fixed, and re-validated
against both controls satisfies WP-L3 more credibly than one never
challenged; the incident MUST be logged (defect history is append-only).

### 3.4 Non-invertibility

The layers are strictly ordered: no WP-L2 claim is valid without a passing
WP-L3 auditor; no WP-L1 pass implies WP-L2. Audit reports MUST NOT invert
this dependency.

---

## 4. MA-Series Gates

Gates are BLOCKING rules with machine-checkable preconditions. This version
defines MA-000 (delivery), MA-007 (Mathlib dual-condition), and MA-008
(per-declaration provenance, §4.3). The full catalogue lives in the
reference implementation profile.

### 4.1 MA-000 — Delivery gate

See [ma_000.md](ma_000.md). Summary: a formal artifact is deliverable to
humans only after passing the four-stage pipeline
(machine工件 → readable summary → knowledge-base sedimentation → delivery
gate), and delivery status MUST NOT be conflated with truth grade.

### 4.2 MA-007 — Dual-condition Mathlib verification gate

See [ma_007.md](ma_007.md). Summary: "Mathlib-verified complete" requires
BOTH (A) import-tree coverage with no high-risk core-object redefinition,
AND (B) full-chain `#print axioms` zero `sorryAx`. Either failing, or
condition B being unverifiable (external corpus), blocks the verdict.

### 4.3 MA-008 — Per-declaration provenance & model-signature registration gate

See [ma_008.md](ma_008.md). Summary: every formal declaration (lemma,
theorem, conditional theorem, conjecture, gap skeleton) MUST carry a
machine-readable provenance record naming the producing model, its version
and weight/release signature, and a `contribution_type` that distinguishes
AI draft from human rewrite from pure-human. Absence of a provenance ledger
on a multi-declaration project forces `INCONCLUSIVE`. Since schema v1.1,
each stage also records its `execution_environment` (internal cluster vs
external API vs local runtime — conditions of generation carry no
credibility weight), and token/cache costs stay out of the manifest: the
manifest carries only a hash pointer (`cost_ledger_ref`) into the
compute-chain billing ledger. The environment gate P9 enters warn-only,
promoted to strict after
[../roadmap/ROADMAP-MA-008.md](../roadmap/ROADMAP-MA-008.md) TP-MA008-1
closes.

---

## 5. Axiom Whitelist (baseline)

| Class | Names | Note |
|-------|-------|------|
| Lean core (always allowed) | `propext`, `Classical.choice`, `Quot.sound` | foundations |
| Registered domain hypotheses | project-specific (e.g. `SchanuelAt`) | MUST be registered as `HYPOTHESIS`, applicability domain stated |
| Forbidden | `sorryAx`, any unregistered axiom, any stub-named axiom | BLOCKING |

The whitelist is append-only per project profile; removals require a
logged re-audit.

---

## 6. R-Series Meta-Rules (R01–R13, baseline extract)

The full R-series is maintained in the reference profile. The rules
normatively required by this specification are:

- **R01 — Label discipline.** "令/believe" splits into `DEFINITION` vs
  registered `HYPOTHESIS`; bare conclusions without premises are forbidden.
- **R02 — Ledger immutability.** Audit ledgers are append-only. Corrections
  append new entries; they never rewrite history.
- **R03 — No recursive upgrade.** §2.1.
- **R04 — Hash locking.** Every published verdict snapshot MUST ship with a
  SHA-256 checksum (see examples/SHA256SUMS).
- **R05 — Negative results are never discarded.** A failed control or a
  blocked artifact is evidence; it MUST be archived, not dropped.
- **R06 — Dual-control requirement for auditors.** No audit tool is trusted
  before passing §3.3(2) positive + negative controls.
- **R07 — Error ≠ PASS.** Toolchain error output MUST map to
  `INCONCLUSIVE`/`FAIL`, never `PASS`.
- **R08 — Facade-import detection.** Import counts MUST be computed over the
  transitive closure, not the entry file.
- **R09 — Independence honesty (C-02).** Re-auditing by same-corpus,
  same-rule-dimension agents does NOT constitute independent verification
  and MUST NOT be described as such. Independence is carried by
  rule/observation-dimension difference, not by model-base difference.
- **R10 — Provenance.** Third-party corpora are surveyed via derived
  statistics only; originals are NOT redistributed.
- **R11 — Version locking.** Toolchain and library pins are part of the
  verdict; a verdict without a recorded pin is void.
- **R12 — Defect history.** Known blind spots and past false-PASS incidents
  are part of the auditor's certificate.
- **R13 — Single source of truth.** Normative text lives in this repository
  only; implementations reference it by version + hash.

*(R01–R13 numbering is protocol-local; origin mapping to the SiliconLifeOS
research tree is recorded in examples/oai_409_entry_audit/README.md §Origin.)*

---

## 7. Verdicts

### 7.1 Closed verdict set

Every audit MUST emit exactly one primary verdict from:

`MATHLIB_VERIFIED_COMPLETE` · `INCONCLUSIVE` · `BLOCK_条件A` ·
`BLOCK_条件B` · `BLOCK双重失败`

(The Chinese-form names are canonical, matching the reference implementation;
an English alias table is provided in schema/verdict_schema.yaml.)

**Provenance axis (MA-008, v2.0):** when MA-008 is applied to the same
artifact, the audit is reported as a **pair** on two axes, e.g.
`MATHLIB_VERIFIED_COMPLETE / PROVENANCE_VERIFIED`. Both members stay inside
their own closed sets (the five values above; the MA-008 closed set in
schema/provenance_schema.yaml). The two axes are **not inferable from each
other** — a mathematical-axis verdict never licenses a provenance-axis
conclusion and vice versa.

### 7.2 Verdict record

Verdicts MUST conform to [schema/verdict_schema.yaml](../schema/verdict_schema.yaml).

### 7.3 Archival states

Superseded material is stamped, never deleted:
`SUPERSEDED` (replaced by newer revision) · `DEPRECATED` (retired) ·
`HISTORICAL` (frozen snapshot for provenance).

---

## 8. Conformance Levels (summary)

| Level | Requirement |
|-------|-------------|
| WP-L1-conformant | §3.1 |
| WP-L2-conformant | §3.2 (implies L1) |
| WP-L3-self-certified | §3.3 for the audit pipeline + §3.2 for the artifact |

Claims of conformance MUST cite the protocol version
(e.g. "conforms to Wangyi Protocol v2.0-draft") and MUST attach the verdict
snapshot + SHA-256.

---

## 9. Amendments

**Version lineage:** `v1.0-draft` (2026-10-15, initial independent release,
archived at the registered DOI) → `v1.x` intra-draft amendments →
`v2.0-draft` (2026-10-18): the first major version carrying MA-008 as a
full gate — normative provenance schema (v1.2), the pinned hash-normalization
annex with conformance vectors, and the execution-environment axis.

Rule: amendments append new versions; this text is never rewritten in
place. Changes are logged in the repository CHANGELOG with hash-chained
references.

---

`Wangyi Protocol spec | v2.0-draft | normative English text | 2026-10-18`
