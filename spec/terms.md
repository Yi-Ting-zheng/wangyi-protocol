# Wangyi Protocol — Glossary / 术语表

> Bilingual, protocol-normative terms. English headword first.
> 双语规范术语表，英文词头在前。

| English | 中文 | Definition / 定义 |
|---------|------|------------------|
| **Formal artifact** | 形式化制品 | A proof script + its build manifest + its ledgers (Lean/Coq/Isabelle). 证明脚本 + 构建清单 + 台账的总称。 |
| **Truth grade** | 真值层级 | `DEFINITION < HYPOTHESIS < EMPIRICAL < THEOREM`, plus `GAP-OPEN` / `FALSIFIED`. 严格有序，禁跳级。 |
| **No-recursive-upgrade** | 递归升级禁令 | Many independent `EMPIRICAL` results never sum to a `THEOREM`. 多次独立实证不构成定理充分条件。 |
| **WP-L1 / L2 / L3** | 语法层 / 语义层 / 元验证层 | Three-layer validity model: compilable / semantically clean / auditor self-certified. 三层有效性模型：可编译 / 语义干净 / 审计器自证。 |
| **Reverse burden of proof gate** | 反向举证门 | "Undetected" describes the scanner, never the mathematics. 「未检出」是关于扫描器的陈述，不是关于数学的陈述。 |
| **INCONCLUSIVE** | 不可判定 | Forced verdict when verification cannot complete; MUST NOT be rounded to PASS. 核验无法完成时的强制判定，禁四舍五入为 PASS。 |
| **Facade import** | 门面导入 | A native-library import present at the entry file but not reflecting real reliance of the main chain; detected via transitive closure. 入口处有、主链实质不依赖的原生库导入；经传递闭包检测。 |
| **Core-object redefinition** | 核心对象重定义 | Project-local def/structure shadowing a native object (e.g. Mathlib `Polynomial`) without importing its provider. HIGH risk. 项目内重定义顶替原生核心对象且未 import 其提供模块，高风险。 |
| **Derived alias** | 派生别名 | `abbrev X := <imported native type>`; permitted (LOW). 基于已导入原生类型的缩写别名，放行（低风险）。 |
| **Closure bloat suspect** | 闭包膨胀嫌疑 | Import closure > 200 internal modules; padding signal, warning-level. 导入闭包超 200 内部模块，填充信号，警示级。 |
| **sorryAx** | sorryAx 公理 | Lean's internal axiom marking incomplete proofs; zero tolerance in condition B. Lean 内部标记未完成证明的公理，条件 B 零容忍。 |
| **Dual control** | 双向对照 | Positive + negative (+ escape + boundary) tests an auditor must pass before being trusted. 审计器受信前必过的阳性+阴性（+逃逸+边界）测试。 |
| **Error ≠ PASS** | 错误输出不等于通过 | Toolchain error maps to UNVERIFIABLE/FAIL, never PASS (R07). 工具链报错只映射到不可验/失败。 |
| **GAP ledger** | 缺口台账 | Append-only register of unfinished obligations and registered hypotheses. 未完成义务与登记假设的只增台账。 |
| **Append-only archive** | 只增归档 | Superseded material is stamped (`SUPERSEDED`/`DEPRECATED`/`HISTORICAL`) + hash-locked, never deleted. 被取代材料盖戳 + 哈希锁定，永不删除。 |
| **Hash lock** | 哈希锁 | SHA-256 checksum shipped with every published verdict snapshot. 每份发布裁决快照随附 SHA-256 校验和。 |
| **C-02** | 同基座复核非独立验证 | Same-base multi-agent review is internal re-orchestration, not independent verification; independence is carried by rule/observation-dimension difference. 同基座多 Agent 复核不构成独立验证；独立性由规则/观测维度差异承载。 |
| **MA gate** | MA 审计门 | BLOCKING machine-checkable rule (MA-000 delivery, MA-007 Mathlib dual-condition, …). 可机器校验的阻断规则。 |
| **Reference implementation profile** | 参考实现 Profile | The concrete toolchain + rule file implementing this spec for one project (e.g. EMF Profile). 某项目落地本规范的具体工具链 + 规则文件。 |
| **v1.0-draft** | 草案规范 | Maturity tag: single implementation, portability unverified; not a "standard". 成熟度标签：单实现、可移植性未验证，不称"标准"。 |
