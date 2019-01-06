# Kindle DX (2nd Gen)

文鎮化からの復旧をしたのでメモ

## 状況

- 端末は kindle DX (2nd Gen) の国際版
- ファームウェアバージョン 2.5.2 に UFHi 0.35 を適用した状態だったところ、フォントハックのアンインストールをせずにファクトリリセットをしてしまい、文鎮化。

    /etc/prettyversion.txt:Kindle 2.5.2 (~~otaVersion~~) + UFHi 0.35

## シリアルコンソール作成

使用したパーツ

- [aitendo ○FFC変換基板(2W) [FFC40(0.5)-2.54DIP]](http://www.aitendo.com/product/4172)
- [aitendo FFCフラットケーブル FFC(0.5)40P55S-H-HDD](http://www.aitendo.com/product/990)
- [秋月 ＦＴ２３２ＲＬ　ＵＳＢシリアル変換モジュール](http://akizukidenshi.com/catalog/g/gK-01977/]
- ヘッダピン 20x2 (適当に)
- リード線(適当に)

参考にしたページ

- [kindle Serial Console Connection - nuke - attack with nuclear weapons - archive.org](http://web.archive.org/web/20130702064252/http://d.hatena.ne.jp/nuke/20100220/1266676412)
- [kindle2のコンソール接続追記 - 記録](http://www.ekesete.net/log/?p=489)
- [kou1のブログ: kindle 文鎮化からその後からのその後](http://kou1gl2k.blogspot.jp/2010/03/kindle_14.html)

要するに、Kindle の裏蓋を開けたところにある 40 ピンの ZIF コネクタに次のようなシリアルコンソールの信号が出ているので、これを秋月の USB シリアル変換モジュールに接続するだけ。一方の TxD と他方の RxD に交差して接続しなければならないことに注意。これで少しはまった（笑

- GND:1番と40番
- TxD:4番
- RxD:5番

## 環境設定

### 端末ソフト

端末ソフトは何でもよい。設定は、115200bps/8bit/No Parity とすること。上記結線に間違いがなければ、特に苦労することもなく端末にコンソールのログが表示されるはず。

### root 権限取得

システムをいじるために、root 権限を使用する必要がある。以下のページを参考に root 権限を取得した。

シェルの起動

    uboot>setenv bootargs $(bootargs_base) root=/dev/mmcblk0p1 rw init=/bin/sh
    uboot>bootm 0xa0060000

ファイルシステムを書き込み可能に再マウント

    #mount -n -o remount,rw /

root パスワードの初期化

    #cd /etc
    #/bin/vi shadow # ←ここでroot 行のパスワード部分をすべて削除して空にする。

参考ページ

- [kindleのrootを取得 - 記録](http://www.ekesete.net/log/?p=411)

### 再起動地獄からの脱出

そのままの状態では、Kindle framework が何度も再起動したり、システムがホルトしたりして、まともにコンソールが使えない。以下の手順でそれを抑止できた。

    touch /mnt/us/DONT_HALT_ON_REPAIR /mnt/us/system/dont-restart-framework

- [kindle起動時の文鎮化要因を改めて見直してみる - 記録](http://www.ekesete.net/log/?p=896)

### コンソールが短時間で停止してしまう症状の抑止

ここまでで、そこそこコンソールは操作できるが、一定時間たつと接続が遮断されて何も操作できなくなってしまう。安心して操作するためには、以下の設定をする必要があるらしい。

    touch /mnt/us/DONT_KILL_UART

ただし、通電中はバッテリを消費するので、充電しながら行うのが良い。

参考ページ

- [kindle2とPCの接続について(勝手に接続が切れる問題もたぶん解決) - 記録](http://www.ekesete.net/log/?p=417)

## 文鎮からの復旧

文鎮からの復旧は状況により方法が異なるので、私が行ったことを記録。

- [kindle文鎮化からの復活 - 記録](http://www.ekesete.net/log/?p=851)

上記ページのコメント欄の、ichinomoto:2010年9月29日 02:56 の発言でリンクされている [uninstall.sh](http://www.ekesete.net/kindle/files/uninstall.sh) の内容をシリアルコンソール経由で流し込み、実行したところ、無事に起動するようになった。

    # cat > uninstall.sh
    # chmod +x uninstall.sh
    # ./uninstall.sh
    # ./uninstall.sh
    ./uninstall.sh: line 43: cannot create /mnt/us/system/fonts/original/font.properties_md5sum: nonexistent directory
    ./uninstall.sh: line 44: cannot create /mnt/us/system/fonts/original/font.properties_md5sum: nonexistent directory
    ./uninstall.sh: line 68: cannot create /mnt/us/system/fonts/original/netfront.ini_md5sum: nonexistent directory
    ./uninstall.sh: line 69: cannot create /mnt/us/system/fonts/original/netfront.ini_md5sum: nonexistent directory

今回は、リカバリモードが使えず、マスストレージ機能が使えなかったため、上記方法をとったが、マスストレージ機能が使える場合には、アンインストーラをマスストレージ機能を利用して転送して実行する方法が手軽と思われる。あと、アンチ文鎮ハックは入れておくべき。

参考ページ

- [kindle2 文鎮からの復活最終版 - 記録](http://www.ekesete.net/log/?p=4391)

## ファームウェアのアップデート

あとで書く

- [Amazon.com Help: Kindle 2nd Generation Software Updates](http://www.amazon.com/gp/help/customer/display.html/ref=hp_navbox_top_kindledxi?nodeId=200529740)
- [Amazon.com Help: Kindle DX Software Updates](http://www.amazon.com/gp/help/customer/display.html?nodeId=201504450)

## 再度フォントハックを適用

あとで書く

- [kindle 2.5系 JailBreakとフォントハック対応 - 記録](http://www.ekesete.net/log/?p=1537)



