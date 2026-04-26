---
mode: agent
description: Markdown変換済みの旧バージョン設計書をAIで解析し、設定項目・値・説明を構造化して抽出する。convert-docsの後に実行。
---

# 設計書 AI 解析

変換済みの Markdown 設計書を読み込み、設定項目・値・説明を構造化して抽出します。

## 使い方

```
#file:.github/prompts/analyze-design.prompt.md を使って [入力ディレクトリ] [バージョン] [種別] を解析して
```

例:
```
#file:.github/prompts/analyze-design.prompt.md を使って ./output/converted rhel9 standard を解析して
#file:.github/prompts/analyze-design.prompt.md を使って ./output/converted rhel9 all を解析して
```

- **バージョン**: `rhel9` / `rhel10` など（任意の文字列）
- **種別**: `standard`（標準設計書）/ `basic`（基本設計書）/ `detail`（詳細設計書）/ `unittest`（単体テスト項目）/ `all`（全種別）

引数省略時は `./output/converted`、バージョン `rhel9`、種別 `all` を使用します。

---

## 実行手順

### ステップ1: 対象ファイルの確認

入力ディレクトリ内の `.md` ファイルを列挙し、ユーザーに提示する。

### ステップ2: 設計書の AI 解析

各 Markdown ファイルを読み込み、以下のプロンプトで解析する:

---

**解析プロンプト（内部使用）**:

```
あなたは RHEL（Red Hat Enterprise Linux）の設計書を解析する専門家です。

以下の設計書（種別: {type}、バージョン: {version}）を読み込み、
設定項目を構造化して抽出してください。

## 出力形式

以下の Markdown テーブル形式で出力してください:

### セクション名

| 設定項目 | 設定値 | 説明・補足 |
|---------|-------|----------|
| 項目名  | 値    | 説明      |

## 抽出ルール

- OSパラメータ、サービス設定、ファイルパス、パーミッション、ネットワーク設定など
  インフラ設計に関わる項目を漏れなく抽出すること
- 値が「環境依存」「要確認」などの場合はそのまま記載すること
- セクションは元の設計書の章立てに従うこと

## 設計書内容

{content}
```

---

### ステップ3: 解析結果の保存

出力先: `./output/analyzed/{version}/{type}-{filename}.md`

例: `./output/analyzed/rhel9/standard-os-design.md`

ファイル先頭にメタ情報を付加する:

```markdown
---
source: <元ファイル名>
version: <バージョン>
type: <種別>
analyzedAt: <ISO8601日時>
---

# 解析結果: <ファイル名>

<AIの解析出力>
```

### ステップ4: 結果サマリーの表示

```
解析完了: 3件
  ✅ standard-design.md → output/analyzed/rhel9/standard-standard-design.md
     抽出セクション: 8件、設定項目: 47件
  ✅ basic-design.md    → output/analyzed/rhel9/basic-basic-design.md
     抽出セクション: 12件、設定項目: 63件
  ❌ detail-design.md  → 解析失敗（原因: ファイルが空です）

次のステップ:
  RHEL10 の設計書も解析する場合 → #file:.github/prompts/analyze-design.prompt.md を使って ./output/converted rhel10 all を解析して
  比較を行う場合                 → #file:.github/prompts/compare-rhel.prompt.md を実行して
```

---

## 注意事項

- 解析結果は AI による自動抽出であり、必ず人によるレビューが必要
- 1ファイルの解析失敗は他のファイルに影響させない
- 解析済みファイルはレビュー後に `#file:.github/prompts/review-design.prompt.md` でステータスを記録する
