# Case: OAI 409-Entry Audit (flagship evidence for MA-007)

> **Protocol verdict class:** MA-007 dual-condition gate, corpus-level survey
> **Truth level:** `EMPIRICAL` (survey result; the corpus is third-party)
> **Date:** 2026-10-14

## 1. What was surveyed

- **Corpus:** a large AI-generated formal-mathematics corpus
  (`math-main/lean/OAI`), ~121,700 Lean files across 23 discipline domains,
  372 manuscript families.
- **Entries:** all 409 files named `Main.lean` (one per manuscript family),
  i.e. every claimed main-theorem entry point.
- **Tool:** `lean_import_audit.py` — transitive import-closure traversal +
  Mathlib coverage + core-object redefinition detection +
  `#print axioms` sorryAx scan (v1.0 at survey time).
- **NOT redistributed:** this snapshot contains **derived statistics only**
  (module counts, import lists, redefinition hits, verdicts). The corpus
  itself is third-party material and is not included, per R10.

## 2. Corpus-level verdict

| Verdict | Count |
|---------|-------|
| `MATHLIB_VERIFIED_COMPLETE` | **0 / 409** |
| `INCONCLUSIVE` (condition B unverifiable — external corpus, cannot compile) | 398 |
| `BLOCK双重失败` | 11 |

## 3. Key findings

| Finding | Value |
|---------|-------|
| Facade-only native import (≤ 1 `Mathlib` import in closure) | **287 / 409 (70%)** |
| HIGH-risk core-object redefinitions | **16** (Ideal / Module / Matrix / Polynomial / Hypergraph) |
| Closures > 200 internal modules (`CLOSURE_BLOAT_SUSPECT`) | 133 |
| Largest closures | Progressions 4,774 / Ostmann 4,358 / MutuallyUnbiased 3,381 |
| Third-party dependency kinds | 27 (`PrimeNumberTheoremAnd.*`, `StrongPNT.*`, `FixedPointTheorems.*`, …) |

**Interpretation (why the corpus's "zero textual sorry" is an artifact):**
most families define core objects locally, prove real lemmas about the
*local* objects, import the native library once at a facade entry, and never
undergo full-chain axiom inspection (impossible anyway: external corpus,
uncompilable in a pinned toolchain). Text-level scanning is blind to this
shape; MA-007's closure + axiom-chain conditions are designed to expose it.

Representative HIGH hits: `Probability.CriticalZ3.HyperedgeComparison`
(project `Hypergraph`), `GroupTheory.BooneHigman.SteinbergPresentation`
(project `Polynomial`), `Analysis.JiangSu.Nonunital.ClosedIdealBasic`
(project `Ideal`), multiple `Computability.*` (`Matrix`).

## 4. Auditor dual-control record (R06/R07/R12)

During the same session the reference auditor produced a **false PASS**: a
boundary case (declaration name without namespace prefix) yielded
`unknown identifier` error output, which the pre-fix code read as "no
sorryAx found". The negative control caught it; the fix (error output →
`UNVERIFIABLE`, never `PASS`) was re-validated on both controls before the
survey above was trusted:

| Control | Target | Result |
|---------|--------|--------|
| Positive | clean theorem (axioms = {propext, Classical.choice, Quot.sound}) | PASS ✅ |
| Negative | theorem depending on `sorryAx` | FAIL_SORRYAX ✅ |
| Boundary | short decl name w/o namespace | UNVERIFIABLE ✅ (post-fix) |

This incident is retained as part of the auditor's WP-L3 defect history.

## 5. Artifacts

| File | Note |
|------|------|
| `audit_snapshot.json` | 409-entry verdict records (schema §7.2) |
| `SHA256SUMS` | snapshot checksum: `f40ab455…` |

## Origin / 编号溯源

R01–R13 and MA-000/MA-007 originate in the SiliconLifeOS research tree
(EMF project): truth-grading & meta-rule discipline from the V∞ methodology
and `P1/vinf` contract chain; MA-007 was instantiated 2026-10-14 in
`docs/math_attack_rules.yaml` v0.6. This repository is the protocol-local
renumbering and normative extraction (R13 single source of truth); the EMF
tree retains the reference implementation as its "EMF Profile".

---
`Wangyi Protocol | examples/oai_409_entry_audit | 2026-10-15`
