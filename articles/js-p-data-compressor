---
title: "Javascriptで文字列圧縮"
emoji: "🗜️"
type: "tech"
topics: ["javascript"]
published: true
published_at: "2025-01-05 00:00"
---

:::message
この記事は[以前公開していたブログの2021-06-19の記事](https://utb.sakura.ne.jp/blog/archives/207)からの転載です
:::

Javascriptで文字列圧縮処理を自作したのでメモしておきます

## 経緯
[てきとーに作ったパズルゲーム](https://utb.sakura.ne.jp/tekitou/p/index.html)では各ステージデータを圧縮してURLの後ろにくっつけてます
今までは某ライブラリで圧縮解凍していたのですがライセンス的にアウトかもしれないと思い変更することにしました
要件は以下
- ライブラリが大きくないこと
- 圧縮後はURLにくっつけやすいこと
- 圧縮前はステージデータに使う`/^[0-9,]+$/`を満たしていればOK

色々ライブラリを漁ってみたんですが、ほとんどがパズルゲーム本体よりもサイズが大きくてダメでした
100行程度の簡潔なサンプルもあったのですが、URLエンコード対象の文字が含まれており却下

## できた
というわけでサクッと自作しました
サイズは600byteくらいです

ソースはこちら
https://github.com/utubo/js-p-data-compressor

デモはこちら
https://utubo.github.io/js-p-data-compressor/demo.html

今後を考えて、圧縮前の条件は`/^[0-9,]+$/`ではなく`/^[0-9a-f,]+$/`の文字列に対応させました

### 解凍のロジック
`g-zA-Z`が予約記号です
2文字1組で登場しg～z=0～45の数値に変換して使います
1文字目=`len`、2文字目=`symbol`としたとき
- 予約記号以外はそのまま出力します
- `symbol`===0の場合、直前の文字を`len`回繰り返します
  ```
  0jg → 000
  ```
- `symbol`!==0の場合、出力結果の`symbol`文字前から`len`文字をコピーします
  ```
  12345jh → 1234534
  ```

圧縮については割愛

## もう少し汎用的に
文字種の制限を外したものも作ってみました
encodeURIComponentとかは自分でやらないとダメかもです
サイズは1kbyteくらい…もう少し整理したい…

スースはこちら
https://github.com/utubo/js-cheep-compressor

デモはこちら
https://utubo.github.io/js-cheep-compressor/demo.html

### 解凍のロジック
圧縮後の文字列は`commands_seed`のように`_`で2ブロックに分かれています
commandsは`0-9a-zA-Z`のみで構成されています
2文字1組で登場し0～Z=0～61の数値に変換して使います
1文字目=`symbol`、2文字目=`len`としたとき
- `symbol`===0の場合、seedの文字を`len`文字コピーしつつ読み進めます
  ```
  03_abc → abc
  ```
- `symbol`===1の場合、seedの文字を1文字読み進め`len`回繰り返します
  ```
  13_a → aaa
  ```
- `symbol`=>2の場合、出力結果の`symbol`文字前から`len`文字をコピーします
  ```
  0632_abcXyz → abcXyzXy
  ```
- commands処理後、残りのseedをすべて出力します
  ```
  13_aXyz → aaaXyz
  _ABC → ABC
  ```

だいたい解凍後の方が小さいですね！
2文字1組じゃなくて3bit+3bitで1文字にしたら小さくなるかな？→ダメでした
圧縮については割愛

