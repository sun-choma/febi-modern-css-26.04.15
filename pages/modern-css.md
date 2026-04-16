---
layout: section
transition: slide-left
---

## 🚀

## 今すぐ使えるモダンCSS

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# @layer

<div class="flex items-center gap-2">
  <span>カスケードを明示的にコントロール — 詳細度バトルはもう終わり</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/@layer" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

::header::

::left::

**❌ Before — 詳細度と戦う**

```css
/* ライブラリの詳細度が高い */
.ui-lib .btn.primary {
  background: blue;
}

/* その1: 無理やり詳細度を上げる */
.my-app .ui-lib .btn.primary {
  background: red;
}

/* その2: !important に頼る 😬 */
.btn {
  background: red !important;
}
```

::right::

**✅ After — レイヤーが優先度を決める**

```css
@import url("ui-lib.css") layer(lib);

@layer lib, components;

@layer components {
  /* 詳細度に関係なく常に勝つ */
  .btn {
    background: red; /* ✅ */
  }
}
```

> 後に宣言したレイヤーが勝つ。自分のコードは常に最後。

---
transition: slide-left
---

# @layer — 実際のユースケース

サードパーティのコンポーネントライブラリを手なずける

```css
/* 1. ライブラリをレイヤーにインポート */
@import url("some-ui-lib.css") layer(lib);

/* 2. レイヤーの順番を宣言 — 後が優先 */
@layer lib, components, utilities;

/* 3. 自分のスタイルを書く — 詳細度の心配なし */
@layer components {
  .btn {
    background: var(--brand);
    border-radius: var(--radius);
  }
}

@layer utilities {
  /* components より常に優先される */
  .mt-0 { margin-top: 0; }
}
```

> ライブラリと戦うんじゃなく、ただ自分のスタイルを書くだけでいい

---
transition: slide-left
---

# @property

<div class="flex items-center gap-2">
  <span>型付きカスタムプロパティ — CSS変数を一級市民に</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/@property" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

```css
@property --brand {
  syntax: '<color>';      /* 型チェック — 無効な値は無視される */
  inherits: false;        /* 子要素に漏れない */
  initial-value: #ff79c6; /* デフォルト保証 — var(--x, fallback) 不要 */
}
```

- **`syntax`** — 型を定義する: `<color>`, `<number>`, `<length>`, `<angle>`
  など <a href="https://developer.mozilla.org/ja/docs/Web/CSS/@property/syntax#angle" target="_blank">さらに多くの型</a>
- **`inherits`** — 子要素への継承を制御する
- **`initial-value`** — デフォルト値を保証する。`syntax` が `*` でない場合は必須

> `@property` なしのカスタムプロパティはただの文字列 — ブラウザには型がわからない

---
transition: slide-left
---

# @property — 問題

`@property` なしでは、カスタムプロパティはアニメーションできない — ブラウザは文字列として扱うから

```css
:root {
  --opacity: 1; /* ただの文字列としてブラウザに認識される */
}

@keyframes pulse {
  0%   { --opacity: 1; }
  50%  { --opacity: 0.2; }
  100% { --opacity: 1; }
}

.item {
  opacity: var(--opacity);
  animation: pulse 1.5s ease infinite; /* ❌ スムーズにならない — 値がジャンプする */
}
```

> `--opacity` が数値だとブラウザが知らないので、補間できない

---
transition: slide-left
---

# @property — 解決策

プロパティを登録すれば、ブラウザがスムーズに補間できる

```css
@property --opacity {
  syntax: '<number>';
  inherits: false;
  initial-value: 1;
}

.item {
  opacity: var(--opacity);
  animation: pulse 1.5s ease infinite;
}

@keyframes pulse {
  0%   { --opacity: 1; }
  50%  { --opacity: 0.2; }
  100% { --opacity: 1; }
}
```

> ブラウザが `--opacity` を数値として認識 — スムーズな補間、GPUアクセラレーション付き

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# @property — ライブデモ

アイテムをクリックして一時停止/再開 — 再開したときの違いに注目

::header::

::left::

<span class="bg-pink-500 text-white text-xs font-bold px-2 py-1 rounded">
animation</span> <span class="text-sm opacity-50">個別アニメーション</span>

<PulseDemo variant="classic" />

```css
.item {
  animation: pulse 1.5s ease infinite;
}

@keyframes pulse {
  0%   { opacity: 1; }
  50%  { opacity: 0.2; }
  100% { opacity: 1; }
}
```

::right::

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">
@property</span> <span class="text-sm opacity-50">--opacity を共有</span>

<PulseDemo variant="property" />

<div class="small-code">

```css
@property --opacity {
  syntax: '';
  inherits: true;
  initial-value: 1;
}

@keyframes pulse {
  0%   { --opacity: 1; }
  50%  { --opacity: 0.2; }
  100% { --opacity: 1; }
}

.item.pulsing {
  opacity: var(--opacity); /* グローバルな値を読むだけ */
}
```

</div>

<style scoped>
.small-code .slidev-code {
  font-size: 0.6rem !important;
  line-height: 1.2 !important;
}
</style>

---
transition: slide-left
---

# @starting-style

<div class="flex items-center gap-2">
  <span>「以前の状態」がない要素に、開始状態を定義する</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/@starting-style" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

CSSトランジションは2つの既知の状態の間でうまく動く — でも「以前の状態」がなかったら？

```css
/* 初回表示時にはこのトランジションは動かない */
.toast {
  opacity: 1;
  transition: opacity 0.3s ease;
}
```

これが起きるのは:

- 🆕 **DOMに追加されたとき** — 以前の状態が存在しない
- 👁️ **`display: none` から表示に変わるとき** — 離散的な変化はトランジションしない
- 🔝 **トップレイヤーに入るとき** — `<dialog>`、popover

> ブラウザにはトランジション元がない — だからパッと表示されてしまう

---
transition: slide-left
---

# @starting-style — 構文

<div class="flex items-center gap-2">
  <span>トランジションの「出発点」を定義する</span>
</div>

```css
.toast {
  /* 最終状態 */
  opacity: 1;
  transform: translateY(0);
  transition: opacity 0.3s ease, transform 0.3s ease;
}

/* 初回表示時にトランジションが始まる「出発点」 */
@starting-style {
  .toast {
    opacity: 0;
    transform: translateY(1rem);
  }
}
```

> `@starting-style` がブラウザに「before」状態を与える — これでトランジション元ができる

---
transition: slide-left
---

# transition-behavior: allow-discrete

<div class="flex items-center gap-2">
  <span>`display` などの離散プロパティのトランジションを有効にする</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/transition-behavior" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

**離散**プロパティは値が瞬時に切り替わる — 中間状態がない。
`transition-behavior: allow-discrete` でそれをアニメーション可能にする。

```css
.dialog {
  opacity: 1;
  transition:
    opacity 0.3s ease,
    display 0.3s allow-discrete,  /* display のトランジションを許可 */
    overlay 0.3s allow-discrete;  /* トップレイヤーのトランジションを許可 */
}

.dialog:not([open]) {
  opacity: 0;
}
```

- **`display`** — 退場アニメーションが終わるまで要素が見える状態を保つ
- **`overlay`** — 退場アニメーションが終わるまでトップレイヤーに留まる

> `allow-discrete` なしだと要素が即座に消える — 退場アニメーションは再生されない

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# @starting-style + \<dialog\>

スムーズな入場・退場 — JavaScriptもライブラリも不要

::header::

::left::

```css
.dialog {
  opacity: 1;
  transform: translateY(0) scale(1);
  transition:
    opacity 0.3s ease,
    transform 0.3s ease,
    display 0.3s allow-discrete,
    overlay 0.3s allow-discrete;
}

/* 退場状態 */
.dialog:not([open]) {
  opacity: 0;
  transform: translateY(1rem) scale(0.95);
}
```

::right::

```css
/* 入場状態 — トランジションの出発点 */
@starting-style {
  .dialog[open] {
    opacity: 0;
    transform: translateY(1rem) scale(0.95);
  }
}
```

<div class="mt-4">
  <DialogDemo />
</div>

> `<dialog>` はフォーカストラップと `Esc` キーによる閉じる動作をネイティブで持つ — Baseline 2022 — ライブラリ不要
