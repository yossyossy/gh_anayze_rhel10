# 差分比較

## 概要

本リポジトリは、**RHEL バージョンアップ時の設計書移行作業**を Claude Code スキル（スラッシュコマンド）で自動化するサンプルです。

コーディングは不要です。対話するだけで、旧バージョン設計書の解析・比較・新バージョン設計書の生成・レビューまでを一貫して行えます。

### 前提

- Python と `markitdown` がインストール済みであること

```bash
pip install markitdown
```

## 使い方

### 3. RHEL 移行フローの実行

旧バージョン設計書（.doc/.docx/.pdf/.pptx/.xlsx）を `./input/rhel9-docs/` に配置し、以下の順でスキルを実行します。

```
# Step 1: 設計書を Markdown に変換
/convert-docs ./input/rhel9-docs

# Step 2: 変換済み設計書を AI で解析
/analyze-design ./output/converted rhel9 all

# Step 3: RHEL10 の設計書も同様に変換・解析
/convert-docs ./input/rhel10-docs
/analyze-design ./output/converted rhel10 all

# Step 4: 新旧バージョンの差分を比較
/compare-rhel

# Step 5: 新バージョン設計書の初稿を生成
/generate-design standard

# Step 6: AI 生成物をレビュー・承認
/review-design ./output/generated/standard-rhel10-draft.md
```

## スキル一覧

| スキル | 用途 |
|--------|------|
| `/convert-docs` | 設計書ファイルを markitdown で Markdown に一括変換 |
| `/analyze-design` | 変換済み設計書を AI で解析し、設定項目を構造化抽出 |
| `/compare-rhel` | 旧新バージョンの解析済み設計書を比較し、変更点を一覧化 |
| `/generate-design` | 比較結果をもとに新バージョン設計書の初稿を AI 生成 |
| `/review-design` | AI 生成物にレビューコメントと承認ステータスを記録 |

## リポジトリ構造

### `.github/.steering/` について

作業単位のドキュメントを管理するディレクトリです。  
作業を開始するたびに `YYYYMMDD-タスク名/`（例: `20250115-add-user-profile`）の形式でサブディレクトリを作成し、以下の3ファイルをセットで管理します。

| ファイル | 内容 |
|---------|------|
| `requirements.md` | 今回の作業の要求内容 |
| `design.md` | 実装アプローチ |
| `tasklist.md` | 具体的なタスクリスト |

作業ごとに新規作成し、削除せず履歴として蓄積します。  
作成・更新の手順は [`.github/prompts/steering.prompt.md`](.github/prompts/steering.prompt.md) に定義されています。

```
.github/
├── copilot-instructions.md          # プロジェクト全体の Copilot 指示
├── prompt.md                        # 汎用プロンプト
├── docs/                            # 永続ドキュメント（何を作るか）
│   ├── architecture.md              # 技術仕様書
│   ├── development-guidelines.md    # 開発ガイドライン
│   ├── functional-design.md         # 機能設計書
│   ├── glossary.md                  # ユビキタス言語定義
│   ├── product-requirements.md      # プロダクト要求定義書
│   ├── repository-structure.md      # リポジトリ構造定義書
├── data/                            # データディレクトリ
│   └── inputs/                      # インプット（壁打ち・技術調査メモ等）
│   └── outputs/                     # スキル実行による成果物（生成物）
├── prompts/                         # Copilot プロンプトファイル
│   ├── add-feature.prompt.md
│   ├── analyze-design.prompt.md
│   ├── compare-rhel.prompt.md
│   ├── convert-docs.prompt.md
│   ├── generate-design.prompt.md
│   ├── review-design.prompt.md
│   ├── setup-project.prompt.md
│   ├── steering.prompt.md
│   └── steering-templates/          # ステアリングファイルのテンプレート
│       ├── design.md
│       ├── requirements.md
│       └── tasklist.md
└── .steering/                       # 作業単位のドキュメント（今回何をするか）
    └── （作業ごとに YYYYMMDD-タスク名/ を作成）
README.md
```

## 出力ディレクトリ構造

スキル実行後に `.github/data/outputs/` 配下に生成されます。

```
.github/data/outputs/
├── converted/    # Markdown 変換済みファイル
├── analyzed/     # AI 解析済みファイル（バージョン別）
│   ├── rhel9/
│   └── rhel10/
├── comparison/   # 新旧比較結果
├── generated/    # AI 生成設計書初稿
├── reviews/      # レビュー履歴
└── logs/         # 変換ログ
```
