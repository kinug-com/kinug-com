Emacs の使いこなし

# 日本語対応版

Windows 上で日本語入力可能な Emacs について以下のサイトが詳しい。

-[日本語入力可能な GNU Emacs のインストール (Windows) ](http://yohshiy.blog.fc2.com/blog-entry-169.html)

今回は、試しに gnupack_basic-11.00.exe を選択。7zip の自己解凍形式で配付されているので、解凍して適当なところに配置。

# スプラッシュ画面

最初に表示されるスプラッシュ画面をオフにする。

    ;; .emacs
    (setq inhibit-startup-screen -1)

# 行番号の表示

行番号を常に表示させる

    ;; .emacs
    ;; http://masutaka.net/chalow/2009-10-25-1.html
    (global-linum-mode t)


# 日本語入力環境

## 設定

OS 環境の日本語入力で基本的には事足りるが、日本語入力モードでは使えないコマンドがあったりする。Emacs の日本語入力環境(leim)では日本語入力モードでもコマンドが使えるので、設定してみる。

* [UbuntuTips/Application/EmacsJapaneseSetup](https://wiki.ubuntulinux.jp/UbuntuTips/Application/EmacsJapaneseSetup)

    ;; .emacs
    (set-language-environment "Japanese")
    (setq default-input-method "japanese-anthy")

これで、Ctrl-\\ で入力モードオン・オフ

## キーバインド

leim のキーバインドを調べるのに手間取ったのでメモ

* K :: 変換前に押すと、平仮名・カタカナ変換
* SPC :: 変換ステージに移行
* l,L :: 前候補リスト、次候補リスト
* H,K :: 文節を平仮名またはカタカナに変換
* C-i,C-o :: 文節を拡大・縮小
* C-f :: 文節を確定
* C-h :: ヘルプを表示


# マイナーモードを作ってみる

ちょっとした自分独自の作業をするときに、キー操作で文字列を埋め込んだり、ちょっとしたテキスト処理をバッファ上で行えると便利である。Easy-Mmode を使うとマイナーモードが比較的簡単に作れる

* [マイナーモードの作り方](http://www.kogu.ch/2007/02/24/emacs-lisp-%E3%83%9E%E3%82%A4%E3%83%8A%E3%83%BC%E3%83%A2%E3%83%BC%E3%83%89%E3%81%AE%E4%BD%9C%E3%82%8A%E6%96%B9/)


# Wanderlust で gmail を使う

* 基本的な設定はここで:http://d.hatena.ne.jp/stanaka/20071025/1193286440
* 複数アカウントの場合:http://tsurukame.blog.ocn.ne.jp/b/2006/08/wanderlust_1a16.html

    $ aptitude install wl gnutls-bin


# ssh 経由でリモートファイルを編集する(tramp)

## 設定

参考：

- [emacs + trampで多段SSHで接続したサーバー上のファイルを直接編集する時のメモ](http://d.hatena.ne.jp/hideden/20090108/1231395529) - 設定の基本
- [TRAMPを使ってリモートのファイルを弄る](http://qiita.com/miyakou1982/items/d05e1ce07ad632c94720) - Proxy を通す場合の設定

## 使い方

[tramp](http://www.gnu.org/software/tramp/) を使うと、リモートファイルを emacs 上で開くことができる。emacs 21.4 以降にはデフォルトで入っているらしく、設定無しでも使えるよう。起動時にリモートファイルを開くときには、

    $ emacs /ssh:user@host#port/path/to/file

で、起動中の emacs からリモートファイルを開くときには、

    C-x C-f /ssh:user@host#port/path/to/file

でいける。


# デフォルトフォントを Ricty に

.emacs に以下の行を追加。

    (add-to-list 'default-frame-alist '(font . "ricty-12"))
