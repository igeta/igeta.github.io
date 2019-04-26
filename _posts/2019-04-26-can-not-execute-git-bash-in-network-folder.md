---
layout: post
title: ネットワークフォルダーのgit-bashが起動できない件について
date: 2019-04-26 01:10:00 +0900
---

Git for Windows / PortableGitの話です。誰も好き好んでネットワーク上の共有フォルダーに `git-bash.exe` を置きたいわけじゃないんですが、やむにやまれぬ事情ってものがあり。

で、表題の通りですが、UNCパス経由で `git-bash.exe` は起動できません。実行してもノーリアクションです。また、ネットワークドライブにマウントしても同じく起動できません。そのくせ、たとえば `\\localhost\c$\path\to\git-bash.exe` などとしてローカル上のexeをあえてネットワークパスから叩いてみると、これまた同様の挙動を見ることができます。なんでや。

解決方法ですが、仮に `Z:\App` 配下に `PortableGit` を置いたとすると、`%windir%\System32\cmd.exe /c "set MSYSTEM=MINGW64& start "Z:\App\PortableGit\usr\bin\mintty.exe" -i "Z:\App\PortableGit\git-bash.exe" /usr/bin/bash --login"` が正解になります。

ショートカットを作成して、［リンク先］に上記コマンドを指定して、［作業フォルダー］には適当なプロジェクト置き場を設定、［実行時の大きさ］は最小化とし、［アイコンの変更…］で `git-bash.exe` のそれを指定しておけばよいでしょう。

`git-bash.exe` なんてどうせGit専用の `mintty.exe` のランチャーでしょと思って、`mintty.exe` を直接実行したら起動できたはいいものの［Shells (bash)］を［MSYS2］か［Mingw-w64 32bit］か［Mingw-w64 64bit］か選ばせるダイアログが出てきてうぜえってなって、ひとまず `git-bash.exe` が普通に動く環境で `uname` してみたら `MINGW64_NT-10.0` と出てきたのでMINGW64なんやなとはわかったけど、だったらと `mintty.exe --help` を確認してもそんなん指定するオプションないやんけって憤慨してひとしきり頭抱えて、ふとそういえばMSYS2のパッケージに起動用バッチが付いてたことに思い当って [msys2_shell.cmd](https://github.com/msys2/MSYS2-packages/blob/master/filesystem/msys2_shell.cmd) を見てみたらそれがすべてで、ていうか環境変数かよ `set MSYSTEM=MINGW64` とかそんなの気付けねえよ、となった次第であります。

ちなみに、`git-cmd.exe` も同じようにダメなんですが、そっちには今のところ用事がないので今回は特に言及なしといたします。あしからずご了承ください。

こちらからは以上です。
