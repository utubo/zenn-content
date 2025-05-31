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
前回、tabpanelに非表示のバッファリストを表示してみました
https://zenn.dev/utubo/articles/20250526-show-hiddens-in-tabpanel

**でも、tabpanelってもっと色々できると思うんです**


# 何でも表示できるプラグインを作ってみた
![](/images/20250602-play-tabpanel/anypanel.png)
https://github.com/utubo/vim-anypanel
tabpanelに何でも表示できるプラグインを作ってカレンダーとかを表示してみました
※というかtabpanelって元々式を設定すれば何でも表示できるので、このプラグインは実はレイアウト補助プラグインです

**でも、tabpanelってもっと色々できると思うんです**

# 🐟
![](/images/20250602-play-tabpanel/aquavium.gif)
https://github.com/utubo/vim-aquavium
魚を飼ってみました
癒されますね
前述のプラグインに組込むこともできます
![](/images/20250602-play-tabpanel/aquavium-in-any.png)

**でも、tabpanelってもっと色々できると思うんです**

# 👾
![](https://raw.githubusercontent.com/utubo/zen-content-blob/main/images/20250602-play-tabpanel/defencetabp.gif)
https://github.com/utubo/vim-defencetabp
記事タイトル回収です
tabpanelで動くゲームを作って遊んでみました！
タブに敵の弾があたると`confirm tabclose`されてしまいます(`tabclose!`でないのは優しさ)
自機の残機は無限ですが、全てのタブが破壊されると

![](/images/20250602-play-tabpanel/defencetabp-gameover.png)
終了です(vimが)

# やっぱり

![](/images/20250602-play-tabpanel/anypanel.png)
やっぱり私はこのくらいで良いです…
正しく用法を守っていきたいと思います

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 6/2 の記事です。
次回の記事は tositada さんによる、6/4 の [:help](https://vim-jp.org/ekiden/#article-2025-06-04)です。🏃
:::

