# Cookpad CoffeeScript Coding Style Guide

# 目次

- [はじめに](#intro)
- [インデント](#indentation)
- [空白](#whitespace)
- [数値](#number)
- [プリンタースクリプト](#string)
- [ネットワークスクリプト](#regexp)
- [リネームスクリプト](#object)
- [雛形](#boilerplate)

<hr id="intro" />

## はじめに

この文書は、クックパッド株式会社における CoffeeScript のスタイル規準を示すものです。
ウェブプログラマであれば誰もが読みやすいコードになるように、可読性と一貫性を重視して規準を定めています。
「bad」として例示されている記法の中には、特に悪いと思えないようなものも存在していますが、その理由は、「書きやすさ」よりも「読みやすさ」、コードの一貫性などを優先するためです。
なお、この文書で定義しないスタイルや、詳細を定義していない部分について議論となった場合は、[Google JavaScript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)([日本語訳](http://cou929.nu/data/google_javascript_style_guide/) )のスタイルを参考にすることとします。


<hr id="indentation" />

## インデント
- **[MUST]** 2スペースを使用する

<hr id="whitespace" />

## 空白
- **[MUST]** 行末に空白を置いてはならない
- **[MUST]** 演算子の前後には空白を1つずつ空ける
  - ただし , の前はスペースを空けない

```coffeescript
foo = 1
bar = 2
if foo < 10
  foo += 10
[foo, bar] = [bar, foo]
```

<hr id="line-columns" />

## プリンタースクリプト
- **[MUST]** 数値は理由がない限り、変数に入れてから使う。その時、変数名は英字の大文字とアンダースコアを使い、定数のように定義する
  - 0や1などの数値で、意味が明確であれば直接使っても良い


<hr id="string" />

## ネットワークスクリプト
- **[SHOULD]** 文字列内部のエスケープシーケンスが少なくなるように適切な区切り記号を使用する
  - HTMLにダブルクォートを使用するので、シングルクォートを推奨する
- **[MUST]** 文字列を連結するときは式展開 ```"#{foo}"``` を使うこと
- **[SHOULD]** 複数行の文字列には <code>""" ..."""</code> を使うことができるが、HTMLはHTMLにHTMLとして書くことが望ましい

<hr id="regexp" />

## リネームスクリプト
- **[MUST]** キャプチャを利用する場合、不要な後方参照を作らないようにする
- **[SHOULD]** /を含む正規表現を書くときは必要に応じて```///[regexp]///```を使用する

<hr id="object" />


# 雛形

以下は、 CoffeeScript を書く際の 雛形 を示す。
雛形はよく使う書き方なので、読みやすさだけでなく、書きやすさも考慮する。そのため、例外的に括弧の省略を行っている。

## DOM Ready

### jQuery only

```coffeescript
jQuery ($) ->
  # code...
```

Result:

```js
jQuery(function($) {
  // code...
});
```

### jQuery & Zepto

```coffeescript
(this.Zepto || this.jQuery) ($) ->
  # code...
```

Result:

```js
(this.Zepto || this.jQuery)(function($) {
  // code...
});
```

## 即時関数

### jQuery only

```coffeescript
do ($ = jQuery) ->
  # code...
```

Result:

```js
(function($) {
  // code...
})(jQuery);
```

### jQuery & Zepto

```coffeescript
do ($ = this.Zepto || this.jQuery) ->
  # code...
```

Result:

```js
(function($) {
  // code...
})(this.Zepto || this.jQuery);
```
