---
layout: post
title: 書評『PowerShell実践ガイドブック』
date: 2018-06-17 18:15:00 +0900
---

PowerShellが嫌いだと言い続けていたら、なぜかPowerShellの書籍を献本いただけることになった、という話で。

* [マイナビブックス](https://book.mynavi.jp/supportsite/detail/9784839965983.html)
* [Amazon](https://www.amazon.co.jp/dp/4839965986)

PowerShell四天王の一角、[ぎたぱそさん](https://twitter.com/guitarrapc_tech)によるPowerShell Coreの書籍です。シェルだけに貝殻の表紙が印象的な、通称 `#貝殻本` です。

## 総評

Webを探せば、それこそ著者のぎたぱそさんや、四天王の残る3人、[牟田口さん](https://twitter.com/mutaguchi)、[しばたさん](https://twitter.com/stknohg)、[あえとすさん](https://twitter.com/aetos382)をはじめとして、他にも多くの人が情報発信されており、都度必要な情報はだいたい調べられますが、やはりどうしても断片的にならざるを得ません。

予想に反して、PowerShellは難しい言語です。片手間のコピペコードでも、ある程度役に立つものを作れはします。ですが、私の実感として、実務に応じて課題駆動でググって学ぶような断片的な学習では、不用意に罠にはまってトラブルを引き起こしたり、理解不能な挙動に無用な労力を浪費させられることになったりして、あまりいいことはありません。

いや本当に。あるんですよ。シェルであり動的言語であるがために、なんか一見動いているように見えて実は肝心のメイン処理がバグってた、みたいな状況が。PowerShellの独特な言語仕様のために、何がどうしてうまく動かないのかわからず調査に時間をかけたものの結局力業でコード修正した、というような状況が。すごくよくあるんです。というか実際あった。

本書は、著者の実務経験に裏打ちされた確かな知見に基づいて、かなり広範に網羅的な内容が記されており、とりあえず一通り読んでおけば、ちぐはぐな理解のままPowerShellを書いてしまうことに起因する危険は避けられるようになるでしょう。

これ、非常に大事な点です。シェルぐらい誰でも書けるでしょって思ってたら痛い目を見ますよ。マトモなPowerShellを書きたいなら買いましょう。十分おすすめできます。

クロスプラットフォームで動作するようになった新しいPowerShell Coreの本ですが、既存のWindows PowerShellでもほとんどそのまま役立てることができるでしょう。差異がある部分については都度言及されています。それどころか、旧来のコマンドプロンプトや、Linux系のBashとの対比まである始末。筆者の並々ならない思いがうかがい知れますね。

あと、付け加えておくと、本書は教科書的ではありません。読み心地としては、PowerShell大好きマンの先輩が横に付いて実際にコードを動かしながら無駄に丁寧に教えてくれる、というのが近い感覚です。割と最初の方にヘルプの読み方とか教えてくれるあたり、先輩「魚を与えるのではなく、魚の釣り方を教えよ」がポリシーっすか？　とか思いながら。

## すごいところ

とまあ普通の感想を書いたところで。

本書が素晴らしいのは、今このタイミングでPowerShell Coreの本が日本語で読める、ということに尽きるでしょう。

PowerShell Coreとして初のバージョンであるv6.0.0がリリースされたのって、2018年1月20日なわけです。そして、いくつかの修正が行われてv6.0系の安定版と目されるv6.0.2がリリースされたのが、同3月16日。で、ちなみに今はv6.1系がプレビュー段階。

3月16日リリースのv6.0.2に基づいて書かれた書籍が、なんと5月30日に発売されているわけです。リードタイムはわずかに2か月半です。しかも600ページ超の大ボリュームっていう。

これが如何にすごいことか。もちろん、おそらくはv6.0系のプレビュー段階からご準備されていたのだろうと思いますが、このリードタイムで出してくるっていうのは、もう著者の熱意の賜物というほかにありません。読むなら今です。

.NET Coreの登場によって.NETエコシステムが移り行く、その中でPowreShellも変わっていく、まさにその「今」を切り取った、大変熱量のある書籍です。この内容が今読めることに大きな価値がある。そんな感想を抱いております。

## ダメなところ

とはいえ。

圧倒的にダメなところがありまして、まだ説明していない内容を使って説明している、という点です。Bの説明にAを出しているのに、Aの説明が後にある、というやつ。

全体にわたってそういうところが多々あります。逆にそういうポリシーなのかなというくらいに。第1章は何はともあれ触ってみようの章であり、また、第2章冒頭はPowerShellとは何ぞやとヘルプの読み方なので、それは別としても、です。

例えば、「2.3.8 エスケープシーケンス」の後に「2.3.9 文字列」となっていますが、明らかに逆にすべきでしょう。文字列リテラルの中で使う特殊な文字であるエスケープシーケンスをなぜ先に説明するのか。

例えば、「2.3.10 スクリプトブロック」の説明において、コマンドを変数に入れておけば、後から好きなタイミングで呼び出せて便利だよねと説いておいて、変数の説明はそのすぐ後の「2.4 変数」になっています。逆でしょう。

MSDNやMicrosoft Docsではなく書籍が必要になるのは、そうした学習の順序がスムーズな理解に重要だからであり、これが適切でないと読むのが辛いです。気にならない人もいるとは思いますが、私はとても気になるし正直しんどいです。

というわけで、本書の取り組み方としては、わからないことが出てきても大体すぐ後でその説明があるから、いったんそれは置いておいてざくざく読み進める、というのがよいでしょう。

## 最後に

ダメなところについても書きましたが、それでもなお良いところが勝る、というのが率直な感想です。PowerShellを学びたい、何かPowerShellの書籍が欲しいと思っているなら、今は間違いなくコレでしょう。気になっているなら買いですよ、“貝”殻本だけに。
