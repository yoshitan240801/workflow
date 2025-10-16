# このフォルダのプログラムについて

このフォルダのmainプログラム(main.ipynb)は、LLM(AWS Bedrockのclaude 4.5 sonnet)を使って、複数のpythonファイルの内容を踏まえて、それらの内容を纏めた形のスライドをMarpで表現、出力するものになります。<br>
(boto3バージョン 1.40.21)


# Claude AIを活用したスライド自動生成システム

---

## システムアーキテクチャ

```mermaid
graph TB
    A[Pythonファイル群] --> B[ファイル読み込み glob]
    B --> C[MCP Client Claude AI]
    C --> D[MCP Server Tool 1<br/>個別スライド生成]
    C --> E[MCP Server Tool 2<br/>統合スライド生成]
    D --> F[個別Markdown]
    F --> E
    E --> G[最終Markdownスライド]
```

---

## ツール定義構造

**MCPクライアントツール**

1. input_data_for_mcp_server_tool_to_make_markdown
   - 個別プログラムファイルのパスを受け取る
   - 各ファイルのMarkdownスライドを生成

2. input_data_for_mcp_server_tool_to_make_integrated_markdown
   - 複数のMarkdownを統合
   - 1つの統合されたスライドを作成

---

## ツール定義構造

**MCPサーバーツール**

1. mcp_server_tool_to_make_markdown
   - プログラムの概要を説明するスライドをMarkdownで作成

2. mcp_server_tool_to_make_integrated_markdown
   - 複数のMarkdownを分かりやすい形に再構成

---

## 処理フロー全体像

```mermaid
flowchart TD
    A[開始] --> B[glob.globでファイルリスト取得]
    B --> C[初期メッセージ作成]
    C --> D[MCPクライアントツール呼び出し]
    D --> E{stopReason判定}
    E -->|tool_use| F[ツール使用情報を抽出]
    E -->|end_turn| Z[終了]
    F --> G{ツール名判定}
    G -->|make_markdown| H[個別スライド生成]
    G -->|make_integrated_markdown| I[統合スライド生成]
    H --> J[結果をメッセージリストに追加]
    I --> J
    J --> D
```

---

# シーケンス図

```mermaid
sequenceDiagram
    participant Main as メイン処理
    participant Client as MCP Client<br/>(Claude AI)
    participant Server as MCP Server<br/>(Claude AI)
    participant File as ファイルシステム

    Main->>File: glob.glob()でファイルリスト取得
    Main->>Client: ファイルリストを送信
    loop 各ファイルに対して
        Client->>Main: tool_use (make_markdown)
        Main->>File: ファイル内容読み込み
        Main->>Server: コード内容を送信
        Server->>Main: Markdownスライド返却
        Main->>Client: 結果を返却
    end
    Client->>Main: tool_use (make_integrated_markdown)
    Main->>Server: 全Markdownを送信
    Server->>Main: 統合Markdownスライド返却
    Main->>Client: 結果を返却
    Client->>Main: end_turn
    Main->>Main: 最終Markdown出力
```

---

## データフロー

```mermaid
graph LR
    A[Pythonファイル群] --> B[ファイルパスリスト]
    B --> C[Claude Client]
    C --> D[ファイル1のMarkdown]
    C --> E[ファイル2のMarkdown]
    C --> F[ファイルNのMarkdown]
    D --> G[統合処理]
    E --> G
    F --> G
    G --> H[最終Markdownスライド]
    H --> I[コンソール出力]
```

---

## 主要な技術要素

| 要素 | 説明 |
|------|------|
| **AWS Bedrock** | Claude AIモデルへのアクセス |
| **Claude AI** | コード解析とMarkdown生成 |
| **MCP (Model Context Protocol)** | クライアント・サーバー型のツール連携 |
| **Tool Use** | Claude AIのツール呼び出し機能 |
| **Marp** | Markdownからスライド生成 |

---

## プログラムの特徴

1. **階層的なAI活用**
   - MCPクライアント: プロジェクト全体の管理
   - MCPサーバー: 個別タスクの実行

2. **反復的な処理**
   - while loopでClaude AIとの対話を継続
   - tool_useとend_turnで処理フローを制御

3. **柔軟な拡張性**
   - ツール定義を動的に生成
   - 新しいツールの追加が容易

---

## まとめ

このプログラムは以下を実現します:

✅ Claude AIによる内容理解と説明生成<br>
✅ 複数スライドの統合と最適化<br>
✅ Marp形式のMarkdown出力<br>

**結果:** 複数プログラムを説明する包括的なスライド資料が自動生成される
