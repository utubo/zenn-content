---
title: "VimでTextObjの末尾や先頭に移動する"
emoji: "🏎️"
type: "tech"
topics: ["vim"]
published: true
published_at: "2024-12-20 00:00"
---

:::message
この記事は[Vim Advent Calendar 2024](https://adventar.org/calendars/10040) と[Vim 駅伝](https://vim-jp.org/ekiden/) の 12/20 の記事です。
Vim駅伝の前回の記事は kamecha さんによる、 12/18 の[ちょっと面倒だなぁ～ってテキスト編集をなんとかする使い捨て十徳ナイフの作り方](TODO)という記事でした。
:::

# 何ができるようになるか？
ノーマルモードにて`g` + テキストオブジェクトで
テキストオブジェクトの先頭や末尾に移動できるようになります！
例えばテキストオブジェクト`t`はHTMLタグなので、
`█`がカーソル位置として`gIt`で以下の様に移動できるようになります。
これが
```html
<div>
  foo
  bar
  ...
  baz█
</div>
```

↓こう

```html
<div>
  █oo
  bar
  ...
  baz
</div>
```

`%`と違い任意のテキストオブジェクトを指定できるので便利！
…たぶん便利！

# 実装
こんな感じでいけます。
- テキストオブジェクトの末尾に移動
  ```vim
  function! s:ToTail(_)
    call getpos("']")->setpos('.')
  endfunction
  noremap <SID>(ToTail) <ScriptCmd>set operatorfunc=<SID>ToTail<CR>g@
  nmap ga <SID>(ToTail)a
  nmap gi <SID>(ToTail)i
  ```

- テキストオブジェクトの先頭に移動
  ```vim
  function! s:ToHead(_)
    let p = getpos("'[")
    call setpos('.', p)
    if p[2] <= 1
      normal ^
    elseif getline(p[1])->len() <= p[2]
      normal j^
    endif
  endfunction
  noremap <SID>(ToHead) <ScriptCmd>set operatorfunc=<SID>ToHead<CR>g@
  nmap gA <SID>(ToHead)a
  nmap gI <SID>(ToHead)i
  ```

# 解説

`ga`、`gi`でテキストオブジェクトの末尾、`gA`、`gI`で先頭に移動します。

## 実装について
### `s:ToTail()`
テキストオブジェクトの末尾にカーソルをセットしています。
これだけで書けちゃうんですね。

### `s:ToHead()`
先頭への移動はちょっと微調整しています。
- インデントを無視したい場面が多かったので`normal ^`で位置調整してます。
- 同様に`gI`での移動で行末に移動した場合も次の行の先頭へ位置調整してます。

## マッピングについて
テキストオブジェクトを扱うときは`operatorfunc`を使いますが、単純に
```
noremap g <ScriptCmd>set operatorfunc=<SID>ToHead<CR>g@
```
としてしまうと、`g`で始まるマッピングが軒並使えなくなってしまいます！
なので、スクリプトローカルな`<SID>(ToTail)`と`<SID>(ToHead)`を定義し回避しています。

### デフォルトのマッピングについて
- `ga`は使う頻度は少ないと思いますが、残したいなら`<Leader>ga`とかに退避させましょう
- `gi`は`g;i`で代用できます

###
`Ga`、`Gi`、`ga`、`gi`にマッピングして`G`は`GG`に退避するのもありかなぁ🤔
そしたら`G`を他のマッピングのprefixにも使えそう
`gg`に対しての`GG`も直感的な気がする

# 追伸
今日12/20は私の誕生日です！わーい！🎂ฅ⊏'ꣲ'|ฅ

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 12/20 の記事です。
次回の記事は tadashi-aikawa さんによる、12/23 の[2024年 Neovim成長日記](TODO)です。🏃
:::

