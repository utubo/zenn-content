---
title: "Vimのビジュアルモードの切り替えについて考える"
emoji: "🇻"
type: "tech"
topics: ["vim"]
published: true
published_at: "2026-07-20 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 7/22 の記事です。
:::

# 文字単位のビジュアルと行ビジュアルと矩形ビジュアル

完全に身体に染み付いてて今まで気付かなかったんですが
よくよく考えると`v`、`V`、`<C-v>`って使い分けるの面倒くさくないですか
ぜんぶ`v`でいいかんじによろしくしたい
思考の速度を超えたい思考したくない(怠惰)

# ビジュアル(文字単位)での行移動

文字単位のビジュアル(`v`)のときって上下にカーソルを動かすことって少ないと思うんです
逆に行ビジュアルのときは複数行を選択したいときです(単一行への操作ならビジュアルを使わなくてよい)
それならば、文字単位のビジュアルで行移動したときは行ビジュアルに移行させちゃえば良さそうです
とはいえ、まったく競合しないというわけでもないので
「ビジュアルモードに入ってまだカーソルを動かしていないなら」
という条件をつけておきます

これで`vj`とか`vk`で即座に複数行を選択できます

```vim
xnoremap <expr> k $'{mode() ==# 'v' && getpos('.') ==# getpos('v') ? 'V' : ''}{v:count1}k'
xnoremap <expr> j $'{mode() ==# 'v' && getpos('.') ==# getpos('v') ? 'V' : ''}{v:count1}j'
```

余談: `''`を`'<C-v>'`にして矩形ビジュアルに移行させるのもありかもしれない

# 行ビジュアルでの横移動

行ビジュアル(`V`)では基本的にカーソルを横に移動させることは無いと思います
ここに何か仕込めそうです

1. 横移動で矩形ビジュアルに移行する

横移動に矩形ビジュアルへの移行を割り当てれば小指を労れそうです
(矩形ビジュアルに移行するまでは行単位でハイライトされるのでちょっと慣れが必要かもです)

```vim
xnoremap <expr> h $'{mode() ==# 'V' ? '<C-v>' : ''}{v:count1}h'
xnoremap <expr> l $'{mode() ==# 'V' ? '<C-v>' : ''}{v:count1}l'
```

2. 横移動でノーマルモードに戻る

小指が平気な人はノーマルモードに戻るようにするのもお勧めです

```vim
xnoremap <expr> h $'{mode() ==# 'V' ? '<Esc>' : ''}{v:count1}h'
xnoremap <expr> l $'{mode() ==# 'V' ? '<Esc>' : ''}{v:count1}l'
```

# ビジュアルモード中の`v`での切り替え

ビジュアルモード中に`v`, `V`, `<C-v>`で各ビジュアルモードへ切り替えられますが
これも`v`だけですませたいところです

1. `v`で文字単位のビジュアル→行ビジュアル→矩形ビジュアル→ノーマルモード

ノーマルモードから`vv`で行ビジュアルになるのがわかりやすいです

```vim
xnoremap <script> <expr> v matchstr('vV<C-v><ESC>', $'{mode()}\@<=.')
```

2. `v`でどのビジュアルモードからでもノーマルモードに戻す

切り替えるのも考えたくないという場合はシンプルに
行ビジュアルや矩形ビジュアルから文字単位のビジュアルに移行できなくなる弊害がありますが
おそらくそんな状況には遭遇しないでしょう

```vim
xnoremap v <Esc>
```

# ビジュアルモード中に`q`で矩形ビジュアル

前述の行ビジュアルでの横移動でも矩形ビジュアルに移行できますが
それとは別に用意しておくと小指が喜びそうです
`<C-q>`にならってビジュアルモード中に`q`で矩形ビジュアルへ
ノーマルモードからなら`vq`で矩形ビジュアルになります
マクロの記録は`Q`へ退避
(そういえばビジュアルモード中にマクロ機能使ったことないな…)

```vim
xnoremap q <C-v>
xnoremap Q q
nnoremap Q q
```

# `I`と`A`

`I`と`A`は矩形ビジュアルで使う便利な機能ですが
どのビジュアルモードでも起動するようにしちゃえば考えることが減ります

```vim
xnoremap <expr> I mode() ==# '<C-v>' ? 'I' : '<C-v>I'
xnoremap <expr> A mode() ==# '<C-v>' ? 'A' : '<C-v>A'
```

# 私のマッピング

今はこんな感じにしてます

- 文字単位のビジュアル直後の行移動で行ビジュアル
- 行ビジュアルでの横移動で矩形ビジュアル
- `v`でノーマルモード
- `q`, `I`, `A`で矩形ビジュアル
- マクロ記録は`Q`
- hjkl以外にも対応
- 登場回数が多い共通処理は`<Plug>`で纏めました

```vim
xnoremap <expr> <Plug>(auto-V) $'{mode() ==# 'v' && getpos('.') ==# getpos('v') ? 'V' : ''}'
xnoremap <expr> k $'<Plug>(auto-V){v:count1}k'
xnoremap <expr> j $'<Plug>(auto-V){v:count1}j'
xnoremap gg <Plug>(auto-V)gg
xnoremap G <Plug>(auto-V)G
xnoremap { <Plug>(auto-V){
xnoremap } <Plug>(auto-V)}

xnoremap <expr> <Plug>(auto-c-v) $'{mode() ==# 'V' ? '<C-v>' : ''}'
xnoremap <expr> h $'<Plug>(auto-c-v){v:count1}h'
xnoremap <expr> l $'<Plug>(auto-c-v){v:count1}l'
xnoremap 0 <Plug>(auto-c-v)0
xnoremap ^ <Plug>(auto-c-v)^
xnoremap $ <Plug>(auto-c-v)$
xnoremap f <Plug>(auto-c-v)f
xnoremap F <Plug>(auto-c-v)F
xnoremap t <Plug>(auto-c-v)t
xnoremap T <Plug>(auto-c-v)T

xnoremap <expr> I mode() ==# '<C-v>' ? 'I' : '<C-v>I'
xnoremap <expr> A mode() ==# '<C-v>' ? 'A' : '<C-v>A'

xnoremap q <C-v>
xnoremap Q q
nnoremap Q q
```

