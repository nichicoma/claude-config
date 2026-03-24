# claude-config

Claude Codeのスキル・コマンド・エージェント設定の共有リポジトリ。

## カスタムコマンド

`commands/` 配下のファイルを `~/.claude/commands/` にコピーすると、全プロジェクトで使えるようになる。

```bash
# 一括コピー
cp -r commands/* ~/.claude/commands/
```

### コマンド一覧

| コマンド | タイミング | 説明 |
|---|---|---|
| `/verify-design` | 実装前 | 設計上の穴を対話で潰してから着手する |
| `/theory-check` | 実装後 | 「なぜこの設計にしたか」を確認し、メンタルモデルの形成を促す |
| `/create-pr` | PR作成 | 設計意図・トレードオフ・影響範囲を織り込んだ説明文 + インラインコメントを生成 |
| `/code-review` | レビュー | ローカル差分を6つの観点でレビュー |
| `/review-pr` | レビュー | PR URLから変更を解説し、問題点や質問を提示 |

### 自動発動

これらはClaude Codeのスキルとして動作し、タスクの内容に応じて自動で発動する。

- 複数ファイルにまたがる実装、状態遷移、外部連携、認証の変更、新規パターン、複雑なビジネスロジック → `/verify-design` が実装前に発動、`/theory-check` が実装後に発動
- PR URLを共有 → `/review-pr` が発動
- 「レビューして」と依頼 → `/code-review` が発動
- 「PR作って」と依頼 → `/create-pr` が発動

typo修正や既存パターンの軽微な修正では発動しない。

### 共通定義

`commands/shared/` には複数コマンドから参照される共通定義がある。

| ファイル | 内容 |
|---|---|
| `review-prefixes.md` | MUST / SHOULD / NIT / IMO / FYI / QUESTION の定義 |
| `mental-model-detection.md` | メンタルモデル不在の検出基準 |
| `memory-save-rules.md` | 対話で得た知見のメモリ保存ルール |

## 設計思想

- コードレビューの本質は「バグ探し」ではなく「これはプロダクトに組み込むべきか」という判断
- AIが書いたコードにメンタルモデルは存在しない。実装者が「自分のもの」にするプロセスが必要
- 実装前の設計検証が、実装後の手戻りを防ぐ最もコスト効率の高い投資

参考記事
- [コードレビューとは何か](https://zenn.dev/baleenstudio/articles/what-is-a-code-review)
- [Code Review Is Not About Catching Bugs](https://www.davidpoll.com/2026/02/code-review-is-not-about-catching-bugs)
- [Google SWE Book - Code Review](https://abseil.io/resources/swe-book/html/ch09.html)
