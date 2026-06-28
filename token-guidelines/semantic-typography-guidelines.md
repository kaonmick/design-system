# Semantic Typography Guidelines

> このガイドラインは、文字サイズ・行間・太さの判断をそろえるためのルールです。
> 見た目の好みではなく、文章の役割から Token を選びます。

---

## 基本方針

Typography Token は、以下の2層で管理する。

- Primitive: 実際の値
- Semantic: 役割に合わせた参照名

Semantic Token は、Primitive Token のどれかを参照する。  
新しい文字サイズ・太さ・行間を直接追加しない。

---

## Primitive Token

Primitive は値の置き場です。UI へ直接指定せず、Semantic Token から参照します。

### Font Size

```
font-size-12
font-size-14
font-size-16
font-size-18
font-size-20
font-size-24
font-size-28
font-size-32
font-size-36
font-size-40
font-size-48
font-size-56
font-size-64
```

### Font Weight

```
font-weight-400
font-weight-500
font-weight-700
```

### Line Height

行間は数値ではなく、用途名で管理する。

| Token | 値 | 使う場面 |
|---|---:|---|
| `line-height-tight` | 120% | 大きい見出し |
| `line-height-snug` | 130% | 小さめの見出し、短いラベル |
| `line-height-normal` | 150% | 本文、説明文、長いテキスト |

---

## Semantic Token

Semantic は文字の役割を表す。  
`fontSize` は必ず Primitive の `font-size-*` を参照する。

### 最小セット

| Semantic Token | fontSize | lineHeight | fontWeight | 使う場面 |
|---|---|---|---|---|
| `title--lg` | `font-size-32` | `line-height-tight` | `font-weight-700` | 画面の主見出し |
| `title--md` | `font-size-24` | `line-height-snug` | `font-weight-700` | セクション見出し |
| `title--sm` | `font-size-20` | `line-height-snug` | `font-weight-700` | 小さい見出し |
| `body--md` | `font-size-16` | `line-height-normal` | `font-weight-400` | 標準の本文 |
| `body--sm` | `font-size-14` | `line-height-normal` | `font-weight-400` | 補足的な本文 |
| `caption` | `font-size-12` | `line-height-normal` | `font-weight-400` | 注釈、補助情報 |
| `label` | `font-size-14` | `line-height-snug` | `font-weight-500` | 入力欄や設定項目の名前 |
| `action` | `font-size-14` | `line-height-snug` | `font-weight-700` | Button、Tab、Menu の操作文字 |

---

## 使い分け

### title

画面やセクションの見出しに使う。

- `title--lg`: 画面全体の見出し
- `title--md`: 大きなまとまりの見出し
- `title--sm`: カード内や小さなまとまりの見出し

### body

説明文や通常の文章に使う。

- `body--md`: 基本の本文
- `body--sm`: 補助説明、短い説明文

### caption

重要度が低い補足に使う。  
本文の代わりに長文へ使わない。

### label

入力欄、設定項目、表の項目名に使う。  
強調したいだけの本文には使わない。

### action

クリックや選択など、操作につながる文字に使う。  
本文中のリンクは、見た目ではなく役割に合わせて別途判断する。

---

## モバイル時の扱い

モバイルでも、基本は同じ Semantic Token を使う。

ただし、画面幅が狭くて `title--lg` が大きすぎる場合は、`title--md` に下げてよい。  
この場合も `font-size-30` のような新しい値は作らない。

---

## 禁止ルール

### 直接値を書かない

```css
/* NG */
font-size: 18px;
line-height: 1.4;
font-weight: 600;
```

Semantic Token を通して指定する。

```css
/* OK */
font-size: var(--font-size-body-md);
line-height: var(--line-height-normal);
font-weight: var(--font-weight-400);
```

### 中途半端な値を作らない

`13px`、`15px`、`22px`、`600`、`140%` など、Primitive にない値を勝手に追加しない。

### Component 名を Token 名に入れない

```text
button-text
card-title
modal-heading
```

上記のような Component 固有の名前は使わない。  
`action`、`title--md`、`body--sm` のように、役割で選ぶ。

### 見た目だけで太字にしない

太字は情報の階層を作るために使う。  
装飾目的だけで `font-weight-700` を増やさない。

---

## 迷った時の確認基準

- 文章の役割は `title / body / caption / label / action` のどれか
- `fontSize` は Primitive の一覧にあるか
- 行間は `tight / snug / normal` のどれか
- 新しい値が必要なら、実装前に人間へ確認する
