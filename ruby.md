# ruby

## rbenv

### インストール

```
% git clone https://github.com/rbenv/rbenv.git ~/.rbenv
% cd ~/.rbenv && src/configure && make -C src
% echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc.local
% ~/.rbenv/bin/rbenv init
% echo 'eval "$(rbenv init -)"' >> ~/.zshrc.local
% . ~/.zshrc.local
% curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
% rbenv git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
% git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
% rbenv install -l |& less
% rbenv install 2.5.0
```

### ruby-buildのアップグレード

```
% cd "$(rbenv root)"/plugins/ruby-build && git pull
```

## rubyの基本

### チートシート

* class ClassName
* module ModuleName
* local_variable
* CONSTANT_VARIABLE
* $global_variable
* @instance_variable
* @@class_variable
* def initialize
* def instance_method
* def self.class_method
* class SubClass < SuperClass   # 継承
* #{var}                        # 式展開
* :symbol
* EOS                         # ヒアドキュメント
```
str = <<EOS
foo
bar
baz
EOS
```
* -EOS                        # ヒアドキュメント (終端の識別子をインデントできる
```
str = <<-EOS
foo
bar
baz
  EOS
```
* 'EOS'                       # ヒアドキュメント (展開を無効に)

### %記法

* %q()                    # 式展開とバックスラッシュ記法は無効 (シングルクオートと同等。但し、エスケープシーケンスは不要)
* %() or %Q()             # 式展開とバックスラッシュ記法が有効 (ダブルクオートと同等。但し、エスケープシーケンスは不要)
* %w(Alice Bob Carol)     # 配列の%記法 (["Alice", "Bob", "Carol"])
* %w(foo\ bar hoge\ fuga) # 空白を含む配列の%記法 (["foo bar", "hoge fuga"])
* %i(Alice Bob Carol)     # シンボルの%記法 ([:Alice, :Bob, :Carol])
* %W or %I                # 式展開およびバックスラッシュ記法が有効
* %r{/usr/bin}            # 正規表現 (/\/usr\/bin/)
* %記法の括弧             # %q{}, %q<>, %q[], %q||, %q!!, %q** なども使える

### 主な組み込みクラス

* Numeric
* String
* Symbol
* Array     # ['foo', 'bar', 'baz']
* Hash      # {"key" => "val"}, {:key => "val"}, {key: "val"}
* Range     # (1..5), (1...5) // ..は5を含む、...は5を含まない
  ```
  # Rangeには日時は文字列も指定できる e.g. 日時の範囲 (Time.atはUNIXTIMEからTimeオブジェクトを生成する)
  vacation = Time.at(1234567890)..Time.at(1234567899)
  vacation.begin  # => 2009-02-14 08:31:30 +0900
  vacation.end    # => 2009-02-14 08:31:39 +0900

  # 整数や文字の範囲はRange#eachにより繰り返し処理が可能
  abc = ('a'..'c')
  abc.each do |c|
    puts c
  end
  ```
* Regexp    # /pattern/
  ```
  # 0から9までの数の並びにマッチする正規表現
  pattern = /[0-9]+/

  # マッチしたかを真偽値で返す (===)
  pattern === 'HAL 9000'      # => true
  pattern === 'Space Odyssey' # => false

  # 最初にマッチした位置を返す
  pattern =~ 'HAL 9000'       # => 4
  pattern =~ 'Space Odyssey'  # => nil

  # リテラルの中では式展開が使える
  name = 'alice'
  /hello, #{name}/  # => /hello, alice/

  # %記法
  %r{/usr/bin}     # => /\/usr\/bin/
  ```
* Proc (手続きオブジェクト)
  ```
  greeter = Proc.new {|name|
    "Hello, #{name}!"
  }
  greeter.call 'Proc'   # => "Hello, Proc!"
  greeter.call 'Ruby'   # => "Hello, Ruby!"

  # 手続きオブジェクトはprocメソッド、lambdaメソッド、->記法などを用いて短く書ける
  by_proc    = proc {|name| "Hello, #{name}!"}
  by_lambda  = lambda {|name| "Hello, #{name}!"}
  by_literal = ->(name) { "Hello, #{name}!" }
  ```
* 多重代入
  ```
  a, b    = 1, 2        # a = 1, b = 2
  a, b    = [1, 2, 3]   # a = 1, b = 2
  a, *b   = [1, 2, 3]   # a = 1, b = [2, 3]
  a, b, c = [1, 2]      # a = 1, b = 2, c = nil

  a, b = ['a', 'b']
  a, b = b, a           # a = 'b', b = 'a' (変数の交換)
  ```
* 例外
  ```
  begin
    1 / 0
  rescue ZeroDivisionError
    # 例外処理を記述
    push 'ZeroDivisionError occurred.'
    :
  end
  ```
* 外部ファイルの読み込み (require)
  ```
  require '/path/to/library.rb'   # /path/to/library.rbを読み込む
  require './library'             # rubyの実行されているディレクトリにあるlibrary.rbを読み込む
  require 'library'               # $LOAD_PATHからlibrary.rbを探して読み込む
  ```
* 組み込み変数
  ```
  $stdout, $>
  $stderr
  $stdin
  $SAFE
  $!
  $@
  $$
  $?
  $LOADED_FEATURES, $"
  $~
  $1, $2, $n...
  $+
  $&
  $'
  $\*
  $/, $-O
  $;, $-F
  $:, $LOAD_PATH, $-I
  $DEBUG
  $VERBOSE
  $PROGRAM_NAME
  $<
  $FILENAME
  $\_
  ```


#### 5-2-1 基本的な振る舞い

* empty?
* length, size, bytesize
* include?
* start_with?
* + (連結), * (繰り返し), % (フォーマット文字列), << (破壊的な文字列の連結)

#### 5-2-2 部分文字列の取得

* slice, slice!, []

#### 5-2-3 文字列の整形

* strip, rstrip, lstrip
* chomp, chop
* squeeze
* downcase, upcase, swapcase, capitalize
* sub, gsub
* strip, strip!
* reverse, reverse!

#### 5-2-4 配列への変換

* split

#### 5-2-5 繰り返し処理

* each_char
* each_byte
* each_line

#### 5-2-6 エンコーディングの扱い

* encoding, encode, encode!
```
> str = 'こんにちは'                      # => "こんにちは"
> str.encoding                            # => #<Encoding:UTF-8>
> new_str = str.encode(Encoding::EUC_JP)  # => "\x{A4B3}\x{A4F3}\x{A4CB}\x{A4C1}\x{A4CF}"
> new_str.encoding                        # => #<Encoding:EUC-JP>

> str.encoding                            # => #<Encoding:UTF-8>
> str.encode!(Encoding::EUC_JP)           # => "\x{A4B3}\x{A4F3}\x{A4CB}\x{A4C1}\x{A4CF}"
> str.encoding                            # => #<Encoding:EUC-JP>

# 異なるエンコードの文字列の比較や連結はできない (ASCII互換を除く)
> utf8 =  こんにちは'.encode('UTF-8')     # => "こんにちは"
> eucjp = 'こんにちは'.encode('EUC-JP')   # => "\x{A4B3}\x{A4F3}\x{A4CB}\x{A4C1}\x{A4CF}"
> utf8 == eucjp                           # => false
> utf8.eql?(eucjp)                        # => false
> utf8 + eucjp                            # Encoding::CompatibilityError (incompatible character encodings: UTF-8 and EUC-JP)

> eucjp = 'Hello'.encode('EUC-JP')        # => "Hello"
> utf8 = 'Hello'.encode('UTF-8')          # => "Hello"
> utf16 = 'Hello'.encode('UTF-16')        # => "\uFEFFHello"
> ascii = 'Hello'.encode('ASCII-8BIT')    # => "Hello"
> utf8 == eucjp                           # => true
> utf8 == ascii                           # => true
> utf8 == utf16                           # => false
> utf8 + eucjp                            # => "HelloHello"
> utf8 + ascii                            # => "HelloHello"
> utf8 + utf16                            # Encoding::CompatibilityError (incompatible character encodings: UTF-8 and UTF-16)
```

### 5-3 Regexp

#### 5-3-1 パターンマッチ

```
> /[0-9]/ === 'ruby'    # => false
> /[0-9]/ === 'ruby5'   # => true
> /[0-9]/ =~ 'ruby'     # => nil
> /[0-9]/ =~ 'ruby5'    # => 4

# 正規表現と文字列両方に対応した関数の例
> def alice?(pattern)
>   pattern === 'alice'
> end
> alice?(/Alice/i)  # => true
> alice?('alice')   # => true

# MatchDataの例
> str = 'ruby5'                     # => "ruby5"
> if matched = /[0-9]/.match(str)
>   p matched                       # #<MatchData "5">
> end
> matched[0]                        # => "5"    (マッチした文字列全体)
> matched[1]                        # => nil    (最初の括弧にマッチした文字列)
> matched.pre_match                 # => "ruby" (マッチ文字列よりも手前の文字列)
> matched.post_match                # => ""     (マッチ文字列よりも後ろの文字列)

# MatchDataの例2
> /(あば).+(ばあ)/ =~ '「あばばばばばば、ばあ！」'
> match = Regexp.last_match     # => #<MatchData "あばばばばばば、ばあ" 1:"あば" 2:"ばあ">
> match.pre_match               # => "「"                   (マッチ文字列よりも手前の文字列)
> match[0]                      # => "あばばばばばば、ばあ" (マッチした文字列全体)
> match[1]                      # => "あば"                 (最初の括弧にマッチした文字列)
> match[2]                      # => "ばあ"                 (2番目の括弧にマッチした文字列)
> match[-1]                     # => "ばあ"                 (最後の括弧にマッチした文字列)
> match.post_match              # => "！」"                 (マッチした文字列よりも後の文字列)

# scan (正規表現を引数に受け取り、マッチした文字列の配列を返す。括弧が指定されている場合は配列の配列を返す)
> str = 'Yamazaki Niizaki'                        # => "Yamazaki Niizaki"
> str.scan(/\w+zaki/)                             # => ["Yamazaki", "Niizaki"]
> str.scan(/(\w+)zaki/)                           # => [["Yama"], ["Nii"]]
> str.scan(/\w+zaki/) {|s| puts s.upcase}         # "YAMAZAKI" "NIIZAKI"と順番に表示
> str.scan(/(\w+)zaki/) {|s| puts s[0].upcase}    # "YAMA" "NII"と順番に表示

# escape
> part = Regexp.escape('(incomplete)')    # => "\\(incomplete\\)"
> /[^.]#{part}\.txt/                      # => /[^.]\(incomplete\)\.txt/ (括弧がエスケープされている)
```

#### 5-3-2 文字クラス

* \w, \W, \s, \S, \d, \D, \h

#### 5-3-3 量指定子

* *, +, ?, {m, n} 
```
> pattern = /\d{3}-\d{4}-\d{4}/
> pattern === '080-1234-5678'     # => true
> pattern === '03-1234-5678'      # => false
```

#### 5-3-4 先頭と末尾

```
# \A(文字列の先頭), \z(文字列の末尾)
> pattern = /\A\d{3}-\d{4}-\d{4}\z/               # => /\A\d{3}-\d{4}-\d{4}\z/
> pattern === '080-1234-5678'                     # => true
> pattern === 'Phone: 080-1234-5678'              # => false
> pattern === 'Phone: 080-1234-5678 (mobile)'     # => false

# \A,\zと^(行頭),$(行末)の違い
> lines = "1234\nabcd"      # 改行を含む文字列
> /\A\d+\z/ === lines       # => false
> /^\d+$/ === lines         # => true  (1行目がマッチ)
```

#### 5-3-5 グルーピングと後方参照/部分式呼び出し

```
# ()でグルーピングした箇所は、後で\1, \2というかたちで後方参照できる
> /(B)\ to\ \1/ === 'B to B'      # => true
> $1                              # => "B"  ($1で直前にマッチしたグルーピングの文字列を取得できる)

# ラベル (?<label>pattern)
> /(?<number>[0-9]+)/ === 'abc-123'   # => true
> Regexp.last_match[:number]          # => "123"

# \k<label>で後方参照もできる
> /(?<num>[0-9]+)[a-c\-]+\k<num>/ === '123-abc-123'    # => true

# 部分式呼び出し (\g<n>)
> phone = '080-1234-5678'
> /([0-9]+)-\g<1>-\g<1>/ === phone              # => true   (\g<1>は([0-9]+)に展開される)
> /([0-9]+)-\1-\1/ === phone                    # => false  (後方参照の\1だとマッチした文字そのもの指すためマッチしない
> /(?<num>[0-9]+)-\g<num>-\g<num>/ === phone    # => true   (部分式呼び出しにもラベルを指定できる)
```

#### 5-3-6 先読みと後読み

```
# 先読み「(?=pattern)」、後読み「(?<=pattern)」
> pattern = /(?<=丁目)(\d+)(?=番地)/                      # "丁目<数字>番地"にマッチした文字列の後読み、先読み以外の部分をマッチ文字列とする
> pattern.match('東京都新宿区市ヶ谷左内町21丁目13番地')   # => #<MatchData "13" 1:"13">

# 否定先読み「(?!pattern)」、否定後読み「(?<!pattern)」
> pattern = /(?<!2012)-(?<month_and_day>\d{2}-\d{2})/     # => /(?<!2012)-(?<month_and_day>\d{2}-\d{2})/
> pattern.match('2012-01-01')                             # => nil
> pattern.match('2011-01-01')                             # => #<MatchData "-01-01" month_and_day:"01-01">
```

#### 5-3-7 バックトラックの抑止

```
# バックトラックあり
# (本来は\wで"ruby5'のすべてマッチするのだが、それだと[0-9]にマッチするものがなくなってしまうので、
# Rubyが調整して、まずは[0-9]をマッチさせてから\wで"ruby"をマッチさせている。これをバックトラックと呼ぶ。)
> pattern = /(\w+)[0-9]/      # => /(\w+)[0-9]/
> pattern.match('ruby5')[1]   # => "ruby"

# バックトラック抑止 (?>)
> pattern = /(?>\w+)[0-9]/    # => /(?>\w+)[0-9]/
> pattern.match('ruby5')      # => nil
```

#### 5-3-8 オプションの指定

* i (大文字小文字の区別を行わない)
* o (正規表現リテラルが最初に評価された際、一度だけ式展開を行う)
* x (正規表現の空白を無視し、#をコメントとみなす)
* m (.に開業もマッチする。複数行モード)
* u (正規表現をUTF-8として解釈する)
* s (正規表現をShift-JISとして解釈する)
* e (正規表現をEUC-JPとして解釈する)
* n (正規表現をASCIIとして解釈する)

```
# oオプションの例
> %w(foo bar).map {|str| /#{str}/}    # => [/foo/, /bar/]
> %w(foo bar).map {|str| /#{str}/o}   # => [/foo/, /foo/]

# 特定部分のみオプションを付ける
# 下記の例では、(?i)から(?-i)までiオプションが適用される
> r = /R(?i)uby(?-i)/   # => /R(?i)uby(?-i)/
> r === 'ruby'          # => false
> r === 'Ruby'          # => true
> r === 'RUBY'          # => true
```

### 5-4 Comparable

#### 5-4-1 比較演算

<=>を実装しているクラスにComparableモジュールをインクルードすると、
<, >, <=, >=, ==, !=が使えるようになる。

```
class Ruler
  include Comparable

  attr_accessor :length

  def initialize(len)
    self.length = len
  end

  def <=>(other)
    length <=> other.length
  end
end
```

### 5-5 Enumerable

#### 5-5-1 Enumerableなオブジェクト

##### 繰り返し処理

* each_with_index (インデックス付きのeach)
```
> %w(Alice Bob Charlie).each_with_index do |v, i|
>   puts "#{i}: #{v}"
> end
0: Alice
1: Bob
2: Charlie
=> ["Alice", "Bob", "Charlie"]
```

* reverse_each (末尾から逆順に繰り返す)
```
>> (1..3).reverse_each do |v|
?>   puts v
>> end
3
2
1
```

* each_slice (要素をn個ずつ区切って繰り返す)
```
>> (1..5).each_slice 2 do |a, b|
?>   p [a, b]
>> end
[1, 2]
[3, 4]
[5, nil]
```

* each_cons (n個の連続した要素を1つずつずらしながら繰り返す)
```
>> (1..4).each_cons 2 do |a, b|
?>   p [a, b]
>> end
[1, 2]
[2, 3]
[3, 4]
```

* succを実装しないオブジェクト(Time, Floatなど)はEnumerableできない
```
>> (0.0..1.0).each do |f|
?>   puts f
>> end
TypeError (can't iterate from Float)

>> 0.0.succ
NoMethodError (undefined method `succ' for 0.0:Float)
>>
```

* cycle (レシーバの要素を先頭から末尾まで永遠に繰り返す)
```
>> (1..3).cycle do |v|
?>   puts v
>>   sleep 1
>> end
1
2
3
1
:
```

##### ブロックを評価した結果の配列

* mapまたはcollect (ブロックを評価した結果の配列を返す)
```
>> %w(ruby rails).map {|s| s.upcase}
=> ["RUBY", "RAILS"]
```

##### 要素が特定の条件を満たしているか

* all? (すべての要素が真ならばtrueをかえす)
```
>> [true, true, true].all?    # => true
>> [true, true, false].all?   # => false

# ブロックを渡すことも可能 (none?, any?, one?も同様)
>> [4, 4, 2, 3].all? {|v| v.is_a?(Integer)}         # => true
>> [4, 4, 2, 'three'].all? {|v| v.is_a?(Integer)}   # => false
```

* none? (すべての要素が偽ならばtrueをかえす)
```
>> [false, false, false].none?  # => true
>> [false, false, true].none?   # => false
```

* any? (ひとつでも真の要素があればtrueをかえす)
```
>> [true, true, false].any?     # => true
>> [false, false, false].any?   # => false
```

* one? (一つだけ真であればtrueを返す)
```
>> [true, false, false].one?  # => true
>> [true, true, false].one?   # => false
```

##### 部分的な要素の取得

* grep (引数と要素を===で比較して真となる要素の配列を返す)
```
>> %w(Alice Bob Charlie).grep(/a/i)   # => ["Alice", "Charlie"]
>> ['a', 'b', 3, 4].grep(String)      # => ["a", "b"]

# e.g. Objectオブジェクトから述語メソッドらしきものをgrepする
>> Object.new.methods.grep(/\?/)
=> [:instance_variable_defined?, :instance_of?, :kind_of?, :is_a?, :eql?, :respond_to?, :nil?, :tainted?, :untrusted?, :frozen?, :equal?]
```

* detectまたはfind (ブロックが真になった最初の要素のみを返す)
```
>> [4, 4, 2, 3].find {|v| v.even?}  # => 4
```

* selectまたはfind_all (ブロックが真になったすべての要素を返す)
```
>> [4, 4, 2, 3].select {|v| v.even?}  # => [4, 4, 2]
```

* reject (selectの逆)
```
>> [4, 4, 2, 3].reject {|v| v.even?}  # => [3]
```

* take (先頭から任意の数の要素を配列として返す)
```
>> [1, 2, 3, 4, 5].take(3)
=> [1, 2, 3]
```

* drop (先頭から任意の数の要素をスキップして残りの要素の配列を返す)
```
>> [1, 2, 3, 4, 5].drop(3)
=> [4, 5]
```

* take_while (ブロックが最初に偽を返すまでの要素の配列を返す)
```
>> [1, 2, 3, 4, 5].take_while {|n| n < 3}
=> [1, 2]
```

* drop_while (ブロックが最初に偽を返してからの要素の配列を返す)
```
>> [1, 2, 3, 4, 5].drop_while {|n| n < 3}
=> [3, 4, 5]
```

* injectまたはreduce (畳み込み演算)
```
>> [4, 4, 2, 3].inject(0) {|result, num| result + num}
=> 13

# 初期値は省略できる
>> [4, 4, 2, 3].inject {|result, num| result + num}
=> 13

# メソッド名をシンボルで受け取ることも出来る
>> [4, 4, 2, 3].inject(:+)
=> 13
```

##### 繰り返しとオブジェクトの更新

* each_with_objec
```
>> %w(Alice Bob Charlie).each_with_object({}) do |name, result|
?>   result[name] = name.length
>> end
=> {"Alice"=>5, "Bob"=>3, "Charlie"=>7}
>>
```

##### 要素のグルーピング

* group_by (ブロックを評価した結果の戻り値をキーとしたハッシュを返す)
```
>> [1, 2.0, 3.0, 4].group_by {|v| v.class}
=> {Integer=>[1, 4], Float=>[2.0, 3.0]}
```

* partition (ブロックの戻り値が真か偽かによって、レシーバを2つのグループに分けた新しい配列を返す)
```
>> [1, 2, 3, 4, 5].partition {|v| v.even?}
=> [[2, 4], [1, 3, 5]]
```

##### 最小値と最大値

* min, max, minmax
```
>> (1..10).min
=> 1
>> (1..10).max
=> 10
>> (1..10).minmax
=> [1, 10]
```

* min_by, max_by, minmax_by (ブロックを評価した結果の値で最大・最小を比較する)
```
>> %w(Alice Bob Charlie).min_by {|name| name.length}
=> "Bob"
>> %w(Alice Bob Charlie).max_by {|name| name.length}
=> "Charlie"
>> %w(Alice Bob Charlie).minmax_by {|name| name.length}
=> ["Bob", "Charlie"]
```

##### ソート

* sort, sort_by
```
>> %w(Bob Alice Charlie).sort
=> ["Alice", "Bob", "Charlie"]
>> %w(Bob Alice Charlie).sort {|a, b| a.length <=> b.length}
=> ["Bob", "Alice", "Charlie"]
>> %w(Bob Alice Charlie).sort_by {|name| name.length}
=> ["Bob", "Alice", "Charlie"]
```

#### 5-5-2 Array


