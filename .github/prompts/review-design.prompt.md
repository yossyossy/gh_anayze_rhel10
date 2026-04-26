---
mode: agent
description: AI生成した解析結果・比較結果・設計書ドラフトにレビューコメントと承認ステータスを記録する。AI生成物の品質確認に使用。
---

# AI 生成物レビュー

AI が生成した解析結果・比較結果・設計書ドラフトに対してレビューコメントと承認ステータスを記録します。

## 使い方

```
#file:.github/prompts/review-design.prompt.md を使って [対象ファイルパス] をレビューして
```

例:
```
#file:.github/prompts/review-design.prompt.md を使って ./output/generated/standard-rhel10-draft.md をレビューして
#file:.github/prompts/review-design.prompt.md を使って ./output/comparison/basic-comparison.md をレビューして
```

引数省略時は `./output/` 配下のレビュー対象ファイルを一覧表示して選択を促します。

---

## 実行手順

### ステップ1: 対象ファイルの読み込みと現状確認

1. 対象ファイルを読み込む
2. 対応するレビュー履歴ファイル `./output/reviews/<ファイル名>.review.md` が存在する場合は読み込む
3. 現在のレビューステータスとコメント一覧をユーザーに提示する

```
対象ファイル: output/generated/standard-rhel10-draft.md
現在のステータス: pending（未レビュー）
コメント数: 0件

AI生成マーク数: 12箇所
TODO（要確認）: 3箇所
```

### ステップ2: レビューサポート（レビューポイントの提示）

ファイル内の以下のマーカーを抽出してレビューポイント一覧を表示する:

- `<!-- AI生成: 要レビュー -->` — AI が変更・生成した箇所
- `<!-- TODO: 要確認 -->` — 値の確認が必要な箇所

```
レビューポイント一覧:

[1] L45 - SELinux モード: permissive に変更（AI生成: 要レビュー）
    → 旧値: enforcing / 新値: permissive
    → 確認: RHEL10 のセキュリティポリシーに合致しているか

[2] L78 - MTU 値: 未設定（TODO: 要確認）
    → RHEL10 の推奨 MTU 値を確認して設定してください

[3] L102 - rsyslog 設定: 新規追加（AI生成: 要レビュー）
    → RHEL10 で追加されたパラメータです。値を確認してください
```

### ステップ3: ユーザーのレビュー入力受付

ユーザーからの入力を元に記録する:

1. **ステータス設定**: `approved`（承認）/ `needs_revision`（要修正）/ `pending`（保留）
2. **コメント追加**: 各レビューポイントへのコメント
3. **TODO 解決**: TODO 箇所への実際の値の記入（ユーザーが指示した場合）

ユーザーが指定した値があれば対象ファイルも直接更新する（`<!-- TODO: 要確認 -->` を実際の値に置換）。

### ステップ4: レビュー履歴の保存

レビュー結果を `./output/reviews/<ファイル名>.review.md` に保存（追記）する:

```markdown
---
targetFile: output/generated/standard-rhel10-draft.md
---

# レビュー履歴

## <ISO8601日時> — approved

**ステータス**: approved
**コメント**:
- [L45] SELinux permissive は意図した設定。RHEL10 の要件に合致している
- [L78] MTU 値を 1500 に設定済み
- [L102] rsyslog 設定を確認。問題なし

---
```

ステータスが `approved` の場合は対象ファイルの frontmatter も更新する:

```yaml
reviewStatus: approved
reviewedAt: <ISO8601>
```

### ステップ5: レビュー完了サマリーの表示

```
レビュー記録完了

対象: output/generated/standard-rhel10-draft.md
ステータス: ✅ approved
コメント: 3件
TODO 解決: 1件（MTU 値を 1500 に更新）

レビュー履歴: output/reviews/standard-rhel10-draft.md.review.md

次のステップ:
  他のファイルのレビュー  → #file:.github/prompts/review-design.prompt.md を使って ./output/generated/basic-rhel10-draft.md をレビューして
  全レビュー状況の確認    → #file:.github/prompts/review-design.prompt.md を実行して（引数なしで一覧表示）
```

---

## レビュー状況の一覧表示

引数なしで実行した場合、`./output/` 配下の全対象ファイルのレビュー状況を表示する:

```
レビュー状況一覧:

  ✅ approved      output/generated/standard-rhel10-draft.md
  ⚠️  needs_revision output/generated/basic-rhel10-draft.md
  🔲  pending       output/generated/detail-rhel10-draft.md
  🔲  pending       output/comparison/standard-comparison.md

承認済み: 1件 / 要修正: 1件 / 未レビュー: 2件
```

---

## 注意事項

- レビューは AI の判定ではなく**必ず人（エンジニア）が実施**すること
- `needs_revision` になったファイルは修正後に再度このプロンプトを実行してステータスを更新すること
- レビュー履歴は削除・上書きせず追記のみ行う
