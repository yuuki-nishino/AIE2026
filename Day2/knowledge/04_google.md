# Google検索、Googleマップ、ベクトル検索を統合したリアルタイムAIエージェントの開発

**イベント:** AI Engineering Summit Tokyo 2026 / Day 2  
**発表者:** 佐藤一憲（グーグル合同会社）

---

## 概要

GoogleのAIエージェント開発における中心概念「Grounding（グラウンディング）」と、新しいベクトル検索サービス「Vector Search 2.0」を軸に、Google検索・Google マップ・ストリートビューを統合したリアルタイムAIエージェントの構築方法を解説したセッション。ラスベガスのコンシェルジュエージェントをデモ事例に、マルチモーダルGroundingの実力が示された。

---

## 主要なポイント

- **Grounding（グラウンディング）** — AIのモデル（偽物の世界）と現実を「接地」させること。AIを現実の情報に基づかせる最重要概念
- **Multimodal Grounding** — Google検索・Google マップ・ストリートビューを組み合わせることで、テキストだけでなく「現実の場所・画像」まで接地できる
- **Vector Search 2.0** — ベクトル検索の複雑な運用（エンベディング・インデックスチューニング・ハイブリッドサーチ等）をすべて自動化。セットアップ5分で利用可能
- **Vertex AI → Agent Platform** に名称変更。AIエージェント開発プラットフォームとして再定義
- **GEAR（Gemini Enterprise Agent Ready）** — Googleが提供するAIエージェント開発者育成プログラム

---

## 詳細内容

### Groundingとは何か

> Groundingとは、AIのモデル（偽物）と現実を接地させること。

AIモデルは学習データに基づいた「仮想の知識」を持っている。これは最新情報が反映されていなかったり、架空の情報（ハルシネーション）を含む場合がある。Groundingはこの問題を解決するため、AIの応答を「現実の情報ソース」に紐づける仕組み。

代表的なGroundingの種類：
- **Google Search Grounding** — リアルタイムのWeb検索結果をAIに提供
- **RAG (Retrieval Augmented Generation)** — 企業内ドキュメントや社内データを検索してAIに提供
- **Maps Grounding** — 現実の地図・位置情報・営業時間・ルーティング情報をAIに提供
- **Street View Grounding (Image Grounding)** — 実際の場所の写真をAIに提供

### Multimodal Grounding のデモ

**ストリートビューを使ったポストカード生成**  
ユーザーが「巨大な黄色いゴム製のアヒルがゴールデンゲートブリッジを歩いている晴れた日」と入力すると:

1. AIが「ゴールデンゲートブリッジ」という場所を特定
2. Google マップ ストリートビューからゴールデンゲートブリッジの実写画像を取得
3. その画像をGroundingとして使用し、リアルな場所にアヒルが合成されたポストカードを生成
4. 出典（Google Maps Street View）を明記

現実の場所の写真を土台にした画像生成が、「Grounded answer with citations（出典付きの根拠ある回答）」として提供される。

### Concierge Agent システムアーキテクチャ

ラスベガスのホテルコンシェルジュAIエージェントを例に、Groundingを統合したマルチソースエージェントの構成を解説。

```
ユーザー: "Best plan for Mandalay Bay stay?"
     ↓
Concierge Agent（Orchestrator）
     ├→ World Intelligence（Google Search Grounding）
     │    └ Visual Search
     │    └ 最新グローバル情報を提供
     │
     ├→ Enterprise Datasource（Vector Search / RAG Engine）
     │    └ 非公開レストランガイド、安全な社内データ検索
     │
     ├→ Spatial Logic（Maps Grounding）
     │    └ 現実の座標・営業時間・ルーティング情報
     │
     └→ Contextual Rendering（Image Grounding）
          └ ストリートビュー画像を使ったイラストルートガイド生成
          ↓
Synthesized Itinerary & UI Presentation
     ├ Agent Chat UI（対話形式）
     ├ Maps Route Info（地図ルート）
     └ Illustrative Itinerary（ビジュアル旅程）
```

4つのGroundingソースをオーケストレーターが統合し、テキスト・地図・画像を組み合わせた総合的な回答を生成する。

### なぜベクトル検索は難しいのか

ベクトル検索を実用レベルで運用するには、以下の複数の課題をすべて解決する必要があった。

1. **Embedding（エンベディング）** — テキストをベクトルに変換するモデルの選択と更新
2. **Feature Store（特徴量の保存）** — ベクトルデータを効率的に保存・管理するインフラ
3. **Index Tuning（インデックスチューニング）** — 検索精度とレイテンシーのトレードオフの最適化
4. **Hybrid Search（ハイブリッドサーチ）** — セマンティック検索とキーワード検索の組み合わせ

これらすべてを自分で運用することは「非常に大変」であり、多くの組織でベクトル検索の本番運用がボトルネックになっていた。

### Vector Search 2.0

上記の課題をすべて解決した、次世代のマネージドベクトル検索サービス。

**自動化された機能**
- **自動エンベディング** — パイプラインを構築不要。ベクトルが自動生成される
- **ストレージ統合** — データの保存・管理が統合
- **自動インデックスチューニング** — 性能の最適化を自動で実施
- **ハイブリッドサーチ** — セマンティック検索 + キーワード検索のRRF Fusionによる最良結果、SQL的なフィルタリングも対応

**実装ジャーニー（セットアップ5分）**

| ステップ | 内容 |
|---------|------|
| STEP 1: Ingest Data | コレクション作成・スキーマ定義・製品データの一括ロード。パイプライン構築不要、ベクトルは自動生成 |
| STEP 2: Run Hybrid Search | セマンティック + キーワード検索の組み合わせ。構造化データフィルタ追加。RRF FusionとSQL的フィルタリング |
| STEP 3: Scale Globally | 10億スケールのインデックス構築。10ms以下のレイテンシーを大規模に実現 |

### Vertex AI → Agent Platform への名称変更

GoogleのAI開発プラットフォームであったVertex AIが、**Agent Platform**という名称に変更された。これはAIエージェント開発・デプロイのためのプラットフォームとして再定義されたことを意味している。

**主要SDK:**
- `google-adk` — Google Agent Development Kit（エージェント開発キット）
- `google-cloud-vectorsearch` — Vector Search 2.0を操作するSDK

### GEAR（Gemini Enterprise Agent Ready）

GoogleがAIエージェント開発者の育成のために提供するプログラム。ビジネスリーダーから経験豊富な開発者まで対象。

**GEAR参加特典**
- **毎月35クレジット** — Google Skillsでの学習に使える学習クレジットを毎月無料提供
- **スキルバッジ & 認定** — 学習パスを完了してスキルバッジを取得。Google Developer Profileで公開可能
- **Get Certified Program** — Google Cloudのお客様向けの9週間無料学習プログラム。Google Cloud認定資格の取得を目指せる（日本語サポートなし、審査プロセスあり）
- **統合エコシステム** — Google Developer ProgramとGoogle Skillsが一体化。最新ツール・コミュニティ・学習リソースに一箇所からアクセス

---

## 社内への示唆・活用ポイント

- **「AI出力の根拠を現実に接地させる」という発想を持つ** — 社内AIシステムの設計において、AIの出力が何に基づいているか（Groundingソース）を明示する仕組みを持つことで、信頼性と透明性が大幅に向上する
- **Google マップ・ストリートビューのGrounding活用を検討する** — 場所・物件・ルート・施設など「現実の空間」に関係するビジネス領域では、Spatial GroundingやImage Groundingが強力な差別化になりうる
- **ベクトル検索の導入ハードルが下がった** — Vector Search 2.0の登場でエンベディング・インデックス管理・ハイブリッドサーチの自動化が実現。以前はチームに専門家が必要だったベクトル検索が、5分のセットアップで利用可能になった
- **Agent Platformへのシフトに乗る** — Vertex AIがAgent Platformとして再定義されたことは、Googleのフルスタックなエージェント開発支援が始まったことを意味する。google-adkとgoogle-cloud-vectorsearchを組み合わせたエージェント開発を試す価値がある
- **GEARプログラムは学習コスト0で始められる** — 月35クレジットの無料提供があり、スキルバッジとGoogle Cloud認定資格の取得につながる。エンジニアのAIエージェント開発スキル習得の手段として活用できる
