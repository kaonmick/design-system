# Semantic Color Token Guidelines

> このガイドラインは **人間とAIの両方** が参照し、トークン設計・命名・分類の判断を統一するためのルールです。
> ドラフトの余白を埋め、「迷ったときにここを見れば決められる」状態を目指します。

---

## 基本構造

### Primitive

Tailwind v4 のカラースケールを採用する。Primitive は **値の宣言のみ** を担う。意味を持たせない。

```
blue-50, blue-100 ... blue-900, blue-950
```

**なぜ Tailwind v4 か**  
OKLCH ベースで設計されているため、hover / active の濃淡設計をスケールの隣接値で表現できる。`brightness()` フィルターは使わない（理由は後述）。

---

### Semantic

Semantic Token は以下の構造で命名する。

```
role--modifier--state
```

- Semantic は **意味の参照先** であり、Primitive の別名ではない
- 1つの Primitive に複数の Semantic が対応することは許容する
- Component 名は **含めない**（詳細は「Component」セクションを参照）

---

## 4分類の判断ルール

| 分類 | 問い | 例 |
|---|---|---|
| Role | 何のための面・文字・枠か？ | `surface`, `text`, `border` |
| Modifier | どんな意味・用途か？ | `brand`, `danger`, `subtle` |
| State | 今どんな状態か？ | `hover`, `disabled`, `focus` |
| Component | 何のUI部品か？ | `button`, `card`（Token名に含めない） |

**判断の優先順位**：Role → Modifier → State の順に考える。Componentは最後に確認して「含まれていないか」をチェックする。

---

## Role

### 定義

Role はトークンが **何の視覚要素か** を表す。

```
surface  field  canvas  floating  overlay
text  icon  border  divider  ring
```

### surface 系の使い分け

迷いが生じやすい `surface` 系は以下の基準で判断する。

| Role | 使う場面 | 使わない場面 | 判断の決め手 |
|---|---|---|---|
| `surface` | Card, Section, Sidebar | Input, Modal | まずこれを検討する |
| `field` | Input, Select, Textarea | Card, Modal | **ユーザーが値を変更できる** 面 |
| `canvas` | Editor, Whiteboard, Preview | Toolbar, Sidebar | **コンテンツそのもの** が乗る領域 |
| `floating` | Dropdown, Tooltip, Dialog 本体 | Backdrop | **一時的に浮いて** 表示される UI |
| `overlay` | Modal Backdrop, Loading Scrim | Dialog 本体 | **背景操作を制御** するレイヤー |

> **ルール**：Dialog は `floating`（本体）と `overlay`（Backdrop）に分けて扱う。1つのコンポーネントに2つの Role が存在してよい。

---

## Modifier

### 定義

Modifier は Role に **意味・強度・前景関係** を与える。

### 意味系

```
brand  danger  warning  success  info
```

### 強度系

```
subtle  muted  strong  inverse
```

### 前景系（on-xxx）

```
on-brand  on-danger  on-warning  on-success
```

`on-xxx` は「その surface の上に乗せる text/icon」を表す。背景色と前景色をペアで設計するときに使う。

**例**

```
surface--brand      → ブランドカラーの面
text--on-brand      → surface--brand の上に乗る文字
border--focus       → フォーカス状態の枠
```

---

## State

### 定義

State は **現在のインタラクション状態** を表す。

```
hover  active  disabled  focus  selected  checked  open  expanded  dragging
```

### Token 化する基準

**色が変わるか、枠が変わるか** を基準にする。

| State | Token 化 |
|---|---|
| `hover` | 色が変わるなら ✅ |
| `active` | 色が変わるなら ✅ |
| `disabled` | 色が変わるなら ✅ |
| `focus` | ring / border に使うなら ✅ |
| `selected` | 色が変わるなら ✅ |
| `open` | 原則しない ❌ |
| `expanded` | 原則しない ❌ |
| `dragging` | 原則しない ❌ |

`open` / `expanded` / `dragging` は JS 側の状態管理で制御し、Token として定義しない。

---

## Component

### 原則：Token 名に Component を含めない

Component 名を Token に含めると、Token が特定の UI 部品に縛られ、再利用できなくなる。

**❌ NG（Component 名が入っている）**

```
button--primary
button--primary--hover
card--surface
dialog--overlay
```

**✅ OK（Role / Modifier / State で表現）**

```
surface--brand
surface--brand--hover
overlay
```

### なぜ Component を分離するか

`button--primary` は Button にしか使えない。  
`surface--brand` は Button にも Tag にも Badge にも使える。  
Token は **UI 部品の設計図ではなく、意味の参照先** である。

### Component ↔ Token のマッピング例

| Component | 使用する Token |
|---|---|
| Button（Primary） | `surface--brand`, `text--on-brand`, `surface--brand--hover` |
| Card | `surface`, `border`, `text` |
| Alert（Error） | `surface--danger`, `text--danger` |
| Toast（Success） | `floating`, `text--on-success` |
| Input | `field`, `border`, `border--focus` |
| Modal Backdrop | `overlay` |

---

## カラー設計（OKLCH 方針）

### hover / active は隣接スケールで表現する

```
surface--brand        → blue-600
surface--brand--hover → blue-700
surface--brand--active → blue-800
```

### brightness() は使わない

```css
/* ❌ 非推奨 */
filter: brightness(0.9);
```

**理由**  
- 色相がズレる
- 彩度が変化する
- 意味のある Token として管理できない

---

## 最小セット

新しいプロジェクトを開始するときは、まずこのセットを定義する。

```
/* surface */
surface
surface--subtle
surface--brand
surface--brand--hover
surface--brand--active
surface--brand--disabled
surface--danger
surface--danger--hover
surface--danger--active

/* floating / overlay */
floating
overlay

/* text */
text
text--subtle
text--danger
text--on-brand
text--on-danger

/* border / ring */
border
border--subtle
border--danger
border--focus
ring--focus
```

---

## AI 向け補足ルール

AI がトークンを生成・提案するときに守るべき追加ルール。

1. **Component 名を含む Token を提案しない**。`button-`, `card-`, `modal-` で始まる名前は却下する。
2. **Role なしの Modifier を作らない**。`--brand` だけでは Token にならない。必ず `surface--brand` のように Role をつける。
3. **State は単独で Token にしない**。`--hover` だけの Token は作らない。`surface--brand--hover` のように Role + Modifier とセットにする。
4. **新しい Role を勝手に作らない**。定義済みの Role 一覧（surface / field / canvas / floating / overlay / text / icon / border / divider / ring）に存在しない Role が必要に思えたときは、人間に確認を求める。
5. **Primitive 値を直接 UI コードに書かない**。`blue-600` を直接使わず、必ず `surface--brand` などの Semantic Token を経由する。

---

## 更新履歴

| バージョン | 内容 |
|---|---|
| v0.3 | Draft |
| v1.0 | AI 向け補足ルール追加・禁止パターン明示化 |
