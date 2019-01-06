[Vimwiki](https://github.com/vimwiki/vimwiki) は Vim 上で動作する個人用 wiki システム。Vim 上で wiki リンクを辿ったりするようなナビゲーションも可能。wiki 記法だけでなく、Markdown も使え、wiki データを HTML としてエクスポートすることが出来るのも特徴。

# インストール

配付されている .vba ファイル（Visual Basic ではなく Vim Ball というものらしい。）を Vim で開き、「:source %」 コマンドを実行するだけでインストールされる。非常に簡単。

# 設定

markdown 記法を使いたかったので、.vimrc に以下の記述を追加した。

    set nocompatible
    filetype plugin on
    syntax on
    
    let g:vimwiki_ext2syntax = {'.md': 'markdown', '.markdown': 'markdown', '.mdown': 'markdown'}
    let g:vimwiki_list = [{'path': '~/vimwiki/', 'syntax':'markdown', 'ext':'.md'}]


# 参考リンク

- [vimwiki/vimwiki・GitHub](https://github.com/vimwiki/vimwiki) - 公式
- [http://code.google.com/p/vimwiki/](vimwiki - Personal wiki for Vim) - 元公式
- [Vimwiki : Vimエディタ上で動作するWiki環境](http://nanasi.jp/vim/vimwiki.html) - 日本語の解説
- [FrontPage - VimWiki](http://vimwiki.net/) - 日本語情報
