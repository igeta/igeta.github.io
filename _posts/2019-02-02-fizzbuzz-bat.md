---
layout: post
title: WindowsバッチファイルでFizzBuzzワンライナー
date: 2019-02-02 17:20:00 +0900
---

需要のないネタですが、かつてないバッチ力の高まりを感じたので。

## ソースコード

209Byte。コード末尾にスペースが1つ入ってますので、ご注意ください。

```bat
@echo off&setlocal enabledelayedexpansion&set f0=Fizz&set b0=Buzz&for /L %%i in (1,1,100) do set/a f=%%i%%3&set/a b=%%i%%5&call set s=%%f!f!%%%%b!b!%% %%i&for /f tokens^=1 %%j in ('echo !s!') do set/p<nul=%%j 
```

出力結果はこちら。

```
1 2 Fizz 4 Buzz Fizz 7 8 Fizz Buzz 11 Fizz 13 14 FizzBuzz 16 17 Fizz 19 Buzz Fizz 22 23 Fizz Buzz 26 Fizz 28 29 FizzBuzz 31 32 Fizz 34 Buzz Fizz 37 38 Fizz Buzz 41 Fizz 43 44 FizzBuzz 46 47 Fizz 49 Buzz Fizz 52 53 Fizz Buzz 56 Fizz 58 59 FizzBuzz 61 62 Fizz 64 Buzz Fizz 67 68 Fizz Buzz 71 Fizz 73 74 FizzBuzz 76 77 Fizz 79 Buzz Fizz 82 83 Fizz Buzz 86 Fizz 88 89 FizzBuzz 91 92 Fizz 94 Buzz Fizz 97 98 Fizz Buzz 
```

理解の助けとするべく、one-linerとかshort codingとかそういったおもしろ要素を取り払って、あるいは業務クオリティを意識して、しかしアイデアは維持しつつ書くならば、次のようになります。

```bat
@rem
@rem FizzBuzz Question
@rem
@echo off
setlocal

call :fizzbuzz 100

endlocal
exit /b 0

@rem
@rem FizzBuzz Subroutine
@rem
:fizzbuzz
setlocal enabledelayedexpansion
set count=%~1
if "%count%"=="" set count=0

set fizz[0]=Fizz
set buzz[0]=Buzz

for /L %%i in (1,1,%count%) do (
    set /a f=%%i %% 3
    set /a b=%%i %% 5
    call set s=%%fizz[!f!]%%%%buzz[!b!]%% %%i
    for /f "usebackq tokens=1" %%j in (`echo !s!`) do (
        set /p < nul ="%%j "
    )
)
echo.

endlocal
goto :eof
```

## 解説

さて、ポイントです。

### 改行なしで標準出力

`echo` の方が短く書けて、5Byte削れるのでそれでもいいんですが、スペース区切りでだーっと出力させる感じのFizzBuzzをよく見るので、まあそれでなんとなく。`set /p` を使います。

```bat
set /p < nul =echo without new line
```

`set /p` は通常、ユーザー入力を得るためのものですが、標準入力は `nul` をリダイレクトして閉じて、変数名を省略してしまえば、プロンプト文字列の表示が行われるだけとなり、つまり改行なしの `echo` として使用できます。`< nul` の記述位置にぎょっとしますが、こんな箇所にも書けます。

蛇足ですが、`set /p` による標準出力は割と独特なので注意が必要です。`echo` とは違い、ダブルクォーテーションで括った文字列がそれっぽく解釈されたり、先頭スペースが削除（LTrim）されたりします。

### 改行なしでコーディング

通常、複数のコマンドを逐次実行するには改行する必要がありますが、`&` で繋げば1行で書けます。ちなみに、先行コマンドの成功時のみ後続コマンドを実行する場合には `&&` を、失敗時のみの場合には `||` で繋ぎます。`&` は、先行コマンドの成否に関わらず後続コマンドを実行します。

ここで、注意点があります。`for` など複文を取るようなものについて、`do` の後ろに `&` で繋いだコマンドを書くと、それらはすべて `do` の中身として解釈され、知る限りですが、改行せずに `for` の外に出る方法がありません。

`if` も同様です。`else` なしの `if` であればいわゆる `then` 節の、`if else` であれば `else` 節の外に出る方法は、改行以外にありません。むしろ、あったら教えてください。

バッチのワンライナーでは、ネストは深くなる一方で外には出れない、という制約があるわけで、そういうものしか書けません、むしろ無理やりにでもそういう形に持っていけ、というわけです。

### 擬似連想配列

環境変数は多重に展開することができます。一つには、遅延環境変数と通常の環境変数との展開タイミングの違いを利用して、`!hash[%key%]!` のように、遅延環境変数名の一部を通常の環境変数で埋めてやることができます。

また別の方法により、環境変数名の一部を遅延環境変数の展開結果で埋めこともできます。要は、遅延環境変数よりさらに遅く展開される環境変数をどう作り出すか。これは `call` を利用して書くことができ、今回のコードではこちらを使用しています。

```bat
@echo off
setlocal enabledelayedexpansion

set fizz[0]=Fizz
set buzz[0]=Buzz

for /L %%i in (1,1,15) do (
    set /a f=%%i %% 3
    set /a b=%%i %% 5
    call echo %%fizz[!f!]%%%%buzz[!b!]%% %%i
)

endlocal
```

元のFizzBuzzコードでは `call set` としていますが、上記、必要箇所のみ抜き出した例では `call echo` とした箇所がそれです。普通の `call` の使い方ではありませんね。これがどういう動きになるか。

`%%i` は `1` とします。まずは `call` が解釈されて、その引数である `echo %%fizz[!f!]%%%%buzz[!b!]%% %%i` に対して変数展開が行われます。環境変数は、`!f!` と `!b!` と `%%i` ですから、`echo %%fizz[1]%%%%buzz[1]%% 1` になります。またここで、`%%` は `%` のエスケープですから、結局 `echo %fizz[1]%%buzz[1]% 1` に展開されます。

そこからさらに、`echo` の実行前にもまた変数展開が行われます。引数の `%fizz[1]%%buzz[1]% 1` に含まれる環境変数は、`%fizz[1]%` と `%buzz[1]%` ですが、いずれも未定義であり、つまり空です。よって、最終的に `echo  1` となり、これが実行されて ` 1` が標準出力に表示されます。

同様にして、`for /L` のステップごとの環境変数の値の変化をまとめると、次の表のようになります。

| `%%i` | `!f!` | `!b!` | `%%fizz[!f!]%%`     | `%%buzz[!b!]%%`     |
|------:|------:|------:|---------------------|---------------------|
|     1 |     1 |     1 | `%fizz[1]%` => 空   | `%buzz[1]%` => 空   |
|     2 |     2 |     2 | `%fizz[2]%` => 空   | `%buzz[2]%` => 空   |
|     3 |     0 |     3 | `%fizz[0]%` => Fizz | `%buzz[3]%` => 空   |
|     4 |     1 |     4 | `%fizz[1]%` => 空   | `%buzz[4]%` => 空   |
|     5 |     2 |     0 | `%fizz[2]%` => 空   | `%buzz[0]%` => Buzz |
|     : |     : |     : | :                   | :                   |
|    15 |     0 |     0 | `%fizz[0]%` => Fizz | `%buzz[0]%` => Buzz |
|     : |     : |     : | :                   | :                   |

つまり、`%%fizz[!f!]%%` と `%%buzz[!b!]%%` は、剰余をキーとした連想配列のようなものです。それぞれ、キーが `0` の場合のみ `Fizz` か `Buzz` の値を持ち、存在しないキーに対しては空を返します。

処理の流れを押さえたところで、実行結果を見ましょう。下記のようになります。`%%i` は `15` までとして、以降は割愛します。

```
 1
 2
Fizz 3
 4
Buzz 5
Fizz 6
 7
 8
Fizz 9
Buzz 10
 11
Fizz 12
 13
 14
FizzBuzz 15
```

`Fizz` か `Buzz` か `FizzBuzz` か、「特別な文字列」が必要な場合はそれを先に出力して、スペースを挟んで数値のカウントを出力しています。

### 空値の置き換え

前段までで、文字列と数値の組が得られましたと。あとは、文字列が存在する場合はその文字列を返し、文字列が空の場合は続く数値を返せばよい、となります。普通に `for /f` を使います。

```bat
@rem !s! は、「文字列（空の場合あり）」と「数値」のスペース区切りの組
for /f "usebackq tokens=1" %%j in (`echo !s!`) do (
    set /p < nul ="%%j "
)
```

`for /f` は、各行に対していわゆる `split` をしてトークンを取り出しますが、空の要素は切り詰められるので、それを利用しています。要素について、優先順にならべて、不要なものは空にしておけば、あとは `for /f` で回して先頭のトークンを取得すればよい、というわけです。

## まとめ

`if` なしでFizzBuzzできたので満足。
