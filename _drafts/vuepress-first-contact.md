---
layout: post
title: SIer的VuePress活用術
date: 2019-04-10 23:40:00 +0900
---

Markdownで書いたドキュメントをいい感じのHTMLに変換して適当にローカル保存して閲覧したい

## はじめに

SIerの業務において、ドキュメントを書く場面は多くあります。いわゆる納品資料や成果物として書く場合には、おおよそ、Word、Excel、PowePointなど、Office製品を使用することになるでしょう。また大抵の場合、フォーマットと呼ばれる文書の雛形やテンプレートさえも、会社として、もしくはお客様によって決められていて、あるいは保守業務ともなれば尚更、既存ドキュメントの該当項目を書き換えるのみで、「何で書くか？」について、我々にはなんら選択の余地はありません。

一方で、自分たちのためのドキュメントについてはどうでしょうか。内部資料や、チーム内共有ナレッジ、あるいは個人的な備忘録などです。これらが業務成果物と同じようにある必要はまったくありません。自分たちが書き、自分たちが見るドキュメントなのですから、それを書くツールも自分たち自身で決めることができるはずです。残された自由が、そこにあるはずです。

そもそも、Office製品は技術ドキュメントに最適な選択肢とは言えません。私は、その辺の技術者と比べて見劣りしないWordスキル、Excelの使いこなしについて、経験に基づく確固たる自信があり、そういった見地に立って言いますが、これは事実です。我々は、我々自身のドキュメントのために、他のもっと快適なツールを見つける必要があります。

幸いなことに、近年、デファクトの地位を占め、多くのエディタやツールによってサポートされる文書フォーマットがあります。もはや説明の必要はないでしょう。Markdownです。Markdownが全てを解決するとは言いませんが、手頃で、現実的な解を提供してくれます。ただし、`.md` ファイルをそのままの形で流通できるほど、社内のインフラが柔軟でないことも、社員のリテラシーが高くないことも、この少し長い物憂げな導入部をここまで飽きずに読んでいるあなたには理解ができることでしょう。

そう、そこでVuePressです。

## VuePressとは

静的サイトジェネレーター

## 開発環境の構築

VSCode, Node.js

## npmコマンドのプロキシ設定

`npm config set proxy`, `npm config set https-proxy`, `npm config set registry`

## Node.jsプロジェクトの作成

`npm init`, `npm install vuepress`

package.json, directory structure

## VuePressサイトの基本設定

config.js

## MarkdownからHTMLを生成

`npm run build` -> `vuepress build src`

`npm run dev` -> `vuepress dev src`

## CSSオーバーロード

Stylus: override.styl, style.styl

Prism.js: prism-tomorrow.css -> prism.css, prism-coy.css is bad

## 記事のページ設定

YAML Front Matter: home, footer

Vue.js: Home.vue, Page.vue

## テーマカスタマイズ

`npx vuepress eject`

edit template: add footer of page to Page.vue

edit template and script: add home-link of site to Navbar.vue

## Webpack設定変更

remove chunkhash from file name

configureWebpack: js

chainWebpack: extract-css

## VuePress改造

disableRenderScripts: build.js, app/index.ssr.html

## コードフォーマッター

`npm run format` -> `prettier --write "doc/**/*.{html,css}"`

## まとめ

大げさな気がしないでもない
