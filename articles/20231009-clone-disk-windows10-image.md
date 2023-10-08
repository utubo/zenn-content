---
title: "Windows 10のブートディスクで換装したときのメモ(2023/10版無料手順)"
emoji: "💻"
type: "tech"
topics: ["windows"]
published: true
published_at: "2023-10-09 00:00"
---

# はじめに
10年くらい前に買ったPCのSSDを容量の大きい別のSSDに換装したときのメモです
記事へのリンクの寄せ集めです
リンク切れになってたらそれっぽい単語で検索して手順を探してください…

# 必要なもの(ここは無料じゃないです)
- 換装用のSSD
- ディスククローンに使うUSB(1G未満で十分)

# クローン前に回復パーティションを削除する

ディスク容量を拡張するときにじゃまになるので、
以下の手順の通り`diskpart`を使って削除しました

[こちら](https://pc-karuma.net/windows-10-delete-recovery-partition-diskpart/)

# 換装前のディスクをBIOSモードからUEFIに変更する

今回は古いマシンだったので換装前のディスクがBIOSモードでした
以下手順の通り`mbr2gpt.exe`を使ってUEFIに変換しました

[こちら](https://jm1xtk.com/cnt/129_mbr2gpt/index.php)

# ディスクをクローンする

2023/10現在、ディスクを無料でクローンする手順は以下しか見つかりませんでした
USBメモリに`Clonezilla`のイメージを焼いて実施します。
間違えたら怖いので不要なディスクは外しておきましょう

[こちら](https://osa030.hatenablog.com/entry/2021/06/21/200246)

※もしかしたらWSL2のlinuxで動く似たようなツール探せばUSBメモリ無くても済んだのかも

# コマンドでWindowsの起動情報を修正する

まだ換装しません
元のディスクのまま再起動してコマンドライン(管理者)を利用します
以下の手順で*換装用のSSD*を修正します

[こちら](https://www.sakura-pc.jp/pc/contents/trouble/winbooterror.html)

- bcdbootの`/s`オプションは付けませんでした
  `bcdboot d:\windows /l ja-jp`
- `bcdedit /delete`がエラーになってしまったのでかわりにVisual BCD Editorを使いました

[こちら](https://www.boyans.net/DownloadVisualBCD.html)

※ひょっとしたら`bcdedit /delete`してから`bcdboot …`したほうが良かったかも🤔

# パーティションを拡張する

ディスクの管理から拡張します
手順割愛

# ディスクを換装して再起動する

ここまでできたらやっと換装して終わりです！

# 試してだめだった事とその他メモ

- Windows 10標準の`イメージから復元`で換装する→BIOSモードとUEFIの問題がどうしても解決しませんでした
- ↑の手順を実施する際、仮想RAMディスクが起動しているとバックアップの作成に失敗しました
- 大手ソフトウェアを使う→2023/10現在、無料でシステムをクローンできるものはありませんでした

