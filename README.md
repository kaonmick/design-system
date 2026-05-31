# Design System Docs

汎用的な design system を構築するためのドキュメント管理 repo です。

他のプロジェクトを立ち上げる時に、ボイラープレートのように参照できる状態を目指します。

この repo はドキュメント専用です。Node.js などの技術的なセットアップは含めません。

## 公開方法

Docsify + GitHub Pages で公開します。

GitHub Pages の設定で、公開元を `main` branch の root にします。

独自ドメインを使う場合は、GitHub Pages の Custom domain にドメインを設定します。

## 構成

- `README.md`: サイトのトップページ
- `_sidebar.md`: サイトのサイドバー
- `token-guidelines/`: Token 設計のガイドライン
- `index.html`: Docsify の入口
