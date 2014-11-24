# Programming Elixir

### Programming should be about transforming data

* オブジェクト指向
  * クラスが振る舞いを、インスタンスが状態を持つ
  * Our goal is data-hiding
* We want to get things done, not maintain state
* I don't want to hide data, I want to transform it

> Most programmers treat threads and processes as a necessary evil.
> Elixir developers feel the> y are an important simplification.

### Think Different(ly)

* オブジェクト指向だけがモデリング法ではない
* 関数型は複雑である必要も数学的である必要もない
* プログラミングで大事なのはifとloop
* Concurrency does not need locks, semaphores, monitors, and the like.
* Processes are not necessarily expensive resources.
* Meta-programmingはオマケじゃない
* Even if it is work, programming should be fun. 楽しくあれ

### ガベコレ

* プロセス毎にヒープが用意される
* プロセスが役目を終えればヒープは捨てられ、ガベコレは走らない

### Immutable

`name.capitalize()`は、`name`の内容を変えるのか、ただcapitalizeして返すのか、それとも両方なのか分からない。`String.capitalize(name)`は、`name`は不変であることを保証する。

### Types

#### 整数
区切りアンダースコアOK。大きさ無限。

#### 浮動小数点
1.0  314159.0e-5 など。IEEE754のdoubleに相当。精度約16桁、マックス10の308乗くらい。

#### アトム
[A-Za-z0-9_@]が使える。クオートすれば何でもあり。でも`:>>>`とかイケるよね。

#### 範囲
`1..5`

#### 正規表現
`~r{regexp}`でつくる。Perl5互換。デリミタをスラッシュにするのは実際よくない。

#### PID
プロセスへの参照

#### Port
リソースへの参照。一般にEerlang VMの外。

#### Reference
グローバルに一意なやつ。

#### タプル
順序付きコレクション。もち編集不可。要素が4つを超えたらそろそろ辛い。

#### リスト
Linked list. なので先頭から舐めるのは得意、ランダムアクセスは不得意。

##### キーワードリスト
key/valueのペアのリスト。ランダムアクセスは不得意。
`[name: "John", age: 25]`は、`[{:name, "John"}, {:age, 25}]`に等しい。

#### マップ
`%{"name" => "John", :age => 25}`. 順番は保持されない。アクセスは速い。キーと値の型はなんでもあり。オススメしないけど。順番に意味が無かったりするなら、キーワードリストよりこっち使うべき。キーがアトムなら、`map.name`ってアクセスできる。

#### バイナリ
ビット列を読むときとかに使う。何も指定しないと1byteの列で解釈される。

```
iex> bin = <<1, 2>>
<<1, 2>>
iex> byte_size(bin)
2
iex> <<3::size(2), 5::size(4)>>
<<53::size(6)>>
```

#### バイナリオペレータ
`and`, `or`, `not`は`true`か`false`を期待している。`||`, `&&`, `!`は、来た値をtruthyかfalsyか判断する。