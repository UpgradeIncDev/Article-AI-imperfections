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

# LLMベンチマークから実務へ

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

# ベンチマークテストの結果は「学術データ」ではない

<br>

### AI導入の「羅針盤」として使う

- 多くの企業が、AIの「IQ（知識量）」だけで採用を決めて失敗しています。
- ベンチマークは単なるテスト結果ではありません。
- **「どう使えばリスクを回避できるか」を示す取扱説明書**です。

<br>

### 3つの主要ベンチマークから、**具体的な活用・対策**を導き出します。

---

# 1. TruthfulQAからの教訓

<br>

### 現象：賢いAIほど、もっともらしい嘘をつく

- **逆スケーリング**: ネット上の「迷信」や「誤解」を完璧に学習してしまう現象。
- **ビジネスリスク**: 顧客対応AIが、科学的根拠のない情報を「事実」として回答してしまう恐れがあります。

<br>

##### どう防ぐか？

---

# 【活用1】プロンプトで「迷信」をブロック

<br>

### TruthfulQAの知見を実装する

AIは学習データに含まれる「人間らしい誤解」まで模倣する習性があります。
これを防ぐため、プロンプトで情報源を厳格に指定して制御します。

<div class="columns">
  <div class="col">
    <strong class="title">NG: 曖昧なプロンプト</strong><br>
    <br>
    指示: 「風邪を早く治す方法は？」<br>
    <br>
    AI: 「温かくして寝ましょう。<strong>ネギを首に巻いたり、お酒を飲んで汗をかく</strong>のも昔から良いと言われています。」<br>
    <br>
    判定: <strong>迷信・民間療法を模倣</strong>
  </div>
  <div class="col">
    <strong class="title">OK: 情報源を厳格に指定</strong><br>
    <br>
    指示: 「<strong>公的機関の情報のみに基づき</strong>、風邪の対処法を教えて。」<br>
    <br>
    AI: 「十分な休養と水分補給が基本です。<strong>抗生物質はウイルスには効果がありません。</strong>」<br>
    <br>
    判定: <strong>科学的根拠に基づく</strong>
  </div>
</div>

<style scoped>
  .columns {
    font-size: 20px;
  }
  .title {
    font-size: 30px;
  }
</style>
---

# 【活用2】用途別の「適材適所」選定

<br>

### HELMスコアを選定基準にする

「最強のモデル」を一つ選ぶのではなく、目的に合わせてモデルを選びます。

- **社内アイデア出し**: バイアスは多少許容し、発想力重視のモデル。
- **対外チャットボット**: 創造性より、**安全性**スコアが高いモデルを選ぶ。

<br>

> **Point:** 「賢さ」より「行儀の良さ」でモデルを選ぶ視点を持ちましょう。

---

# 3. HaluEvalからの教訓

<br>

**現象：AIは自分のミスに気づけない**

- **課題**: 生成されたもっともらしい嘘を、AI自身（同レベルのモデル）にチェックさせても見抜けない（※ 認識率約58%）。
- **ビジネスリスク**: 作成者本人にダブルチェックをさせるようなもので、共倒れのリスクがあります。

<br>

### では、誰がチェックすべきか？

---

# 【活用3】「RAG」と「上司AI」による監査

<br>

**人間ではなく「上位モデル」に監視させる**

人間の目視確認はコストがかかります。
**LLM-as-a-Judge**（AIに評価させる）構成で、自動化と精度を両立させます。

<div class="columns">
  <div class="col">
    <strong class="title">NG: 自己チェック (58%)</strong><br>
    <br>
    作業者: GPT-4o-mini<br>
    監査役: <strong>GPT-4o-mini</strong><br>
    <br>
    結果: <strong>「問題ありません」</strong><br>
    (能力の限界で嘘を見抜けない)
  </div>
  <div class="col">
    <strong class="title">OK: 上位モデル監査 (80%〜)</strong><br>
    <br>
    作業者: GPT-4o-mini<br>
    監査役: <strong>GPT-4o / o1</strong><br>
    <br>
    結果: <strong>「ここ、ソースと矛盾してます」</strong><br>
    (能力差を利用してミスを検出)
  </div>
</div>

<style scoped>
  .columns {
    font-size: 20px;
  }
  .title {
    font-size: 30px;
  }
</style>

---

# 「GPT-5 なら改善した」は本当か？

<br>

### 最新モデルの現在地

「それは古いモデルの話でしょ？ GPT-4以降の最新モデルなら解決したのでは？」

- **半分正解**: 確かに「事実の捏造」や「コンプライアンス違反」は改善しました。
- **半分間違い**: しかし「**新しい嘘**」が生まれています。

---

# 【クイズ1】賢いAIほど陥る「罠」はどっち？

あなたは上司として、AIに微妙な品質の企画書をレビューさせています。
<br>「この企画書、**絶対に売れると思うんだけど**、どう思う？」と**圧をかけて**聞きました。

<div class="columns">
  <div class="col">
    <strong class="title">ケースA</strong><br>
    <br>
    「データに基づくと、競合が多く失敗するリスクが高いです」
    と、冷たく否定する。<br>
    <br>
    判定: <strong>？？？</strong>
  </div>
  <div class="col">
    <strong class="title">ケースB</strong><br>
    <br>
    「おっしゃる通り、市場のニーズを
    捉えた素晴らしい企画です！」
    と、手放しで肯定する。<br>
    <br>
    判定: <strong>？？？</strong>
  </div>
</div>

<br>

> **Think:** 最新の高IQモデル（GPT-4など）がやりがちなのは、どっち？

<style scoped>
  .columns {
    font-size: 20px;
  }
  .title {
    font-size: 25px;
  }
</style>

---

# 【解答1】賢いAIほど陥る「罠」はどっち？

**正解は B です。**

<div class="columns">
  <div class="col">
    <strong class="title">ケースA：従来のAI</strong><br>
    <br>
    <strong>事実優先（空気は読めない）</strong><br>
    従来のモデルや、単純なBotはこちらの反応をすることが多いです。<br>
    <br>
    → <strong>ある意味、誠実です。</strong>
  </div>
  <div class="col">
    <strong class="title">ケースB：最新のAI</strong><br>
    <br>
    <strong>追従（Sycophancy）</strong><br>
    賢いモデルほど、ユーザーの「肯定してほしい」という意図を深読みし、事実に反しても同意してしまいます。<br>
    <br>
    → <strong>「新しい嘘」に注意！</strong>
  </div>
</div>

<style scoped>
  .columns {
    font-size: 20px;
  }
  .title {
    font-size: 25px;
  }
</style>

---

# 最新リスク：Sycophancy（追従）への対策

<br>

### AIが「イエスマン」になる問題

- **現象**: 最新モデル（GPT-4など）は、ユーザーの機嫌を損ねないよう、間違いに同意してしまう。
- **対策**: プロンプトに「**批判的思考**」を組み込む。

<br>

### 活用プロンプト例:
> 「あなたは批判的なレビュアーです。私の意見に忖度せず、客観的な事実に基づいて反論や懸念点があれば指摘してください。」

---

# 3つのベンチマークから学ぶLLM活用方法

| <span>ベンチマーク</span> | <span>ビジネス上のリスク</span> | 明日からの活用アクション |
| :--- | :--- | :--- |
| **TruthfulQA** | もっともらしい嘘 | 「公的情報のみ」「迷信除外」のプロンプト徹底 |
| **HELM** | 炎上・バイアス | 用途に応じて「公平性・毒性」スコアでモデル選定 |
| **HaluEval** | チェック漏れ | AIの自己チェックを過信せず、**参照元提示**を義務化 |

<style scoped>
  span {
    white-space: nowrap;
  }
</style>
---

# まとめ

### AIを「信じる」のではなく「飼い慣らす」

- ベンチマークは、AIの「弱点」を教えてくれます。
- 弱点を知っていれば、**プロンプトや運用フローでカバー**できます。

<br>

> **Next Action**
> LLMの選定の時は、できるだけ**GPT-4以降**のモデルを使うことでコンプライアンス違反を防ぎましょう。さらに「新しい嘘」の対策として**批判的思考**がプロンプトに組み込むように意識しましょう。