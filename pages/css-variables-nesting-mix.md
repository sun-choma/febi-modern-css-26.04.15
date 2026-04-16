---
layout: section
---

## 💪

## CSSはプリプロセッサに追いついた

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# Custom Properties

書き方は似てる、でもブラウザネイティブ

::header::

::left::

### SCSS

```scss
$brand: #ff79c6;
$spacing: 1rem;

.button {
  background: $brand;
  padding: $spacing;
}
```

::right::

### CSS

```css
:root {
  --brand: #ff79c6;
  --spacing: 1rem;
}

.button {
  background: var(--brand);
  padding: var(--spacing);
}
```

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# Custom Properties — リアクティビティ

SCSSの変数はコンパイル時に置き換えられる。CSSカスタムプロパティはブラウザ上で生きている。

::header::

::left::

### 一度定義して、どこでも使う

```css
:root {
  --accent: #ff79c6;
}

.button {
  background: var(--accent);
}

.link {
  color: var(--accent);
}

.badge {
  border-color: var(--accent);
}
```

::right::

### どこでも上書きできる

```css
/* ダークモード — 変数を上書きするだけ */
@media (prefers-color-scheme: dark) {
  :root {
    --accent: #bd93f9;
  }
}

/* コンポーネント単位でも */
.card {
  --accent: #50fa7b;
}
```

> 変数ひとつ。あとはカスケードがやってくれる。

<div class="flex gap-2 items-center text-sm opacity-75">
<span>💡</span>
<span>カスタムプロパティのアニメーションと組み合わせると、さらに強力になる</span>
</div>

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# Custom Properties — スコープ

同じ変数名でも、DOMのどこにいるかで値が変わる。

::header::

::left::

<span class="bg-pink-500 text-white text-xs font-bold px-2 py-1 rounded">SCSS</span> <span class="text-sm opacity-70">
静的、コンパイル時 ❌</span>

```scss
.card {
  color: #44475a;
  border: 1px solid #44475a;
}

.card:hover {
  // その1: 両方手動で更新
  color: #6272a4;
  border: 1px solid #6272a4;

  // その2: 新しい変数を作る
  $hl: #6272a4;
  color: $hl;
  border: 1px solid $hl;
}
```

::right::

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span> <span class="text-sm opacity-70">
動的、スコープ付き ✅</span>

```css
.card {
  --accent: #44475a;
  color: var(--accent);
  border: 1px solid var(--accent);
}

.card--highlight {
  --accent: #6272a4;
  /* color も border も両方更新される */
}
```

> * プロパティの宣言はひとつ。変数を上書きするだけでスタイルが変わる。
> * デザイントークンとして定義すれば、デザイナーが直接更新できる

---
transition: slide-left
---

# Custom Properties — 注意点

ランタイムで動く分、知っておくべきことがいくつかある

- フォールバック値 — `var(--accent, #ff79c6)` — 安全網として常に用意しておこう
- 無効な値は `initial` にフォールバックする — `--accent` がセットされていても無効な値なら `var(--accent, red)` は効かない
- メディアクエリの条件式では使えない — `@media (max-width: var(--bp))` ❌
- 大文字小文字を区別する — `--accent` と `--Accent` は別の変数
- プロパティ名には使えない — 値の部分にしか使えない

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# CSS Nesting

<div class="flex items-center gap-2">
  <span>おなじみの書き方、ブラウザネイティブ — Baseline 2024</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/CSS_nesting" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

::header::

::left::

<span class="bg-pink-500 text-white text-xs font-bold px-2 py-1 rounded">SCSS</span>

```scss
.card {
  padding: 1rem;

  &:hover {
    background: #44475a;
  }

  .title {
    font-size: 1.5rem;
  }
}
```

::right::

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span>

```css
.card {
  padding: 1rem;

  &:hover {
    background: #44475a;
  }

  & .title {
    font-size: 1.5rem;
  }
}
```

> `&` なしだと暗黙的にスペースが追加される — 明示的に `&` を使おう

---
transition: slide-left
---

# CSS Nesting — 注意点

SCSSから来た人が知っておくべきこと

<div class="grid grid-cols-2 gap-4 mt-4">
<div>

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span> **⚠️ `&` なしは暗黙的スペース**

```css
.card {
  & p {} /* ✅ 明示的 — .card p */

  p {} /* ⚠️ 暗黙的 — やはり .card p */

  &:hover {} /* ✅ .card:hover */

  :hover {} /* ⚠️ .card *:hover — 子要素すべて */
}
```

</div>
<div>

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span> **✅ at-rules はネスト可能**

```css
.card {
  font-size: 1rem;

  @media (width >= 768px) {
    font-size: 1.25rem;
  }
}
```

</div>
<div>

<span class="bg-pink-500 text-white text-xs font-bold px-2 py-1 rounded">SCSS</span> **✅ `&-suffix` 連結**

```scss
.card {
  &-title {} /* → .card-title */
}
```

</div>
<div>

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span> **❌ 連結は非対応**

```css
.card {
  & -title {} /* 無効 */
}
```

</div>
</div>

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# color-mix()

<div class="flex items-center gap-2">
  <span>ネイティブカラー操作 — プリプロセッサ不要</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/color_value/color-mix" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

::header::

::left::

<span class="bg-pink-500 text-white text-xs font-bold px-2 py-1 rounded">SCSS</span>

```scss
$color: #ff79c6;

.button {
  background: $color;

  &:hover {
    background: darken($color, 15%);
  }

  &:active {
    background: lighten($color, 15%);
  }
}
```

::right::

<span class="bg-purple-500 text-white text-xs font-bold px-2 py-1 rounded">CSS</span>

```css
.button {
  --color: #ff79c6;
  background: var(--color);

  &:hover {
    background: color-mix(in oklch, var(--color) 85%, black);
  }

  &:active {
    background: color-mix(in oklch, var(--color) 85%, white);
  }
}
```

<div class="grid grid-cols-2 gap-8 mt-4">
  <div>
    <div class="text-xs opacity-50 mb-1">sRGB — 中間が濁る</div>
    <div class="h-6 rounded" style="background: linear-gradient(to right, red, green)"></div>
  </div>
  <div>
    <div class="text-xs opacity-50 mb-1">oklch — 知覚的に均一</div>
    <div class="h-6 rounded" style="background: linear-gradient(to right in oklch, red, green)"></div>
  </div>
</div>