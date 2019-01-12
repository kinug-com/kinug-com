Gnupack は Cygwin で、よく使うものをいい感じにまとめてある便利なパッケージ。

# apt-cyg で 404 Not Found になる

apt-cyg スクリプトが、リポジトリの新しいディレクトリ構造に対応していないのが問題らしい。

２０１３年１２月１６日時点では、本家では問題の修正は行われていないが、この問題を修正したものを、[GitHub で公開](https://github.com/kou1okada/apt-cyg)してくださっている方が居るので、ありがたくダウンロードして apt-cyg を上書きすればよい。(コメントを頂いて、内容を修正しました。)

- [apt-cyg (Google Code)](http://code.google.com/p/apt-cyg/)
- [apt-cyg (GitHub)](https://github.com/ernierasta/apt-cyg)
- [kou1okada / apt-cyg](https://github.com/kou1okada/apt-cyg) - 問題が修正されたブランチ

- [minttyのパッケージ（コマンド）追加 - IT MEMO](http://itmemo.hatenadiary.jp/entry/2013/09/26/164056)
- [apt-cygでsetup.iniが404 Not Foundになった](http://rcmdnk.github.io/blog/2013/08/08/computer-windows-cygwin/)
- [CygWinのapt-cygが上手くいかない場合](http://hujo.hateblo.jp/entry/2013/11/10/213119)

# apt-cyg リポジトリを変更する

    apt-cyg -m http://ftp.jaist.ac.jp/pub/cygwin/ update

- [cygwinのapt-cygでリポジトリを国内リポジトリに変更するコマンド](http://qiita.com/koheiSG/items/585e799a1a5e6f37107d)

# ssh でプロキシ越え

.ssh/config に設定を作り、ProxyCommand ディレクティブを利用して接続時に corkscrew コマンドを利用するように設定すればよい。

    ProxyCommand corkscrew squid.example.com 3128 %h %p ~/.ssh/proxy.login.ini

引数で指定した、ファイル(proxy.login.ini)にプロキシ認証用のユーザ：パスワードを書いておけば、認証に用いられる。

- [HTTPプロキシ経由でのSSH（その１）corkscrewを使う](http://takuya-1st.hatenablog.jp/entry/20110813/1313222344)

# 手動でパッケージをインストール

apt-cyg コマンドを参考に手動でダウンロードしたパッケージをインストールしてみた。

    cd /setup/http%3a%2f%2fftp.jaist.ac.jp%2fpub%2fcygwin%2f/ ; 適当なディレクトリを使う。
    cat setup.ini | awk -v package="python" 'BEGIN{RS="\n\n@ "; FS="\n"} {if ($1 == package) {desc = $0; px++}} END {if (px == 1 & else print "Package not found"}' > "release/python/desc"
    cd release/python
    cat python-2.5.1-2.tar.bz2 | bunzip2 | tar > "/etc/setup/python.lst" xvf - -C /
    gzip -f "/etc/setup/python.lst"
    cd ../..
    cat /etc/setup/installed.db | awk > /tmp/awk.$$ -v pkg="python" -v bz=/setup/http%3a%2f%2fftp.jaist.ac.jp%2fpub%2fcygwin%2f/release/python/python-2.5.1-2.tar.bz2 '{if (ins != 1 & ins=1}; print $0} END{if (ins != 1) print pkg " " bz " 0"}'
    mv /etc/setup/installed.db /etc/setup/installed.db-save
    mv /tmp/awk.$$ /etc/setup/installed.db
    cd /etc/postinstall/   ; ここの *.sh を実行して、*.done に mv する。
    which python

