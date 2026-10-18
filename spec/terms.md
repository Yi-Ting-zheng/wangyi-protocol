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
| **MA gate** | MA 审计门 | BLOCKING machine-checkable rule (MA-000 delivery, MA-007 Mathlib dual-condition, MA-008 provenance, …). 可机器校验的阻断规则。 |
| **Provenance ledger** | 溯源台账 | Append-only, per-declaration production record: model family/version/signature + contribution_type + audit verdict. 只增的逐声明生产记录：模型家族/版本/签名 + 贡献类型 + 审计裁决。 |
| **Contribution type** | 贡献类型 | `AI_DRAFT` / `AI_LEMMA_SUGGEST` / `HUMAN_FULL` / `AI_MINOR_TWEAK` / `AI_COMPLETE`. 强制区分 AI 初稿/人工重构/纯手写，禁止"AI 辅助"模糊表述。 |
| **Model signature** | 模型签名 | Open-weight: weight-checkpoint SHA-256; closed-source: official release identifier + release digest. Permanently pins which model produced a draft. 开源填权重 sha256，闭源填官方发布标识+发布摘要；永久锁定初稿出自哪个模型。 |
| **Execution environment** | 执行环境 | Closed enum `internal_cluster` / `external_api` / `local_runtime`; records the conditions of generation per stage (serving regime: truncation, sampling caps, metering). No credibility weight; mixed environment must be split into separate stages. 闭集枚举，逐 stage 记录生成条件（服务侧截断/采样上限/计量口径）；不携带可信度权重；混环境必须拆条。 |
| **Cost ledger reference** | 成本账本指针 | SHA-256 pointer into the compute-chain billing ledger. Token/cache/quota figures are self-attested and live ONLY in that ledger, never in the provenance manifest. 指向算力链计费账本切片的哈希指针。token/缓存/配额是自报量，只入计费账本，不入溯源清单。 |
| **Knowledge-provenance chain** | 知识制品溯源链 | Cryptographic evidence applied to the genealogy of formal artifacts; distinct from a compute work-proofs chain, it competes for no compute reward. 密码学存证在形式化制品谱系的落地；区别于算力工作量证明链，不争夺算力奖励。 |
| **Audit verdict (per-stage)** | 阶段审计裁决 | The WP verdict recorded on a provenance stage at production time; draft disclosure set `audit_verdict_enum` (v1.3) is warn-level — unknown values are disclosed, not blocked. 生产时刻逐 stage 记录的 WP 裁决；v1.3 的初稿披露集为 warn 级，不合值只披露不阻断。 |
| **Provenance verdict record** | 溯源裁决记录 | Optional published shape of an MA-008 adjudication (v1.3 `provenance_verdict_record_fields`); publishing is RECOMMENDED, a non-conforming record is discarded not BLOCKed. MA-008 裁决发布的可选记录形状（v1.3）；发布为推荐项，不合形状丢弃而非阻断。 |
| **Gap-ledger entry shape** | 缺口台账对账锚 | The field set a GAP ledger entry must parse to (v1.3 `gap_ledger_entry_fields`); P8's strict key = `gap_id ↔ GAP-SKELETON record's artifact_id`. GAP 台账条目对账字段集（v1.3）；P8 严格对账键 = gap_id ↔ 骨架记录 artifact_id。 |
| **Escape-class inventory** | 逃逸对照类清单 | Registered detector classes for source-lying in the anti-inflation field (§5.5: draft-hash replay / same-family fingerprints / timeline reconciliation / environment–billing reconciliation), each with its residual blind spot; the remainder outside all classes is undetectable by design and is stated as such. 反通胀字段来源撒谎的登记对照类（§5.5 四类，各带盲区边缘）；类外余部如实登记为不可检测。 |
| **Reference implementation profile** | 参考实现 Profile | The concrete toolchain + rule file implementing this spec for one project (e.g. EMF Profile). 某项目落地本规范的具体工具链 + 规则文件。 |
| **v2.0-draft** | 草案规范 | Maturity tag: single implementation, portability partially supported (hash conformance vectors), not a "standard". 成熟度标签：单实现、可移植性有向量支撑但未验证，不称"标准"。 |
