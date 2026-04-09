---
type: schema
version: 1.0
updated: 2026-04-09
author: 冯念牧师 × Antigravity
methodology: Karpathy LLM Wiki
---
# LLM Wiki Schema — AI 行为规范

> 本文件定义 AI 如何维护这个知识库。它是三层架构的"大脑"。
> 参考：[Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

## 核心原则

1. **知识编译一次，持续更新** — 不是每次从零检索，而是增量构建
2. **交叉引用是一等公民** — 每个页面都应有入站和出站链接
3. **人类选素材定方向，AI 做整理维护** — 明确分工
4. **Wiki 是活的** — 新素材进来后，旧页面也要更新

## 三层架构

### Layer 1: Raw Sources（原始素材，不可变）
- **路径**: `20_Resources/`, `神学书籍/`, `00_Inbox/`（剪藏后的原始文章）
- **权限**: AI **只读**，绝不修改
- **内容**: 文章、论文、剪藏、书籍笔记、参考资料
- **说明**: 这是真相来源 (source of truth)

### Layer 2: Wiki（知识层，AI 维护）
- **路径**: `30_Wiki/`
- **权限**: AI **读写**，人类只读（浏览和思考）
- **内容**: 摘要页、实体页、概念页、综合分析、对比分析
- **子目录**:
  - `entities/` — 人物、机构、教会、地点
  - `concepts/` — 神学概念、写作方法论、技术概念
  - `sources/` — 每个原始来源的摘要页
  - `syntheses/` — 跨文档深度分析
  - `comparisons/` — 不同观点/理论对比
- **核心文件**:
  - `index.md` — 全局目录索引（AI 导航入口）
  - `log.md` — 操作日志（append-only 时间线）

### Layer 3: Schema（行为规范）
- **路径**: `WIKI_SCHEMA.md`（本文件）
- **权限**: 人类和 AI 协同演化
- **内容**: 定义 Wiki 结构、操作流程、页面格式

---

## 操作流程

### 1. Ingest（摄入新素材）

当用户说"摄入这篇文章"或"把这个加入知识库"时：

```
1. 读取原始素材（不修改）
2. 与用户讨论关键要点（如需要）
3. 在 sources/ 创建摘要页，包含：
   - YAML frontmatter（来源、日期、标签）
   - 核心观点摘要
   - 关键引用
   - 与现有知识的关联
4. 更新或创建相关 entities/ 和 concepts/ 页面
5. 在已有页面中添加交叉引用 [[WikiLinks]]
6. 更新 index.md（添加新条目）
7. 追加 log.md（记录本次操作）
```

**一次 Ingest 可能触碰 10-15 个 Wiki 页面**，这是正常的。

### 2. Query（查询知识）

当用户提问时：

```
1. 先读 index.md 定位相关页面
2. 读取相关 Wiki 页面（不是读原始素材！）
3. 如需深入，再追溯到 Raw Sources
4. 综合回答，带 [[WikiLinks]] 引用
5. 如果答案有独立价值，建议归档为新的 Wiki 页面
```

### 3. Lint（健康检查）

定期或用户要求时执行：

```
检查项：
- 🏝️ 孤岛页面：没有任何入站链接的页面
- 👻 幽灵节点：被多次提及但没有独立页面的概念
- 📅 过时信息：新素材与旧页面的矛盾
- 🔗 断链：指向不存在页面的 WikiLinks  
- 📊 索引同步：index.md 是否与实际页面一致
- 🏷️ 标签一致性：frontmatter 标签是否规范

输出一份健康报告，建议修复动作。
```

---

## 页面格式规范

### YAML Frontmatter 模板

```yaml
---
title: 页面标题
type: entity | concept | source | synthesis | comparison
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [标签1, 标签2]
sources: [来源文件名或链接]
related: [相关Wiki页面]
---
```

### 正文规范

- 使用 `[[WikiLinks]]` 进行交叉引用，不用普通 Markdown 链接
- 每个页面开头有一段简要定义/摘要
- Source 页面必须标注原始文件路径
- 引用原文用 `> 引用格式`
- 页面底部有 `## 相关页面` section

### 命名规范

- 文件名用中文，简洁明确
- 实体：`张三.md`、`武汉圣光堂.md`
- 概念：`因信称义.md`、`LLM Wiki方法论.md`
- 来源摘要：`S-原文标题简写.md`（S 前缀区分来源页）
- 综合分析：`基督教中国化路径比较.md`
- 对比：`C-RAG vs LLM Wiki.md`（C 前缀区分对比页）

---

## 附件管理

- 图片存储到**当前文件夹下** `attachments/` 子目录
- 引用格式：`![[attachments/image.png]]`
- 剪藏文章后按 `Ctrl+Shift+D` 下载外链图片到本地

---

## Git 版本管理

- 远程仓库：`https://github.com/elfwind724/antigravity`（私有）
- Obsidian Git 插件自动 commit + push，间隔 10 分钟
- AI 批量修改前建议手动 commit 一次作为"保存点"

---

## 与写作系统集成

本 Wiki 与 Write Master 写作大师系统联动：
- **Phase 1 浸泡期**：从 Wiki 的 concepts/ 和 syntheses/ 获取已积累的知识
- **素材采集**：新素材先 Ingest 进 Wiki，再用于写作
- **作品归档**：完成的作品归档到 `99_Archives/作品展厅/`，摘要归入 Wiki

---

## 领域特色

本知识库的核心领域：
1. **神学与教牧** — 系统神学、教会史、讲道学、教牧辅导
2. **基督教中国化** — 政策研究、神学反思、实践案例
3. **写作与内容创作** — 公众号文章、讲章、论文
4. **AI 与知识管理** — 工具方法论、工作流优化
