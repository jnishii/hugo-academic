---
title: "Slide by Markdown"
date: 2017-11-20
categories: ["comp"]
tags: ["markdown"]
---

数式を使ったプレゼンを作る時は大抵Keynote+LatexItを使っているが，もう少し簡単に作れないかなと
考え中。
普段のメモではMarkdown形式にしているので，それをそのままスライドにする方法を調べたら，
有名どころでは[reveal.js](https://github.com/hakimel/reveal.js/)と[remark](https://github.com/gnab/remark)があって，どちらもLaTeX記法をMathJax表示できる。
しばらくremark.jsを使ってみることにした。

<!--more-->

## remark vs reveal.js

どちらもMarkdownをhtml化してwebサーバを使って表示する。

### remark

**良いところ**
- スライド作成のための最低限の機能は簡単な方法で実現できる
  - コンテンツを少しづつ表示するには`--`を行間に入れるだけ
  - センタリングは`.center[テキスト]`と書けばOK
- 書式設定のためのシンプルなマクロを簡単に作れる
- 書式設定マクロをMarkdownファイルに記載しても，それほどゴチャゴチャしたかんじにはならない。
- 軽い
- 好きなページにすぐ行ける(スワイプでスライドの高速ページめくり可能)
- URLで表示ページの指定もできる
- インストールが簡単。必要なファイルは1つのみ。

**悪いところ**
- 初期設定のままだと文字サイズとか行間とかいろいろ気になる。調整や作り込みが必要
- Markdownファイルに書式設定のマクロを加えると，他のMarkdownビューアで見た時にそれが表示されてしまう。


### reveal.js

**良いところ**
- テーマが何種類か用意されていて，デフォルトでもそこそこに良いかんじ(文字の大きさや配置等)にスライドが表示される。
- もともとhtml形式でスライドを作るツールなので，htmlで使える書式は大抵使える
- Markdown形式の拡張コマンドはなく，様々なスライド機能(アニメーション指定など)はhtml形式のコメント文を使って指定する。だから，書式指定を加えたMarkdownファイルを他のビューアで見ても，書式設定のための命令は無視されてキレイに表示される。
- ページ区切りは改行2つとか3つとかに指定できる(カスタマイズ可能)ので，表示形式にこだわらなければ，メモ用に作っていたMarkdownファイルを(ほぼ)そのままスライド利用できる。

**悪いところ**
- いろいろなファイルを一式手元に揃えておかないといけない。
  - (必要なファイルはCDNにアクセスするように修正すれば手元のファイルは最低限に抑えることも可能なはずだが，未確認)
- シンプルに書きたいのでMarkdownを使っているのに，htmlでフォーマット指定をするとゴチャゴチャする。
  - コンテンツを少しづつ表示するには`<!-- .element: class="fragment"-->`などと指定する。長くて覚えるのもタイプするのもちょっと嫌。(remarkなら"--"と書くだけ)
- URLで表示ページの指定をすることもできない
  - 再読込のたびに先頭ページになる
- ページめくりにいちいちクリックしないといけない。ページ数が多いと，目的のスライドページに移動するのが面倒
  - (クリックではなくスワイプのみでページ遷移するように設定できるのかもしれないが，見つけられなかった)

### とりあえずの結論
remarkは, Markdownファイルのポータビリティを下げるかわりに，カスタマイズの容易さと簡単にかけることを重視。
reveal.jsはMarkdownファイルをポータビリティは確保する代わりに，記載が面倒。
とりあえず，
- コンテンツ修正後にその結果を表示するためのページ移動がremarkのほうが楽
- 一度マクロを整備すれば書くのもremarkのほうが楽

と思ったので，当面はremarkを使うことにした。

## remarkの使い方

[remark](https://github.com/gnab/remark)から[boilerplate-single.html](https://github.com/gnab/remark/blob/develop/boilerplate-single.html)をダウンロードして，この中にスライドコンテンツをつめこんで，webサーバに置けばよい。このあたりの方法はgoogleで検索するとたくさん出てくるので，ここには書かない。

スライドのコンテンツを複数のファイルに分けたい場合を考えて，設定とコンテンツを分離し，コンテンツを読み込むためのスクリプトを作った。

1. 設定ファイル: `remark.html`
2. スライドコンテンツ: `slides.md` (名前はなんでも良い)
  - Markdown形式で書くコンテンツ
3. 起動スクリプト: `open.sh`
  - 上記のスライドコンテンツと設定ファイルを合体させて表示
  - 発表に使うプレゼンファイルを分割して作りたいとき便利

それぞれこんなかんじ。`reveal.js`でも同様の手法で表示できる。

### 設定ファイルと起動スクリプト

<script src="https://gist.github.com/jnishii/0cd7f5d5d1d536639d14b0804cead510.js"></script>

#### 起動スクリプトについて

- 実行方法:
```
$ ./open.sh <スライドファイル名>
```
- 起動スクリプトがやってること: 設定ファイル`remark.html`の`<<<CONTENTS>>>`の部分を引数で指定したMarkdownファイル名に置き換えて，`_index.html`というファイルを出力し，そのファイルパスをURLに置き換えてブラウザに投げている。
- webサーバ上のパスはそれぞれ適当に書き換える必要あり。
- Macのopenコマンドを使っているので，Linuxではブラウザ名に置き換える等すればよい。

`open.sh`はどこかパスの通ったところに置いといても良い。
設定ファイルもどこか決まったフォルダにおいて，絶対パスを指定して読み込んでも良いが，プレゼン内容に応じて設定を変えたいことが有るかもしれないので，コンテンツファイルと同じフォルダにそれぞれコピーを用意するほうが便利かなと思う。

#### 設定ファイルについて
`remark.html`は，remarkのデフォルトファイルでは`index.html`となっているもの。
インターネット上で拾ったマクロ等を加え，`open.sh`で任意のコンテンツファイルを開けるようにした。

### スライドコンテンツ
コンテンツ例はいろいろとインターネット上にあるので，細かい説明はここには書かないが，以下のような感じになる。
スライドの改ページは`---`で指定する。
ファイル名は適当でよいが，拡張子はお作法に従ってMarkdownおきまりの`.md`にする。

```
# スライド1だよ
## 今日のお話

- その1: 内緒の話
- その2: びっくりする話

---
# スライド2だよ

.center[内緒の話だよ!]

--

## 内緒の話その1

```