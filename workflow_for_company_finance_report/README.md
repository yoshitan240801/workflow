# このフォルダのプログラムについて

このフォルダのmainプログラム(main.ipynb)は、LLMアプリ的なものを作ってみようと思って作成してみたものになります。


## 概要

このmainプログラム(main.ipynb)は、ユーザプロンプトに決算半期報告書を確認したい旨を会社名も含めて入力する事で、
その会社の決算半期報告書をEDINETから取得して、その内容をLLMで要約してその結果を表示します。

以下に、mainプログラムの概要図と処理フローを示す図を掲載します。

![処理概要図1](./slide_01.png)


# EDINETデータ取得・分析プログラム
証券コード検索から半期報告書の要約までを自動化

---

# プログラムの主な機能

1. 会社名から証券コードの特定
2. EDINETからの半期報告書PDF取得
3. PDFからのテキスト抽出
4. Claude (Bedrock)による報告書の要約

---

# システム構成図

```mermaid
graph TB
    A[ユーザー入力<br>会社名] --> B[Claude<br>証券コード特定]
    B --> C[EDINET API<br>半期報告書検索]
    C --> D[PDF取得]
    D --> E[テキスト抽出]
    E --> F[Claude<br>報告書要約]
    F --> G[結果出力]
```

---

# 主要な関数の説明

- `call_claude_for_seccode()`: 会社名から証券コードを取得
- `search_target_doc_id()`: EDINETで半期報告書を検索
- `get_pdf_as_object()`: PDFファイルを取得
- `extract_text_from_pdf_object()`: PDFからテキストを抽出
- `call_claude_for_summarized_report()`: 報告書の要約を生成

---

# 使用している主なライブラリ/API

- **AWS Bedrock**: Claude AIモデルの利用
- **EDINET API**: 企業の開示書類取得
- **PyMuPDF (fitz)**: PDF処理
- **boto3**: AWS SDK
- **requests**: HTTP通信
- **dotenv**: 環境変数管理

---

# 処理フロー詳細

```mermaid
sequenceDiagram
    participant User
    participant Claude
    participant EDINET
    participant PDF

    User->>Claude: 会社名入力
    Claude->>Claude: 証券コード特定
    Claude->>EDINET: 半期報告書検索
    EDINET->>PDF: PDF取得
    PDF->>Claude: テキスト抽出
    Claude->>User: 要約結果出力
```
