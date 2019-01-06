---
categories: Git Web Linux
...

Gitit をセットアップしたときのメモ

# インストール

cabal を使う場合には

    cabal install gitit

Ubuntu の場合、apt を使って

    sudo aptitude install gitit

で簡単にインストールできた。パッケージの依存関係がこじれないためには apt を使った方が無難っぽい。


# gitit の起動

デフォルト状態ではコマンドラインから

    /path/to/gitit -f <設定ファイル>

で起動するものらしいが、常用するにはやはりデーモン化したいのが人情。
デーモン化して起動するためのスクリプトが @master_q さんにより[公開されている](https://gitorious.org/masterq-docs/masterq-docs/blobs/master/presentations/20110528_qpstudy/gitit_daemon.sh)ので、これをさくっと編集して /etc/init.d/gitit にでも置けば幸せになれる。


# apache 連携
既に Apache でウェブサーバを立ち上げていて他の用途でも利用している場合には、Gitit を 80 番で起動するのはハードルが高い。なので、Apache の mod_proxy を使って特定のバーチャルホストや特定のパス配下に gitit を割り当てるのが運用的には良さそうな感じ。

設定内容については、[Gitit のドキュメント](https://github.com/jgm/gitit)に詳しい設定が載っているので、適当にアレンジして使えば問題ない。

Ubuntu の場合、別途モジュールを一つインストールする必要があった。

    sudo aptitude install libapache2-mod-proxy-html

これで OK


# サイトのセットアップ

最初に gitit.conf を作成してgitit を起動すると指定したディレクトリ配下に各種ファイルが作成される。デフォルトでは wikidata 配下にサイトデータのリポジトリが作成されるので、これを git clone して編集すればよい。ウェブ上からも編集可能。


# jsMath のセットアップ

Gitit では、texmath を使っており、LaTex の記法で数式を書くことが出来るが、chrome ではうまく表示できないので、より多くのブラウザをサポートする [jsMath](http://www.math.union.edu/~dpvc/jsMath/) を使った設定を使ってみる。

## 準備

wiki のディレクトリの static/js/jsMath 配下に、jsMath-3.6e.zip と、jsMath-fonts-1.3.zip の中身を展開する。


## 設定

gitit の設定ファイルの math の設定を以下のように変更する

    math: jsMath

ブラウズ時にフォントが無いとかなんとかエラーが表示されるが、特に何もしなくても数式は表示されるので、単純に「Hide」を押せば良い（フォントをローカルにインストールすれば高速に表示されるらしい）。


## サンプル

$x = \frac{{ - b \pm \sqrt {b^2 - 4ac} }}{{2a}}$

でも、表示が汚い！ブラウザを識別して表示方法を変えるようなオプションが欲しい。

## 参考

* [LaTeX コマンド一覧](http://www1.kiy.jp/~yoka/LaTeX/latex.html)

# プラグインの使い方

## Dot.hs

Ubuntu のパッケージで入る Gitit はプラグインが無効になってビルドされているものらしいので、プラグインを使うには cabal で入れる必要がある。gitit.conf 内に使用するプラグインを指定できるので、

    plugins: /path/to/Dot.hs

などのようにプラグインモジュールのファイルを指定すればよいらしいが・・・ロード時に以下のようなエラーが出る

    Ambiguous module name `Control.Monad.Trans'
    it was found in multiple packages: mtl-1.1.0.2 monads-fd-0.1.0.1
    
これは、Control.Monad.Trans が複数のパッケージに存在していると言うことらしい、パッケージを指定してモジュールをロードする方法もあるようだが、ここでは、monad-fd と依存パッケージをを消してしまうことにした。

    ghc-pkg unregister data-accessor-monads-fd
    ghc-pkg unregister monads-fd
    
これでロード時のエラーは出なくなった""が、画像ファイルがリンク切れとして表示される。proxy したときに、画像ファイルへのパスが変わっているのが html 上で書き替えられていないらしい、proxy-html の設定がおかしいのかもしれないが、ここでは、 Dot.hs を直接編集して正しいパスを出力させるようにした。"" サーバのリブートでうまく出るようになった。以下は不要なコード（いずれ消す）。

    then return $ Para [Image name ("/wiki/img" </> outfile, "")]
                                     ^^^^^ ここを追加
				     
場当たり的だけど、スマートな方法はあとで検討する。

## サンプル

~~~ {.dot name="diagram1"}
digraph G {Hello->World}
~~~

## 参考

* [Graphviz チュートリアル](http://homepage3.nifty.com/kaku-chan/graphviz/index.html)
* [Dot Documentation](http://www.graphviz.org/Documentation.php)

# pandoc カスタマイズ

gitit.conf には、pandoc のユーザーディレクトリを指定する設定がある。

    pandoc-user-data: pandoc

こんな風に設定すると、gitit のワークディレクトリ配下の pandoc ディレクトリにカスタマイズ用のファイルが置ける。例えば、pandoc/templates の下にテンプレートファイルをコピーして編集することデフォルトのテンプレートに代えて、こちらのテンプレートが使われるようになる。css だけではどうにもならない場合にはこの方法を利用。


# 使い方

## Metadata

ページデータの先頭にメタデータを記述できる。詳しくは[こちら](http://gitit.johnmacfarlane.net/README#page-metadata)

    ---
    format: markdown
    categories: Git Haskell
    toc: yes
    title : hogehoge
    ....

メタデータ内に空行が含まれていると内容がうまく反映されないようなので注意


## Emacs Markdown Mode を使う

Emacs には [Markdown Mode](http://jblevins.org/projects/markdown-mode/) がある。Ubuntu では、emacs-goodies-el パッケージに入っているので apt でインストールできる。Gitit 向けの設定として、*.page のファイルを編集するときに Markdown Mode になるように .emacs を設定しておく。

    (autoload 'markdown-mode "markdown-mode.el" "Major mode for editing Markdown files" t)
    (setq auto-mode-alist (cons '("\\.page" . markdown-mode) auto-mode-alist))

日本語の記事だと、「[Emacsのmarkdown-modeを使ってみる](http://blog.s-amemiya.com/development/emacs%E3%81%AEmarkdown-mode%E3%82%92%E4%BD%BF%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%8B/)」が詳しい。

まあ、キーバインド覚えるより記法を覚える方が先だけど、構文チェック程度には使える。
