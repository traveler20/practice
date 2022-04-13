# LP をつくって学ぶ Sass×FLOCSS

CSS のメタ言語である Sass や CSS 設計で注目されている FLOCSS を用いてランディングページを制作するチュートリアルのソースコードです。

## デモサイト

https://traveler20.github.io/practice/00/

## Zenn（無料書籍）

https://zenn.dev/yurukei20/books/10e7322a762178

## GitHub

[Lib Sass](https://github.com/traveler20/practice/tree/654f38747740d577913cadf393816e52891e0b67/00)

[Dart Sass 移行後のコード](https://github.com/traveler20/practice/tree/master/00)

---

今回使用した Sass の書き方は Lib Sass で、将来的に`@import`が使うことができなくなるとのこと。
今後は Dart Sass を用いた Sass の書き方がスタンダードになるので、今回作成したランディングページについても Dart Sass へ移行していこうと思います。
（※お手数おかけして申し訳ないです…。）

# DartJS Sass Compiler and Sass Watcher 　のインストール

Dart Sass は基本的に gulp を用いることが多かったですが、Dart Sass が利用できる VSCode の拡張機能も充実してきました。

今回は、DartJS Sass Compiler and Sass Watcher 　を使用します。

VSCode の拡張機能で`DartJS Sass Compiler and Sass Watcher `を検索して有効化しましょう。

CSS のコンパイル先をあわせるために、VSCode の`setting.json`も以下のように書きます。

```json:.vscode/setting.json
{
	"dartsass.targetDirectory": "asset/css"
}
```

# @import を @use に書き換える

まずはわかりやすい`style.scss`を修正します。

`@import`で各 Sass ファイルをインポートしているのを`@use`に書き換えます。

```scss:style.scss

```

ただ、このままだと変数などの書き方が Dart Sass に沿っていないのでエラーが発生します。

なので、一旦全てコメントアウトして、段階的に Dart Sass の書き方を反映させていきます。

:::message
エラーが気にならないからといって一気に Dart Sass に移行するのもおすすめではありません。
段階的に移行することで、どこのファイルのどの部分が Dart Sass に移行できていないかなどが明確になるので。
:::

`_base.scss`は変数を使用していないので、まずは`_base.scss`から反映させましょう。

```scss:style.scss

```

# 変数を一括で管理する

次に、変数の管理を整理しましょう。

今回の LP では、`_base.scss`と`_color.scss`で変数を管理していました。

Dart Sass の場合、変数を別ファイルで使用するには各 Sass ファイルで`@use "./../../foundation/color"`,`@use "./../../foundation/base"`を書く必要があります。

2 ファイル全部書くのも面倒なので、`_variable.scss`という Sass ファイルをつくって変数を一括管理・一括利用していきます。

```scss:foundation/_variable.scss

```

では、このファイルをもとに、各 Sass ファイルのほうも編集していきましょう。

# Sass ファイルを編集する

まずはテスト的に`_header.scss`を Dart Sass 用に編集しましょう。

最初に、`style.scss`で`_variable.scss`を`@use`でインポートします。

```scss:style.scss

```

次に、`_header.scss`で変数が扱えるようにしていきます。
まずは`_header.scss`で`_variable.scss`を使用できるよう`@use`を用います。

```scss:_header.scss

```

`@use "./../foundation/variable";`だけでも使えますが、変数を扱いやすいように`@use "./../foundation/variable" as v;`とします。

そうすると、変数を利用している箇所で、`v.$color-base`という書き方で利用できるようになります。

コードとしては以下のようになります。

```scss:_header.scss

```

これを`style.scss`で`@use`を用いればコンパイルされます。

# 各ファイルを編集する

あとは、`_header.scss`と同様に`@use`や変数をいじれば大丈夫です。

編集するファイルは変数を使っている Sass ファイルとメディアクエリを用いている Sass ファイルなので、以下になります。
（`_inner.scss`と`_display.scss`以外ですね。）

- `_header.scss`
- `_footer.scss`
- `_button.scss`
- `_section.scss`
- `_cv.scss`
- `_faq.scss`
- `_fv.scss`
- `_solution.scss`
- `_theme.scss`

最終的なソースコードは GitHub にあげているので参照ください。
