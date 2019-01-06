# Cinema 4D

## スクリプティング関係の情報

Cinema 4D の開発関係の情報は、Plugin Cafe などでリファレンスが配布されてはいるものの、十分な情報が網羅されていないように見える。

### リファレンス等

- [MAXON SDK SUPPORT - Development Support](https://developers.maxon.net/) - 各種リファレンス

### 情報サイト

- [村人とC4D](http://villager-and-c4d.cocolog-nifty.com/) - 色々と試されていて情報豊富です。
- [Python/C4DSDK - ThirdPartyWiki](http://www.peranders.com/w/index.php?title=Python/C4DSDK) - 閉鎖？
- [Smart-Page.net](http://www.smart-page.net/blog/category/cinema-4d/)
- [Cineversity Scripting Wiki](http://www.cineversity.com/wiki/Category:Scripting/)

### チュートリアル

- [fxphd](https://www.fxphd.com/fxphd/courseInfo.php) - C4D208: Python Scripting in Cinema 4D
- [Cineversity](http://www.cineversity.com/search/tutorials/search&date-from=20110801) - Utilized:Python Process:Scripting でフィルタすると、いくつかチュートリアルがまとまっている。

- [Cinema 4d Tutorial 05 - Python SDK and plugins](https://vimeo.com/33497410) - Michael Auerswald による Python で作成したプラグインの比較的丁寧な解説。

### デバッグ

- [Debugging Cinema 4D Python Plugins with PyDev](http://www.smart-page.net/blog/2011/05/29/debugging-cinema-4d-python-plugins-with-pydev/)


## Cinema4D Lite にプラグインをインストールする

Ciname4D Lite は、C++, Python, C.O.F.F.E.E. などの拡張 API には対応していないとのこと。

- [機能比較表](http://www.maxon.net/ja/products/general-information/general-information/product-comparison.html)

プラグインは、以下の場所に plugins ディレクトリを作成して置けばよいらしい。対応していれば、動く。

> Mac:
> Applications > Adobe After Effects CC > Plug-ins > MAXON CINEWARE AE > (CINEWARE Support) > lite
> 
> PC:
> C:Program FilesAdobeAfter Effects CCSupport FilesPlug-insMAXON CINEWARE AE(CINEWARE SUPPORT)lite

- [Rob Bellon	Install Plugins for Cinema 4D Lite (on the After Effects CC Install)](http://forums.creativecow.net/thread/2/1039216)

# Python Tips

## 呼び出しスタックの表示

traceback モジュールをインポートして、好きな場所に以下のコードを入れると、メソッド呼び出しのスタックがコンソールに表示される。プラグインのメソッドがどのタイミングで呼ばれているのかを調べるのに便利。

> traceback.print_stack()
