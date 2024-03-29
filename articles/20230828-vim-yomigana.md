---
title: "mecabを使って漢字をよみがなに変換するプラグインetc."
emoji: "📕"
type: "tech"
topics: ["vim"]
published: true
published_at: "2023-08-28 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 08/28 の記事です。
前回の記事は kuu さんによる、 08/26 の[「cmdwinっぽい物を数行で作る」](https://zenn.dev/vim_jp/articles/20230825_ekiden_cmdwin)という記事でした。
:::

# はじめに
いきさつ
:::details どうでもいいと言う人が多いと思うので折りたたんでおきます
「傀異化」
これ読めますか？変換できますか？
モンスターハンター(以下モンハン)の中で出てくる単語で「かいいか」と読みます。
音読みなので勘でも読めたかとは思いますが、変換すると「怪異化」等になってしまうと思います。
一発で変換できないことに不便を感じたので、Google日本語入力に登録できるモンハンのユーザー辞書を作ってみました。

https://github.com/utubo/mh-dict-jp

ユーザー辞書は単なるtsvファイルなので単語を集めてvimで編集して作ったのですが、
漢字を変換したり変換後に微調整をする必要があったので幾つかプラグインを作成しました。
というわけでプラグイン紹介をします。
:::

以下のプラグインを作ったので紹介します。
- mecabを使って漢字をよみがなに変換するプラグイン
- 濁点や半濁点を切り替えるプラグイン

:::message
- ここで紹介するプラグインはVim9 scriptで書いているのでnvimでは動きません
:::

# mecabを使って漢字をよみがなに変換するプラグイン

https://github.com/utubo/vim-yomigana

mecabを利用して漢字をよみがなに変換します。(事前にmecabのインストールが必要です)
```
漢字をよみがなに変換します。
```

上記の行にカーソルを合わせて`<Leader>HH`と打つと

```
かんじをよみがなにへんかんします。
```

と変換できます。
(カタカナにする場合は`<Leader>KK`)

`<Leader>H`と`<Leader>K`はオペレーターとなっているので、
例えば
```
(括弧の中だけ)変換
```

`<Leader>Hi(`と打てば

```
(かっこのなかだけ)変換
```

こんな感じに括弧の中だけ変換できます。
(オペレーターにするアイデアは[kuu](https://zenn.dev/kuu)さんにいただきました感謝🙏)

プラグインで公開している関数を利用すれば置換を使って読みを追加できたりします。
(ちょっと面倒かな…？)
```
漢字
変換
```
↓
`:%s/.*/\=yomigana#GetHira(submatch(0)).."\t"..submatch(0).."\t名詞"/g`
↓
```
かんじ	漢字	名詞
へんかん	変換	名詞
```

モンハンのユーザー辞書を作ったときは上記のコマンドで半分以上の単語に対応しました。


# 濁点や半濁点を切り替えるプラグイン

https://github.com/utubo/vim-dakuten

このプラグインは`<Leader>d`でカーソル下の濁点や半濁点を切り替えます。
例
```
は→ば→ぱ
```

「一カ月」なんかの文字にも対応するために`カ`は特殊な変換をします
```
カ→ガ→ヵ→ヶ
```

nvimユーザーの人は[dial.nvim](https://github.com/monaqa/dial.nvim)とかで対応できそうです。

# (おまけ)ユーザー辞書作成のためのひな形リポジトリ

https://github.com/utubo/template-dict-jp

モンハンユーザー辞書のリポジトリをより一般化したものです。
タグを更新すると`src`ディレクトリにある辞書ファイルを読み込んで、コメントを除去したものと全てを結合したものを`.output`ディレクトリに吐き出すGit Actionsが組み込んであります。
他のゲームのユーザー辞書を作りたい方はご自由にご利用ください。

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 08/28 の記事です。
次回の記事は tani さんによる、08/30 の[「Artemis: Bridging gap between Vim and Neovim」](https://www.gengo.cc/blog/artemis_bridging_gap.html)です。🏃
:::

