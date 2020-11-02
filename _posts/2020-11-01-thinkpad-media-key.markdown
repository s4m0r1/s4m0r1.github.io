---
layout: post
title:  "ノートパソコンについている音量キーをLinuxで扱う"
date:   2020-11-01 21:42:24 +0900
categories: Linux Thinkpad
---

最近一昔前のThinkPadを引っ張りだしArchLinux+WayLand+Swayという環境でインターネッツをしているが音量キーがあるにもかかわらず押しても反応しないため設定をしてみる

この情報は2020年11月の情報です


## 環境
一応上にも書いたが今回作業を行った環境を書いておく

PC
Lenovo ThinkPad X1 (2013)

ディストリビューション
ArchLinux

ウィンドウ環境
WayLand+GDM+Sway

## 大体のやること

キーコードを取得しSwayのキーバインドに登録する


## キーコードをスキャンしてみる

Waylandでキーコードを調べる場合`wshowkeys`というパッケージをインストールする必要がある

https://git.sr.ht/~sircmpwn/wshowkeys

ビルドなどに必要なものをインストール
```
pacman -S meson cairo pango udev
```

クローン
```
git clone https://git.sr.ht/~sircmpwn/wshowkeys
cd wshowkeys/
```

インストール
```
meson build
ninja -C build
ninja -C build install
ninja -C build install
chmod a+s /usr/local/bin/wshowkeys
```



## Swayのコンフィグに書き込む

音量の変更コマンドで`amixer`というコマンドを使用する

音量を上げる場合(下げる場合は+ではなく-)
`amixer -q -D pulse sset Master 10%+`

音量をミュートする(ミュートを解除する場合はunmute)
`amixer -q -D pulse sset Master mute`

