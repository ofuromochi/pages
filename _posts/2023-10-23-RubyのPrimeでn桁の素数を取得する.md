---
title: "RubyのPrimeでn桁の素数を取得する"
date: 2023-10-23
---

RubyのPrimeを使って何桁の素数を出すのが目標です。

```ruby
 Prime.each(10) { pp _1 }
2
3
5
7
=> 7
```

`Prime`の`each`は特異メソッドです。`nil`なら無限、整数で素数の上限値を指定します。範囲指定はできなさそうです。

[class Prime (Ruby 3.2 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/class/Prime.html)

使うのは `prime?`メソッドです。

[Prime#prime? (Ruby 3.2 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Prime/i/prime=3f.html)

3桁の素数を配列で取得してみます。

```ruby
numbers = Array(100..999)
=>
[100,
...

numbers.filter { Prime.prime?(_1) }
=>
[101,
 103,
 107,
...
```

`Array`で作った配列のうち `Prime.prime?(i)==true`のみを取り出しています。ただ10桁を取り出そうとすると低速だったので便利に使えるのは少ない数に限られそうです。
