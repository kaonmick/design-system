# Anti-patterns

このページは、Token 設計で避ける判断をまとめたものです。

迷ったときは「見た目が近いか」ではなく、「意味が合っているか」を先に確認します。

---

## 1. Component 名 Token を作らない

### NG

```txt
button--primary
button--primary--hover
card--surface
dialog--overlay
```

### なぜ避けるか

Component 名を Token に入れると、その Token が特定の UI 部品に閉じてしまいます。

Button 以外でも同じ意味の色を使いたいときに再利用しにくくなります。

### OK

```txt
surface--brand
surface--brand--hover
surface
overlay
```

Component 側で「Button には `surface--brand` を使う」と対応づけます。

---

## 2. Primitive 値を UI に直接書かない

### NG

```css
.button {
  background: blue-600;
  color: white;
}
```

### なぜ避けるか

Primitive は値そのものです。

UI に直接書くと、「これはブランド色なのか」「危険操作なのか」といった意味が分からなくなります。

### OK

```css
.button {
  background: var(--surface--brand);
  color: var(--text--on-brand);
}
```

UI では Semantic Token を使い、Primitive は Semantic Token の参照元として扱います。

---

## 3. 色を後から加工しない

### NG

```css
.button:hover {
  filter: brightness(0.9);
}
```

### なぜ避けるか

`brightness()` などで後から色を加工すると、色相や彩度が意図せず変わります。

Token として管理できないため、画面ごとに微妙な差が増えます。

### OK

```css
.button {
  background: var(--surface--brand);
}

.button:hover {
  background: var(--surface--brand--hover);
}
```

hover / active などの状態は、状態つきの Semantic Token で表します。

---

## 4. 余白や角丸を場当たり的に増やさない

### NG

```css
.card {
  padding: 18px;
  border-radius: 13px;
}
```

### なぜ避けるか

その場だけの数値を増やすと、画面全体のリズムが崩れます。

あとから別の UI を作るときにも、どの値を使えばよいか判断しにくくなります。

### OK

```css
.card {
  padding: var(--space--md);
  border-radius: var(--radius--md);
}
```

余白や角丸も、用途に合う Token を選びます。

まだ Token がない場合は、新しい値を直接足さず、まず必要性を確認します。

---

## 5. 見た目だけで意味の違う Token を使わない

### NG

```css
.delete-button {
  background: var(--surface--brand);
}
```

### なぜ避けるか

見た目が近くても、意味が違う Token を使うと、レビューや変更時に判断を誤ります。

削除や危険操作は、ブランドではなく danger の意味を持ちます。

### OK

```css
.delete-button {
  background: var(--surface--danger);
  color: var(--text--on-danger);
}
```

Token は「色の見た目」ではなく「UI 上の意味」で選びます。

---

## 6. Role なしの Token を作らない

### NG

```txt
brand
danger
hover
disabled
```

### なぜ避けるか

Role がないと、それが背景なのか、文字なのか、枠なのか分かりません。

State だけの Token も、何の状態なのか判断できません。

### OK

```txt
surface--brand
text--danger
surface--brand--hover
border--focus
```

Token 名は、基本的に `role--modifier--state` の順で考えます。

---

## レビュー用チェックリスト

- Component 名が Token 名に入っていないか
- Primitive 値を UI に直接書いていないか
- `brightness()` などで色を後加工していないか
- 中途半端な余白・角丸の値を直接足していないか
- 見た目ではなく意味で Token を選んでいるか
- Role なし、State だけの Token を作っていないか

