# Design System Roadmap

このドキュメントは、現在の Design System Docs に不足している内容を、アジャイルで進めやすい単位に分けた作業リストです。

作業が被らないように、先に各領域の方針を分けて作り、最後に全体をつなぐタスクを行います。

---

## 進め方

- 1タスクは、1つの判断領域だけを扱う
- 先に方針ドキュメントを作り、最後に一覧表や整合性チェックでまとめる
- 迷う内容は、仮決めせず「要確認」として残す
- Docsify + GitHub Pages 前提のため、Node.js などのセットアップは追加しない

---

## Task 1: Typography Token の基準を作る

### 目的

文字サイズ・行間・太さの判断を統一する。

### 対象ファイル

- `token-guidelines/semantic-typography-guidelines.md`

### 作業内容

- Typography の基本構造を定義する
- 見出し、本文、補助テキスト、ラベル、ボタン文字の使い分けを書く
- font-size、line-height、font-weight の最小セットを決める
- モバイル時の文字サイズの扱いを書く
- AI が勝手にサイズを増やさないための禁止ルールを書く

### 完了条件

- Codex が `title / body / caption / label / action` の使い分けを判断できる
- 文字サイズや行間をハードコードせず、Token 経由で説明できる

---

## Task 2: Dimension Token の基準を作る

### 目的

余白・角丸・線幅・高さ・サイズの判断を統一する。

### 対象ファイル

- `token-guidelines/semantic-dimension-guidelines.md`

### 作業内容

- spacing、gap、radius、border width、size、height の分類を定義する
- 小さい UI と大きい UI で使う値の基準を書く
- Button、Input、Card、Dialog で使う dimension の例を書く
- レイアウト用の余白と、部品内の余白を分ける
- AI が勝手に `13px` などの中途半端な値を作らないための禁止ルールを書く

### 完了条件

- Codex が余白、角丸、高さを場面ごとに選べる
- 値の追加が必要な場合に、人間へ確認すべき基準がある

---

## Task 3: Accessibility の基準を作る

### 目的

見た目だけでなく、読みやすさ・操作しやすさを DS のルールに入れる。

### 対象ファイル

- `guidelines/accessibility-guidelines.md`

### 作業内容

- 色のコントラスト基準を書く
- focus ring の最低条件を書く
- disabled 状態の見え方を書く
- 文字サイズの下限を書く
- キーボード操作で見失わないためのルールを書く

### 完了条件

- Codex が hover や focus を作る時に、見た目だけで判断しない
- 色・文字・操作状態の最低品質が説明されている

---

## Task 4: Component への適用例を作る

### 目的

Token を実際の UI 部品へどう当てるかを分かりやすくする。

### 対象ファイル

- `component-guidelines/component-token-examples.md`

### 作業内容

- Button の Token 使用例を書く
- Input の Token 使用例を書く
- Card の Token 使用例を書く
- Dialog の Token 使用例を書く
- Toast、Badge、Tabs などの補助 UI の例を書く
- Component 名を Token 名に含めない理由を再確認できる形にする

### 完了条件

- Codex が UI 部品ごとに Semantic Token を選びやすい
- Component 固有 Token を増やさずに説明できる

---

## Task 5: 禁止パターン集を作る

### 目的

AI や人間がやりがちな判断ミスを先に潰す。

### 対象ファイル

- `guidelines/anti-patterns.md`

### 作業内容

- Component 名 Token の禁止例を書く
- Primitive 値の直接指定の禁止例を書く
- `brightness()` など、色を後から加工する指定の禁止例を書く
- 余白や角丸を場当たり的に増やす例を書く
- 見た目だけを合わせて意味の違う Token を使う例を書く

### 完了条件

- Codex が「やってはいけない判断」を参照できる
- レビュー時に指摘しやすい共通言語がある

---

## Task 6: Primitive Token の採用範囲を決める

### 目的

Semantic Token の参照元になる基本値を整理する。

### 対象ファイル

- `token-guidelines/primitive-token-guidelines.md`

### 作業内容

- Color の primitive 採用範囲を決める
- Typography の primitive 採用範囲を決める
- Dimension の primitive 採用範囲を決める
- Tailwind v4 を使う場合の扱いを書く
- primitive を UI に直接使わないルールを書く

### 完了条件

- `blue-600` や `spacing-4` のような値の位置づけが分かる
- Semantic Token と Primitive Token の役割が分かれている

---

## Task 7: AI 向け判断ルールを全体化する

### 目的

Color だけにある AI 向けルールを、Typography と Dimension にも広げる。

### 対象ファイル

- `guidelines/ai-decision-rules.md`

### 作業内容

- AI が勝手に Role を増やさないルールを書く
- AI が値を直接書かないルールを書く
- AI が迷った時に確認すべき内容を書く
- Color、Typography、Dimension の判断順をまとめる

### 完了条件

- Codex が迷った時に、まず参照する共通ルールがある
- 「決めてよいこと」と「人間に確認すること」が分かれている

---

## Task 8: Semantic Token Map を作る

### 目的

Semantic Token と Primitive Token の対応表を作る。

### 対象ファイル

- `token-guidelines/semantic-token-map.md`

### 作業内容

- Color の対応表を作る
- Typography の対応表を作る
- Dimension の対応表を作る
- `未決定` の値を分かるようにする
- 各 guideline への参照リンクを入れる

### 完了条件

- Codex が実装時に参照できる Token 一覧がある
- 未定義の値が曖昧なまま使われない

### 注意

このタスクは、Task 1、Task 2、Task 6 の後に行う。

---

## Task 9: 既存 Color Guidelines の揺れを直す

### 目的

現在の Color Guidelines 内で、Codex が迷いやすい部分を修正する。

### 対象ファイル

- `token-guidelines/semantic-color-guidelines.md`

### 作業内容

- `icon` token を最小セットに含めるか決める
- `text--on-success` の背景になる token を明確にする
- `dragging` など、原則 token 化しない state の例外ルールを書く
- Toast の token 例を整える
- 最小セットと Component 例の矛盾を解消する

### 完了条件

- Color Guidelines 内の例が、最小セットと矛盾しない
- Codex が例をそのまま使っても破綻しない

### 注意

このタスクは、Task 4 と Task 8 の後に行う。

---

## Task 10: Docsify のナビゲーションを整理する

### 目的

追加したドキュメントを探しやすくする。

### 対象ファイル

- `_sidebar.md`
- `README.md`

### 作業内容

- Sidebar に新しいページを追加する
- README の構成説明を更新する
- 読む順番を追加する
- 最初に読むべきページを明確にする

### 完了条件

- Docsify 上で全ページに移動できる
- 新しく参加した人や Codex が、読む順番を迷わない

### 注意

このタスクは、すべてのページ追加後に行う。

---

## 推奨スプリント分け

### Sprint 1: 土台を埋める

- Task 1: Typography Token の基準を作る
- Task 2: Dimension Token の基準を作る
- Task 3: Accessibility の基準を作る

### Sprint 2: 使い方を増やす

- Task 4: Component への適用例を作る
- Task 5: 禁止パターン集を作る
- Task 6: Primitive Token の採用範囲を決める

### Sprint 3: AI が迷わない形にする

- Task 7: AI 向け判断ルールを全体化する
- Task 8: Semantic Token Map を作る

### Sprint 4: 統合と整理

- Task 9: 既存 Color Guidelines の揺れを直す
- Task 10: Docsify のナビゲーションを整理する

---

## 並行作業しやすい分担

| 担当領域 | 対応タスク | 被りにくい理由 |
|---|---:|---|
| Typography | Task 1 | 文字だけを扱うため、他領域と衝突しにくい |
| Dimension | Task 2 | 余白・サイズだけを扱うため、他領域と衝突しにくい |
| Accessibility | Task 3 | 品質基準の整理なので、Token の値決めと分けて進められる |
| Component Examples | Task 4 | 既存ルールを使う側なので、方針決定とは分けられる |
| Anti-patterns | Task 5 | 禁止例の整理なので、各 guideline と並行しやすい |
| Primitive | Task 6 | Semantic の参照元だけを扱うため、独立して進めやすい |

---

## 最後に回す作業

以下は他タスクの内容を参照するため、最後に行う。

- Task 8: Semantic Token Map を作る
- Task 9: 既存 Color Guidelines の揺れを直す
- Task 10: Docsify のナビゲーションを整理する
