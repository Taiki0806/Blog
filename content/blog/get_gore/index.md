---
title: goreなるものがあるらしい
date: "2020-04-17T14:08:44.000Z"
description: "goreのinstall"
---

# goreのinstall

## 結論

- 以下を実行すればOK！

```zsh
go get -u github.com/motemen/gore/cmd/gore
```

- 起動

```zsh
$ gore -autoimport
gore version 0.5.0  :help for help
gore> fmt.Println(time.Now())
2020-04-17 14:08:44.388842 +0900 JST m=+0.000278630
52
nil
```

## goreなるものがあるらしい

自分は普段業務で主にRubyを利用して開発しているのですが、  
趣味が仕事みたいなもんでここ１年ずっとRuby、Javascriptばっかみてきたので、  
静的型付け言語のGoをやってみたいと思って、最近Goで遊んでます。  

今の会社に入社させて頂く前(1年以上前)にもGo(gin)でBlogアプリみたいなものは作ってみたんですが、  
全然理解できてなかったので、復習がてらまたやろうかなと（本心はミーハーなんで流行りのもの触りたい。。）。

そこで１年前には知らなかったgoreというものを入れてみました。 

github: https://github.com/motemen/gore

goreはRubyのirbみたいなREPLでターミナル上で簡単にGoのプログラムを実行できるというものです。

前はファイル作ってひたすらbuildして実行して試すみたいなことしてたので１年前の自分に教えてあげたい。。

##　let's gore!

- install

試す
```zsh
$ go get -u github.com/motemen/gore
>>> elapsed time 20s

$ gore -autoimport
zsh: correct gore to gcore [nyae]? n
zsh: command not found: gore
```

あれなんで？  
goreがないらしいです。。。

。
。
。

Readme.mdにちゃんと書いてた。

> Installation
> The gore command requires Go tool-chains on runtime, so standalone binary is not distributed.
> 
> `go get -u github.com/motemen/gore/cmd/gore`

引用: https://github.com/motemen/gore

再実行
```zsh
$ go get -u github.com/motemen/gore/cmd/gore
>>> elapsed time 20s

$ gore -autoimport
gore version 0.5.0  :help for help
gore> fmt.Println(time.Now())
2020-04-17 14:08:44.388842 +0900 JST m=+0.000278630
52
nil
```

おおー！
ちゃんと現在時刻が出力されましたね！

ちなみに`-autoimport`オプションをつけるかgore起動したあとに明示的に  
importしてあげないと`undefined`になっちゃうので注意です！

```zsh
$ gore
gore version 0.5.0  :help for help
gore> fmt.Println(time.Now())
undefined: fmt
undefined: time
```

## 学び

- Readmeちゃんと読む

## 参考

- みんなのGo言語（書籍）
- https://github.com/motemen/gore
