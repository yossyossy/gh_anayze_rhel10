# リポジトリ構造定義書 (Repository Structure Document)

## プロジェクト構造

```
project-root/
├── input/                     # 入力設計書（変換前の元ファイル）
│   ├── rhel9-docs/            # RHEL9 旧バージョン設計書
│   └── rhel10-docs/           # RHEL10 新バージョン設計書
├── output/                    # スキル実行による生成物
│   ├── converted/             # Markdown 変換済みファイル
│   │   ├── rhel9/             # RHEL9 変換済み
│   │   └── rhel10/            # RHEL10 変換済み
│   ├── analyzed/              # AI 解析済み構造化データ
│   │   ├── rhel9/             # RHEL9 解析結果
│   │   └── rhel10/            # RHEL10 解析結果
│   ├── compared/              # 差分比較結果
│   ├── generated/             # AI 生成設計書ドラフト
│   └── reviewed/              # レビュー記録
├── docs/                      # プロジェクトドキュメント（永続的）
│   ├── data/inputs/           # 壁打ち・ブレインストーミングメモ（インプット）
│   └── data/outputs/          # スキル実行による成果物（生成物）
│   ├── product-requirements.md
│   ├── functional-design.md
│   ├── architecture.md
│   ├── repository-structure.md
│   ├── development-guidelines.md
│   └── glossary.md
├── .claude/                   # Claude Code 設定
│   └── skills/                # スキル定義（Markdown）
│       ├── convert-docs/
│       ├── analyze-design/
│       ├── compare-rhel/
│       ├── generate-design/
│       └── review-design/
├── .steering/                 # 作業単位ドキュメント（一時的）
├── .env.example               # 環境変数テンプレート
├── .gitignore
├── CLAUDE.md                  # Claude Code プロジェクト設定
└── README.md
```

---

## ディレクトリ詳細

### input/ (入力設計書ディレクトリ)

#### input/rhel9-docs/

**役割**: RHEL9 対象の旧バージョン設計書を格納。スキル実行の起点となる入力データ。

**配置ファイル**:
- `*.doc` / `*.docx`: Word 形式の設計書
- `*.pdf`: PDF 形式の設計書
- `*.pptx`: PowerPoint 形式の設計書
- `*.xlsx`: Excel 形式の設計書
- `*.md` / `*.yml` / `*.yaml`: 変換不要なテキスト形式

**命名規則**:
- ファイル名は元の設計書名をそのまま使用（変更不要）
- 設計書種別がわかる名称が望ましい（例: `standard-design.docx`, `basic-design.pdf`）

**依存関係**:
- 依存可能: なし（入力専用）
- 依存禁止: output/ への直接書き込み禁止

#### input/rhel10-docs/

**役割**: RHEL10 対象の新バージョン設計書を格納。差分比較のために rhel9-docs と同種の設計書を配置する。

**配置ファイル**: rhel9-docs/ と同じ拡張子に対応

---

### output/ (生成物ディレクトリ)

#### output/converted/

**役割**: `/convert-docs` スキルにより生成された Markdown ファイルを格納。

**構造**:
```
output/converted/
├── rhel9/
│   ├── standard-design.md
│   ├── basic-design.md
│   └── ...
└── rhel10/
    ├── standard-design.md
    └── ...
```

**命名規則**: `{元ファイル名（拡張子なし）}.md`

#### output/analyzed/

**役割**: `/analyze-design` スキルにより生成された構造化解析結果を格納。

**構造**:
```
output/analyzed/
├── rhel9/
│   ├── standard.md    # 標準設計書の解析結果
│   ├── basic.md       # 基本設計書の解析結果
│   ├── detail.md      # 詳細設計書の解析結果
│   └── test.md        # 単体テスト項目の解析結果
└── rhel10/
    └── ...（同上）
```

**ファイル形式**: 設定項目・設定値・説明の Markdown テーブル

#### output/compared/

**役割**: `/compare-rhel` スキルにより生成された差分比較結果を格納。

**構造**:
```
output/compared/
└── diff-result.md     # 変更種別・影響度を含む差分一覧
```

再実行時は `diff-result_v2.md` のようにサフィックスを付与。

#### output/generated/

**役割**: `/generate-design` スキルにより生成された RHEL10 対応設計書ドラフトを格納。

**構造**:
```
output/generated/
├── standard-rhel10-draft.md
├── basic-rhel10-draft.md
├── detail-rhel10-draft.md
└── test-rhel10-draft.md
```

#### output/reviewed/

**役割**: `/review-design` スキルにより記録されたレビュー結果を格納。

**構造**:
```
output/reviewed/
├── standard-rhel10-draft-review.md
└── ...
```

---

### docs/ (ドキュメントディレクトリ)

**配置ドキュメント**:
- `data/inputs/`: 壁打ち・ブレインストーミングメモ（自由形式）、インプットファイル
- `data/outputs/`: スキル実行による成果物（生成物）
- `product-requirements.md`: プロダクト要求定義書
- `functional-design.md`: 機能設計書
- `architecture.md`: アーキテクチャ設計書
- `repository-structure.md`: リポジトリ構造定義書（本ドキュメント）
- `development-guidelines.md`: 開発ガイドライン
- `glossary.md`: 用語集

---

### .claude/skills/ (スキル定義ディレクトリ)

**役割**: Claude Code スキルの定義ファイルを格納。各サブディレクトリが1つのスキルに対応。

**構造**:
```
.claude/skills/
├── convert-docs/
│   └── SKILL.md        # /convert-docs スキル定義
├── analyze-design/
│   └── SKILL.md        # /analyze-design スキル定義
├── compare-rhel/
│   └── SKILL.md        # /compare-rhel スキル定義
├── generate-design/
│   └── SKILL.md        # /generate-design スキル定義
└── review-design/
    └── SKILL.md        # /review-design スキル定義
```

**命名規則**:
- ディレクトリ名: kebab-case（コマンド名と一致させる）
- スキルファイル: `SKILL.md` 固定

---

## ファイル配置規則

### 入力ファイル

| ファイル種別 | 配置先 | 命名規則 |
|------------|--------|---------|
| RHEL9 設計書 | `input/rhel9-docs/` | 元ファイル名をそのまま使用 |
| RHEL10 設計書 | `input/rhel10-docs/` | 元ファイル名をそのまま使用 |

### 出力ファイル

| ファイル種別 | 配置先 | 命名規則 |
|------------|--------|---------|
| 変換済み Markdown | `output/converted/{version}/` | `{元ファイル名}.md` |
| 解析結果 | `output/analyzed/{version}/` | `{docType}.md` |
| 差分比較結果 | `output/compared/` | `diff-result.md`（再実行時は `_v2` 付与） |
| 生成ドラフト | `output/generated/` | `{docType}-rhel10-draft.md` |
| レビュー記録 | `output/reviewed/` | `{対象ファイル名}-review.md` |

### 設定ファイル

| ファイル種別 | 配置先 | 命名規則 |
|------------|--------|---------|
| 環境変数テンプレート | プロジェクトルート | `.env.example` |
| 環境変数（実際） | プロジェクトルート | `.env`（Git 管理外） |
| Claude Code 設定 | プロジェクトルート | `CLAUDE.md` |

---

## 命名規則

### ディレクトリ名
- **バージョン識別子**: `rhel9` / `rhel10`（ハイフンなしの小文字）
- **スキルディレクトリ**: kebab-case（例: `convert-docs`, `analyze-design`）

### ファイル名
- **設計書種別識別子**: `standard`（標準）/ `basic`（基本）/ `detail`（詳細）/ `test`（単体テスト）
- **ドラフトファイル**: `{種別}-rhel10-draft.md`
- **レビューファイル**: `{対象ファイル名}-review.md`
- **再実行時のバージョンサフィックス**: `_v2`, `_v3`（既存ファイルの上書き防止）

---

## 特殊ディレクトリ

### .steering/ (ステアリングファイル)

**役割**: 特定の開発作業の「今回何をするか」を定義する一時的なドキュメント群

**構造**:
```
.steering/
└── [YYYYMMDD]-[task-name]/
    ├── requirements.md   # 今回の作業の要求内容
    ├── design.md         # 変更内容の設計
    └── tasklist.md       # タスクリスト
```

**命名規則**: `20250426-add-mw-support` 形式

### .claude/ (Claude Code 設定)

**役割**: Claude Code のスキル・コマンド定義

**構造**:
```
.claude/
└── skills/               # スキル定義（Markdown）
```

---

## 除外設定

### .gitignore 推奨設定

```gitignore
# 環境変数（API キーを含む）
.env

# 入力設計書（機密情報を含む可能性がある）
input/

# 生成物（再生成可能）
output/

# OS・エディタ固有ファイル
.DS_Store
Thumbs.db
*.swp

# Python キャッシュ
__pycache__/
*.pyc
```

**注意**: `input/` と `output/` は機密設計書を含む可能性があるため、原則 Git 管理外とする。チームの方針に応じて管理対象を調整すること。
