# Wangyi Protocol — L3 Audit & Truth-Grading Protocol for Multi-Agent Formal Artifacts

> **Maturity: v1.0-draft (Draft Specification + Reference Implementation)**
> The normative text of this specification is `DEFINITION`-level (a convention).
> The empirical results it has produced are `EMPIRICAL`-level. Neither is a
> mathematical theorem; see [Truth Grading](spec/wangyi_spec.md#2-truth-grading).

**One-line positioning:** MCP transports context, A2A transports messages —
**Wangyi Protocol transports claims + evidence + verdicts.** It is an evidence
layer that composes with communication protocols, not a competitor to them.

---

## What this is (and is not)

Wangyi Protocol is an **audit-adjudication and evidence-archival contract** for
formal artifacts (Lean/Coq/Isabelle proofs, mechanized scientific claims)
produced by AI multi-agent pipelines.

It **does** define: how to judge whether an Agent's formal artifact is
trustworthy, how to record the evidence, how to attach a truth grade, and how
to archive gaps permanently.

It **does not** define: how models or agents exchange messages, transport
bytes, or negotiate context. Those are transport/communication protocols
(MCP, A2A, ...). Wangyi Protocol runs *above* them, auditing the artifacts
they carry.

**Core honesty guarantee (built in, not bolted on):** when an auditor cannot
verify a claim — missing context, unresolvable imports, uncompilable external
corpus — the protocol **forces `INCONCLUSIVE`** and forbids `PASS`. "We did
not detect a defect" is a statement about the scanner, never a statement about
the mathematics (reverse-burden-of-proof gate).

## The three-layer validity model

The protocol instantiates the three-layer validity model of formal proofs:

| Layer | Name | Gate | Question it answers |
|-------|------|------|---------------------|
| **WP-L1** | Syntax | builds, no `sorry`, no unresolved imports | *Is it even valid Lean?* |
| **WP-L2** | Semantics | axiom-dependency clean (`#print axioms`), core objects bound to native libraries (e.g. Mathlib), every gap registered in a GAP ledger | *Does it mean what it claims?* |
| **WP-L3** | Meta-verification | the audit tools themselves pass positive/negative/escape/boundary controls; hash-frozen; version-locked | *Why should anyone trust the auditor?* |

WP-L3 is the **foundation**, not an add-on: without a self-tested auditor,
WP-L1/L2 verdicts carry no guarantee. A project may claim any level it has
*actually passed*; the ladder is per-level, not all-or-nothing.

## Core components

- **Truth-grading spec** — `DEFINITION` / `HYPOTHESIS` / `EMPIRICAL` /
  `THEOREM` / `GAP-OPEN` / `FALSIFIED`, with a strict no-recursive-upgrade rule
  (many independent empirical confirmations never add up to a theorem).
- **MA-series audit gates** — `MA-000` delivery gate, `MA-007` dual-condition
  Mathlib verification gate, ... ([spec/ma_000.md](spec/ma_000.md),
  [spec/ma_007.md](spec/ma_007.md))
- **R-series meta-rules** — R01–R13 audit discipline (append-only ledgers,
  hash locking, label discipline, ...)
- **Append-only archival** — superseded/defective material is never deleted;
  it is stamped `SUPERSEDED`/`DEPRECATED`/`HISTORICAL` and hash-locked.
- **Machine-readable verdict schema** —
  [schema/verdict_schema.yaml](schema/verdict_schema.yaml)

## Flagship evidence: the OAI 409-entry audit

The protocol's first full-scale demonstration surveyed **all 409 manuscript
entry points** of a large AI-generated formal-mathematics corpus
(~121,700 Lean files). Corpus-level verdict:

| Verdict | Count |
|---------|-------|
| `MATHLIB_VERIFIED_COMPLETE` | **0 / 409** |
| `INCONCLUSIVE` (condition B unverifiable) | 398 |
| `BLOCK` (dual failure) | 11 |

Additional findings: **287/409 (70%)** entry points rely on a facade-only
`import Mathlib`; **16** high-risk redefinitions of core mathematical objects
(Polynomial / Hypergraph / Ideal / Matrix / Module); **133** entries exceed a
200-module import closure (padding signal); largest closures: Progressions
4,774 / Ostmann 4,358 / MutuallyUnbiased 3,381.

The corpus's "zero textual `sorry`" was therefore an artifact of
project-internal redefinition plus facade imports — exactly the escape
channel the protocol's MA-007 gate is designed to expose.

- Full snapshot: [examples/oai_409_entry_audit/audit_snapshot.json](examples/oai_409_entry_audit/audit_snapshot.json)
  (SHA-256 `f40ab455…`, see `SHA256SUMS`)
- Case notes: [examples/oai_409_entry_audit/README.md](examples/oai_409_entry_audit/README.md)

Derived statistics only; the audited corpus itself is third-party material and
is **not redistributed** here.

## Reference implementation

The reference implementation lives in the `SiliconLifeOS` research tree
(`脚本/lean_import_audit.py`, `脚本/lean_sorry_scan.py`,
`docs/math_attack_rules.yaml` as the "EMF Profile"). It has been
**dual-control validated** — the positive control (a clean theorem whose only
dependencies are Lean's three core axioms) passes; the negative control (a
theorem that depends on `sorryAx`) is correctly flagged. An auditor that was
caught producing a false PASS and fixed (error-output-is-not-PASS rule) before
being trusted.

Conformance is per-artifact, judged by the gates; the reference implementation
is one toolchain that implements them, not part of the normative text.

## Maturity & limitations (read before citing)

1. **Single implementation.** The R/MA rules have one reference
   implementation. Portability to a second, independent codebase is
   **unverified**; that is why this is `v1.0-draft`, not "standard".
2. **Scope.** Verified on Lean 4 + Mathlib artifacts. Coq/Isabelle adapters
   are designed-for, not yet demonstrated.
3. **No truth guarantee.** The protocol certifies *audit-process integrity*,
   not mathematical truth. Per C-02 (same-base multi-agent review is not
   independent verification), same-corpus re-auditing never upgrades a truth
   grade.
4. **Truth level of this document.** The specification text is a
   `DEFINITION`-level convention; its published audit results are
   `EMPIRICAL`. Citation should say "conforms to Wangyi Protocol v1.0-draft",
   never "proven by Wangyi Protocol".

## How to use (no code ships here)

This repository contains **no executable scripts**. It is a contract, not a
toolchain: you feed the protocol text to an AI agent, and the agent writes
the audit scripts that conform to it.

Recommended usage loop:

1. **Feed** — attach `spec/wangyi_spec.md` (plus the relevant MA gate files,
   e.g. [spec/ma_007.md](spec/ma_007.md), and `spec/terms.md`) to your
   agent's context.
2. **Instruct** — ask the agent to implement an auditor for your artifact
   tree that satisfies the spec: transitive import-closure traversal,
   core-object redefinition detection, `#print axioms` full-chain
   sorryAx scan, and verdict output conforming to
   [schema/verdict_schema.yaml](schema/verdict_schema.yaml).
3. **Validate before trusting** — require the agent to run the WP-L3
   dual controls (§3.3): a positive control (clean artifact) and a negative
   control (artifact with an implanted defect, e.g. a `sorryAx` dependency).
   An auditor that has not passed both MUST NOT be trusted
   ([spec/wangyi_spec.md](spec/wangyi_spec.md) §3.3, R06).
4. **Archival** — ship each verdict with its snapshot + SHA-256 (R04),
   register gaps in an append-only GAP ledger, and grade every claim per §2.

For a worked example of what the agent should produce, see the flagship case
in [examples/oai_409_entry_audit/](examples/oai_409_entry_audit/).

## Citation / DOI

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23376296.svg)](https://doi.org/10.5281/zenodo.23376296)

Archived release: <https://doi.org/10.5281/zenodo.23376296>

```bibtex
@software{wangyi_protocol_v1,
  title   = {Wangyi Protocol: L3 Audit & Truth-Grading Protocol for
             Multi-Agent Formal Artifacts},
  author  = {Yi-Ting-zheng},
  year    = {2026},
  version = {v1.0-draft},
  doi     = {10.5281/zenodo.23376296},
  url     = {https://github.com/Yi-Ting-zheng/wangyi-protocol}
}
```

## License

Protocol text (spec, examples, schema, documentation) © 2026 Yi-Ting-zheng,
licensed under **CC BY-NC-ND 4.0** (see [LICENSE](LICENSE)): share with
attribution, noncommercial, no derivatives. Independent *implementations* of
the protocol are separate works and may be licensed by their authors
(LICENSE §Scope clarification 2).

## Repository layout

```
wangyi-protocol/
├── LICENSE                      # CC BY-NC-ND 4.0 + scope clarification
├── README.md                    # this file (bilingual: EN first, then中文)
├── spec/
│   ├── wangyi_spec.md           # normative specification (English)
│   ├── ma_000.md                # MA-000 delivery gate
│   ├── ma_007.md                # MA-007 dual-condition Mathlib gate
│   └── terms.md                 # bilingual glossary
├── examples/
│   └── oai_409_entry_audit/     # flagship case: snapshot + hash + notes
└── schema/
    └── verdict_schema.yaml      # machine-readable verdict schema
```

---
---

# 中文全文

**望易协议（Wangyi Protocol）——面向多智能体形式化制品的 L3 审计与真值分层协议**

> **成熟度：v1.0-draft（草案规范 + 参考实现）**
> 规范正文真值层级为 `DEFINITION`（约定），已产出的判定效果为 `EMPIRICAL`，
> 均非数学定理，详见[真值分层](spec/wangyi_spec.md#2-truth-grading)。

**一句话定位：** MCP 传上下文，A2A 传消息，**望易协议传"主张 + 证据 + 裁决"**。
它是叠在通信协议之上的证据层，与 MCP/A2A 是组合关系，不是竞争关系。

## 这是什么（不是什么）

望易协议是一份面向**形式化制品**（Lean/Coq/Isabelle 证明、机械化科学主张）
的**审计裁决与证据归档契约**，服务对象是 AI 多智能体流水线的产出。

**它定义**：如何判定 Agent 产出的形式化工件是否可信、如何记录证据、
如何标注真值层级、如何永久归档缺口。

**它不定义**：模型/智能体之间如何收发消息、传输字节、协商上下文——
那是传输/通信协议（MCP、A2A……）的职责。望易协议运行在它们之上，
审计它们承载的工件。

**内建的诚实保证（不是外挂）**：当审计器无法核验——上下文残缺、
import 不可解析、外部语料不可编译——协议**强制返回 `INCONCLUSIVE`**，
禁止输出 `PASS`。「未检出缺陷」永远是关于扫描器的陈述，不得转写为
关于数学对象的陈述（反向举证门）。

## 三层有效性模型

协议把形式化证明的三层有效性模型实例化为三级符合性梯级：

| 层级 | 名称 | 门槛 | 回答的问题 |
|------|------|------|-----------|
| **WP-L1** | 语法层 | 可构建、无 `sorry`、import 可解析 | *它算合法 Lean 吗？* |
| **WP-L2** | 语义层 | 公理依赖干净（`#print axioms`）、核心本体绑定原生库（如 Mathlib）、每个缺口登记入 GAP 台账 | *它说的就是它证的吗？* |
| **WP-L3** | 元验证层 | 审计工具本身通过阳性/阴性/逃逸/边界四类对照测试；哈希固化；版本锁定 | *凭什么相信审计器？* |

WP-L3 是**底座**不是附加项：没有自测过的审计器，WP-L1/L2 的裁决就没有
效力背书。任何项目只能声称它**实际通过**的层级——按级认证，非全有全无。

## 核心构件

- **真值分层规范**——`DEFINITION` / `HYPOTHESIS` / `EMPIRICAL` / `THEOREM` /
  `GAP-OPEN` / `FALSIFIED`，含递归升级禁令（多次独立实证不构成定理的充分条件）
- **MA 系列审计门**——MA-000 交付门、MA-007 Mathlib 双条件核验门等
- **R 系列元规则**——R01–R13 审计纪律（append-only 台账、哈希锁、标签纪律……）
- **Append-only 不可变归档**——被取代/缺陷材料永不删除，盖 `SUPERSEDED` /
  `DEPRECATED` / `HISTORICAL` 戳并哈希锁定
- **机器可读裁决 schema**——[schema/verdict_schema.yaml](schema/verdict_schema.yaml)

## 标杆实证：OAI 409 入口全量审计

协议的首次全量演示：对一个大型 AI 批量生成形式化数学语料库
（约 121,700 个 Lean 文件）的**全部 409 个手稿入口**完成测绘。语料级判决：

| 判定 | 数量 |
|------|------|
| `MATHLIB_VERIFIED_COMPLETE` | **0 / 409** |
| `INCONCLUSIVE`（条件 B 不可验） | 398 |
| `BLOCK`（双重失败） | 11 |

另发现：**287/409（70%）**入口依赖门面式 `import Mathlib`；**16 处**核心
数学对象高风险重定义（Polynomial / Hypergraph / Ideal / Matrix / Module）；
**133 个**入口闭包超 200 模块（填充信号）；最大闭包 Progressions 4,774 /
Ostmann 4,358 / MutuallyUnbiased 3,381。

该语料的「文本级零 sorry」因此被证实为**项目内重定义 + 门面 import** 造成的
逃逸假象——正是 MA-007 双条件门设计要暴露的通道。

- 完整快照：`examples/oai_409_entry_audit/audit_snapshot.json`（SHA-256 `f40ab455…`）
- 案例说明：`examples/oai_409_entry_audit/README.md`

仅含派生统计数据；被审计语料为第三方材料，**不在本仓再分发**。

## 参考实现

参考实现位于 `SiliconLifeOS` 研究树（`脚本/lean_import_audit.py`、
`脚本/lean_sorry_scan.py`、`docs/math_attack_rules.yaml` 作为 "EMF Profile"）。
已做**双向对照验证**：阳性对照（仅依赖 Lean 三条基础公理的干净定理）通过；
阴性对照（依赖 `sorryAx` 的定理）被正确拦截。审计器曾被自己的阴性对照
抓出假 PASS（错误输出误判），修复 `error≠PASS` 纪律后才被采信。

符合性按工件逐个裁决；参考实现只是一条工具链，不属于规范正文。

## 成熟度与局限（引用前必读）

1. **单实现**：R/MA 规则目前只有一个参考实现，向第二套独立代码库的可移植性
   **未验证**——这正是挂 `v1.0-draft` 而非宣称"标准"的原因。
2. **范围**：已在 Lean 4 + Mathlib 制品上验证；Coq/Isabelle 适配器有设计无实证。
3. **不作真值保证**：协议认证的是**审计过程完整性**，不是数学真值。
   依 C-02（同基座多 Agent 复核不构成独立验证），同语料重审永不升格真值层级。
4. **本文档自身真值层级**：规范正文是 `DEFINITION` 级约定，已发布审计结果为
   `EMPIRICAL` 级。引用应写"符合望易协议 v1.0-draft"，不得写"经望易协议证明"。

## 引用 / DOI

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23376296.svg)](https://doi.org/10.5281/zenodo.23376296)

存档发布：<https://doi.org/10.5281/zenodo.23376296>（BibTeX 见英文区 Citation 节）

## 使用方法（本仓不产脚本）

本仓库**不含任何可执行脚本**——它是一份契约，不是工具链：把协议规范喂给
AI Agent，由 Agent 写出符合规范的审计脚本。

推荐使用循环：

1. **喂**——把 `spec/wangyi_spec.md`（及相关 MA 门文件如
   [spec/ma_007.md](spec/ma_007.md)、术语表 `spec/terms.md`）附给 Agent
2. **指令**——要求 Agent 为你的制品树实现审计器：传递闭包遍历、核心对象
   重定义检测、`#print axioms` 全链路 sorryAx 扫描、按
   [schema/verdict_schema.yaml](schema/verdict_schema.yaml) 输出裁决
3. **先验后信**——必须先跑 WP-L3 双向对照（§3.3）：阳性对照（干净制品）+
   阴性对照（植入缺陷的制品，如依赖 `sorryAx`）。未过双控的审计器不得采信
4. **归档**——每份裁决随附快照 + SHA-256（R04），缺口入 append-only 台账，
   每条主张按 §2 标真值层级

Agent 应产出什么的完整范例，见
[examples/oai_409_entry_audit/](examples/oai_409_entry_audit/) 标杆案例。

## 许可证

协议文本（规范、实例、schema、文档）© 2026 Yi-Ting-zheng，采用
**CC BY-NC-ND 4.0**（见 [LICENSE](LICENSE)）：署名、非商业、禁演绎。
协议的独立**实现**是另行作品，可由实现者自行选择许可证
（LICENSE §Scope clarification 2）。

## 历史脉络

协议核心（三层有效性模型、AI 形式化缺陷公理、望易闭环范式对比）源自
SiliconLifeOS 研究体系；本文档为该体系审计纪律的**首次独立打包发布**。
完整规范正文见 [spec/wangyi_spec.md](spec/wangyi_spec.md)（英文）。

---

`Wangyi Protocol | v1.0-draft | L3 audit & truth-grading for formal artifacts | 2026-10-14`
