---
title: "🐟Vimのtabpanelで遊んんでみた"
emoji: "🐟"
type: "tech"
topics: ["vim"]
published: true
published_at: "2025-06-02 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 6/2 の記事です。
前回の記事は こまもか さんによる、 5/30 の[Emacsを使い始めて半年が経った](https://vim-jp.org/ekiden/#article-2025-05-30)という記事でした。
:::

# もっと遊ぼう

前回、tabpanelに表示のバッファリストを表示してみました
https://zenn.dev/utubo/articles/20250526-show-hiddens-in-tabpanel

**でも、tabpanelってもっと色々できると思うんです**


# 何でも表示できるプラグインを作ってみた

![](https://github.com/user-attachments/assets/ff276d1d-2afc-4367-9be5-3891b43426ea)

https://github.com/utubo/vim-anypanel

何でも表示できるプラグインを作ってカレンダーとか表示してみました
他にもファイルの内容を表示したりだとか、式をいれちゃえば何でも表示できます

**でも、tabpanelってもっと色々できると思うんです**

# 🐟

![](https://github.com/user-attachments/assets/a404d444-8276-4af2-aaa7-ed9f46f6e451)

https://github.com/utubo/vim-aquavium

魚を飼ってみました
癒されますね

前述のプラグインに組込むこともできます

![](/images/20250602-play-tabpanel/aquavium-in-any.png)


**でも、tabpanelってもっと色々できると思うんです**

# 👾

![](/images/20250602-play-tabpanel/defencetabp.png)

https://github.com/utubo/vim-defencetabp

記事タイトル回収です
タブに敵の弾があたると`confirm tabclose`されてしまいます(`tabclose!`でないのは優しさ)
全てのタブが破壊されると

![](/images/20250602-play-tabpanel/defencetabp-gameover.png)

終了です(vimが)

# やっぱり

![](/images/20250602-play-tabpanel/anypanel.png)

やっぱりこのくらいで良いです…
正しく用法を守っていきたいと思います

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 6/2 の記事です。
次回の記事は tosida さんによる、6/4 の [:help](https://vim-jp.org/ekiden/#article-2025-06-04)です。🏃
:::

