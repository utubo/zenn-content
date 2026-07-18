---
title: "Vimで撰択したテキストをその場にポップアップするプラグイン"
emoji: "🍙"
type: "tech"
topics: ["vim"]
published: true
published_at: "2026-07-17 00:00"
---

:::message
この記事は[Vim駅伝](https://vim-jp.org/ekiden/)の2026-07-17の記事です。
Vim駅伝は常に参加者を募集しています。詳しくは[こちらのページ](https://vim-jp.org/ekiden/about/)をご覧ください。
:::

:::message
このプラグインはVim9 script製です
:::

# vim-popclip

https://github.com/utubo/vim-popclip

WindowsとAndroidにRapture(おにぎり)という、画面上の指定した範囲だけをキャプチャして最前面に表示させておくアプリがあり、よろずのことに使いけりなので
Vimでもやってみたら使いどころがあるかもしれんと思って作ってみました

↓動作はこんな感じです

![](https://raw.githubusercontent.com/utubo/zen-content-blob/main/images/vim-popclip/image.gif)

# 使い方

↓こんな感じでマッピングのベースのキーを指定して(この場合は`C`を指定)

```vim
vim9script
packadd vim-popclip
popclip#Init({ key: 'C' })
```

テキストを撰択して`C`を押すと、その場でポップアップウインドウでクリップします
ノーマルモードで`Ciw`で単語や`CC`でカレント行をポップアップできたりもします

ポップアップしたあとは以下の操作ができます

- `cC`で移動
- `yC`でヤンク
- `dC`でクローズ

# 終りに

果して使いどころあるのか…？

