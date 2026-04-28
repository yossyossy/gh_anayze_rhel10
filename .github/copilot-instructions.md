# プロジェクトメモリ

## 技術スタック

| 要素 | 内容 |
|------|------|
| 言語 | Python（markitdown による設計書変換） |
| プロンプト | GitHub Copilot プロンプトファイル（`.github/prompts/*.prompt.md`） |
| 実装方針 | コーディング不要 — 実装はプロンプトファイル（Markdown）のみ |

---

## ディレクトリ構造

### `docs/` — 永続的ドキュメント（何を作るか）

| パス | 役割 |
|------|------|
| `data/inputs/` | 壁打ち・技術調査メモ（自由形式）。プロジェクトセットアップ時に自動読み込み |
| `docs/product-requirements.md` | プロダクト要求定義書 |
| `docs/functional-design.md` | 機能設計書 |
| `docs/architecture.md` | 技術仕様書 |
| `docs/repository-structure.md` | リポジトリ構造定義書 |
| `docs/development-guidelines.md` | 開発ガイドライン |
| `docs/glossary.md` | ユビキタス言語定義 |

- 基本設計を記述し、頻繁に更新しない
- プロジェクト全体の「北極星」

### `.steering/` — 作業単位のドキュメント（今回何をするか）

命名規則: `.steering/YYYYMMDD-タスク名/`（例: `20250115-add-user-profile`）

| ファイル | 内容 |
|---------|------|
| `requirements.md` | 今回の作業の要求内容 |
| `design.md` | 実装アプローチ |
| `tasklist.md` | 具体的なタスクリスト |

- 作業ごとに新規作成し、履歴として保持する

---

## 開発ルール

### ドキュメント作成時

**1ファイルずつ作成し、必ずユーザーの承認を得てから次に進む**

```
「[ドキュメント名]の作成が完了しました。内容を確認してください。
承認いただけたら次のドキュメントに進みます。」
```

### 実装前の確認

新しい実装を始める前に必ず実施:

1. `.github/copilot-instructions.md` を読む
2. 関連する永続ドキュメント（`docs/`）を読む
3. 既存の類似実装を検索
4. 既存パターンを理解してから実装開始

### ステアリングファイルの管理

**作業計画・実装・検証時は `.github/prompts/steering.prompt.md` の手順に従う**

| タイミング | 操作 |
|-----------|------|
| 作業計画時 | steering プロンプト モード1（ステアリングファイル作成） |
| 実装時 | steering プロンプト モード2（実装と tasklist.md 更新管理） |
| 検証時 | steering プロンプト モード3（振り返り） |

詳細な手順は `.github/prompts/steering.prompt.md` に定義されています。

### RHEL バージョン差異を表示する際の必須ルール

**RHEL バージョン間の差異（機能・コマンド・設定等）を表示する際は、以下を必ず全て含めること：**

1. **ライフサイクル情報**（各バージョンのフェーズ・サポート終了日）
2. **主要コンポーネントのバージョン変更**（カーネル・GCC・Python・OpenSSL 等）
3. **主な新機能・変更点**（カテゴリ別に整理）
4. **廃止・非推奨コマンド・機能の全一覧**（省略禁止）
5. **新機能の全一覧**（省略禁止）— 以下の2つを別々に表示すること：
   - **旧バージョンリリース時の新機能**（旧バージョン GA 時点での追加機能）
   - **新バージョンリリース時の新機能**（新バージョン GA 時点での追加機能）

主要コンポーネントの表示形式:

| コンポーネント | 旧バージョン | 新バージョン |
|-------------|------------|------------|
| `例: Python` | 3.9 / 3.11  | 3.12       |

廃止・非推奨コマンドの表示形式:

| コマンド / 機能 | 旧バージョンでの状態 | 新バージョンでの状態 | 代替手段 |
|----------------|---------------------|---------------------|---------|
| `例: iptables` | 対応（非推奨）       | **削除**            | `nftables` |

- 「一部のみ抜粋」「主要なもの」などの省略表現は禁止。把握している限り全て列挙すること。
- 状態は「対応」「非推奨」「**削除**」「新規追加」のいずれかで統一すること。
- **比較解析結果は必ず `.github/data/outputs/comparison/` に Markdown ファイルとして保存すること。**
  - 比較結果: `{type}-comparison.md`
  - レビューポイント: `{type}-review-points.md`

### RHELライフサイクルの日付について

**RHEL バージョン間の差異（機能・コマンド・設定等）を表示する際は、必ずライフサイクル情報（各バージョンのフェーズ・サポート終了日）もあわせて表示すること。**

**AIが学習データから日付・EOLを答えることは禁止。必ず以下の公式データを参照すること。**

公式URLからJSON・CSV形式でライフサイクルデータを取得できる:

```
https://access.redhat.com/product-life-cycles/?product=Red%20Hat%20Enterprise%20Linux
```

- 取得したデータは `data/inputs/product_lifecycle_data_YYYY-MM-DD.json` に保存して参照する
- EOLや日付に関する質問を受けた場合は、必ずユーザーに公式データの取得・参照を案内する

---

## 開発プロセス

### 初回セットアップ

1. このプロジェクトを使用
2. `#file:.github/prompts/setup-project.prompt.md` で永続的ドキュメントを対話的に作成（6ファイル）
3. `#file:.github/prompts/add-feature.prompt.md` で機能実装

### 日常的な使い方

基本は普通に会話で依頼する:

```
# ドキュメントの編集
PRDに新機能を追加してください
architecture.mdのパフォーマンス要件を見直して
glossary.mdに新しいドメイン用語を追加

# 機能追加（定型フローはプロンプトファイルを参照）
#file:.github/prompts/add-feature.prompt.md を使ってユーザープロフィール編集を実装して

# 詳細レビュー（詳細なレポートが必要なとき）
#file:.github/prompts/review-design.prompt.md を使って docs/product-requirements.md をレビューして
```

### RHEL 移行作業のフロー

**前提**: `markitdown` が必要（`pip install markitdown`）

```
# Step 1: 旧バージョンの設計書を Markdown に変換
#file:.github/prompts/convert-docs.prompt.md を使って ./input/rhel9-docs を変換して

# Step 2: 変換済み設計書を AI で解析
#file:.github/prompts/analyze-design.prompt.md を使って ./output/converted rhel9 all を解析して

# Step 3: RHEL10 の設計書も同様に変換・解析
#file:.github/prompts/convert-docs.prompt.md を使って ./input/rhel10-docs を変換して
#file:.github/prompts/analyze-design.prompt.md を使って ./output/converted rhel10 all を解析して

# Step 4: 新旧バージョンの差分を比較
#file:.github/prompts/compare-rhel.prompt.md を実行して

# Step 5: 新バージョン設計書の初稿を生成
#file:.github/prompts/generate-design.prompt.md を使って standard を生成して

# Step 6: AI 生成物をレビュー・承認
#file:.github/prompts/review-design.prompt.md を使って ./output/generated/standard-rhel10-draft.md をレビューして
```
