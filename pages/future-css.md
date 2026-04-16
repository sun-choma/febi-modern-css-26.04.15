---
layout: section
transition: slide-left
---

## 🔭

## これからに期待

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# interpolate-size

<div class="flex items-center gap-2">
  <span>intrinsicなサイズキーワードへのアニメーション — JS不要</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/interpolate-size" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>
<div class="flex items-center gap-2 mt-1">
  <logos-chrome class="size-5" />
  <logos-microsoft-edge class="size-5" />
  <span class="text-xs opacity-50">Chromiumのみ — Firefox・Safariは未対応</span>
</div>

::header::

::left::

```css
.content {
  interpolate-size: allow-keywords;
  height: 0;
  overflow: hidden;
  transition: height 0.5s ease;
}

.content.open {
  height: auto; /* スムーズにアニメーションする */
}
```

> これまでは `max-height` ハックやJS の `scrollHeight` 計算が必要だった

::right::

<InterpolateDemo />

---
layout: two-cols-header
layoutClass: gap-8
transition: slide-left
---

# corner-shape

<div class="flex items-center gap-2">
  <span>border-radiusを超えた角の形</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/corner-shape" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>
<div class="flex items-center gap-2 mt-1">
  <logos-chrome class="size-5" />
  <logos-microsoft-edge class="size-5" />
  <span class="text-xs opacity-50">Chromiumのみ — Firefox・Safariは未対応</span>
</div>

::header::

::left::

```css
.card {
  border-radius: 1.5rem;
  corner-shape: round; /* デフォルト */
}

.card-squircle {
  corner-shape: squircle; /* iOSスタイル */
}
```

<div class="flex gap-8 justify-center mt-4">
  <CornerShapeDemo shape="round" />
  <CornerShapeDemo shape="squircle" />
</div>

::right::

```css
.card-bevel {
  corner-shape: bevel; /* 面取り */
}

.card-scoop {
  corner-shape: scoop; /* 内側にくぼむ */
}
```

<div class="flex gap-8 justify-center mt-4">
  <CornerShapeDemo shape="bevel" />
  <CornerShapeDemo shape="scoop" />
</div>

---
transition: slide-left
---

# CSS if()

<div class="flex items-center gap-2">
  <span>インライン条件分岐 — 将来が楽しみな機能</span>
  <a href="https://developer.mozilla.org/ja/docs/Web/CSS/if" target="_blank">
    <simple-icons-mdnwebdocs class="size-4 m-1" />
  </a>
</div>

<div class="flex items-center gap-2 mt-1">
  <logos-chrome class="size-5" />
  <logos-microsoft-edge class="size-5" />
  <span class="text-xs opacity-50">Chromiumのみ — Firefox・Safariは未対応</span>
</div>

```css
.button {
  /* style queryに基づいた条件値 */
  background: if(style(--variant: primary): #bd93f9; else: #44475a);
  padding: if(style(--size: large): 1rem 2rem; else: 0.5rem 1rem);
}

/* 修飾クラスがなくても書ける */
.button--primary { background: #bd93f9; }
.button--large   { padding: 1rem 2rem; }
```

これで何が変わる:
- **インライン条件分岐** — 修飾クラスや属性セレクタが不要に
- **値の中で style query** — カスタムプロパティの値に直接反応できる
- **SCSSとのギャップを埋める** — ビルドステップなしで `@if` ロジックが書ける
- **すっきりしたコンポーネントAPI** — クラスひとつ、振る舞いはカスタムプロパティで