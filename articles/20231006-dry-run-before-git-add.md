---
title: "vimで`git add`する前に`--dry-run`して確認する"
emoji: "🐙"
type: "tech"
topics: ["vim"]
published: true
published_at: "2023-10-06 00:00"
---

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 10/06 の記事です。
前回の記事は kamecha さんによる、 10/04 の[ddu.vimのアクション周りを便利にしよう](https://zenn.dev/kamecha/articles/18d244603c85fd)という記事でした。
:::

# はじめに
vimでコードを修正した後に`git add -A`でステージングする前に、
想定外のファイルが無いか`--dry-run`オプションの結果を表示するスクリプトです
自分はこれが無いと生きていけません

```vimscript
:GitAdd -A
```
と入力すると


```
git add --dry-run -A
add ここにファイル名
add ここにファイル名
︙
execute ? (y/n) > y
```
とechoされるので、そのまま`y`を入力するとステージングしてくれます

# スクリプト

Vim9 script版とLegacy Vim script版、両方おいておきます
私は`<Space>ga`で`git add -A`するようにマッピングしています

```vimscript:Vim9 script版
def GitAdd(args: string)
  const current_dir = getcwd()
  try
    chdir(expand('%:p:h'))
    echoh MoreMsg
    echo 'git add --dry-run ' .. args
    const list = system('git add --dry-run ' .. args)
    if !!v:shell_error
      echoh ErrorMsg
      echo list
      return
    endif
    if !list
      echo 'none.'
      return
    endif
    for item in split(list, '\n')
      execute 'echoh' (item =~# '^remove' ? 'DiffDelete' : 'DiffAdd')
      echo item
    endfor
    echoh Question
    if input('execute ? (y/n) > ', 'y') ==# 'y'
      system('git add ' .. args)
    endif
  finally
    echoh Normal
    chdir(current_dir)
  endtry
enddef
command! -nargs=* GitAdd GitAdd(<q-args>)
nnoremap <Space>ga <Cmd>GitAdd -A<CR>
```

※ legacyの方は動作確認できてないです🙇無責任ですまない…

```vimscript: Legacy版
function s:GitAdd(args) abort
  let l:current_dir = getcwd()
  try
    call chdir(expand('%:p:h'))
    echoh MoreMsg
    echo 'git add --dry-run ' .. a:args
    let l:list = system('git add --dry-run ' .. a:args)
    if !!v:shell_error
      echoh ErrorMsg
      echo l:list
      return
    endif
    if !l:list
      echo 'none.'
      return
    endif
    for l:item in split(l:list, '\n')
      execute 'echoh' (l:item =~# '^remove' ? 'DiffDelete' : 'DiffAdd')
      echo l:item
    endfor
    echoh Question
    if input('execute ? (y/n) > ', 'y') ==# 'y'
      call system('git add ' .. a:args)
    endif
  finally
    echoh Normal
    call chdir(l:current_dir)
  endtry
endfunction
command! -nargs=* GitAdd s:GitAdd(<q-args>)
nnoremap <Space>ga <Cmd>GitAdd -A<CR>
```

----

:::message
この記事は [Vim 駅伝](https://vim-jp.org/ekiden/) の 10/06 の記事です。
次回の記事は kyoh86 さんによる、10/09 の[deno/nodeのlspスイッチング意外と気を遣うよと言う話](https://zenn.dev/vim_jp/articles/69d26e3f7b0e35)です。🏃
:::

