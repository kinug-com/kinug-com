# 概要

[WebExtensions](https://developer.mozilla.org/ja/Add-ons/WebExtensions/What_are_WebExtensions) とは、クロスブラウザで動作可能なアドオンの開発するための技術。Chrome や Opera 、Microsoft Edge などとも互換性があるため、クロスブラウザのアドオンの作成が容易になるらしい。

ここでは、Firefox アドオンの開発についてメモする。

# 基本情報

- [WebExtensions - Mozilla | MDN](https://developer.mozilla.org/ja/Add-ons/WebExtensions) - Mozilla 公式リファレンス。基本的なチュートリアルやサンプルコードもある。

# パッケージ化とインストール

開発中のパッケージを一時的に読み込む場合は、[about:debugging](about:debugging) ページから読み込みができる。Firefox を再起動するまで、アドオンを使用することが出来る。継続的に使用する場合は、パッケージ化する必要がある。Firefox Nightly や Firefox Developer Edition を除き、パッケージ化したアドオンを読み込ませる際には、アドオンの xpi ファイルに署名する必要がある。

WebExtensions アドオンの場合は、[AMO](https://addons.mozilla.org/ja/firefox/) を通じて署名をするか、[web-ext](https://developer.mozilla.org/ja/Add-ons/WebExtensions/Getting_started_with_web-ext) ツールを用いて署名を行うことが出来る。

- [パッケージ化とインストール - Mozilla | MDN](https://developer.mozilla.org/ja/Add-ons/WebExtensions/Packaging_and_installation)
- [Signing and distributing your add-on - Mozilla | MDN](https://developer.mozilla.org/en-US/Add-ons/Distribution)

# web-ext コマンドを用いたパッケージの作成と署名

web-ext コマンドを用いてパッケージを作成する際には、カレントディレクトリをパッケージの構成ファイルのあるディレクトリに移して web-ext build コマンドを実行すると、作業ディレクトリ配下に web-ext-artifacts というサブディレクトリが作成され、その中に作成されたパッケージ（zip ファイル）が保存される。

    $ cd /path/to/addon
    $ web-ext build

パッケージに署名を行う場合には、同様にカレントディレクトリをパッケージの構成ファイルのあるディレクトリに移して web-ext build コマンドを実行する。この場合、コマンドライン引数として api-key と api-secret を指定する必要があるが、これらは、[Manage API Keys :: Developer Hub :: Add-ons for Firefox](https://addons.mozilla.org/en-US/developers/addon/api/key/) ページで作成する必要がある。web-ext sign コマンドを実行すると、AMO に通信が行われ、アドオンの署名が行われる。web-ext build コマンドと同様に、作業ディレクトリ配下に web-ext-artifacts というサブディレクトリが作成され、その中に署名済みのパッケージ（xpi ファイル）が保存される。

    $ cd /path/to/addon
    $ web-ext sign --api-keys=kkkkkkk --api-secret=sssssss

# AMO(addons.mozilla.org) を通じたパッケージの署名

作成したパッケージは、[Mozilla](https://addons.mozilla.org/ja/developers/addon/submit/2) へアップロードすることで、自動検査ののちに署名済みのパッケージを得ることが出来る。前述の web-ext コマンドを用いた署名もコマンド内部で addons.mozilla.org へパッケージをアップロードして署名を行っているよう。アップロードする際にパッケージを公開（Listed）とするか非公開（Unlisted）とするかを選択できるので、プライベートなパッケージの場合でも AMO に掲載せずに署名することが出来るが、署名を行えるのは Mozilla だけなので、Mozilla に送信せずにパッケージを署名することはできない。
