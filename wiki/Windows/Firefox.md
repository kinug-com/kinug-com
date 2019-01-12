Windows 版 Firefox に関するメモ

# タブバーを多段表示にしたい

Firefox Quantum になり、TabMixPlus が使えなくなった。そのうちサポートされるかもしれないが、userChrome.css を使ってタブバーを多段表示にする方法があったのでメモ。


- [It_Was_The_Other_GuyがMulti-row tabs in FF57](https://www.reddit.com/r/FirefoxCSS/comments/7dclp7/multirow_tabs_in_ff57/dpwsh6s/) - userChrome.css のサンプル
- [UserChrome-Tweaks/multi_row_tabs_firefox_v57.css](https://github.com/andreicristianpetcu/UserChrome-Tweaks/blob/09fa38a304af88b685f4086bc8ea9997dd7db0fd/tabs/multi_row_tabs_firefox_v57.css) - userChrome.css の設定集
- [FirefoxのuserChrome.cssがある場所（私的メモ）](http://team-mrc.com/archives/1912) - userChrome.css の場所

自分の環境では、ディレクトリ構成が少し違っていたので、以下の場所に保存した。

    C:\Users\username\AppData\Roaming\Mozilla\Firefox\Profiles\*****.default\chrome


