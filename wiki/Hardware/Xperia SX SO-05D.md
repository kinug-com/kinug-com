# Root 化

以下の方法で成功。

- [キューブ実験室: 【Xperia】各機種ワンクリックroot取得【Z SO-02E / A SO-04E / UL SOL22 等】](http://cubeundcube.blogspot.jp/2013/11/xperiarootz-so-02e-so-04e.html)

以下を参考に不要なアプリを削除。

- [Xperia SX (SO-05D) アプリ凍結リスト - MONO*LOG](http://blog.lunasys.net/entry/1852.php) - リンク切れ
- [【Xperia GX・SX】非rootで削除/無効にできるサービス一覧(ドコモ独自サービスほか)](http://xperia-freaks.org/2012/08/13/nvalid-docomo-service/)

# MVNO SIM でのテザリング対応

以下のＵＲＬに手順が記載されている。

- [【要root】Xperia Z3 Compact SO-02GでドコモのテザリングAPNの制限を解除する方法 | でこにく](http://decoy284.net/2015/02/01/xperia-z3-compact-docomo-tethering-apn-lock-unlock/)
- [XPERIA GX / SXをroot化し、テザリング時の「APN強制変更」を無効化する方法。 – すまほん!!](http://smhn.info/201210-rooted-xperiagx-sx-can-disable-apn-dcmtrg)

注意点は以下の通り。

- 解説には、settings.db の global を編集するように書いてあるが、global がなく、secure に該当のレコードがあった。
- settings.db 書き換え後はリブートすると問題なくテザリング可能となった。
- このハックは改造にあたるので、自己責任。

# ネットワークモードの 3G 固定化

ダイアルアプリで「\*\#\*\#4636\#\*\#\*」を入力して、メンテナンスモードに入るのがメジャーな方法らしい。

- [Xperia GX/SX を 3G のみにする方法](http://67.org/uk/2012/09/xperia-3g-only.html)

Root 化端末では、システムのファイルを修正することで、モバイルネットワークの設定に「WCDMA Only」を表示させる方法もあるらしいが、自分の端末では、そもそも修正すべきファイルが存在せず、それらしいファイルも発見できなかった。

- [【こねた】WCDMA 表示固定する策（要Root）](http://xperia-freaks.org/2012/11/10/wcdma/)

# モバイル Suica アプリ

モバイル Suica アプリでチャージしようとすると、白い画面になってしまってどうしようもなくなってしまう症状が出た。

どうやら、「開発者オプション」の「GPU レンダリングを使用」にチェックを入れているとこうなってしまうらしい。
下記のページによると FAQ にようなものだそう。
Xperia SX 固有の症状なのかは不明。


- [Xperia SX のモバイル Suica でトラブル（→解決） | b's mono-log](http://www.mono-log.jp/archives/2012/09/xperia_sx_suica.php)
