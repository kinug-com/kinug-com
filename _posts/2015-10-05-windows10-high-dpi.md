---
title: "Windows 10 で高 DPI モニタを使う"
date: 2015-10-05
date_label: "2015〜2018年頃"
description: "表示倍率の設定と、マニフェストで直すアプリの場合。"
field: software
tags: [windows]
restored: true
redirect_from:
  - "/wiki/Tips/Windows 10/Tips/"
  - "/wiki/2015/windows10-high-dpi/"
---

Windows 10 に関するメモ

## 高ＤＰＩモニタ利用の際の設定について

### 一般的な解決方法

- 表示が小さくなりすぎてしまう場合、一般的には「ディスプレイのカスタマイズ」の「テキスト、アプリ、その他の項目のサイズを変更する」で適宜の倍率を指定する。

### 特殊な挙動をするアプリケーションの場合

アプリケーションによっては、高ＤＰＩ対応でないのに高ＤＰＩ対応を騙ってしまうものがあり、そのため表示がおかしくなってしまう場合がある。その場合、Manifest ファイルを別途供給することで解決可能。

- [Adobe App Scaling on High DPI Displays (FIX)](http://www.danantonielli.com/adobe-app-scaling-on-high-dpi-displays-fix/) - Adobe ソフトの場合

Manifest ファイルは、使いまわしが効く場合と効かない場合があるので注意。
