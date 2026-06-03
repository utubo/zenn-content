---
title: "タスクスケジューラーでPowerShell Scriptのバッチを.vbsを使わず非表示で実行する"
emoji: "👀"
type: "tech"
topics: ["Windows"]
published: true
published_at: "2026-06-03 00:00"
---

## TL;TD

.vbsではなく.jsをwscriptで実行すればOK

## やりかた

1. 以下のようなをバッチを用意する
  (ためしにエクスプローラーを起動する例)

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
    - 引数の追加: 用意したjsファイルのパス

## 注意と原理

堅牢な作りではないので、どういう仕組みか把握しておいたほうがよいかもです
例えばPowerShell Script内に`*/`という文字列が含まれていると動かないかもです

### wscript.exeから以下を実行
1. powershellで後述の処理を実行する
2. 一応`Quit`で終了しておく
3. `/* [main]` ～ 最終行`# */`はコメントなので無視

### powershell.exeでの処理
1. `Get-Content`でスクリプト自身を読み込む
2. `[main]`とかかれた行までスキップ
3. 以降の行を`Invoke-Expression`で実行
4. 最終行は`#`で開始されているのでコメント


※なんで.vbsを使わないのとかcscript.exeとwscript.exeの違いにとかついては割愛します🙏
