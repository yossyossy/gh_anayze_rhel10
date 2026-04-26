# 開発ガイドライン (Development Guidelines)

## 基本方針

本プロジェクトは **Claude Code スキル（Markdown ファイル）** として実装するため、従来のコーディング規約よりも **スキル記述の規約** と **ファイル管理・運用プロセス** が中心となります。

---

## スキル記述規約

### スキルファイルの構成

各スキルは `.claude/skills/{スキル名}/SKILL.md` に定義します。

**必須セクション**:
```markdown
# {スキル名}

## 概要
[このスキルが何をするか 1〜2 文で]

## 前提条件
[実行前に必要なファイル・環境]

## 処理手順
[ステップ番号付きの処理フロー]

## 出力
[生成されるファイルと保存先]

## エラー処理
[エラー発生時の挙動]
```

### 処理手順の記述原則

**具体的な手順を番号付きで書く（曖昧な指示は禁止）**:

```markdown
# ✅ 良い例
## 処理手順
1. `input/rhel9-docs/` ディレクトリの存在を確認する
2. `.doc`/`.docx`/`.pdf`/`.pptx`/`.xlsx` ファイルを列挙する
3. 各ファイルに対して `markitdown {ファイルパス}` を実行する
4. 変換結果を `output/converted/rhel9/{元ファイル名}.md` に保存する

# ❌ 悪い例
## 処理手順
1. ファイルを変換する
2. 結果を保存する
```

**エラー処理を必ず記述する**:
```markdown
## エラー処理
- markitdown が未インストールの場合: `pip install markitdown` の実行を案内して処理を中断
- 個別ファイルの変換失敗: `[SKIP] {ファイル名}: 変換に失敗しました` を表示してスキップ（他ファイルの処理は継続）
- 出力先ディレクトリが存在しない場合: 自動作成する
```

### 出力ファイルの命名規則

スキルが生成するファイルは以下の規則に従います:

| 種別 | 命名パターン | 例 |
|------|------------|-----|
| 変換済み Markdown | `{元ファイル名}.md` | `standard-design.md` |
| 解析結果 | `{docType}.md` | `standard.md`, `basic.md` |
| 差分比較結果 | `diff-result.md` | `diff-result.md` |
| 生成ドラフト | `{docType}-rhel10-draft.md` | `standard-rhel10-draft.md` |
| レビュー記録 | `{対象ファイル名}-review.md` | `standard-rhel10-draft-review.md` |

**再実行時の上書き防止**: 既存ファイルが存在する場合は `_v2`, `_v3` のサフィックスを付与する。

---

## Git 運用ルール

### ブランチ戦略

**ブランチ種別**:
- `main`: 安定動作するスキルセット（リリース済み）
- `feature/{スキル名}`: 新スキルの開発
- `fix/{スキル名}-{修正内容}`: 既存スキルのバグ修正
- `docs/{ドキュメント名}`: ドキュメントのみの変更

**フロー**:
```
main
  ├─ feature/add-mw-support
  ├─ fix/compare-rhel-impact-score
  └─ docs/update-architecture
```

### コミットメッセージ規約

**フォーマット**:
```
<type>(<scope>): <subject>

<body>

<footer>
```

**Type**:
- `feat`: 新スキルの追加・既存スキルへの機能追加
- `fix`: スキルの動作バグ修正
- `docs`: ドキュメント（`docs/` 配下）の変更
- `refactor`: スキル記述の整理・改善（動作変更なし）
- `chore`: 設定ファイル・`.gitignore` 等の変更

**例**:
```
feat(compare-rhel): 影響度スコアの高/中/低判定基準を追加

差分比較結果の影響度が「高/中/低」で分類されるよう
判定ロジックをスキル定義に追記した。

- セキュリティ・ネットワーク関連を「高」に分類
- サービス起動・パッケージ管理を「中」に分類
- ドキュメント記載のみの変更を「低」に分類
```

### プルリクエストプロセス

**作成前のチェック**:
- [ ] サンプルデータでスキルの正常系動作を確認済み
- [ ] エラーケース（入力なし・API 失敗）の動作を確認済み
- [ ] SKILL.md の必須セクションが全て記載されている
- [ ] `docs/` の関連ドキュメントが更新されている

**PR テンプレート**:
```markdown
## 概要
[変更内容の簡潔な説明]

## 変更理由
[なぜこの変更が必要か]

## 変更内容
- [変更点1]
- [変更点2]

## 動作確認
- [ ] 正常系: [確認したコマンドと結果]
- [ ] 異常系: [確認したエラーパターン]

## 関連 Issue
Closes #[Issue 番号]
```

---

## テスト戦略

### テストの種類

#### スキル動作テスト（手動 E2E）

**対象**: 各スキルの正常系・異常系

**手順**:
1. `tests/fixtures/` のサンプル設計書を `input/rhel9-docs/` にコピー
2. 対象スキルを実行
3. `output/` 配下の生成物を目視確認
4. 期待値チェックリストに照らして検証

**実行コマンド例**:
```bash
# 正常系テスト
/convert-docs ./tests/fixtures/rhel9-sample

# 異常系テスト（空ディレクトリ）
/convert-docs ./tests/fixtures/empty-dir
```

#### 統合テスト（フルパイプライン）

フル実行フローの動作確認:
```bash
/convert-docs ./tests/fixtures/rhel9-sample
/analyze-design ./output/converted rhel9 all
/convert-docs ./tests/fixtures/rhel10-sample
/analyze-design ./output/converted rhel10 all
/compare-rhel
/generate-design standard
/review-design ./output/generated/standard-rhel10-draft.md
```

**確認ポイント**:
- 各ステップの出力ファイルが正しいパスに生成されている
- 差分比較結果の変更種別が期待値と一致している
- 生成ドラフトに変更箇所マーカー（`<!-- AI生成: 要レビュー -->`）が付与されている

### テスト用フィクスチャ

```
tests/
└── fixtures/
    ├── rhel9-sample/        # RHEL9 サンプル設計書（.docx / .pdf）
    ├── rhel10-sample/       # RHEL10 サンプル設計書
    ├── empty-dir/           # 空ディレクトリ（異常系テスト用）
    └── expected/            # 期待される出力（差分比較の検証用）
        └── diff-result.md
```

---

## コードレビュー基準

スキル（SKILL.md）のレビューでは以下を確認します:

**完全性**:
- [ ] 必須セクション（概要・前提条件・処理手順・出力・エラー処理）が全て記載されている
- [ ] 処理手順が具体的で番号付きになっている
- [ ] エラーケースが網羅されている

**正確性**:
- [ ] ファイルパスが `docs/repository-structure.md` の規則と一致している
- [ ] 前提条件に記載されたファイル・ツールが実際に利用可能か
- [ ] 出力ファイルの命名が命名規則と一致している

**保守性**:
- [ ] スキル間の依存関係が明示されている（例: 「/compare-rhel の前に /analyze-design が必要」）
- [ ] 再実行時の挙動が定義されている

### レビューコメントの書き方

**優先度を明示する**:
- `[必須]`: マージ前に修正必要
- `[推奨]`: 修正推奨（しなくても可）
- `[提案]`: 将来的に検討してほしい
- `[質問]`: 意図の確認

**例**:
```markdown
[必須] `output/converted/` のパスが repository-structure.md と異なります。
`output/converted/rhel9/` に統一してください。

[推奨] markitdown 未インストール時のエラーメッセージに、
インストールコマンド（`pip install markitdown`）を含めると親切です。
```

---

## 開発環境セットアップ

### 必要なツール

| ツール | バージョン | インストール方法 |
|--------|-----------|-----------------|
| Claude Code CLI | 最新版 | [公式サイト参照](https://claude.ai/code) |
| Python | 3.10 以上 | 公式サイト or pyenv |
| markitdown | 最新安定版 | `pip install markitdown` |
| Git | 2.x 以上 | 公式サイト |

### セットアップ手順

```bash
# 1. リポジトリのクローン
git clone {リポジトリ URL}
cd {プロジェクト名}

# 2. markitdown のインストール
pip install markitdown

# 3. 環境変数の設定
cp .env.example .env
# .env を編集して ANTHROPIC_API_KEY を設定

# 4. 入力ファイルの配置
mkdir -p input/rhel9-docs input/rhel10-docs
# 変換対象の設計書ファイルを配置

# 5. 動作確認
claude
> /convert-docs ./input/rhel9-docs
```

### 推奨開発ツール

- **VS Code + Claude Code 拡張**: スキル定義の編集・スキル実行を IDE から行う
- **Git GUI（Sourcetree / Fork 等）**: スキルの変更履歴確認に便利

---

## セキュリティガイドライン

- **API キーの管理**: `ANTHROPIC_API_KEY` は `.env` に記載し、Git にコミットしない（`.gitignore` で除外必須）
- **設計書の機密度確認**: Claude API に設計書を送信する前に、組織のセキュリティポリシーで外部 API 送信の可否を確認する
- **`input/` の Git 管理外**: 機密設計書を誤ってリポジトリに含めないよう `.gitignore` に `input/` を追加する
