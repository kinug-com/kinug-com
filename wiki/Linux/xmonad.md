xmonad は haskell 製の軽量ウィンドウマネージャ。作りがシンプルで使い勝手が良い。

# インストール

	$ aptitude install xmonad xmobar trayer

# 設定

LXDE で xmonad を使うことにした。

~/.config/lxsession/Lubuntu/desktop.conf の２行目を[このサイト](http://www.haskell.org/haskellwiki/Xmonad/Using_xmonad_in_LXDE)に従って修正して、LXDE のウィンドウマネージャを xmonad に変更した。

    window_manager=xmonad

xmonad.hs と .xmobarrc は[このサイト](http://blog.blueblack.net/item_422)のものをほぼそのまま使用。（一応動いたっぽいが微妙に挙動が怪しい気がする。）

	--------------------------- .xmonad/xmonad.hs
	import System.IO

	import XMonad
	import XMonad.Hooks.DynamicLog
	import XMonad.Hooks.ManageDocks
	import XMonad.Util.Run(spawnPipe)
	import XMonad.Config.Desktop

	main = do
		xmproc <- spawnPipe "xmobar"
		xmonad $ defaultConfig
			{ manageHook = manageDocks <+> manageHook defaultConfig
			, layoutHook = avoidStruts  $  layoutHook defaultConfig
			, logHook = dynamicLogWithPP xmobarPP
							{ ppOutput = hPutStrLn xmproc
							, ppTitle = xmobarColor "green" "" . shorten 50
							}
			, modMask = mod4Mask
			, borderWidth = 2
			, normalBorderColor = "#333333"
			, focusedBorderColor = "#cd8b00"
			}

　

	--------------------------- .xmobarrc
		Config { font = "xft:Sans-9:bold"
			   , bgColor = "black"
			   , fgColor = "grey"
			   , position = TopW L 100
			   , lowerOnStart = False
			   , commands = [ Run Weather "EGPF" ["-t"," <tempF>F","-L","64","-H","77","--normal","green","--high","red","--low","lightblue"] 36000
							, Run Network "eth0" ["-L","0","-H","32","--normal","green","--high","red"] 10
							, Run Cpu ["-L","3","-H","50","--normal","green","--high","red"] 10
							, Run Memory ["-t","Mem: <usedratio>%"] 10
							, Run Swap [] 10
							, Run Date "%a %b %_d %Y %H:%M:%S" "date" 10
							, Run StdinReader
							]
			   , sepChar = "%"
			   , alignSep = "}{"
			   , template = "%StdinReader% }{ %cpu% | %memory% * %swap% | %eth0%   <fc=#ee9a00>%date%</fc> | %EGPF%"
			   }


# その他便利なもの
## brightness-indicator

brightness-indicator はトレイに常駐して LCD の輝度を変更できるツール。

参考ページ：
http://codevanrohde.nl/wordpress/?p=128
https://launchpad.net/~indicator-brightness/+archive/ppa

リポジトリを追加して、indicator-brightness パッケージを追加。

    $ aptitude install indicator-brightness

再起動すると、ステータスバーに表示された。

## キーボードの設定

Dynabook AZ はキー配列が特殊なので、[このあたり](http://d.hatena.ne.jp/kenbeese/20120214/title)を参考に、xev コマンドを利用してキーを設定した。

	--- .Xmodmap
	keycode 81 = backslash bar
	keycode 84 = backslash underscore
	keycode 234 = Escape

	keycode 225 = Control_R NoSymbol

	clear Lock
	add Control = Control_R
	--- .Xmodmap

Ctrl+Space で Anthy が起動してしまう問題は、iBus の設定で回避できる。

