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
`Wangyi Protocol | changelog | v1.0-draft`
