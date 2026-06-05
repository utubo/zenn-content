---
title: "タスクスケジューラーでPowerShell Scriptのバッチを.vbsを使わず非表示で実行する"
emoji: "👀"
type: "tech"
topics: ["Windows"]
published: true
published_at: "2026-06-03 00:00"
---

それでいて且つ
- バッチファイルは1ファイルにまとめたい
- ログオンしているユーザー/セッションで実行したい

## TL;TD

.vbsではなく.jsをwscriptで実行すればOK

## やりかた

1. 以下のようなを.jsファイルを用意する
  (ためしに黒いコンソールウインドウを表示させずにエクスプローラーを起動する例)

```js:example.js
new ActiveXObject("WScript.Shell").Run("powershell.exe -NoProfile -ExecutionPolicy Bypass -Command \"Get-Content '" + WScript.ScriptFullName + "' | % { $p = $false }{ if ($p) { $_ } if ($_ -match '\\[main\\]') { $p = $true } } | Invoke-Expression\"", 0, true);
WScript.Quit();

/* [main] Write your PowerShell code directly below

# example
explorer .

# */
```

2. タスクスケジューラーでタスクを以下の様に作成する
  - 全般
    - 「表示しない」にチェック
  - 操作
    - プログラムの開始
    - プログラム/スクリプト: wscript
    - 引数の追加: 用意した.jsファイルのパス

## 注意と原理

堅牢な作りではないので、どういう仕組みか把握しておいたほうがよいかもです
例えばPowerShell Script内に`*/`という文字列が含まれていると動かないかもです

### 原理
#### wscript.exeから以下を実行
1. powershell.exeで後述の処理を実行する
2. 一応`Quit`で終了しておく(無くてもよい)
3. `/* [main]` ～ 最終行`# */`はコメントなので無視

#### powershell.exeでの処理
1. `Get-Content`で.jsファイル自身を読み込む
2. `[main]`とかかれた行まで読み飛ばす
3. 以降の行を`Invoke-Expression`で実行
4. 最終行は`#`で開始されているのでコメント

## 少しコンパクトに

`[main]`を探すのではなく、「固定で1行目を読み飛す」で良ければ以下のようにも書けます

```js:example.js
new ActiveXObject("WScript.Shell").Run("powershell.exe -NoProfile -ExecutionPolicy Bypass -Command \"Get-Content '" + WScript.ScriptFullName + "' | Select-Object -Skip 1 | Invoke-Expression\"", 0, true);/*
# Write your PowerShell code directly below
# example
explorer .
# */
```

※なんで.vbsを使わないのとかcscript.exeとwscript.exeの違いにとかついては割愛します🙏
