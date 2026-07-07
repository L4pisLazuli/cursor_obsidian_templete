---
created: 2026-07-01
updated: 2026-07-01
tags:
  - chat
  - persistent
  - vault-design
source: cursor
status: active
aliases: []
related:
  - analysis/2026-07-01-analysis-vault-setup.md
  - chats/persistent/2026-07-01-vault-setup-test.md
---

## 概要

Obsidian Vault + Cursor Rules の設計方針（サンプルノート）。

## 要点

- 一時チャット（`chats/temp/`）と継続蓄積（`chats/persistent/`）を分離
- Templater + Cursor rules で frontmatter を統一管理
- 分析は `analysis/` フォルダで横断的に実施

## 関連リンク

- [[chats/persistent/2026-07-01-vault-setup-test]]
- [[analysis/2026-07-01-analysis-vault-setup]]
