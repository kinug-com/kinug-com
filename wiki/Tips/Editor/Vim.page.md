Vim とは Vi IMproved の略らしい。

# 参考サイト
- [公式サイト ドキュメント](http://vimdoc.sourceforge.net/)
- [香り屋](http://www.kaoriya.net/software/vim/) Windows 向けバイナリなど。
- [Vim UTF-8 日本語版](https://sites.google.com/site/fudist/Home/vim-nihongo-ban)
- [Vimチートシート作りました](http://folioscope.hatenablog.jp/entry/vimcheatsheet) なかなかキレイなカンペ

# インストール

今回は、vim-utf8 版の zip を解凍してそのまま使用。他と比べてないので、どう違うかは不明。


# プラグインについて

- [パワーランチ（2010/04/09） Vimの話](http://nanasi.jp/articles/howto/note/powerlaunch-20100409.html)

gnupack 版の場合は、%HOME%\.vim では使えないよう。%INST_DIR%\app\vim\runtime に入れると使用できる。



# 起動時設定

個別の設定は、ホームディレクトリの _vimrc 、_gvimrc に書く。gnupack 版の場合は、%INST_DIR%\\home
 がホームディレクトリになる。

## IME のオンオフ設定

挿入モードに入ったときに前回の状態を覚えておいて欲しかったので。

    set iminsert=0
    set imsearch=0

## 文字エンコーディングの設定

都合により Shift-JIS 優先にしたかったので、今回はこんな感じ

    :set encoding=sjis
    :set fileencodings=iso-2022-jp,euc-jp,sjis,utf-8
    :set fileformats=dos,unix,mac

## フォント設定

表示用フォントと印刷用フォントを別々に指定できる。

    set guifont=Ricty_Regular:h12:cSHIFTJIS
    set printfont=Ricty_Regular:h12:cSHIFTJIS

## 全角記号の文字幅がおかしい場合

以下を .vimrc などに設定。

    set ambiwidth=double

- [Vimで全角記号が変になる](http://www.graviton-jk.net/linux_n_me/vim_ambiwidth.html)

## 起動時のカラースキーム指定

    colorscheme mycolorscheme

## シンタックスハイライト

    syntax on

## 不可視の文字を見えるようにする。

    set list
    set listchars=tab:\|\ ,eol:↲
    highlight SpecialKey term=underline ctermfg=darkgray guifg=darkgray

文字はお好みで。

参考：

- [【Vim】タブ、空白、改行を可視化する](http://blog.remora.cx/2011/08/display-invisible-characters-on-vim.html)

## 折り返し行でのカーソル移動を表示通りにする。

- [vim/gvimで日本語を使いやすくする/表示行で移動する](https://sites.google.com/site/fudist/Home/vim-nihongo-ban/vim-japanese#TOC--3)

これは結構効く。

## Markdown の階層構造を折りたたむ

- [markdown を折り畳む](http://d.hatena.ne.jp/thinca/20100302/1267462867)

## Vim でも follow-mode

以下の設定を .vimrc に付け加える。開始は、\\ef

    " --------------------------------------------------------------------- 
    " emacs follow: scroll bind two windows one screenful apart nmap <silent> <Leader>ef :vsplit<bar>wincmd l<bar>exe "norm! Ljz<c-v><cr>"<cr>:set scb<cr>:wincmd h<cr>:set scb<cr> 

参考：[Re: emacs follow mode in vim](http://markmail.org/message/kbnyhxffy6zs7pic)

## gVim で全画面トグル

- [gVimで全画面トグル(タスクバー/キャプション含)](http://nanabit.net/blog/2007/11/01/vim-fullscreen/)

# vim で置換の際に改行コードを入力する

Ctrl+v Ctrl+m で改行が入力できる。