# MA-008 Hash Conformance Vectors

> **Truth level:** `DEFINITION` (convention data, not code — the repository
> still ships no executable scripts, per README "How to use").
> **Normative source:** [spec/ma_008.md](../../spec/ma_008.md) §4.2
> (hash-normalization appendix), added as a v1.2-draft schema change.
> **Date:** 2026-10-18

## Purpose

A conforming MA-008 implementation MUST agree with the reference algorithm
on **every vector** in [`vectors.yaml`](vectors.yaml). The vectors pin the
hash normalization contract so that a second independent implementation can
prove hash interoperability — the missing precondition for moving past
"single implementation, portability unverified".

## The five vectors, in one line each

| id | Pins |
|---|---|
| `stmt-01-multiline` / `stmt-02-oneline-equal` | statement hashes are whitespace-collapsed → the same statement wrapped across lines hashes identically to its one-line form |
| `proof-01-raw-bytes` | proof hashes are **byte-exact** (only trailing whitespace stripped) — collapsing whitespace before hashing fails here |
| `proof-02-conjecture-empty` | `CONJECTURE` proof hash = SHA-256 of the empty string |
| `proof-03-skeleton-sorry` | `GAP-SKELETON` proof slice is the literal `sorry` |

## Verification note

All five expected hashes were computed by replicating the algorithm of the
reference implementation and cross-checked against the recorded
`manifest_sha256` conventions used by that implementation on frozen real
manifests. The vectors were re-parsed and re-hashed after writing
(5/5 OK, same session). If an implementation disagrees with any vector,
the implementation is non-conformant; if the vector set itself is wrong,
the defect report goes to this repository's CHANGELOG (append-only, R02).

## Equality-note (statement vectors 1–2)

Because statement hashing collapses whitespace, two *different*
declarations that differ only in layout hash identically — that is
intended (layout is not semantic content for statements). Proof hashing
does NOT collapse: tactic indentation is a distinguishable production
state, deliberately preserved.
