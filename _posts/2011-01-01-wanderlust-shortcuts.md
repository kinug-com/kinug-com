---
title: "Wanderlust のショートカット"
date: 2011-01-01
date_label: "2011〜2015年頃（推定）"
description: "フォルダ、サマリ、メッセージ、ドラフトの各モードのキー操作。"
field: software
tags: [editor, emacs]
redirect_from:
  - "/wiki/Editor/Wanderlust.html"
  - "/wiki/Tips/Editor/Wanderlust/"
  - "/wiki/2011/wanderlust-shortcuts/"
---

Wanderlust のよく使うショートカットをメモっておく。.

参考：http://www.gohome.org/wl/doc/wl-euc.html#SEC_Top

## 設定

設定が必要なファイルは、.emacs .wl .folders の３つ。

## フォルダモード

I:カーソルのあるフォルダの新着記事を再帰的にフェッチする

## サマリモード

<>:リストの先頭／最後に移動
V:メッセージの検索
C-p,C-n:前のメッセージ、後のメッセージ

s:同期(update で差分を同期)
S:ソート prefix argument 付きだと逆順（C-数字とか C-- とか C-u とか）

l:フォルダ表示のオンオフ
T:スレッド表示のオンオフ
v:メッセージ表示をトグル

/:カーソル行のスレッドを開閉する
[:全てのスレッドを開く

a:返信

Enter:メッセージを開く
q:フォルダモードに戻る

w:新しいドラフトを作成する（新規メール作成）

## メッセージモード

## ドラフトモード

C-c C-c : ドラフトの送信
C-c C-k : ドラフトの破棄
C-c C-z : ドラフトの保存
