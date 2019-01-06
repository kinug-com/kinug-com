Toshiba AC100(Dynabook AZ) に関するメモ

# Ubuntu 化したときに便利なアプリケーション

## indicator-multiload

cpu メモリなどなどの状態を画面の上にグラフ表示


## brightside

画面の端でクリックしてワークスペースの切り替え

画面の端にマウスを置くことで画面を暗くしたりスクリーンセーバを起動（起動抑止）したりもできる。

これ欲しかったんだよなぁ。

    $ aptitiude install brightside

でインストールできた。[システム]→[設定]にメニューが出てこなかったので、手動で、

    brightside-properties

を実行して設定を行った。


## brightness indicator

https://launchpad.net/~indicator-brightness/+archive/ppa

簡単操作で画面の輝度を変更する。python なので AC100 でも動く。


## xmonad

haskell 製のウィンドウマネージャ。軽くて快適。


## midori

軽量ウェブブラウザだが、xmonad 上ではツールバーの表示がおかしい


# その他

* 開発に気合が入ってなかったのか、リファレンスマシンのような素晴らしい出来栄え
* 割とパッケージが揃っているので、概ね普通の Ubuntu マシンとして使える
* Tegra とはいえ Ubuntu はちょっと重荷。Atom ネットブックの Ubuntu 化より重い
* グラフィック周りの動作が少々怪しい
* Ubuntu 12.04 では、Tegra のサポートがもうちょっと期待できるかも？
