Atom を使い始めてみたので、メモ

# 使っている拡張機能

- minimap -- ミニマップを表示
- minimap-cursorline -- ミニマップ上にカーソル行を表示
- fonts -- フォントパッケージ
- markdown-writer -- マークダウンサポート
- document-outline -- ドキュメントのアウトラインを表示

# プロキシ設定

Atom はパッケージによって機能拡張できるのが売り。以下のページに認証付きのプロキシを通る方法が説明されている。

- [Atom - 続けたい日記](http://yossk.hatenablog.com/entry/2015/03/17/225308)
- [プロキシ認証のある Windows で Atom を使う — a wandering wolf](http://gab-km.bitbucket.org/blog/html/2014/10/23/using_atom_in_windows_with_proxy_authentication.html) - こちらを採用


# パッケージが上手くインストールできない場合

Atom でパッケージをインストールしようとして、以下のようなエラーが出ることがある。

    Compiler tools not found
    Packages that depend on modules that contain C/C++ code will fail to install.
    Read here for instructions on installing Python and Visual Studio.
    Run apm install --check after installing to test compiling a native module.

以下のページによると、Atom のバージョンによってこのようなエラーが誤って出ることがあるらしい。

- [Windows版Atom 0.187.0（付近）＋Proxy環境でPackageの](http://qiita.com/kisato/items/77671f382b22b93c60d0)

解決策は、エラーの出ないバージョンの Atom を使用してパッケージをインストールすること。今回は、0.174.0 を用いてインストールを試みたところ成功した。パッケージの保存される場所は共通なので、そのまま新しいバージョンの Atom でインストールしたパッケージが使用できる。

# フォントの変更

設定画面から、普通にフォントを設定できる。

- [Windows 版 Atom のフォントを変更する - 冬のニムロッド](http://winternimrod.hateblo.jp/entry/2015/01/27/103640)

# Markdown Preview のフォントの変更

Atom は何も設定しなくても Markdown のプレビューがリアルタイムで出来るのが良いところ。しかし、このプレビュー画面のフォントが何かちょっと変なので、これを設定する。

- [Atom Editor Markdown Perview の フォントについて(Windows) - clash_m45の開発ブログ](http://clash-m45.hatenablog.com/entry/20140823/1408779941) - Markdown-preview パッケージを修正する方法
- [Atom で Windows でも快適 Markdown 環境! | 株式会社バニーホップ](http://www.bunnyhop.jp/tips-20140811/) - 後ろの方に、ユーザースタイルシートを修正して Markdown のフォントを変更する方法が書かれている。

今回は、ユーザースタイルシートを修正する方法を採用した。


# ショートカットキー

- Ctrl+] / Ctrl+[ ブロックのインデント/アンインデント
- Ctrl+Shift+m Markdown プレビューを開く
