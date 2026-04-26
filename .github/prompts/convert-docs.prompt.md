---
mode: agent
description: 設計書ファイル（.doc/.docx/.pdf/.pptx/.xlsx）をmarkitdownでMarkdownに一括変換する。RHEL移行作業の第一ステップとして使用。
---

# 設計書フォーマット変換

markitdown を使って旧バージョン設計書を AI 解析可能な Markdown 形式へ変換します。

## 使い方

```
#file:.github/prompts/convert-docs.prompt.md を使って [入力ディレクトリ] を変換して
```

例:
```
#file:.github/prompts/convert-docs.prompt.md を使って ./input/rhel9-docs を変換して
```

引数省略時は `./input` を入力ディレクトリとして使用します。

---

## 実行手順

### ステップ1: markitdown のインストール確認

ターミナルで以下を実行:

```bash
python3 -c "import markitdown; print('markitdown: OK')" 2>/dev/null || echo "markitdown: 未インストール"
```

インストールされていない場合はユーザーに確認してからインストールする:

```bash
pip install markitdown
```

### ステップ2: 入力ファイルの確認

指定ディレクトリ内のファイルを列挙し、種別をユーザーに提示する:

- **変換対象**: `.doc` `.docx` `.pdf` `.pptx` `.xlsx`
- **スキップ（変換不要）**: `.md` `.yml` `.yaml`

### ステップ3: 一括変換の実行

出力先: `./output/converted/`

ターミナルで以下を実行:

```bash
python3 - <<'EOF'
import sys, os
from pathlib import Path
from markitdown import MarkItDown

INPUT_DIR = sys.argv[1] if len(sys.argv) > 1 else "./input"
OUTPUT_DIR = "./output/converted"
CONVERT_EXTS = {".doc", ".docx", ".pdf", ".pptx", ".xlsx"}
SKIP_EXTS = {".md", ".yml", ".yaml"}

os.makedirs(OUTPUT_DIR, exist_ok=True)
md = MarkItDown()
results = []

for path in sorted(Path(INPUT_DIR).rglob("*")):
    if not path.is_file():
        continue
    ext = path.suffix.lower()
    out_path = Path(OUTPUT_DIR) / (path.stem + ".md")

    if ext in SKIP_EXTS:
        results.append(("SKIP", str(path), str(out_path)))
        continue
    if ext not in CONVERT_EXTS:
        continue

    try:
        result = md.convert(str(path))
        out_path.write_text(result.text_content, encoding="utf-8")
        results.append(("OK", str(path), str(out_path)))
    except Exception as e:
        results.append(("ERROR", str(path), str(e)))

for status, src, dst in results:
    icon = "✅" if status == "OK" else ("⏭️ " if status == "SKIP" else "❌")
    print(f"  {icon} {src} → {dst}")
EOF
```

### ステップ4: 結果サマリーの表示

処理完了後に以下の形式でサマリーを表示する:

```
変換完了: 5件
  ✅ standard-design.docx → output/converted/standard-design.md
  ✅ basic-design.pdf     → output/converted/basic-design.md
  ⏭️  config.md           → スキップ（変換不要）
  ❌ old-spec.xlsx        → 変換失敗（原因: <エラーメッセージ>）

次のステップ: #file:.github/prompts/analyze-design.prompt.md を実行して設計書を解析してください
```

### ステップ5: ログ記録

処理結果を `./output/logs/convert-YYYYMMDD.log` に記録する。

---

## 注意事項

- 入力ファイルは**絶対に上書きしない**。出力は必ず `./output/converted/` に保存する
- 1ファイルの変換失敗は他のファイルの処理に影響させない
- 変換後は `#file:.github/prompts/analyze-design.prompt.md` で解析を行う
