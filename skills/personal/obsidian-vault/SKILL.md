---
name: obsidian-vault
description: 在 Obsidian vault 中搜索、创建和管理笔记，支持 wikilinks 和索引笔记。当用户想要在 Obsidian 中查找、创建或整理笔记时使用。 (Search, create, and manage notes in the Obsidian vault with wikilinks and index notes. Use when user wants to find, create, or organize notes in Obsidian.)
---

# Obsidian Vault

## Vault 位置 (Vault location)

`/mnt/d/Obsidian Vault/AI Research/`

根目录基本是扁平结构。
> Mostly flat at root level.

## 命名约定 (Naming conventions)

- **索引笔记**：聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
  > **Index notes**: aggregate related topics (e.g., `Ralph Wiggum Index.md`, `Skills Index.md`, `RAG Index.md`)
- 所有笔记名称使用**首字母大写**（Title Case）
  > **Title case** for all note names
- 不使用文件夹进行组织 - 用链接和索引笔记代替
  > No folders for organization - use links and index notes instead

## 链接 (Linking)

- 使用 Obsidian `[[wikilinks]]` 语法：`[[Note Title]]`
  > Use Obsidian `[[wikilinks]]` syntax: `[[Note Title]]`
- 笔记在底部链接到相关/依赖笔记
  > Notes link to dependencies/related notes at the bottom
- 索引笔记就是 `[[wikilinks]]` 列表
  > Index notes are just lists of `[[wikilinks]]`

## 工作流 (Workflows)

### 搜索笔记

```bash
# 按文件名搜索
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# 按内容搜索
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或直接在 vault 路径上使用 Grep/Glob 工具。
> Or use Grep/Glob tools directly on the vault path.

### 创建新笔记

1. 文件名使用 **Title Case**
   > Use **Title Case** for filename
2. 以学习单元的形式编写内容（按 vault 规则）
   > Write content as a unit of learning (per vault rules)
3. 在底部添加到相关笔记的 `[[wikilinks]]`
   > Add `[[wikilinks]]` to related notes at the bottom
4. 如果是编号序列的一部分，使用层级编号方案
   > If part of a numbered sequence, use the hierarchical numbering scheme

### 查找相关笔记

在 vault 中搜索 `[[Note Title]]` 以查找反向链接：
> Search for `[[Note Title]]` across the vault to find backlinks:

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找索引笔记

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
