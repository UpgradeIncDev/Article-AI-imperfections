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
     1. ヘッダー・フッター・ページ番号
     ------------------------------------------------ */
  header, footer, section::after {
    background: linear-gradient(109.07deg, #28B3EE -18.03%, #1884D0 21.63%, #1271C4 53.6%, #1975C5 66.14%, #77ADD7 118.92%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
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

  /* ------------------------------------------------
     2. h1 タイトル
     ------------------------------------------------ */
  h1 {
    background: linear-gradient(91.67deg, #00A5EC -10.12%, #0152A3 108.32%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    display: inline-block;
  }

  /* ------------------------------------------------
     3. 追加スタイル
     ------------------------------------------------ */
  h2 {
    color: #0152A3;
    border-bottom: 2px solid #e0e0e0;
    padding-bottom: 0.2em;
  }
  strong {
    color: #E65100;
  }
  
  /* メッセージスライド用 */
  section.message {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
  }
  section.message h1 {
    font-size: 2.5em;
    background: none;
    color: #0152A3;
    -webkit-background-clip: border-box;
    background-clip: border-box;
    margin-bottom: 0.5em;
  }
  section.message p {
    font-size: 1.5em;
    color: #555;
  }
  
  /* 2カラムレイアウト用 */
  .columns {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
  }
  .col {
    background: #f9f9f9;
    padding: 15px;
    border-radius: 8px;
  }

---

# ハルシネーションの分類から改善へ

<span>『AIの不完全さを科学的に管理する』</span>

<br><br><br><br><br><br>
<p>日付: 2026年1月4日</p>

<style scoped>
h1 {
  position: absolute;
  width: 100%;
  left: 0;
  top: 300px; 
  font-size: 70px;
  white-space: nowrap;
  text-align: center;
  margin: 0;
}
p {
  font-size: 30px;
  line-height: 2;
  display: flex;
  justify-content: end;
}
span {
  background: linear-gradient(91.67deg, #00A5EC -10.12%, #0152A3 108.32%);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  display: inline-block;
  position: absolute;
  top: 220px;
  left: 0;
  width: 100%;
  text-align: center;
  font-weight: bold;
}
</style>

---

# AIはなぜ、息をするように嘘をつくのか？

LLMは「真実」を知っているわけではありません。
確率的に「もっともらしい次の単語」を繋げているに過ぎないからです。

<br>

> **"Hallucination is not a bug, but a feature."**
> (ハルシネーションはバグではなく、生成AIの特性そのものである)

この前提に立ったとき、私たちはどう向き合うべきでしょうか？

---

# その「嘘」は、<br>誰に対する裏切りですか？

世界への裏切りか、ユーザーへの裏切りか。

---

# ハルシネーションの「2つの正体」

最新の4つの論文（Huang, Liu, Wang, Tonmoy et al.）は、
ハルシネーションを **何と矛盾しているか** で明確に区別しています。

1. **事実性**
   * **VS 世界知識**: 現実世界の事実と矛盾している。
   * 「もっともらしい嘘」「知識の捏造」

2. **誠実性**
   * **VS 文脈・指示**: 与えられた情報や指示と矛盾している。
   * 「指示の無視」「文脈の無視」

---

# 1. 事実性の欠如（事実を捏造してしまう現象）

ユーザーがもっとも警戒する「嘘」です。
これはモデルが「知らない」と言えないことに起因します。

* **Fabrication (捏造)**
  * 存在しない論文、架空の法律、起きていない歴史的イベント。
* **Entity Error**
  * 日付、人物名、関係性の取り違え。

> **Point:** 外部ソース（Wikipedia等）と照らし合わせなければ判定できません。

---

# 2. 誠実性の欠如（プロンプトの指示を守れない現象）

事実は合っていても、ユーザーの意図に沿わなければハルシネーションです。

* **Instruction Inconsistency**
  * 「翻訳して」と言ったのに、質問に回答してしまう。
* **Context Inconsistency**
  * 「以下の文章に基づいて」と言ったのに、文章にない外部知識を混ぜる。
  * **RAG（検索拡張生成）において致命的な問題**となります。

---

# では、その嘘を<br>「自動で」見抜けますか？

---

# 「外」を見るか、「内」を見るか

人間が1つ1つ確認するのは限界があります。
自動検出のアプローチは、大きく分けて2つの方向性があります。

| アプローチ | 直感的なイメージ | 何をするのか |
| :--- | :--- | :--- |
| <span>**1. 裏取りする**</span><br><span>(Fact Checking)</span> | **「カンニング」**<br>教科書を見て答え合わせをする | 外部ツール（検索エンジンやWiki）を使って、事実確認を行う。<br>（FActScore / FacTool） |
| <span>**2. 迷いを検知する**</span><br><span>(Uncertainty)</span> | **「取り調べ」**<br>証言のブレから嘘を見抜く | モデルに何度も同じ質問をする。<br>回答がブレたら「嘘」とみなす。<br>（Self-Consistency） |

<style scoped>
span {
  white-space: nowrap;
}
</style>
---

# 詳細A：「裏取り」の仕組み

AIの生成した長い文章を、検証可能な最小単位（原子的事実）に分解し、それぞれをGoogle検索やWikipediaで判定します。

<div class="columns">
  <div class="col">
    <strong>STEP 1: 生成文の分解</strong><br><br>
    「A氏は2026年にB社を買収した」<br>
    ↓<br>
    ① [A氏] は [B社] を買収した<br>
    ② それは [2026年] である
  </div>
  <div class="col">
    <strong>STEP 2: 外部検索で検証</strong><br><br>
    ① Wikipedia: "A氏はB社を買収" → <strong>True</strong><br>
    ② ニュース検索: "2025年完了" → <strong>False</strong><br>
    <br>
    判定: <strong>Hallucination (事実誤認)</strong>
  </div>
</div>

<br>
<small>※ FActScoreなどの手法で採用されているアプローチです。</small>

---

# 詳細B：「迷い」の検知 (Self-Consistency)

モデルは「知識がある事柄」には常に同じ回答をしますが、「知らない事柄」には確率的に適当な答え（嘘）を出します。この性質を利用します。

<div class="columns">
  <div class="col">
    <strong>質問: 「X社のCEOは？」</strong><br>
    <br>
    回答1: A氏です。<br>
    回答2: A氏です。<br>
    回答3: A氏です。<br>
    <br>
    判定: <strong>一貫性あり (信頼できる)</strong>
  </div>
  <div class="col">
    <strong>質問: 「Y社のCEOは？」</strong><br>
    <br>
回答1: <strong>B氏</strong>です。<br>
    回答2: <strong>C氏</strong>かもしれません。<br>
    回答3: <strong>D氏</strong>です。<br>
    <br>
    判定: <strong>一貫性なし (ハルシネーション)</strong>
  </div>
</div>

---

# どうすれば、<br>信頼できるAIになりますか？

100%改善することはありませんが、軽減するための「戦術」はあります。

---

# まとめ

ハルシネーションを単なる「ミス」として片付けず、分類し、対処することが重要です。

1. **分類**: それは「知識の捏造」か？「指示の無視」か？
2. **検出**: 外部での裏取り、または内部の「迷い」を監視できているか？
3. **対策**: RAGに加え、自己修正（Self-Correction）のループはあるか？

<br>

> **Next Action:**
> 自社のAIアプリケーションで起きているエラーが、FactualityなのかFaithfulnessなのか、まずは分類することから始めませんか？