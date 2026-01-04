---
marp: true
theme: default
paginate: true
header: "AIの不完全さを科学的に管理する"
footer: "2026 Upgrade Inc"
backgroundColor: "#FFF"
headingDivider: 0
math: true
style: |
  /* ------------------------------------------------
     基本デザイン設定
     ------------------------------------------------ */
  header, footer, section::after {
    background: linear-gradient(109.07deg, #28B3EE -18.03%, #1884D0 21.63%, #1271C4 53.6%, #1975C5 66.14%, #77ADD7 118.92%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    font-weight: bold;
  }
  footer {
    display: flex;
    align-items: center;
    gap: 15px;
  }
  footer::before {
    content: "";
    display: block;
    width: 10px;
    height: 40px;
    background: linear-gradient(109.07deg, #28B3EE -18.03%, #1884D0 21.63%, #1271C4 53.6%, #1975C5 66.14%, #77ADD7 118.92%);
  }
  
  /* タイトル装飾 */
  h1, h2 {
    color: #0152A3;
  }
  h1 {
    font-size: 200%;
    border-bottom: 2px solid #28B3EE;
    margin-bottom: 20px;
    padding-bottom: 10px;
  }
  
  /* 箇条書き・強調 */
  ul { padding-left: 1.5rem; }
  li { margin-bottom: 8px; line-height: 1.6; }
  strong { color: #E65100; font-weight: bold; } /* オレンジ系のアクセント */
  
  /* セクション区切りスライド（青背景） */
  section.lead {
    background: linear-gradient(135deg, #0152A3, #28B3EE);
    color: #fff;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  section.lead h1 {
    color: #fff;
    border: none;
    font-size: 300%;
  }
  section.lead header, section.lead footer, section.lead section::after {
    display: none;
  }

  /* テーブル設定 */
  table { width: 100%; border-collapse: collapse; font-size: 90%; }
  th { background-color: #f0f8ff; color: #0152A3; border-bottom: 2px solid #28B3EE; }
  td { border-bottom: 1px solid #ddd; padding: 10px; }
---

# ハルシネーションの<br>分類・検出・対策

<div style="font-size: 1.5rem; margin-top: 20px;">
最新論文に基づく統合的サーベイと体系化
</div>

<br>
<div style="font-size: 1rem; opacity: 0.8;">
Based on: Liu et al., Wang et al., Huang et al., Tonmoy et al.<br>
2026年1月4日
</div>

---

# 本日のアジェンダ

ハルシネーションに関する最新の4つの主要論文を統合し、以下の3つの観点から整理します。

1. **Taxonomy (分類)**
   ハルシネーションをどう定義し、どのような種類に分けるか？
2. **Detection (検出)**
   発生したハルシネーションをどのように検知するか？
3. **Mitigation (対策)**
   モデルの学習から運用まで、どのフェーズで防ぐか？

---

# 1. Taxonomy
ハルシネーションの分類

---

# ハルシネーションの「2つの共通軸」

各論文で用語の差異はありますが、概念的には以下の2つに大別されます。

1. **事実性 (Factuality / Extrinsic)**
   * **「世界知識」** との整合性
   * 現実の事実に反している、または捏造されている状態。
   * キーワード: *Fabrication, Factuality Issue*

2. **誠実性 (Faithfulness / Intrinsic)**
   * **「入力・文脈」** との整合性
   * ユーザーの指示や参照ドキュメント（Context）を無視する状態。
   * キーワード: *Instruction Inconsistency, Unfaithfulness*

---

# A. 事実性ハルシネーション (Factuality)

生成内容が現実世界の事実と矛盾、または検証できない現象です。

* **事実の矛盾 (Contradiction)**
  * 既知の事実と異なる内容を出力する。
  * 例：人物の生没年、歴史的な日付、首都の間違いなど。
* **事実の捏造 (Fabrication)**
  * 検証不可能な事柄をでっち上げる。
  * 実在しない論文、架空の法律、根拠のない科学的主張など。
  * **Extrinsic Hallucination**（ソースから検証不能）とも呼ばれます。

> **Point:** たとえ論理的に正しくても、「嘘の事実」であればこれに該当します。

---

# B. 誠実性ハルシネーション (Faithfulness)

生成内容がユーザーの指示やコンテキストに従っていない現象です。

* **指示不整合 (Instruction Inconsistency)**
  * 例：「翻訳して」という指示に対して質問に答えてしまう。
* **コンテキスト不整合 (Context Inconsistency)**
  * RAGなどで与えられた参照記事と矛盾する回答をする。
* **論理的不整合 (Logical Inconsistency)**
  * 生成テキスト内部で、前半の主張と後半の結論が矛盾する。

> **Point:** 事実として正しくても、プロンプトの指示（例：「本文のみに基づいて」）を無視して外部知識を持ってくる場合はハルシネーション（Hallucinated but Factual）とみなされます。

---

# 2. Detection
ハルシネーションの検出方法

---

# 検出手法の2つの方向性

検出は「何を根拠にするか」で大きく2つに分かれます。

| アプローチ | 概要 | 主な手法 |
| :--- | :--- | :--- |
| **外部情報の利用** | WikipediaやGoogle検索などの**信頼できる外部ソース**と照合する。 | FActScore, FacTool, BSChecker |
| **モデル内部の利用** | モデルが出力する**確率分布（Logits）**や、自己矛盾の度合いを測定する。 | Self-consistency, Logits監視 |

---

# A. 事実性の検出 (Factuality Detection)

「嘘をついていないか」を検証する技術です。

* **FActScore / FacTool**
  * 生成文を「原子的な事実 (Atomic facts)」に分解し、それぞれを検索エンジンやナレッジベースで裏取りする**ファクトチェック**アプローチ。
* **Self-consistency (自己無撞着性)**
  * 同じプロンプトで複数回推論を行い、回答がばらつく場合は「自信がない＝ハルシネーション」の可能性が高いと判断する。
* **LLM-as-a-Judge**
  * GPT-4などの強力なモデルに「この文は事実に即しているか」を判定させる。

---

# B. 誠実性の検出 (Faithfulness Detection)

「指示や文脈に従っているか」を検証する技術です。

* **NLI (自然言語推論)**
  * ソース文（前提）に対して、生成文（仮説）が「含意 (Entailment)」しているか、「矛盾 (Contradiction)」しているかを分類器で判定。
* **QA-based Metrics (QuestEval等)**
  * 生成された要約から逆に質問(Q)を作成し、元のドキュメントでその質問に答えられるかを確認する（情報が維持されているかのテスト）。
* **Overlap Metrics**
  * ROUGEやBERTScoreなど、単語やベクトルの類似度を見る古典的手法。

---

# 3. Mitigation
ハルシネーションの対策

---

# A. 学習・データ段階 (Training Phase)

モデルを作る段階でハルシネーションを減らすアプローチです。

1. **高品質なデータセット**
   * 重複排除 (Deduplication) や、事実性の高い教科書的データを重視。
2. **SFT & 知識注入**
   * ドメイン知識を学習させつつ、**R-Tuning (拒絶学習)** により「分からないことは分からない」と答える能力を付与する。
3. **RLHF (強化学習)**
   * 事実性・誠実性を報酬モデルに組み込む。
   * ※注意: ユーザーの意見に過剰に同調する**Sycophancy（追従）**を防ぐ工夫が必要。

---

# B. 推論・デコーディング段階 (Inference Phase)

モデルの重みは変えず、出力の生成過程を工夫するアプローチです。

* **DoLa (Decoding by Contrasting Layers)**
  * モデルの層ごとの情報の差異を利用し、事実知識が豊富な層を強調してデコードする。
* **推論時介入 (ITI)**
  * 推論中のモデル内部のActivationを調整し、真実性の高い方向へベクトルを誘導する。
* **CAD (Context-Aware Decoding)**
  * コンテキストがある場合とない場合の出力確率差を増幅し、コンテキスト無視を防ぐ。

---

# C. RAGと自己修正 (System Architecture)

現在最も実用的な、外部知識とループ構造を用いた対策です。

* **RAG (検索拡張生成)**
  * **生成前**: 関連情報を検索してプロンプトに追加。
  * **生成中**: 自信がない時だけ検索する (Active RAG)。
  * **生成後**: 生成した回答を事後検索で修正する (RARR)。
* **Self-Reflection (自己修正)**
  * **CoVe (Chain-of-Verification)**: モデル自身に検証用の質問を作らせ、回答を自己チェックして修正させる。
  * **CoNLI**: 推論チェーンの中で矛盾を検知し、修正する。

---

# まとめ：信頼できるAIのために

ハルシネーション対策には「銀の弾丸」は存在しません。以下のレイヤーを組み合わせる必要があります。

1. **理解する**: 事実性（嘘）と誠実性（無視）を区別する。
2. **鍛える**: 高品質データと拒絶学習でモデルの基礎体力を上げる。
3. **支える**: RAGによる外部知識で補完する。
4. **疑う**: Self-consistencyやFact-checkingで出力を監視する。

---