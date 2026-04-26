# プロジェクト用語集 (Glossary)

## 概要

このドキュメントは、RHELMigrationAssistant プロジェクト内で使用される用語の定義を管理します。

**更新日**: 2026-04-26

---

## ドメイン用語

### 設計書

**定義**: システムの仕様・構成・動作を記述したドキュメント群の総称

**説明**: 本プロジェクトでは標準設計書・基本設計書・詳細設計書・単体テスト項目書の 4 種別を対象とする

**関連用語**: 標準設計書、基本設計書、詳細設計書、単体テスト項目書

**英語表記**: Design Document

---

### 標準設計書

**定義**: OS・ミドルウェアの標準的な設定値・ポリシーを定義したドキュメント

**関連用語**: 基本設計書、詳細設計書

**英語表記**: Standard Design Document

---

### 基本設計書

**定義**: システムの全体構成・コンポーネント間の関係・主要な機能仕様を定義したドキュメント

**関連用語**: 標準設計書、詳細設計書

**英語表記**: Basic Design Document

---

### 詳細設計書

**定義**: 実装レベルの詳細な設定・手順・パラメータを定義したドキュメント

**関連用語**: 基本設計書、単体テスト項目書

**英語表記**: Detail Design Document

---

### 単体テスト項目書

**定義**: 各コンポーネント・機能の単体テスト手順・期待値を定義したドキュメント

**英語表記**: Unit Test Document

---

### 変換ジョブ (ConversionJob)

**定義**: 1 つのファイルを対象とした変換処理の実行単位

**説明**: 入力ファイルパス・出力パス・ステータス・エラー情報などを保持する。ステータスは `pending / in_progress / completed / failed / skipped` の 5 種別

**関連用語**: FileFormat, JobStatus

**英語表記**: ConversionJob

---

### 解析結果 (DesignDocument)

**定義**: 変換済み Markdown ファイルを AI が解析した結果を構造化したデータ

**説明**: 設計書の種別・バージョン・セクション一覧・設定項目を保持する

**関連用語**: DocumentSection, SettingItem

**英語表記**: DesignDocument

---

### 比較結果 (ComparisonResult)

**定義**: 旧バージョンと新バージョンの DesignDocument を比較した結果

**説明**: 各設定項目の変更種別（unchanged / modified / added / removed）と影響度（high / medium / low）を含む

**関連用語**: ComparisonItem, ChangeType, ImpactLevel

**英語表記**: ComparisonResult

---

### 変更種別 (ChangeType)

**定義**: 新旧バージョン比較における設定項目の変更の分類

**説明**:
- `unchanged`: 変更なし
- `modified`: 値が変更された
- `added`: 新バージョンで追加された
- `removed`: 新バージョンで削除された

**英語表記**: ChangeType

---

### 影響度 (ImpactLevel)

**定義**: 変更点がシステムに与える影響の大きさの分類

**説明**:
- `high`: 設計書への反映が必須
- `medium`: 確認・判断が必要
- `low`: 影響が軽微

**英語表記**: ImpactLevel

---

### レビューポイント

**定義**: AI が生成した成果物に対して、人間がレビューすべきポイントの一覧

**説明**: 確認すべき理由と判断基準を含む Markdown 形式で出力される

**英語表記**: Review Points

---

## 技術用語

### markitdown

**定義**: Microsoft が開発・公開している、各種ファイル形式を Markdown に変換する Python ライブラリ

**本プロジェクトでの用途**: .doc/.docx/.pdf/.pptx/.xlsx ファイルを Markdown に一括変換する `/convert-docs` スキルの中核ツール

**バージョン**: 最新安定版

**関連用語**: /convert-docs, Python

---

### Claude API (Anthropic)

**定義**: Anthropic が提供する大規模言語モデル（LLM）の API。日本語の長文処理に優れる

**本プロジェクトでの用途**: 設計書の解析・差分比較・影響度判定・設計書ドラフト生成

**使用モデル**: claude-sonnet-4-6 以降

**関連用語**: AIClient, プロンプト, LLM

---

### Claude Code スキル

**定義**: Claude Code CLI の `/スキル名` 形式で呼び出せる、Markdown で記述された処理定義ファイル（SKILL.md）

**説明**: `.claude/skills/{スキル名}/SKILL.md` に配置する。コードを書かずに AI の処理フローを定義できる

**本プロジェクトでの用途**: /convert-docs・/analyze-design・/compare-rhel・/generate-design・/review-design の 5 スキルとして実装

**英語表記**: Claude Code Skill

---

### プロンプト

**定義**: AI（Claude API）に送信する指示テキスト

**説明**: 各スキルの SKILL.md 内に処理手順として記述される。ファイルの内容と処理指示をまとめて Claude API へ送信する

**英語表記**: Prompt

---

### Python

**定義**: 汎用プログラミング言語。markitdown の実行環境として使用

**本プロジェクトでの用途**: markitdown のインストール・実行環境

**バージョン**: 3.10 以上

---

## 略語・頭字語

### RHEL

**正式名称**: Red Hat Enterprise Linux

**意味**: Red Hat 社が開発・提供するエンタープライズ向け Linux ディストリビューション

**本プロジェクトでの使用**: 移行対象の OS。RHEL9（旧バージョン）→ RHEL10（新バージョン）の移行を主な対象とする

---

### PRD

**正式名称**: Product Requirements Document

**意味**: プロダクト要求定義書。プロダクトの目的・ターゲットユーザー・機能要件・非機能要件を定義するドキュメント

**本プロジェクトでの使用**: `docs/product-requirements.md`

---

### MVP

**正式名称**: Minimum Viable Product

**意味**: 最小限の機能で成立するプロダクト。検証に必要な最低限の機能セット

**本プロジェクトでの使用**: P1 優先度の機能セット（変換・解析・比較・レビュー支援）を MVP と定義

---

### LLM

**正式名称**: Large Language Model

**意味**: 大規模言語モデル。大量のテキストデータで学習した AI モデル

**本プロジェクトでの使用**: 設計書解析・差分比較・レビューポイント生成に使用する AI の総称

---

## アーキテクチャ用語

### パイプライン型スキルアーキテクチャ

**定義**: 独立したスキルが順番に実行され、ファイルシステムを介してデータを受け渡すアーキテクチャパターン

**本プロジェクトでの適用**: /convert-docs → /analyze-design → /compare-rhel → /generate-design → /review-design の順に実行

**図解**:
```
ユーザーインターフェース層 (Claude Code CLI)
  ↓
スキル実行層 (.claude/skills/*/SKILL.md)
  ↓
外部ツール層 (markitdown / Claude API)
  ↓
データ永続化層 (input/ / output/)
```

---

### ConverterService

**定義**: /convert-docs スキルが担うファイル変換処理の論理コンポーネント

**説明**: 指定ディレクトリを走査し、markitdown で対象ファイルを Markdown に変換。変換結果を `output/converted/` に保存する

**関連コンポーネント**: markitdown, FileStore

---

### AnalyzerService

**定義**: /analyze-design スキルが担う設計書解析処理の論理コンポーネント

**説明**: 変換済み Markdown を Claude API に送信し、設定項目・値・説明を構造化して抽出。解析結果を `output/analyzed/` に保存する

**関連コンポーネント**: Claude API, FileStore

---

### ComparatorService

**定義**: /compare-rhel スキルが担う差分比較処理の論理コンポーネント

**説明**: RHEL9 と RHEL10 の解析結果を突合し、変更種別・影響度を Claude API で判定。差分一覧を `output/compared/` に保存する

**関連コンポーネント**: Claude API, FileStore

---

### GeneratorService

**定義**: /generate-design スキルが担う設計書生成処理の論理コンポーネント

**説明**: 差分比較結果と旧設計書をもとに、Claude API が RHEL10 対応設計書ドラフトを生成。変更箇所にマーカーを付与して `output/generated/` に保存する

**関連コンポーネント**: Claude API, FileStore

---

### ReviewerService

**定義**: /review-design スキルが担うレビュー記録処理の論理コンポーネント

**説明**: AI 生成物にレビューコメント・承認ステータスを付与し、`output/reviewed/` に保存する

**関連コンポーネント**: FileStore

---

## RHEL ライフサイクル用語

### Full Support Phase（フルサポートフェーズ）

**定義**: RHELメジャーバージョンのライフサイクル最初のフェーズ（約5年間）

**説明**: 新機能追加・新ハードウェア対応・マイナーリリース（9.4, 9.6 など）・全種類の修正が提供される、最も手厚い期間。マイナーリリースは半年ごとに出る

---

### Maintenance Support Phase（メンテナンスサポートフェーズ）

**定義**: RHELライフサイクルの第2フェーズ（約5年間）

**説明**: 新機能追加とマイナーリリースは打ち切り。セキュリティ修正と重要なバグ修正のみ提供される。「機能は増えないが、セキュリティは守られる」段階

---

### Extended Life Phase（延長ライフフェーズ）

**定義**: RHELライフサイクルの最終フェーズ（約2年間）

**説明**: バグ修正もセキュリティ修正もなし。既存インストール向けの限定的なサポートのみ。実質「稼働環境を移行するための猶予期間」

---

### Errata Advisory（エラータアドバイザリ）

**定義**: RHELのアップデートとして配信される修正情報の総称

**説明**: `dnf update` で適用されるパッケージはすべていずれかのエラータに紐づく。3種類ある

| 略語 | 正式名称 | 内容 |
|------|---------|------|
| RHSA | Red Hat Security Advisory | セキュリティ修正（CVE対応） |
| RHBA | Red Hat Bug Fix Advisory | バグ修正 |
| RHEA | Red Hat Enhancement Advisory | 機能拡張 |

---

### EUS（Extended Update Support）

**定義**: 特定のマイナーリリースに24か月間留まれる有償アドオンサブスクリプション

**説明**: 通常は次のマイナーリリースが出ると旧マイナーの修正が止まるが、EUSを契約すると対象マイナー専用の修正ストリームが独立して継続される。偶数マイナー（9.2, 9.4, 9.6）のみ対象

**英語表記**: Extended Update Support

---

### Enhanced EUS（Enhanced Extended Update Support）

**定義**: 特定のマイナーリリースに48か月間（4年）留まれる有償アドオンサブスクリプション。RHEL 9・10 で新登場

**説明**: EUSの強化版。EUSより長く同じマイナーで運用したい大規模環境向け

**英語表記**: Enhanced Extended Update Support

---

### E4S（Update Services for SAP Solutions）

**定義**: SAP環境専用の48か月延長サポートアドオンサブスクリプション

**説明**: SAPワークロードを動かしている環境向けの選択肢

**英語表記**: Update Services for SAP Solutions

---

### ELS（Extended Life-cycle Support）

**定義**: 10年のライフサイクル終了後（Extended Life Phase中）にセキュリティ修正のみを延命する有償アドオン

**説明**: RHEL 6・7 向けに提供実績あり。RHEL 9 はまだ対象外

**英語表記**: Extended Life-cycle Support

---

### CVSS（Common Vulnerability Scoring System）

**定義**: 脆弱性の深刻度を0〜10のスコアで表す業界標準の評価指標

**説明**: Red Hat は脆弱性に Critical / Important / Moderate / Low の4段階で重要度を付与する。2025年4月以降、「Moderate以上かつCVSSスコア7以上」が修正提供の基準となった。Low やCVSS 7未満のModerate は原則として修正されない

---

### AAP（Ansible Automation Platform）

**定義**: Red Hat が提供する IT 自動化プラットフォーム

**説明**: 本プロジェクトの運用対象環境では RHEL 9 上で AAP 2.5 が稼働している。実行ノードのRHELマイナーバージョン固定方針（EUS契約有無）と、AAPのサポート対象RHELバージョンの組み合わせが運用上の重要な判断ポイントになる

**英語表記**: Ansible Automation Platform
