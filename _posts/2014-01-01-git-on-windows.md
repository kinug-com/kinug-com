---
title: "Windows で Git を使う"
date: 2014-01-01
date_label: "2014年頃（推定）"
description: "Git for Windows と、ssh でリモートのリポジトリに接続するときの指定。"
field: programming
tags: [git, windows]
restored: true
redirect_from:
  - "/wiki/Tips/Windows/Git/"
---

Windows での Git 環境の利用についてメモ

- [WindowsにおけるGit利用環境は整った： Git for Windows と SourceTree for Windows - 檜山正幸のキマイラ飼育記](http://d.hatena.ne.jp/m-hiyama/20140203/1391381365)
- [Git for Windows](https://msysgit.github.io/)

## ssh 経由でリモートのリポジトリにアクセス。

ssh 経由でリモートのリポジトリにアクセスする際には、以下のように指定した。

    ssh://user@domain:port/path/to/repo

鍵は以下のディレクトリに置いた。

    C:\Users\username\.ssh
