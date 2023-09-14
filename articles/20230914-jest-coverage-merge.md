---
title: "複数のjestのカバレッジ結果をマージする"
emoji: "🧪"
type: "tech"
topics: ["test","javascript"]
published: true
published_at: "2023-09-14 00:00"
---

# はじめに
タイトルの通りです
nycのオプション指定にちょっと詰まったので自分用にメモしておきます

# 前提
以下の通りインストールされているものとします(必須)
```powershell
npm i -D jest nyc
# typescriptの場合これも
npm i -D source-map-support ts-node @istanbuljs/nyc-config-typescript
```

今回は、レポートの出力先は以下の通りとします
```json:jest.config
  coverageDirectory: 'test/.report/coverage',
```
※jest.configのその他の設定については割愛します

今回は、ディレクトリ構成は以下の通りとします
```
foo
├package.json
├test
│└.report … 私はこれを.gitignoreに追加しています
│  └coverage … ここにマージしたカバレッジを出力する
└bar
  ├buzz
  │├test
  ││└coverage
  ││  └coverage-final.json … jestで出力したカバレッジレポート
  │└src
  └qux
    ├test
    │└coverage
    │  └coverage-final.json … jestで出力したカバレッジレポート
    └src
```


# スクリプト
`foo/test`に置いてください
ディレクトリ構成が前提と異なる場合は、最初の数行を書き換えてください
今回はpowershellで書きましたが、そんなに複雑なことはしてないので適宜好きな言語に書き換えてください

```powershell:foo/test/merge_coverage.ps1
$currentDir = pwd
$testDir = $PSScriptRoot
$coverageDir = '.report/coverage'
$coverageJsons = Get-ChildItem -Recurse ../bar/*/test/$coverageDir/coverage-final.json

cd $testDir

# coverrage-final.jsonを1つのディレクトリに集める
if (Test-Path '$coverageDir/work') {
  rm $coverageDir/work/coverage-*.json
} else {
  mkdir $coverageDir/work
}
cd $coverageDir/work
$index = 0
foreach ($c in $coverageJsons) {
  $index++
  $filename =  'coverage-' + $index + '.json'
  cp $c $filename
}

# 集めたcoverrage-final.jsonをマージする
npx nyc merge ./ ../coverage-merged.json

# clover.xmlとHTMLを出力する
npx nyc report -t test/$coverageDir --report-dir test/$coverageDir/Icov-report --reporter html --reporter clover --exclude-after-remap false
mv -Force $testDir/$coverageDir/Icov-report/clover.xml $testDir/$coverageDir/

# 念のため元のディレクトリにcdしておわり
cd $currentDir
```

以下が出力されます。
・`./test/.report/coverage/coverage-merged.json` … coverage-final.jsonをマージしたもの
・`./test/.report/coverage/clover.xml` … Open Cloverレポート
・`./test/.report/coverage/Icov-report` … HTMLレポート

スクリプト、もう少し整理できそうですが今は時間がないので！

