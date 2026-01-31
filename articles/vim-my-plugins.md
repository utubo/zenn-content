---
title: "自作Vimプラグインまとめ"
emoji: "🧩"
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

※Legacy Vim scriptでも動くものには`(レ)`ってつけときます

- [anypanel](#anypanel)
  tabpanelのレイアウトを補助するプラグイン
- [colorscheme-girly](#colorscheme-girly) (レ)
  うちのvimが一番kawaii!
- [ezpack](#ezpack)
  自作プラグインマネージャ
- [headtail](#headtail) (レ)
  Textobjの先頭や末尾に移動
- [hlpairs](#hlpairs) (Lua版もあります)
  括弧をハイライト強化版
- [minviml](#minviml)
  Vim scriptをminify
- [popselect](#popselect)
  ポップアップで色々開くやつ
- [previewcmd](#previewcmd)
  コマンド補完
- [reformatdate](#reformatdate) (レ)
  <C-a>で日付と曜日をインクリメントとか
- [registers-lite](#registers-lite)
  registers.nvimライクなプラグイン
- [skipslash](#skipslash) (レ)
  `:%s/foo/bar/`のとき<Tab>でfooからbarへ移動
- [update](#update)
  gvim.exeを最新へ更新する
- [vim9skkp](#vim9skkp)
  Vim9 scriptで作ったskk
- [yomigana](#yomigana)
  漢字やひらがなをカタカナに変換したり
- [zenmode](#zenmode)
  cmdheight=0エミュレータ(statuslineも非表示)

----

以下リポジトリと記事へのリンクです

## anypanel

https://github.com/utubo/vim-anypanel
https://zenn.dev/utubo/articles/20250602-play-tabpanel

tabpanelのレイアウトを補助するプラグインです


## colorscheme-girly

https://github.com/utubo/vim-colorscheme-girly

うちのvimが一番kawaii!
ピンク基調のカラースキームです


## ezpack

https://github.com/utubo/vim-ezpack

自作プラグインマネージャ
よくある(?)packaddを利用したやつです
Vim9 scriptで作ったやつが欲しいなと思って作りました

## headtail

https://github.com/utubo/vim-headtail
https://zenn.dev/utubo/articles/vim-move-to-head-of-textobj

Textobjの先頭や末尾に移動
(これはあんまり使ってないな…)


## hlpairs

https://github.com/utubo/vim-hlpairs
https://github.com/utubo/hlpairs.nvim
https://zenn.dev/utubo/articles/7e12d9684cd2af

括弧をハイライト強化版


## minviml

https://github.com/utubo/vim-minviml
https://zenn.dev/utubo/articles/01788284d5e21b

Vim scriptをminify


## popselect

https://github.com/utubo/vim-popselect

ポップアップで色々開くやつ
MRUやバッファの一覧などをさくっと開くプラグインです


## previewcmd

https://github.com/utubo/vim-previewcmd

コマンドをプレビューします
以下のissueを見て作ってみました
https://github.com/vim/vim/issues/18843


## reformatdate

https://github.com/utubo/vim-reformatdate
https://zenn.dev/utubo/articles/6e2d2cfa049414

日付と曜日を<C-a>でインクリメントとかできるようにします


## registers-lite

https://github.com/utubo/vim-registers-lite
https://zenn.dev/utubo/articles/e56fe4b53dda3d

registers.nvimライクなプラグイン


## skipslash

https://github.com/utubo/vim-skipslash

`:%s/foo/bar/`のとき<Tab>でfooからbarへ移動できるようにします


## update

https://github.com/utubo/vim-update

Windowsのgvim.exeをvim/vim-win32-installerの最新へ更新する関数を提供します
プラグイン名が雑すぎる…


## vim9skkp

https://github.com/utubo/vim-vim9skkp

Vim9 scriptで作ったSKK日本語入力プラグインです


## yomigana

https://github.com/utubo/vim-yomigana
https://zenn.dev/utubo/articles/20230828-vim-yomigana

SKKの辞書やMecabを利用して、漢字やひらがなをカタカナに変換するプラグインです


## zenmode

https://github.com/utubo/vim-zenmode

cmdheight=0をエミュレートし、statuslineも非表示にします

----
Zennの記事になってないものもそのうち詳しく紹介するかもです


