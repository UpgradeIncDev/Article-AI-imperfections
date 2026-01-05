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

最新論文では、ハルシネーションを **何と矛盾しているか** で明確に区別しています。

1. **事実性**
   - **VS 世界知識**: 現実世界の事実と矛盾している。
   - 「もっともらしい嘘」「知識の捏造」

2. **誠実性**
   - **VS 文脈・指示**: 与えられた情報や指示と矛盾している。
   - 「指示の無視」「文脈の無視」

---

# 1. 事実性の欠如（事実を捏造してしまう現象）

ユーザーがもっとも警戒する「嘘」です。
これはモデルが「知らない」と言えないことに起因します。

- **Fabrication (捏造)**
  - 存在しない論文、架空の法律、起きていない歴史的イベント。
- **Entity Error**
  - 日付、人物名、関係性の取り違え。

> **Point:** 外部ソース（Wikipedia等）と照らし合わせなければ判定できません。

---

# 2. 誠実性の欠如（プロンプトの指示を守れない現象）

事実は合っていても、ユーザーの意図に沿わなければハルシネーションです。

- **指示不整合**
  - 「翻訳して」と言ったのに、質問に回答してしまう。
- **コンテキスト不整合**
  - 「以下の文章に基づいて」と言ったのに、文章にない外部知識を混ぜる。

---

# 【クイズ1】あなたの目の前のAIは、どのタイプ？

ビジネス現場でよくある2つのエラー。
どちらが「事実性の欠如」で、どちらが「誠実性の欠如」でしょうか？

<div class="columns">
  <div class="col">
    <strong class="title">ケースA</strong><br>
    <br>
    「この議事録を要約して」と頼んだら、<br>
    勝手にネット上のニュース情報を混ぜて<br>
    解説し始めた。<br>
    <br>
    判定: <strong>？？？</strong>
  </div>
  <div class="col">
    <strong class="title">ケースB</strong><br>
    <br>
    「競合X社の来期の売上予測は？」<br>
    と聞いたら、決算発表前なのに<br>
    もっともらしい数字を断言した。<br>
    <br>
    判定: <strong>？？？</strong>
  </div>
</div>

<br>

> **Think:** 「指示に違反している」か、「現実と矛盾している」か。

<style scoped>
  .columns {
    font-size: 20px;
  }
  .title {
    font-size: 25px;
  }
</style>

---

# 【解答1】あなたの目の前のAIは、どのタイプ？

<div class="columns">
  <div class="col">
    <strong class="title">ケースA：誠実性の欠如</strong><br>
    <br>
    <strong>「文脈無視」のハルシネーション</strong><br>
    事実は正しいかもしれませんが、ユーザーの「要約して（外部情報は入れないで）」という指示を裏切っています。<br>
    <br>
    → <strong>RAGで最も警戒すべきエラー</strong>
  </div>
  <div class="col">
    <strong class="title">ケースB：事実性の欠如</strong><br>
    <br>
    <strong>「捏造」のハルシネーション</strong><br>
    AIが知らない情報を、確率的に穴埋めしてしまった典型的な「知ったかぶり」です。<br>
    <br>
    <br>
    → <strong>外部検索での裏取りが必要</strong>
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

# では、その嘘を<br>「自動で」見抜けますか？

---

# 検出のアプローチ：何を「正解」とするか？

いくつかの工夫により、自動でハルシネーションを抑制できます。

| アプローチ | 有効なケース | 具体的な手法 |
| :--- | :--- | :--- |
| <span>**1. 事実性の検出**</span> | <span>**知識の捏造**</span><br>嘘の事実、架空の情報の生成 | **外部検索**<br>WikiやGoogle検索で事実の裏取りを行う。<br>**自己整合性 (Self-Consistency)**<br>何度も同じ質問をして、回答のブレを見る。 |
| <span>**2. 誠実性の検出**</span> | <span>**文脈・指示の無視**</span><br>RAGでの矛盾、指示違反 | **NLI (自然言語推論)**<br>参照文と生成文の間に「矛盾」がないかを見る。<br>**LLM-as-a-Judge**<br>GPT-4などの別のLLMに「指示に従っているか」を判定させる。 |

<style scoped>
  span {
    white-space: nowrap;
  }
  table {
      font-size: 85%;
}
</style>

---

# 事実性の検出「外部検索」

AIの生成した長い文章を、検証可能な最小単位（原子的事実）に分解し、それぞれをGoogle検索やWikipediaで判定します。

<div class="columns">
  <div class="col">
    <strong>STEP 1: 生成文の分解</strong><br><br>
    「A氏は2026年にB社を買収した」<br>
    <span>↓</span>
    ① [A氏] は [B社] を買収した<br>
    ② それは [2026年] である
  </div>
  <div class="col">
    <strong>STEP 2: 外部検索で検証</strong><br><br>
    ① Wikipedia: "A氏はB社を買収" → <strong>True</strong><br>
    ② ニュース検索: "2025年完了" → <strong>False</strong><br>
    <br>
    判定: <strong>事実誤認（ハルシネーション）</strong>
  </div>
</div>

<br>
<small>※ FActScoreなどの手法で採用されているアプローチです。</small>

<style scoped>
span {
  width: 100%;
  display: flex;
  justify-content: center;
}
</style>
---

# 事実性の検出の仕組み「Self-Consistency」

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

# 誠実性の検出「NLI (自然言語推論)」

「AIの回答」が「参照元の文章」と論理的に矛盾していないかを、専用のモデルで判定します。RAGシステムにおいて特に重要です。

<div class="columns">
  <div class="col">
    <strong>参照ドキュメント（前提）</strong><br>
    「当社の新製品Xは、2025年12月に発売予定である。」
  </div>
  <div class="col">
    <strong>AIの回答（仮説）と判定</strong><br>
    <br>
    ①「製品Xは来年末に出ます」<br>
    → <strong>含意 → OK</strong><br>
    <br>
    ②「製品Xはすでに発売中です」<br>
    → <strong>矛盾 → NG</strong>
  </div>
</div>

---

# 誠実性の検出「LLM-as-a-Judge」

論理チェックだけでは難しい「指示への従順さ」は、より高性能なAI（GPT-4など）を**裁判官**として使い、判定させます。

<div class="columns">
  <div class="col">
    <strong>プロンプト（指示）</strong><br>
    「以下の文章を、<span style="color:red">3行の箇条書き</span>で要約してください。」
  </div>
  <div class="col">
    <strong>判定プロセス (Judge)</strong><br>
    <br>
    <strong>回答A:</strong> (長い文章で回答)<br>
    Judge: 「箇条書きではない」→ <strong>NG</strong><br>
    <br>
    <strong>回答B:</strong> (3行のリスト)<br>
    Judge: 「指示に従っている」→ <strong>OK</strong>
  </div>
</div>

---

# 【クイズ2】最適な「嘘発見器」を選んでください

あなたの会社でRAG（社内検索AI）を運用中、以下のトラブルが発生しました。
**再発防止策として実装すべき機能は A, B どちらでしょうか？**

<br>

**トラブル内容:**
社内規定PDFには「副業は許可制」とあるのに、AIが「副業は原則禁止です」と回答した。

<div class="columns">
  <div class="col">
    <strong class="title">対策 A</strong><br>
    <br>
    <strong>Google検索機能</strong>を実装し、<br>
    一般的な日本企業の副業ルールを<br>
    参照させる。<br>
  </div>
  <div class="col">
    <strong class="title">対策 B</strong><br>
    <br>
    <strong>NLI（含意関係認識）</strong>を実装し、<br>
    参照元PDFと回答の間に<br>
    矛盾がないかチェックさせる。<br>
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

# 【解答2】最適な「嘘発見器」を選んでください

**正解は B です。**

<div class="columns">
  <div class="col">
    <strong class="title">A: Google検索 → 不適切</strong><br>
    <br>
    社内規定（クローズドな情報）はネットに落ちていません。<br>
    検索させると、他社の規定や一般的なルールを拾ってきてしまい、<strong>かえって混乱（ハルシネーション）が悪化</strong>します。
  </div>
  <div class="col">
    <strong class="title">B: NLIチェック → 最適</strong><br>
    <br>
    「参照元（PDF）」と「回答」を見比べ、論理的に矛盾していないかを機械的に判定するのが正解です。<br>
    これが<strong>「誠実性の検出」</strong>の基本アプローチです。
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

# まとめ

ハルシネーションを単なる「ミス」として片付けず、分類し、対処することが重要です。

1. **分類**: それは「知識の捏造」か？「指示の無視」か？
2. **検出**: 外部での裏取り、または内部の「迷い」を監視できているか？
3. **対策**: RAGに加え、自己修正（Self-Correction）のループはあるか？

<br>

> **Next Action:**
> 自社のAIアプリケーションで起きているエラーが、「事実性の欠如」なのか「誠実性の欠如」なのか、まずは分類することから始めてみましょう。