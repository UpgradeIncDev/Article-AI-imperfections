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
     1. ヘッダー・フッター・ページ番号（さきほどのグラデーション）
     ------------------------------------------------ */
  header, footer, section::after {
    background: linear-gradient(109.07deg, #28B3EE -18.03%, #1884D0 21.63%, #1271C4 53.6%, #1975C5 66.14%, #77ADD7 118.92%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
  }
  footer {
    display: flex;
    align-items: center; /* 垂直方向の真ん中に合わせる */
    gap: 15px;           /* 文字と棒の間隔 */
  }

  /* 2. footerの右側に要素を作る */
  footer::before {
    content: "";       /* これがないと表示されません */
    display: block;
    
    /* サイズ指定 */
    width: 10px;       /* 棒の太さ */
    height: 40px;      /* 棒の長さ（footerの高さに合わせて調整） */
    
    /* 色指定 */
    /* background-color ではなく background を使います */
    background: linear-gradient(109.07deg, #28B3EE -18.03%, #1884D0 21.63%, #1271C4 53.6%, #1975C5 66.14%, #77ADD7 118.92%);
  }

  /* ------------------------------------------------
     2. h1 タイトル（今回指定の新しいグラデーション）
     ------------------------------------------------ */
  h1 {
    background: linear-gradient(91.67deg, #00A5EC -10.12%, #0152A3 108.32%);
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    display: inline-block;
  }

---

# タイトルスライド

<span>『AIの不完全さを科学的に管理する』</span>

<br><br><br><br><br><br>
<p>日付: 2026年1月4日</p>

<style scoped>
h1 {
/* 余白を無視して配置するための設定 */
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

