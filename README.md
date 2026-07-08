# cursor_obsidian_templete

Cursor Cloud Agent / Gemini / ChatGPT 等で使う、設計用プロンプト兼 Obsidian ナレッジベース（vault テンプレート）。

このリポジトリは **private 運用を前提** とするため、クローン後は `profile/user.example.md` を `profile/user.md` にコピーして個人設定を記入します。

## フォルダ構成

```
cursor_obsidian_templete/
├── .cursor/rules/          # Cursor Agent 向けルール
├── .obsidian/              # Obsidian 設定（workspace は Git 除外）
├── templates/              # Templater テンプレート
├── profile/                # 本人属性（private 運用前提の個人設定）
├── chats/
│   ├── temp/               # 一時チャット（短命・削除可）
│   └── persistent/         # 継続蓄積（長期保持・分析対象）
│       └── _archive/       # アーカイブ済み persistent
├── analysis/               # persistent の横断分析結果
└── assets/                 # 添付画像など
```

## クイックスタート

1. リポジトリをクローン
2. Obsidian で「フォルダを vault として開く」→ このフォルダを選択
3. `profile/user.example.md` を `profile/user.md` にコピーし、個人設定を記入
   - `profile/user.md` は `.gitignore` 対象外（private 運用前提）
4. Templater をインストール（下記 Obsidian セットアップ参照）
5. Cursor でこのフォルダを開き、Agent が `.cursor/rules/` を自動参照

## 初回セットアップ（チェックリスト）

- Obsidian でこのフォルダを **vault** として開けている
- `profile/user.md`（`user.example.md` からコピー）が存在し、個人設定を記入済み
- Obsidian の **Templater** 設定で Template folder location が `templates` になっている
- Cursor でこのフォルダを開き、チャット作成時に `chats/temp/` に保存される状態になっている

## サンプルノート

`chats/persistent/` と `analysis/` に **運用例** が入っています。クローン後に削除して空の vault から始めても構いません。

| ファイル | 役割 |
|---------|------|
| `chats/persistent/2026-07-01-vault-design.md` | 蓄積ノートの見本 |
| `chats/persistent/2026-07-01-vault-setup-test.md` | promote 後の見本 |
| `analysis/2026-07-01-analysis-vault-setup.md` | 横断分析の見本 |

## Cursor Agent での使い方

### 新規チャット（一時）

Cursor Agent に会話内容を記録させる場合:

> `chats/temp/` に今日のチャットをノートとして保存して

- ファイル名: `YYYY-MM-DD-HHmmss.md`（自動生成時）→ promote 時に `YYYY-MM-DD-トピック.md` へリネーム
- frontmatter に日付・タグが自動付与される

### 一時 → 蓄積への昇格（promote）

価値のあるチャットを長期保存する場合:

> このノートを蓄積して / persistent に移して / 保存して

Agent が以下を実行する:
1. `chats/temp/` → `chats/persistent/` へ移動
2. タグを `temp` → `persistent` に変更
3. `status` を `active` に更新
4. トピック名にリネーム

### 横断分析

蓄積ノートをまとめて分析する場合:

> persistent の ○○ について分析して

Agent が `chats/persistent/` を検索し、`analysis/` に分析ノートを作成する。

### 整理・アーカイブ

| やりたいこと | 指示例 |
|-------------|--------|
| temp 整理 | 「temp の古いノートを整理して」 |
| アーカイブ | 「この persistent をアーカイブして」 |

## Obsidian セットアップ

1. Obsidian で「フォルダを vault として開く」→ この `cursor_obsidian_templete` フォルダを選択
2. 設定 → コミュニティプラグイン → **Templater** をインストール・有効化
3. Templater 設定で Template folder location を `templates` に設定
4. 新規ノートのデフォルト保存先は `chats/temp/`（`.obsidian/app.json` で設定済み）

### テンプレート一覧

| テンプレート | 用途 |
|-------------|------|
| `templates/chat-temp.md` | 一時チャット |
| `templates/chat-persistent.md` | 蓄積ノート |
| `templates/analysis.md` | 分析ノート |

Templater コマンド（`Ctrl/Cmd + P` → Templater）から各テンプレートを挿入できる。

## frontmatter スキーマ

```yaml
---
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags:
  - chat
  - temp          # または persistent / analysis
source: cursor   # cursor | gemini | chatgpt | manual
status: draft    # draft | active | archived
aliases: []
related: []
sources_updated: YYYY-MM-DD  # analysis のみ（参照元確認日）
---
```

## 命名規則

- 一般: `YYYY-MM-DD-短いトピック.md`
- 分析: `YYYY-MM-DD-analysis-トピック.md`
- トピック: 英語ケバブケース推奨（例: `vault-design`）。日本語も可
- assets: `YYYY-MM-DD-説明.ext` または元ノート名を接頭辞に

## Cursor Rules

`.cursor/rules/` に 6 つのルールが配置されている:

| ルール | 適用範囲 |
|-------|---------|
| `vault-core.mdc` | 常時適用（vault 全体の方針） |
| `chat-temp.mdc` | `chats/temp/**/*.md` |
| `chat-persistent.mdc` | `chats/persistent/**/*.md` |
| `analysis-workflow.mdc` | `analysis/**/*.md` |
| `obsidian-frontmatter.mdc` | 全 `**/*.md` |
| `git-workflow.mdc` | Git 関連ファイル・ルール変更時 |

## Git 運用（private 運用前提）

| 変更内容 | 運用 |
|---------|------|
| ノート追記・promote・analysis 追加 | `main` 直コミット |
| `.cursor/rules/` の変更 | ブランチ + PR 推奨 |
| フォルダ再構成 | ブランチ + PR 必須 |
| private 運用方針の変更（`.gitignore` 含む） | ブランチ + PR 推奨 |

- `.gitignore`: workspace 系・`.trash/` を除外
- `.gitattributes`: LF 統一（Windows 対策）
- `profile/user.md`: `.gitignore` 対象外。private 運用ではコミットして運用してOK（公開する場合は再度 ignore する）
- コミット前: `profile/user.md` を含める/含めない方針がブレていないか確認、不要なローカル設定が含まれていないか確認

## トリガー文言の例

| やりたいこと | 指示例 |
|-------------|--------|
| 一時保存 | 「この会話を temp に保存して」 |
| 蓄積 | 「蓄積して」「persistent に移して」 |
| 分析 | 「persistent の API設計について分析して」 |
| 整理 | 「temp の古いノートを整理して」 |
| アーカイブ | 「このノートをアーカイブして」 |


## ライセンス

[CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/) — 詳細は [LICENSE](LICENSE) を参照。自由にコピー・改変・再配布してよい。
