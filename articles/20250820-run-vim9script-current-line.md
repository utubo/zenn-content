---
title: "カーソル行のVim9 scriptを実行する"
emoji: "📜"
type: "tech"
topics: ["vim"]
published: true
published_at: "2025-08-20 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 8/20 の記事です。
前回の記事は kuuote さんによる、 8/18 の[Vimのaugroup名に `#` は使ってはいけない](https://zenn.dev/vim_jp/articles/20250818_ekiden_augroup_hash)という記事でした。
:::

# まずはLegacy Vim scriptから

私は`g:`にマッピングしています

```vim
nnoremap g: <Cmd>.source<CR>
```

`source`がレンジを受け付けてくれるので カーソル行を示す`.`を指定すればOKです


# Vim9 scriptを実行する

`g9`にマッピングする場合、以下の通りです

```vim
nnoremap g9 <Cmd>vim9cmd :.source<CR>
```

単純に`<Cmd>.vim9cmd source<CR>`のように`vim9cmd`にレンジを渡そうとすると以下のエラーになってしまいます
```
E481: No range allowed.
```

そこで`:.`でsourceにレンジを渡しています

# ビジュアルモードの選択範囲を実行する

ビジュアルモードでは以下のようにマッピングします
レンジに`'<,'>`を渡すだけなので簡単！

```vim
xnoremap g: :source<CR>
xnoremap g9 :vim9cmd source<CR>
```

あ、`:'<,'>vim9cmd source`はエラーになりません(なんで？)

# まとめ

これでちょっとvenry!ฅ⊏'ꣲ'|ฅ

```vim
nnoremap g: <Cmd>.source<CR>
nnoremap g9 <Cmd>vim9cmd :.source<CR>
xnoremap g: :source<CR>
xnoremap g9 :vim9cmd source<CR>
```

