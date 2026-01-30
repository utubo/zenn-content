---
title: "自作Vimプラグインまとめ"
emoji: "⌨"
type: "tech"
topics: ["vim"]
published: true
published_at: "2099-12-31 00:00"
---

自作Vimプラグインのうち使い続けているものを軽く紹介します
環境の都合でほぼVim9 script製で
> Not for you, but for me.
なものばかりです
身勝手の極意

## 早見表
- anypanel
  tabpanelのレイアウトを補助するプラグイン
- colorscheme-girly
  うちのvimが一番kawaii!
- ezpack
  自作プラグインマネージャ
- headtail
  Textobjの先頭や末尾に移動
- hlpairs
  括弧をハイライト強化版
- minviml
  Vim scriptをminify
- popselect
  ポップアップで色々開くやつ
- previewcmd
  コマンド補完
- reformatdate
  <C-a>で日付と曜日をインクリメントとか
- registers-lite
  registers.nvimライクなプラグイン
- skipslash
  `:%s/foo/bar/`のとき<Tab>でfooからbarへ移動
- update
  gvim.exeの最新版をgithubから落とす
- vim9skkp
  Vim9 scriptで作ったskk
- yomigana
  漢字やひらがなをカタカナに変換したり
- zenmode
  cmdheight=0エミュレータ(statuslineも非表示)


## anypanel

https://www.github/utubo/vim-anypanel
https://zenn.dev/utubo/articles/20250602-play-tabpanel

tabpanelのレイアウトを補助するプラグインです


## colorscheme-girly

https://www.github/utubo/vim-colorscheme-girly

うちのvimが一番kawaii!
ピンク基調のカラースキームです


## ezpack

https://www.github/utubo/vim-ezpack

自作プラグインマネージャ
よくある(?)packaddを利用したやつです
Vim9 scriptで作ったやつが欲しいなと思って作りました

## headtail

https://www.github/utubo/vim-headtail
https://zenn.dev/utubo/articles/vim-move-to-head-of-textobj

Textobjの先頭や末尾に移動
(これはあんまり使ってないな…)


## hlpairs

https://www.github/utubo/vim-hlpairs
https://zenn.dev/utubo/articles/7e12d9684cd2af

括弧をハイライト強化版


## minviml

https://www.github/utubo/vim-minviml
https://zenn.dev/utubo/articles/01788284d5e21b

Vim scriptをminify


## popselect

https://www.github/utubo/vim-popselect

ポップアップで色々開くやつ
MRUやバッファの一覧などをさくっと開くプラグインです


## previewcmd

https://www.github/utubo/vim-previewcmd

コマンドをプレビューします
以下のissueを見て作ってみました
https://github.com/vim/vim/issues/18843


## reformatdate

https://www.github/utubo/vim-reformatdate
https://zenn.dev/utubo/articles/6e2d2cfa049414

日付と曜日を<C-a>でインクリメントとかできるようにします


## registers-lite

https://www.github/utubo/vim-registers-lite
https://zenn.dev/utubo/articles/e56fe4b53dda3d

registers.nvimライクなプラグイン


## skipslash

https://www.github/utubo/vim-skipslash

`:%s/foo/bar/`のとき<Tab>でfooからbarへ移動できるようにします


## update

https://www.github/utubo/vim-update

Windowsのgvim.exeを最新に更新する関数を提供します


## vim9skkp

https://www.github/utubo/vim-vim9skkp

Vim9 scriptで作ったSKK日本語入力プラグインです


## yomigana

https://www.github/utubo/vim-yomigana
https://zenn.dev/utubo/articles/20230828-vim-yomigana

MecabやSKKの辞書を利用して、漢字やひらがなをカタカナに変換するプラグインです


## zenmode

https://www.github/utubo/vim-zenmode

cmdheight=0をエミュレートし、statuslineも非表示にします

----
Zennの記事になってないものはそのうち詳しく紹介するかもです


