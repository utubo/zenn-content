---
title: "Vimに新しく追加されたtabpanelに非表示のバッファのリストを表示する"
emoji: "📑"
type: "tech"
topics: ["vim"]
published: true
published_at: "2025-05-26 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 5/26 の記事です。
前回の記事は ryoppippi さんによる、 5/23 の[vimラジオのwebpageの目に見えないアプデ情報](https://zenn.dev/ryoppippi/articles/d787bce6dae9c5)という記事でした。
:::

# はじめに

先日ついに追加されたtabpanelでなにかできないかなぁと考え
どのタブのどのウィンドウにも表示されていないバッファも表示したら便利かも？？
と思って実装してみました！
以下画像のHIDDENのリストがそれです

![](/images/20250526-show-hiddens-in-tabpanel/overview.png)

# 実装

ちょっと長いです
このあと解説しますから逃げないでください

```vimscript
vim9script

def BufLabel(b: dict<any>): string
  const current = b.bufnr ==# bufnr('%') ? '>' : ' '
  const mod = !b.changed ? '' : '+'
  const nr = !b.hidden ? '' : $'{b.bufnr}:'
  const name = b.name->fnamemodify(':t') ?? '[No Name]'
  const width = &tabpanelopt
    ->matchstr('\(columns:\)\@<=\d\+') ?? '20'
  return $' {current}{mod}{nr}{name}'
    ->substitute($'\%{width}v.*', '>', '')
enddef

export def TabPanel(): string
  var label = [$'{g:actual_curtabpage}']
  for b in tabpagebuflist(g:actual_curtabpage)
    label->add(b->getbufinfo()[0]->BufLabel())
  endfor

  # Show Hiddens
  if g:actual_curtabpage ==# tabpagenr('$')
    const hiddens = getbufinfo({ buflisted: 1 })
      ->filter((_, v) => v.hidden)
    if !!hiddens
      label->add('%#TabPanel#Hidden')
      for h in hiddens
        label->add($'%#TabPanel#{h->BufLabel()}')
      endfor
    endif
  endif

  return label->join("\n")
enddef

set tabpanel=%!vimrc#tabpanel#TabPanel()

augroup showbuffers_in_tabpanel
  autocmd!
  autocmd BufDelete * autocmd SafeState * ++once redrawtabp
augroup END
```

# 解説

今回のメインは非表示のバッファのリストを表示するところなので
他の箇所は簡単に流しますね

## `BufLabel(b: dict<any>): string`
`getbufinfo()`の結果を元に表示する文字列を返します
ポイントとしては、以下のように
長い文字列の場合に先頭ではなく末尾を省略するようにしているところ
ですかね
```vimscript
const width = &tabpanelopt
  ->matchstr('\(columns:\)\@<=\d\+') ?? '20'
return $' {current}{mod}{nr}{name}'
  ->substitute($'\%{width}v.*', '>', '')
```
tabpanelの幅は`&tabpanelopt`の`columns`で指定されています
指定がない場合は`20`です(`h: tabpanelopt`)
全角文字などを考慮し指定のVirtual column幅以降を`>`へ置換しています (`h: regex`)

また、非表示のバッファは`:b`でアクセスし易いようにbufnrを表示するようにしてみました

## `TabPanel(): string`
タブパネル全体の内容を返します
前4行は普通にタブの情報を表示しています
5行目以降でバッファのリストを表示しています
タブのリストの下に表示したいので、最後のタブの情報に追加する形です
最後のタブが選択中の場合に追加したバッファのリストもハイライトされてしまうのを`%#TabPanel#`によって防いでいます

## `tabpanel`への設定
私はこの関数を`~/.vim/autoload/vimrc/tabpanel.vim`に定義しているので
以下のように設定しています
```vimscript
set tabpanel=%!vimrc#tabpanel#TabPanel()
```
個人々々の環境にあわせて設定してください
(`g:MyTabPanel()`とかにしちゃっても良い気もする…)

## BufDeleteへの対応
非表示のバッファを`:bd`などでこっそり閉じた場合に即座にtabpanelを更新させるため ~~`&showtabpanel`を再セットしています~~ `redrawtabp`で再描画しています
`SafeState`を挟んでいるのは`BufDelete`のタイミングではまだバッファが削除されていない(そして`<abuf>`を見るのが面倒くさい)からです
(`redrawtabp`の情報はthincaさんから🙇)
```vimscript
augroup show_hiddens_in_tabpanel
  autocmd!
  autocmd BufDelete * autocmd SafeState * ++once redrawtabp
augroup END
```

(余談)`SafeState`にモヤる場合はこう書いたら納得するかな…

```vimscript
augroup show_hiddens_in_tabpanel
  autocmd!
  autocmd User BufDeletePost redrawtabp
  autocmd BufDelete * autocmd SafeState * ++once doautocmd User BufDeletePost
augroup END
```

# おわりに
いかがだったでしょうか
今回の実装は簡単なものです
色をかえたり、アイコンを表示したりと色々カスタマイズしてもいいかもしれません

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 5/26 の記事です。
:::
<!-- 次回の記事は TODO さんによる、5/28 の[TODO](TODO)です。🏃 -->

