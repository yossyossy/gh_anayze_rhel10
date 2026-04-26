---
mode: agent
description: 初回セットアップ: 6つの永続ドキュメントを対話的に作成する
---

# 初回プロジェクトセットアップ

このプロンプトは、プロジェクトの6つの永続ドキュメントを対話的に作成します。

## 実行前の確認

`data/inputs/` ディレクトリ内のファイルを確認します:

```
✅ data/inputs/initial-requirements.md が見つかりました
   この内容を元にPRDを作成します

または

⚠️  data/inputs/ にファイルがありません
   対話形式でPRDを作成します
```

## 手順

### ステップ0: インプットの読み込み

1. `data/inputs/` 内のマークダウンファイルを全て読む
2. 内容を理解し、PRD作成の参考にする

### ステップ1: プロダクト要求定義書の作成

1. `data/inputs/` の内容を元に `docs/product-requirements.md` を作成
2. 壁打ちで出たアイデアを具体化:
   - 詳細なユーザーストーリー
   - 受け入れ条件
   - 非機能要件
   - 成功指標
3. ユーザーに確認を求め、**承認されるまで待機**

**以降のステップはプロダクト要求定義書の内容を元にするため、自動的に作成する**

### ステップ2: 機能設計書の作成

1. `docs/product-requirements.md` を読む
2. `docs/functional-design.md` を作成（以下の構成に従う）:
   - システム概要
   - 機能一覧（ユーザーストーリーベース）
   - 画面・API 仕様
   - データモデル
   - 外部連携

### ステップ3: アーキテクチャ設計書の作成

1. 既存のドキュメントを読む
2. `docs/architecture.md` を作成（以下の構成に従う）:
   - アーキテクチャ概要
   - 技術スタック
   - コンポーネント構成
   - データフロー
   - 非機能要件（性能・セキュリティ・可用性）

### ステップ4: リポジトリ構造定義書の作成

1. 既存のドキュメントを読む
2. `docs/repository-structure.md` を作成（以下の構成に従う）:
   - ディレクトリ構造
   - 各ディレクトリの役割
   - ファイル命名規則

### ステップ5: 開発ガイドラインの作成

1. 既存のドキュメントを読む
2. `docs/development-guidelines.md` を作成（以下の構成に従う）:
   - 開発フロー
   - コーディング規約
   - テスト方針
   - レビュー基準

### ステップ6: 用語集の作成

1. 既存のドキュメントを読む
2. `docs/glossary.md` を作成:
   - プロジェクト固有の用語定義
   - ドメイン用語
   - 略語

## 完了条件

6つの永続ドキュメントが全て作成されていること

完了時のメッセージ:
```
初回セットアップが完了しました!

作成したドキュメント:
✅ docs/product-requirements.md
✅ docs/functional-design.md
✅ docs/architecture.md
✅ docs/repository-structure.md
✅ docs/development-guidelines.md
✅ docs/glossary.md

これで開発を開始する準備が整いました。

今後の使い方:
- ドキュメントの編集: 普通に会話で依頼してください
  例: 「PRDに新機能を追加して」「architecture.mdを見直して」

- 機能の追加: #file:.github/prompts/add-feature.prompt.md を使ってユーザー認証を実装して

- ドキュメントレビュー: #file:.github/prompts/review-design.prompt.md を使って docs/product-requirements.md をレビューして
```
