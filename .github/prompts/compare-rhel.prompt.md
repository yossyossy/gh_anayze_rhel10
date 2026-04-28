---
mode: agent
description: RHEL旧バージョンと新バージョンの解析済み設計書を比較し、変更点（変更/追加/削除/変更なし）と影響度を一覧化する。analyze-designの後に実行。
---

# 新旧バージョン比較

RHEL9 と RHEL10 の解析済み設計書を比較し、変更点と影響度を一覧化します。

## 使い方

```
#file:.github/prompts/compare-rhel.prompt.md を使って [旧バージョンディレクトリ] [新バージョンディレクトリ] を比較して
```

例:
```
#file:.github/prompts/compare-rhel.prompt.md を使って ./output/analyzed/rhel9 ./output/analyzed/rhel10 を比較して
```

引数省略時は `./output/analyzed/rhel9` と `./output/analyzed/rhel10` を使用します。

---

## 実行手順

### ステップ1: 対象ファイルの確認

旧バージョンディレクトリと新バージョンディレクトリの `.md` ファイルを列挙し、
同一種別（ファイル名のプレフィックス: standard / basic / detail / unittest）でペアリングする。

ペアリング結果をユーザーに提示して確認を得る:

```
比較対象（3ペア）:
  📄 standard: rhel9/standard-os-design.md ↔ rhel10/standard-os-design.md
  📄 basic:    rhel9/basic-network.md ↔ rhel10/basic-network.md
  ⚠️  detail:  rhel9/detail-apache.md ↔ 対応ファイルなし（新バージョンで追加予定）
```

### ステップ2: 設計書ペアの比較（AI 解析）

各ペアについて両ファイルを読み込み、以下のプロンプトで比較する:

---

**比較プロンプト（内部使用）**:

```
あなたは RHEL 設計書の差分を分析する専門家です。

以下の 2 つの設計書を比較し、設定項目ごとの変更点を分析してください。

## 出力形式

### セクション名

| 設定項目 | 旧バージョン値 | 新バージョン値 | 変更種別 | 影響度 | レビューポイント |
|---------|-------------|-------------|---------|-------|--------------|
| 項目名  | 旧値         | 新値         | modified | high  | 確認事項      |

**変更種別**:
- `unchanged`: 変更なし
- `modified`: 値が変更された
- `added`: 新バージョンで追加された設定
- `removed`: 旧バージョンから削除された設定

**影響度**:
- `high`: システム動作・セキュリティ・パフォーマンスに直接影響する変更
- `medium`: 設定の調整が必要だが影響範囲が限定的
- `low`: 軽微な変更、またはデフォルト値のまま使用可能

## 旧バージョン設計書（{old_version}）

{old_content}

## 新バージョン設計書（{new_version}）

{new_content}
```

---

### ステップ3: 比較結果の保存

出力先: `.github/data/outputs/comparison/`

各ペアについて以下を保存する:

**比較結果 Markdown**: `.github/data/outputs/comparison/{type}-comparison.md`

```markdown
---
oldVersion: rhel9
newVersion: rhel10
documentType: standard
createdAt: <ISO8601>
---

# 比較結果: 標準設計書（RHEL9 → RHEL10）

## サマリー

| 変更種別     | 件数 |
|------------|------|
| 変更なし     | 32   |
| 変更あり     | 8    |
| 追加         | 5    |
| 削除         | 3    |
| **合計**    | 48   |

影響度別: 高 3件 / 中 7件 / 低 6件

## 詳細比較

<AI の比較出力>
```

**レビューポイント一覧**: `.github/data/outputs/comparison/{type}-review-points.md`

```markdown
# レビューポイント: 標準設計書（影響度：高）

影響度が「高」の変更点を優先してレビューしてください。

| # | セクション | 設定項目 | 変更内容 | 確認事項 |
|---|----------|---------|---------|---------|
| 1 | セキュリティ | SELinux モード | enforcing → permissive | 意図した変更か確認 |
...
```

### ステップ4: 全体サマリーの表示

```
比較完了: 3ペア

  ✅ standard-comparison.md
     変更あり 8件（高 3 / 中 4 / 低 1）、追加 5件、削除 3件

  ✅ basic-comparison.md
     変更あり 2件（高 0 / 中 2 / 低 0）、追加 1件、削除 0件

  ⚠️  detail-apache.md
     新バージョン対応ファイルがありません。新規作成が必要です。

出力先: .github/data/outputs/comparison/

次のステップ:
  レビュー実施  → #file:.github/prompts/review-design.prompt.md を使ってレビューして
  設計書の生成  → #file:.github/prompts/generate-design.prompt.md を実行して
```

---

## 注意事項

- 比較結果は AI による自動分析であり、必ず人によるレビューが必要
- 影響度「高」の項目から優先的にレビューする
- 対応ファイルが片方のみ存在する場合は「対象なし」として記録する
