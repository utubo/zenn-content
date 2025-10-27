---
title: "Vimのポップアップウインドウを使ってregisters.nvimっぽいプラグインを作った"
emoji: "📋"
type: "tech"
topics:
  - "vim"
  - "vimplugin"
published: true
published_at: "2022-07-26 21:57"
---

# registers.nvim

普段Vimを使っているのですが、レジスタにヤンクした値を忘れがちなので手がないか検索したところ
素敵なプラグインにめぐりあいました

https://github.com/tversteeg/registers.nvim

ノーマルモードで`"`をタイプしたりすると以下のようなポップアップが表示されるみたいです
素敵すぎる…
```
"" aaaa
"0 bbbb
"1 ccccccc
"2 zzz
…
```
……nvim用じゃあないか！

# できました

https://github.com/utubo/vim-registers-lite

README.mdの画像だけ見てVimでそれっぽいのをつくりました
自分用なのでVim9 scriptです
逆にnvimで動かない
(nvimは入れてないのにVimだけ新しくしてる人少なそう…需要…)
ともかくこれでどこにyankされてたっけ？って迷わずにすみます
registers.nvimに感謝🙏


# ポップアップウインドウのハマりポイント

プラグインを作成するに当たりハマりポイントがあったので記載しておきます
今回、popup_menuのようにカーソルを移動させて項目を選べるようにしたのですが
ポップアップウインドウが下にはみ出る場合(ステータスラインに到達する場合)、
スクロールが正常に動きません。
以下の手順で確認できます
```vim
:call popup_menu(split(join(range(1,100), ','),','),{})
```

これを回避するために概ね以下のようにpopup_create-argumentに設定するようにしました
```
" 現在の画面上のカーソル行を取得
var winline = win_screenpos(0)[0] + winline() - 1
(…省略)…
" popup_atcursorの第2引数Dictに以下を設定
'maxheight':(winline <= &lines / 2 + 1) ? &lines - winline - 2 : &lines
```
`popup_atcursor()`で表示位置が不定なため面倒なコードになってます
`popup_create()`や`popup_menu()`なら`'maxheight': &lines - 2`でいいんじゃないかなぁと思います

issue投げたほうがいいのかなー？

# LICENSEについて

クリーンルームで作ったので多分コードは本家と全く異なりますが、アイデアの模倣なので一応コピーレフトにしたがってGPL3にしています
法的にはアイデアだけならGPLの対象外らしいですがモラルというか何より素晴らしいアイデアに感謝しているので…
