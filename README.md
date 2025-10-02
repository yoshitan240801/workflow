# LLMワークフロー

このリポジトリでは、**大規模言語モデル（LLM）** を活用したLLMワークフローのコードを自学自習で作成したものをまとめています。

データ生成や資料作成など、LLMのユースケースを試したい方向けです。

---

## 🔧 使用技術

- **Python 3.10 以上**
- **Claude**（※外部LLM利用を前提とした設計）
- **Marp**（Markdownベースのスライド生成ツール）

---

## 📁 ディレクトリ構成

| ディレクトリ名 | 概要 |
|----------------|------|
| [`workflow_for_company_finance_report`](./workflow_for_company_finance_report) | 企業名を入力し、財務データを取得・分析し、自然言語でレポートを出力する自動化ワークフロー。 |
| [`workflow_for_make_marp_from_program`](./workflow_for_make_marp_from_program) | プログラムコードを解析し、Marp形式のプレゼン資料を自動生成するプロセス。技術資料作成を支援。 |

---