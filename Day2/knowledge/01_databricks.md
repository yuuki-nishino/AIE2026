# Building GenAI Systems that Scale

**イベント:** AI Engineering Summit Tokyo 2026 / Day 2  
**発表者:** Edwards Chase（Databricks）

---

## 概要

GenAIシステムを企業本番環境にスケールさせるための課題と解法を体系的に整理したセッション。「デモは週末で作れる。本番は1年かかる」という言葉に象徴されるように、PoC→本番の間に存在するギャップをどう乗り越えるかがテーマ。ガバナンスを「ブレーキ」ではなく「アクセレレーター」として位置づけたことが印象的だった。

---

## 主要なポイント

- マルチエージェントの成長率は**4ヶ月で327%**、74%の企業が2年以内にAIエージェントをデプロイ予定（Databricks 2026 State of AI Agents report / Deloitte）
- **「デモは週末で作れる。本番は1年かかる。」** — このギャップに多くの価値が失われている
- AIガバナンスを整備すると**プロジェクトの本番到達率が12倍**、投資対効果は9ヶ月で7倍になる
- **評価（Eval）チームを持つ企業は6倍本番到達しやすい**
- **社内向けAIが最もレバレッジが高い** — コスト・レイテンシーともに優位

---

## 詳細内容

### エンタープライズAIが直面する壁：「The Gap」

> A demo takes a weekend. Production can take a year.  
> The gap between them is where most of the value is lost.

PoC/デモは1人のエンジニアが週末で作れる。しかし本番に持っていくには、ガバナンス・評価・コスト・信頼・スケール・人材という複数の課題を乗り越える必要があり、ここで多くの企業が止まっている。

### スケーリングの6つの障壁

Databricks 2026 State of AI Agents reportによると、AIエージェントのスケール化を阻む主な障壁は以下の通り。

1. セキュリティ・プライバシー・コンプライアンス（24%）
2. 急速な技術変化についていくこと（18%）
3. 精度・ハルシネーション（17%）
4. AIタレント・オペレーティングモデル（16%）
5. コンテキストの品質
6. コストの最適化

また、現在本番で動いているAIエージェントの56%は**read-only（読み取り専用）以下**の操作範囲に留まっており、本格的な自律型エージェントへの移行はこれからという状況。

### 6つの障壁 × 3つの越え方（Three Crossings）

障壁を乗り越えるためのアプローチを3つに整理。

**Crossing 1: Data & Governance（データとガバナンス）**
- 課題: セキュリティ・コンプライアンス・既存システムとの統合
- 解決策: データ・モデル・エージェント・アクセスを横断した**統一ガバナンスレイヤー**を持つ
- → One governed copy of your data and AI assets

**Crossing 2: Context Quality & Trust（コンテキスト品質と信頼）**
- 課題: 出力品質・ハルシネーション・推論コスト
- 解決策: **早期に計測し、高品質なエンタープライズコンテキストを提供する**
- → Measure early, provide high-quality Enterprise context

**Crossing 3: People & Operating Model（人材とオペレーティングモデル）**
- 課題: AIに懐疑的なユーザー・急速な技術変化への対応
- 解決策: **信頼・ツール・スキルの構築**、ユーザーを中心に設計する
- → Build trust, tooling, skills

### ガバナンスはアクセレレーター

> Governance shouldn't slow you down, it accelerates.

AIガバナンスを適切に整備（データ・モデル・エージェント・アクセスの統一ガバナンスレイヤー）することで:
- **プロジェクトの本番到達率が12倍**になる（Databricks 2026 State of AI Agents report）
- **ガバナンス投資のROIが9ヶ月で7倍**になる

「AIが来るのではなく、AIはデータのある場所に来る。だから governed perimeter は絶対に破れない設計にする必要がある。」

### Agentic System Reference Architecture

Databricksが提案するエンタープライズ向けAIエージェントの参照アーキテクチャは以下4要素で構成される。

**Agentic Core（中核）**
- Planner / Orchestrator: タスクを計画・割り振り
- Reasoning (LLM): 推論
- Tool Invocation: ツール呼び出し
- Memory & State: 状態管理

**Human in the Loop（人間の関与）**
- 意図確認・タスクのフレーミング
- 承認ゲート
- レビューとフィードバック
- エスカレーション・オーバーライド

**MCP Servers / API Layer（外部接続）**
- Model Context Protocol経由でのツール接続
- 検索・SaaS・内部ツール・APIへのアクセス

**Governed Enterprise Context（エンタープライズコンテキスト）**
- アクセス制御とポリシー
- リネージと監査
- セマンティックレイヤー / メトリクス
- キュレートされたナレッジとRAGソース

**Continuous Quality Improvement（品質改善ループ）**
- Golden Dataset（期待出力・ラベルケース・バージョン管理された正解）
- Evaluation Loop（精度・安全性・ポリシー準拠・コスト・レイテンシーで自動採点）
- Quality Improvement Loop（回帰のトリアージ・プロンプト改善・ポリシー更新・再実行）

### 評価（Eval）の重要性

> Eval構築チームは6倍本番到達しやすい

評価フレームワークを持つ企業は、そうでない企業に比べてPoCを本番環境に届ける確率が6倍高い。スケール化する前に評価機構を置くことが鍵。

### AI導入は行動変容の問題

> AI adoption is a behaviour-change problem

- **Users are AI skeptics**: ツールを展開しても、信頼を勝ち取れていないとユーザーは静かに古い方法を続ける
- **Design with users in mind**: AIツールはプロダクトとして設計する。CUJ（Critical User Journey）を定義し、UXを設計し、展開計画を立てる
- **Align user expectations**: 品質向上をデータで見せる。信頼度スコアを表示する。期待値を正確に伝える

### Databricks内部の取り組み：KARLとFDE

**KARL（カスタムRLエンタープライズ知識エージェント）**  
フロンティアモデルとの比較において、より低コスト・低レイテンシー・高品質を実現したDatabricksの自社エージェント。KARLBenchでClaude Sonnet/Opusに比肩する性能をより低コストで達成。

**Field problems flow to Research. Research flows back as product.**  
現場の課題がリサーチに流れ、リサーチが製品になって返ってくるというサイクルを構築。

### Databricksのデータプラットフォーム製品群

エンタープライズAIのベースとなるデータ基盤として以下の製品群を提供。

- **Lakeflow**: データ取り込み・ETL・ストリーミング
- **Lakehouse**: データウェアハウジング
- **Lakebase**: サーバレスPostgres
- **Unity Catalog**: 統合ガバナンス
- **Open Formats**: Delta Lake / Iceberg / Postgres

### AIエージェント機能

- **Genie**: AI with Enterprise Context
- **Genie Agents**: ビジネスユーザー・アナリスト向けエージェント
- **Genie Code / Coding Agents**: 技術ユーザー・データサイエンティスト・データエンジニア向け
- **AI Gateway**: 全ロールへのアクセスを統制

---

## 社内への示唆・活用ポイント

- **PoCが成功したら「本番化コスト」を最初から見積もる** — デモは週末で作れても本番は1年かかる。PoCと並行して、ガバナンス・評価・スケール設計を開始する
- **ガバナンスを後回しにしない** — ガバナンス整備で本番到達率が12倍になるというデータは重要。セキュリティ・権限・監査ログの設計をAI導入の初期から組み込む
- **評価フレームワークを最初に作る** — Evalチームを持つ企業は6倍本番到達しやすい。「このAIが良い出力をしているか」を測る指標と仕組みをスケール化の前に整備する
- **AI導入は行動変容の問題として扱う** — ツールを展開するだけでは使われない。ユーザーの信頼を獲得するためのUX設計・期待値管理・教育が必要
- **社内AIは外部サービス以上のレバレッジ** — コスト・レイテンシー・品質・セキュリティのすべてで、自社データを使った社内AIの方が優位になりやすい。Databricksのような統合プラットフォームの採用を検討する価値がある

**参考データ:** Databricks 2026 State of AI Agents report
