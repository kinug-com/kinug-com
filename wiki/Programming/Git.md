---
format: markdown
categories: Git Linux
toc: yes
...

Git の使い方メモ

# コミットメッセージの書き方

- [Gitのコミットメッセージの書き方 - Qiita](https://qiita.com/itosho/items/9565c6ad2ffc24c09364) - 参考例


# 既に存在するリポジトリを持ってきて変更を加えた後に push する手順

シナリオとして、既存のリポジトリをローカルにクローンして修正を行う手順を考える。手順としては、以下の流れになる。

1. リモートリポジトリをクローンしてローカルリポジトリを作成
1. ローカルのファイルに変更を加える
1. ローカルリポジトリに変更をコミット
1. ローカルリポジトリでの変更をリモートリポジトリにプッシュ


## リモートリポジトリをクローンしてローカルリポジトリを作成

リモートリポジトリの内容をローカルに持ってくるには git clone を使う。

    $ git clone /path/to/repo
    Cloning into repo...
    done.

この操作で、ローカルにリモートと同じ内容のリポジトリとファイルがコピーされる。リモートリポジトリを指定する方法は、 local/WebDav/git/rsync/ssh が使える。ssh の場合は、

    $ git clone ssh://user@example.com/path/to/repo

のように指定すればいいらしい。


## ローカルのファイルに変更を加える

好きなようにローカルをいじる。ファイルの削除、ファイル名の変更は git rm , git mv を使えばリポジトリごと修正することが出来る。

疑問点：普通にファイルシステム上で操作してから git add を使ってローカルリポジトリに反映するのと、git rm/mv を使用するのと違いはあるのか？


## ローカルリポジトリに変更をコミット

* 新規ファイル／修正ファイルをコミット対象にするには git add ファイル名
* 修正をコミットする場合は、git commit -m 'コメント'
* git commit -a -m 'コメント' とすれば、git add 抜きで変更されたファイルを自動的にコミットすることが出来るが、新規ファイルは git add が必要。


## ローカルリポジトリでの変更をリモートリポジトリにプッシュ

    $ git push origin master

これでローカルリポジトリの変更がリモートリポジトリに反映される。ローカルでの変更をきちんとコミットしてないとややこしくなるので注意。

疑問点：push したときには、コミットの履歴はどのようにリモートリポジトリに反映されるのか？


## リポジトリのマージ

コミットの順番を間違えると、各所の変更をマージする必要がある。その辺をまとめる。

### リモートリポジトリからの変更をローカルに適用する（競合が無い場合）

変更分をリモートリポジトリからローカルに適用するには、

    git pull /path/to/repo

取り込み元のリポジトリの指定方法は git clone と同じ。元々 git clone などで複製した元のリポジトリの場合は単に

    git pull

だけで良い。

差分をローカルリポジトリに適用したら、ローカルリポジトリの変更分をリモートへ反映させる。

    git push origin master

競合が無ければこの方法で大丈夫。

### リモートリポジトリからの変更をローカルに適用する（競合が無い場合）

調べる。


* [分散バージョン管理システムGitの使い方入門(5/5)](http://sourceforge.jp/magazine/09/02/02/0655246/5)


## SSH 経由で Git を使う

順を追って手順が解説されているので、↓を参考にするとよい。

[[Linux]Gitのリモート環境を作ってSSHで繋げてみた - 日々量産](http://d.hatena.ne.jp/ryousanngata/20120119/1326906033)

# Windows での Git 環境の利用についてメモ

- [WindowsにおけるGit利用環境は整った： Git for Windows と SourceTree for Windows - 檜山正幸のキマイラ飼育記](http://d.hatena.ne.jp/m-hiyama/20140203/1391381365)
- [Git for Windows](https://msysgit.github.io/)

# ssh 経由でリモートのリポジトリにアクセス。

ssh 経由でリモートのリポジトリにアクセスする際には、以下のように指定した。

    ssh://user@domain:port/path/to/repo

鍵は以下のディレクトリに置いた。

    C:\Users\username\.ssh


## 参考 URL

- [せっかちな人のための git 入門 - git をインストールし、共同で開発できる環境を整えるまで](http://blog.champierre.com/670)
- [分散バージョン管理システムGitの使い方入門](http://sourceforge.jp/magazine/09/02/02/0655246/4)
- [Git入門/ドキュメント](http://www8.atwiki.jp/git_jp/)
- [Git入門 ゼロから始めるGitドリル](http://engineerflies.blogspot.com/2010/03/git.html)
