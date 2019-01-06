Viliv S10 Blade に Windows 8 Consumer Preview をセットアップする

# セットアップ

## インストール

* プリインストールの Windows 7 上で Windows8-ConsumerPreview-setup.exe を実行した
* セットアップには 16GB の空き容量が必要(ISO から入れる場合は条件が違うはず)
* ドライブのフォーマットなどは行われず、元の OS は C:\Windows.old に残された

## グラフィックスドライバ(GMA 500)

* ドライバはインテルのサイトから [Windows 7 用](http://downloadcenter.intel.com/Detail_Desc.aspx?agr=Y&DwnldID=19155&ProdId=3001&lang=eng&OSVersion=Windows%207%20(32-bit)*&DownloadType=%0ADrivers%0A)のものをダウンロードして使用
* インストールプログラムの Setup.exe のプロパティから Windows 7 互換モードを設定して実行した。

## タッチパネル

* eGalaxTouch ドライバは上手くインストール出来なかったが、セットアップ後の素の状態でタッチパネル自体は反応している
* Control Panel -> Hardware and Sound -> Tablet PC Settings -> Calibrate the screen for pen or touch input からキャリブレーションしてタッチパネルを使用できた。

## Wifi

* 素の状態では動作していない。
* [ここ](http://www.distant-earth.com/htc-shift/Marvell-SDIO-WiFi-W8-HTC-Shift.zip)のドライバをインストールして動作した

## 3G

* Dynamism.com の情報によるとコントローラは Huawei 製らしい:Internal 3G (Huawei EM770W: HSPA/UMTS 850/900/1900/2100MHz, HSUPA: 5.76Mbps (UL) / HSDPA: 7.2Mbps (DL)) 
* [Huawei EM770](http://3g-modem.wetpaint.com/page/Huawei+EM770)
* [これ](http://www.wireless-driver.com/em770w-3g-wireless-windows-driver-utility/)を試してみた。ドライバとダイアルアップ用のツール（Mobile-Partner）がインストールされ、デバイスも認識された。
* [設定方法の参考](http://yukotan.blogspot.com/2010/05/docomosim.html)

## Bluetooth

* 素の状態で動作した。

## WebCam

* 動作していないよう。Windows 7 用のドライバを入れてもダメみたい

## 関連サイト

* [Windows 8 Consumer Preview](http://windows.microsoft.com/ja-JP/windows-8/consumer-preview)
* [Installing Windows 8 Preview on the HTC Shift](http://blogs.distant-earth.com/wp/?p=313)
* [Installing Windows 8 on the Viliv S10 Blade](http://quinxy.com/technology/installing-windows-8-on-the-viliv-s10-blade/)

