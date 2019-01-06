Apache2 に関するメモ

# Could not reliably determine the server's fully qualified domain name

起動時に以下のワーニングが出るようになった

    apache2: Could not reliably determine the server's fully qualified domain name, using ::1 for ServerName

サーバの FQDN が無いってのが問題らしい。放っておいても問題なく動くが、/etc/apache2/httpd.conf に適当なFQDN を書いたら直った。

    ServerName hoge.kinug.com

VirtualHost の設定はこれとは無関係に有効だけど、何でもいいから名前付けて欲しいってことかな。ただし、バーチャルホストの設定と名前が重なるとバーチャルホストの設定が利かなくなるので適当なホスト名をくっつけるとうまく行った。

- [fedora14でapacheを起動すると](http://oshiete.goo.ne.jp/qa/6990584.html)


# NameVirtualHost has no VirtualHosts

VirtualHost を色々作っていたら以下のようなワーニングが出るようになった。

    [error] VirtualHost *:80 -- mixing * ports and non-* ports with a NameVirtualHost address is not supported, proceeding with undefined results
    [Thu Feb 02 22:55:12 2012] [warn] NameVirtualHost *:0 has no VirtualHosts
    [Thu Feb 02 22:55:12 2012] [warn] NameVirtualHost *:80 has no VirtualHosts

ポート指定のバーチャルホストと、ポート指定なしのバーチャルホストは混在できないよう。sites-available/* の各ファイルの先頭に

    NameVirtualHost *

の行が入っていたのを削除し、VirtualHost の設定行を

    <VirtualHost *>

のように統一して、ports.conf の NameVirtualHost 行を以下のようにしたら解決した。

    NameVirtualHost *

ワーニングの原因は、昔の設定のコピペと思われる。

- [Apache 2 startup warning: NameVirtualHost *:80 has no VirtualHosts](http://serverfault.com/questions/1405/apache-2-startup-warning-namevirtualhost-80-has-no-virtualhosts)
- [Apache2で「NameVirtualHost *:80 has no VirtualHosts」の対処](http://www.maocat.net/?p=786)
- [バーチャルホストの設定をするには - NameVirtualHost](http://www.ksknet.net/apache/_namevirtualhos.html)

# command not in docroot (/var/www)

デフォルトの docroot 以外で CGI を SuEXEC しようとすると色々エラーが出る。セキュリティの観点から簡単には設定できないように Apache は設計されているのだが、その簡単な回避方法。

Ubuntu では、apache2-suexec-custom パッケージが準備されており、これを使うと /etc/apache2/suexec/www-data というファイルに、SuEXEC を有効にしたいディレクトリを書ける。

    $cat www-data
    /path/to/custom-dir
    public_html

こんな感じ。あとは、apache を再起動すれば、有効になるはず。１行目には SuEXEC を有効にしたいパスを、２行目には userdir の文字列を書かないといけないらしい。

# mod_auth_openid を使ってみる。

ubuntu でのインストールは以下のような感じ。

    sudo aptitude install libapache2-mod-auth-openid
    sudo a2enmod authopenid

apache のセットアップは以下のような感じでとりあえず動いた。

    <Location />
      AuthType OpenID
      Require user https://login.ubuntu.com/+id/hPQWPsH
      AuthOpenIDTrusted ^https://login.ubuntu.com/\+openid
      AuthOpenIDCookiePath /
    </Location>

Require user の id の値は、launchpad へログインしたときのページのソース中の、openid.delegate の値で知ることができる。
この状態では、OpenID URL を手入力しなければならないが、ログインページを別途設定することで、ログインの手順をより簡単にすることもできる。
デフォルトでは認証クッキーはブラウザを閉じる時までが有効期限らしい。これを設定するには、以下のディレクティブを利用する。

> AuthOpenIDCookieLifespan: The number of seconds that the session cookie should live after being set. Default: If the cookie lifespan is not set than it will expire at the end of the browser session (when the browser is closed).

- [第4回 モジュールでOpenIDを簡単に実現！ - ThinkIT](http://thinkit.co.jp/article/120/4)
- [Using mod_auth_openid with Ubuntu SSO](http://rtg.in.ua/2011/11/12/modauthopenid-and-ubuntu-sso/)
- [Introduction : The Apache OpenID Module](http://findingscience.com/mod_auth_openid/)