---
title: インスタンス変数の隠蔽
date: "2020-07-26T19:26:44.000Z"
description: "オブジェクト指向設計実践ガイドを読んだ（第２章）"
---

# 同クラス内で定義したインスタンス変数も隠蔽する

実務でインスタンス変数を直接参照して、  
状態を頻繁に変更する実装をちょこちょこみていて、  
テストしにくい気がしていたのと違和感を感じていたのですが  
オブジェクト指向設計実践ガイドにインスタンス変数も隠蔽しなさいって書いてあったので  
ちょうどいいと思って備忘録として書きました。

## 結論

- インスタンス変数をラップするメソッドを用意する
- attr_readerを利用する

```Ruby
  # bad
  class Gear
    def initialize(chainring, cog)
      @chainring = chainring
      @cog = cog
    end

    def ratio
      @chainring / @cog.to_f  # ← bad！
    end
  end

  # good
  class Gear
    attr_reader :chainring, :cog
    def initialize(chainring, cog)
      @chainring = chainring
      @cog = cog
    end

    def ratio
      chainring / cog.to_f
    end
  end
```

## インスタンス変数の隠蔽

- インスタンス変数は常にアクセサメソッドで包み、直接参照しないようにしたほうがいいい。
- 変数はそれらを定義しているクラスからも隠蔽する
- `attr_reader`を使うと、Rubyは自動でインスタンス変数用の以下のようなラッパーメソッドを作る。

```Ruby
  # e.g.)
  def cog
    @cog
  end
```

- このcogメソッドはコード内で唯一cogが何を意味するのかをわかっている箇所になる
  - コグはメッセージの戻り値となる。
  - このメソッドの実装によって、コグはデータ（どこからでも参照されるもの）から振る舞い（一箇所で定義される）へと変わる

- メリット
  - @cogインスタンス変数が10箇所で参照されているとする。
  - そこで突然@cogインスタンス変数を修正する必要が生じた場合は何箇所ものコードを変更する必要が出てくる。
  - しかし@cogがメソッドで包まれていれば、コグが意味するものを変えてしまえる。コグについて独自の実装をすればよい。(cogの意味はcogメソッドのみが知っている)

```Ruby
  # e.g.）
  # 下のようなインスタンス変数の修正が必要になった複数箇所にするのは大変だしロジックの見通しが悪くなる。
  # なのでこの例のようにラップしたメソッドのなかにcogの内容を実装する（cogの意味を知っているのはcogメソッドのみという隠蔽の状態をつくりだせている）
  def cog
    @cog * (foo? ? bar_adjustment : baz_adjustment)
  end
```

- Q
  - initialize内でロジックを変えてデータに代入する形で十分じゃない？

```Ruby
  # e.g.) これでよくない？
  def initialize(chainring, cog)
    @chainring = chainring
    @cog = cog * (foo? ? bar_adjustment : baz_adjustment)
  end
```

- A
  - No！
  - コンストラクタの責務を超えてしまっている（単一責任でない）し見通しが悪くなる。
  - このクラスのcogの意味そのものが変わるならcogメソッドで振る舞いを変えればいい。（cog以外がcogの意味を知るべきでない。）

## 参考

- オブジェクト指向設計実践ガイド
