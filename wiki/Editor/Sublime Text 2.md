# Sublime Text メモ

# 環境設定

職場環境では、プロキシを越えなければならないので、パッケージを入れるときには以下のコマンドを使う。

    import urllib2,os; pf='Package Control.sublime-package';ipp=sublime.installed_packages_path(); os.makedirs(ipp) if not os.path.exists(ipp) else None; urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler({'http': 'http://user:pass@proxy.example.com:8080'}))); open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read()); print 'Please restart Sublime Text to finish installation'


こちらは、Package Control->Settings-User

    {
    	"repository_channels": [
    		"http://sublime.wbond.net/repositories.json"
    	],
        "http_proxy": "http://user:pass@proxy.example.com:8080",
        "https_proxy": "https://user:pass@proxy.example.com:8085"
    }

# フォントの指定

ちゃんと等幅フォントを使わないと色々面倒なので。

    "font_face": "ＭＳ ゴシック",
    "font_size": 10


# Markdown

Markdown のシンタックスハイライトがしっかりできていれば、メモ用途ではプレビュー機能は重要じゃないかも？

→ SmartMarkdown を利用

ただ、シンタックスハイライトでずいぶん見やすくなるが、プレビューには文字サイズ等が反映されず（当然）、出力には色が反映されないので、最終イメージと印象が違ってしまう。

## Markdown Preview

- http://perl.no-tubo.net/2013/03/26/markdown%E6%9B%B8%E3%81%8F%E6%99%82%E3%81%AF%E3%83%AA%E3%82%A2%E3%83%AB%E3%82%BF%E3%82%A4%E3%83%A0%E3%83%97%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E5%87%BA%E6%9D%A5%E3%82%8B-sublime-text2-markdown-prev/
- http://codedehitokoto.blogspot.jp/2012/04/sublimetext2markdown.html
- http://blog.boyet.com/blog/blog/using-markdown-in-sublime-text-2-on-windows/
- http://brandonjcarroll.com/blog/getting-down-with-markdown

    // Default.sublime-keymap
    [
        { 
            "keys": ["alt+m"], 
            "command": "markdown_preview", 
            "args": {"target": "browser"}
        }
    ]


これで、ALT+M でプレビューできる。

    //MarkdownPreview.sublime-settings
    {
        "browser": "C:\\Program Files\\Mozilla Firefox\\firefox.exe"
    }

これで、ブラウザ指定。

プレビュー使用時にエラーが出るので、MarkdownPreview.py の 236 行を以下のように修正（コメントアウト）。

    print 'error' # sublime.error_message('cannot execute "%s" Please check your Markdown Preview settings' % config_browser)


## ブラウザでのプレビューの自動更新

以下のパッケージがある

- LiveReload(こっちのほうがメジャー？)
- Browser Refresh(若干マイナーだけど、ブラウザ側にプラグイン不要)

Browser Refresh を採用・・・したけど、動いてない？


## Markdown のシンタックスハイライト

    http://www.bram.us/2013/02/08/sublime-text-markdown-syntax-highlighting/

これをスタイルファイルに貼り付けた。

# リモートファイルの編集
やはり、サーバに置いているメモをいつも整理できるようにしておきたい。
以下のものがあるらしい。これから検討。

- FtpSync（SSH のみで動作。ディレクトリの同期を行う）
- Rmate（サーバ側にインストールが必要）


# IMESupport プラグイン

このプラグインを使えば、インライン変換ができる。ちょっと Buggy だけど、実用レベル。

- http://qiita.com/items/5ccbe63d36009680e0e6
- http://webkikaku.co.jp/staff/software/windows-sublime-text-2/


# emacs キーバインド

使わないようにしようと思ってたけど、 emacs キーバインドが使えるとやっぱり幸せ。

# 感想

Sublime Text 2 は、エディタとしての完成度はそこそこ良くて、使い勝手も安定している。特に、minimap の使い勝手が非常によいので、これからのエディタは minimap を標準機能とすべきなんじゃないかと思った。

一方で、vim キーバインドやら、emacs キーバインドは設定できるので、マウスから解放された編集作業も一応できるものの、ファイル保存のダイアログなどが出てしまうと、マウスを使うか、Windows 標準のキーバインドで操作することを強いられてしまう。この辺りの操作性に少し難があると感じた。
