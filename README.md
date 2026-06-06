# claude-config

ニチコマ合同会社の Claude Code スキル・コマンドを配布する plugin marketplace。

## インストール

Claude Code 上で marketplace を追加し、欲しいものだけを選んで導入できる。

```bash
# 1. marketplace を追加（最初の一度だけ）
/plugin marketplace add nichicoma/claude-config

# 2-a. 欲しいものだけ個別に入れる
/plugin install verify-design@nichicoma
/plugin install create-pr@nichicoma

# 2-b. まとめて全部入れる
/plugin install nichicoma-all@nichicoma
```

`/plugin marketplace add` に渡すのは GitHub の `owner/repo`。`@nichicoma` の部分は marketplace 名（`.claude-plugin/marketplace.json` の `name`）。

## 配布している plugin

### スキル（自動発動）

タスクの内容に応じて Claude が自動で発動する。

| plugin | タイミング | 説明 |
|---|---|---|
| `verify-design` | 実装前 | 設計上の穴を対話で潰してから着手する |
| `theory-check` | 実装後 | 「なぜこの設計にしたか」を確認し、メンタルモデルの形成を促す |
| `code-review` | レビュー | ローカル差分を設計の妥当性・意図の明確さの観点でレビュー |

発動条件: 複数ファイルにまたがる実装、状態遷移、外部連携、認証の変更、新規パターン、複雑なビジネスロジック。
typo 修正や既存パターンの軽微な修正では発動しない。

### コマンド（手動）

| plugin | コマンド | 説明 |
|---|---|---|
| `create-pr` | `/create-pr` | 設計意図・トレードオフ・影響範囲を織り込んだ説明文 + インラインコメントを生成 |
| `review-pr` | `/review-pr` | PR URL から変更を解説し、問題点や質問を提示 |

### 全部入り

| plugin | 内容 |
|---|---|
| `nichicoma-all` | 上記の skill 3個と command 2個をまとめて導入 |

## リポジトリ構成

```
.
├── .claude-plugin/
│   └── marketplace.json   # marketplace 定義（個別 plugin + 全部入り）
├── skills/                # 自動発動スキル
│   ├── verify-design/SKILL.md
│   ├── theory-check/SKILL.md
│   └── code-review/SKILL.md
└── commands/              # スラッシュコマンド
    ├── create-pr.md
    └── review-pr.md
```

各 plugin は `source: "./"`（リポジトリ root）を共有し、`skills` / `commands` フィールドで含める対象を絞り込んでいる。新しい skill や command を追加するときは、`skills/` または `commands/` にファイルを置き、`marketplace.json` の `plugins` に entry を 1つ足す。

## 管理者向け: marketplace の更新

plugin を追加・変更したら、利用者は次で最新を取り込む。

```bash
/plugin marketplace update nichicoma
```
