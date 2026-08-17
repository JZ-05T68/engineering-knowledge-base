# Engineering Knowledge Base v0.5.0

面向个人工程资料的 Windows 本地知识管理系统：把用户主动导入的 PDF 转换为可阅读、可检索、
可复核、可引用和可长期复用的工程知识资产。

> 本仓库是公开展示仓库。完整源码、依赖、启动脚本和恢复说明位于
> [engineering-knowledge-base-src](https://github.com/JZ-05T68/engineering-knowledge-base-src)。
> 当前不提供独立安装包。

| 项目状态 | v0.5.0 当前事实 |
|---|---|
| 当前公开版本 | **v0.5.0 — AI Foundation / Optional Hybrid Retrieval** |
| 运行边界 | Windows、单用户、offline-first、仅 `127.0.0.1` |
| 本地事实来源 | 用户文件 + SQLite / FTS5 |
| AI | 可选；默认 manual；无 API Key 时核心功能完整可用 |
| 源码 | 已在独立源码仓库公开 |

[v0.5.0 公开发布摘要](docs/v0.5.0-release-summary.md)

## 从文档到工程能力

```text
Document → Understanding → Retrieval → Reuse → Engineering Capability
```

EKB 不是云文档平台，也不是 PDF 摘要器或聊天机器人外壳。它围绕原始页、人工笔记、证据来源
和可回溯引用建立个人工程知识工作流：

1. 导入 PDF，保留原件并按页渲染 PNG；
2. 提取已有文本层，对扫描、手写或异常页面标记待复核；
3. 阅读原始页面，编辑页面级 Markdown 与结构化笔记；
4. 使用关键词、向量或 Hybrid Retrieval 定位候选页；
5. 把整页、文字选区或图片区域加入证据篮并人工确认；
6. 生成带文档、文件名、页码和来源位置的引用证据包；
7. 在本地完成备份、诊断、恢复与继续复用。

## v0.5.0 真实能力

- 文档导入、SHA-256 去重、逐页渲染、文本层提取和本地 OCR；
- 页面阅读、复核、Markdown、四类结构化笔记、标签、项目与文档管理；
- SQLite FTS5 lexical retrieval，支持关键词高亮、筛选、排序和结果状态恢复；
- page-level embedding 持久化、SHA-256 freshness 与 persistent vector recall；
- lexical + vector 候选并集、RRF 融合、来源标签与关键词 fallback；
- 整页、文字选区、图片区域证据及 confirmed / unconfirmed 人工边界；
- 引用证据包、来源失效 fail-closed、完整备份和恢复预检；
- production `data/` / 8501 与 staging `staging-data/` / 8502 隔离。

### AI 的边界

v0.5.0 提供厂商无关 provider 契约和可选 Qwen 基础设施。当前默认配置包括
`qwen3.7-plus`、`qwen3.7-text-embedding` 与预留的 `qwen3-rerank` 契约；rerank 运行时通道
尚未接入。只有用户显式配置 API Key 并触发相应操作时才可能访问外部服务。

AI 不可用或没有 API Key 时，PDF 导入、阅读、关键词检索、笔记、证据、文档管理、备份与恢复
仍在本地工作。production 不自动生成 embedding；真实付费索引受 staging guard 与 cost guard
约束。当前索引粒度是 page-level，不宣称 retrieval 已解决所有工程领域问题。

## v0.5.0 界面

以下截图均来自 v0.5.0 实际界面，使用隔离的合成工程演示资料；不包含真实用户资料、数据库、
API Key、Token、账号、私密文件名或机器绝对路径。

### 首页

![EKB v0.5.0 首页概览](assets/home-overview.jpg)

### 页面阅读与 Markdown 整理

![双栏页面阅读器](assets/page-reader.jpg)

### 关键词检索与命中依据

![关键词检索结果](assets/keyword-search-results.jpg)

### Hybrid Retrieval 产品门控

![AI 混合检索模式](assets/hybrid-retrieval-mode.jpg)

### 证据来源工作流

![证据篮与来源确认](assets/evidence-source-workflow.jpg)

### 文档管理与删除安全边界

![文档管理](assets/document-management.jpg)

## 架构与数据边界

```text
Streamlit UI (127.0.0.1 only)
        ↓
application services
        ↓
SQLite + FTS5 + local files  ← single source of truth
        ↓
optional provider interface  → Qwen only after explicit configuration/action
```

- production：`127.0.0.1:8501` + `data/`；
- staging：`127.0.0.1:8502` + `staging-data/`；
- 原始 PDF 和页面 PNG 不会被自动覆盖或自动删除；
- 系统不包含注册、登录、OAuth、JWT、用户角色或云同步；
- 只处理用户主动提供或明确授权的材料，不扫描私人聊天或抓取未授权内容。

## 验证事实

v0.5.0 runtime closure baseline 的正式发布证据包括：

- Gate 2B：17 / 17 PASS；
- pytest：1399 / 1399 passed；
- Ruff：clean；
- production health：HTTP 200；
- SQLite schema v8，integrity ok，foreign key violations = 0；
- 首页与 13 个页面人工验收 PASS；
- Phase 11D frozen 5-query benchmark：Hybrid 5 / 5 PASS。

这些数字描述已执行的 v0.5.0 发布门禁，不是对所有机器性能或所有查询质量的保证。

## 获取与恢复

软件环境的恢复基线在源码仓库中维护：

- [源码与依赖](https://github.com/JZ-05T68/engineering-knowledge-base-src)
- [Windows 安装与运行](https://github.com/JZ-05T68/engineering-knowledge-base-src#首次安装)
- [Windows 灾备恢复](https://github.com/JZ-05T68/engineering-knowledge-base-src/blob/main/docs/windows-recovery.md)

GitHub 不保存 `.env`、真实 Key、私人数据库、原始用户文档、页面图像、日志、PID、缓存或未脱敏
备份。更换电脑时需重新创建本地 `.env` 并人工注入自己的凭据；私人知识资产应通过用户控制的
安全介质和 EKB 完整备份流程迁移。

## 版本历史与后续方向

历史摘要：[v0.2.0](docs/v0.2.0-release-summary.md) ·
[v0.3.0](docs/v0.3.0-release-summary.md) ·
[v0.4.0](docs/v0.4.0-release-summary.md) ·
[v0.4.3](docs/v0.4.3-release-summary.md) ·
[v0.5.0](docs/v0.5.0-release-summary.md)

v0.5.x 后续重点是真人测试、retrieval quality、benchmark 扩展、缺陷修复、稳定性、成本与 UX。
chunk-level indexing、运行时 rerank 等尚未完成的事项不会在本仓库中表述为现有功能。
