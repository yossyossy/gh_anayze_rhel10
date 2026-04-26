---
mode: agent
description: RHEL比較結果と旧設計書をもとに、新バージョン（RHEL10）対応の設計書ドラフトをAIで自動生成する。compare-rhelの後に実行。
---

# 新バージョン設計書自動生成

比較結果と旧設計書を元に、RHEL10 対応の設計書初稿を AI が生成します。

## 使い方

```
#file:.github/prompts/generate-design.prompt.md を使って [種別] [旧設計書パス] [比較結果パス] を生成して
```

例:
```
#file:.github/prompts/generate-design.prompt.md を使って standard ./output/analyzed/rhel9/standard-os-design.md ./output/comparison/standard-comparison.md を生成して
```

引数省略時はファイル選択をインタラクティブに行います。

---

## 実行手順

### ステップ1: 入力ファイルの確認

以下の 2 ファイルを読み込む:
1. 旧バージョンの解析済み設計書（`./output/analyzed/rhel9/*.md`）
2. 新旧比較結果（`./output/comparison/*-comparison.md`）

読み込んだ内容を元に、生成する設計書の概要をユーザーに提示して確認を得る。

### ステップ2: 設計書ドラフトの AI 生成

以下のプロンプトで新バージョン設計書の初稿を生成する:

---

**生成プロンプト（内部使用）**:

```
あなたは RHEL 設計書の作成専門家です。

以下の情報をもとに、RHEL10 対応の {type} 設計書の初稿を作成してください。

## 生成ルール

1. 旧設計書の構成（章立て・セクション）を基本的に踏襲すること
2. 比較結果の「変更あり」「追加」「削除」の項目を新バージョンの値・仕様に更新すること
3. AI が生成・変更した箇所には `<!-- AI生成: 要レビュー -->` コメントを付与すること
4. 値が不明・確認必要な箇所は `<!-- TODO: 要確認 -->` を付与して空欄または仮値とすること
5. 「変更なし」の項目は旧設計書の値をそのまま引き継ぐこと

## 旧バージョン設計書（RHEL9）

{old_design}

## 新旧比較結果

{comparison}
```

---

### ステップ3: 生成結果の保存

出力先: `./output/generated/{type}-rhel10-draft.md`

ファイル先頭にメタ情報を付加する:

```markdown
---
generatedAt: <ISO8601>
basedOn:
  oldDesign: <旧設計書パス>
  comparison: <比較結果パス>
status: draft
reviewStatus: pending
---

> ⚠️ このファイルは AI により自動生成された初稿です。
> `<!-- AI生成: 要レビュー -->` マークの箇所を中心に必ず人によるレビューを実施してください。
> `<!-- TODO: 要確認 -->` マークの箇所は値の確認・入力が必要です。

# {type} 設計書 RHEL10 版（初稿）

<AI の生成出力>
```

### ステップ4: TODO 項目の一覧表示

生成後、`<!-- TODO: 要確認 -->` が含まれる箇所を抽出して一覧表示する:

```
生成完了: output/generated/standard-rhel10-draft.md

AI生成マーク: 12箇所
TODO（要確認）:
  [L45] Section: ネットワーク設定 > MTU 値（RHEL10 推奨値を確認してください）
  [L78] Section: SELinux > カスタムポリシー（環境依存のため手動設定が必要）
  ...

次のステップ: #file:.github/prompts/review-design.prompt.md を使って ./output/generated/standard-rhel10-draft.md をレビューして
```

---

## 注意事項

- 生成された設計書は**初稿（ドラフト）**であり、そのまま本番利用しないこと
- `<!-- AI生成: 要レビュー -->` マークの全箇所をエンジニアが確認すること
- 生成後は必ず `#file:.github/prompts/review-design.prompt.md` でレビューステータスを記録すること
- 旧設計書の入力ファイルは上書きしない
