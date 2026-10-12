---
title: "ヒンジ付き2画面モニタで、ヒンジの位置にウインドウをスナップさせた"
date: 2026-10-04
description: "上下2パネルのモバイルモニタで、macOS 標準のタイル配置がヒンジとずれる問題を Hammerspoon で解決した。"
field: software
tags: [mac]
---

上下2枚のパネルをヒンジでつないだモバイルモニタ（InnoView）を2台、MacBook につないで使っている。ウインドウをヒンジの位置で上下に分割したかったので、その設定のメモ。

## 前提

- macOS 27.0.1。
- このモニタは上下2枚のパネルを1枚の縦長の画面として出力するので、Mac からは1台のディスプレイに見える。ヒンジは画面のちょうど中央にある。
- macOS 標準のタイル配置は、メニューバーと Dock を除いた領域を二等分する。そのため、上下に分割すると境目がヒンジとずれる。Dock は片方のモニタにしかないので、ずれ方もモニタごとに違う。
- 同じモニタを2台つなぐと、macOS は名前の後ろに番号を付けて「InnoView (1)」「InnoView (2)」と区別する。この番号は、つなぎ直すと入れ替わることがあるらしい。

## 実装

Hammerspoon で、ヒンジの位置（画面全体の中央）を境に分割するようにした。上段はメニューバーの下からヒンジまで、下段はヒンジから Dock の上まで。名前が「InnoView」で始まる画面だけをヒンジ付きとして扱い、それ以外の画面は普通に二等分する。

キー操作は、リーダーキーを押してから2つ目のキーで位置を選ぶ形にした。他のアプリのショートカットと競合しないか気にするのが、リーダーキーの1つだけで済むため。

- `⌥⌘7` で待ち受けを始める。3秒で自動的に終わる。
- 続けて、⌥⌘ を離してから次のキーを押す。

        U I O    左上 / 上半分 / 右上
        J K L    左半分 / 最大化 / 右半分
        M , .    左下 / 下半分 / 右下

- `7` で隣のモニタへ移す。待ち受けは続くので、そのまま移動先で位置を選べる。スナップした状態のウインドウは、移動先でも同じ区画に収まる。
- `Space` か `Enter` で、スナップせずに確定する。`Esc` で元の位置に戻す。

右手の9キーが、そのまま画面の9区画に対応している。隣のモニタへの移動は最初 `Y` にしていたが、リーダーキーと同じ `7` にしたら迷わなくなった。⌥⌘ を押したままでも2つ目のキーが効くようにする案もあったが、動作が複雑になるので見送った。

`⌥⌘7` は、macOS と主要な純正アプリの公式ヘルプを見た範囲では使われていなかった。最初に考えた `⌥⌘Y` は、Finder の Quick Look スライドショーと重なっていた。

## 設定

Hammerspoon は Homebrew で入れた。

    brew install --cask hammerspoon

macOS 27 では、許可を出す場所の名前が変わっていた。「システム設定」→「プライバシーとセキュリティ」の「アクセシビリティ」が、「デバイスの制御とデータへのアクセス」になっている。ここで Hammerspoon をオンにする。

モニタの名前は、Hammerspoon の Console で次のように確認できる。

    for _, s in ipairs(hs.screen.allScreens()) do print(s:name()) end

設定は `~/.hammerspoon/init.lua` に書く。全文は次のとおり。

    -- ============================================================
    -- ヒンジ付きデュアルモニタ用 ウインドウスナップ設定（Hammerspoon）
    --
    -- 操作:
    --   ⌥⌘7 で待ち受け開始 → 続けて以下のキー
    --     U I O   左上 / 上半分 / 右上
    --     J K L   左半分 / 最大化 / 右半分
    --     M , .   左下 / 下半分 / 右下
    --     7       次のモニタへ移動（待ち受け継続）
    --     Space / Enter   スナップせずに確定
    --     Esc     取り消し（元の位置に戻す）
    --   何もせず TIMEOUT 秒経過で自動終了（確定と同じ）
    -- ============================================================

    -- ▼ 環境に合わせて変更する項目 ------------------------------
    local HINGE_NAME = "InnoView"   -- ヒンジ付きモニタの名前（前方一致。"InnoView (1)" "InnoView (2)" の両方に一致）
    local LEADER_MODS = {"alt", "cmd"}
    local LEADER_KEY  = "7"
    local TIMEOUT = 3               -- 待ち受け時間（秒）
    -- ▲ ---------------------------------------------------------

    local function isHinge(s)
      local n = s:name() or ""
      return n:sub(1, #HINGE_NAME) == HINGE_NAME
    end

    -- 画面 s の区画 (row, col) の矩形を計算
    -- row: "top" / "bottom" / nil(全高)、col: "left" / "right" / nil(全幅)
    local function rectFor(s, row, col)
      local full, v = s:fullFrame(), s:frame()   -- 物理全体 / メニューバー・Dockを除いた領域
      local ySplit = isHinge(s) and (full.y + full.h / 2) or (v.y + v.h / 2)
      local xSplit = v.x + v.w / 2
      local y1, y2 = v.y, v.y + v.h
      if row == "top" then y2 = ySplit elseif row == "bottom" then y1 = ySplit end
      local x1, x2 = v.x, v.x + v.w
      if col == "left" then x2 = xSplit elseif col == "right" then x1 = xSplit end
      return hs.geometry.rect(x1, y1, x2 - x1, y2 - y1)
    end

    local last = {}  -- ウインドウごとの直前のスナップ位置

    local function place(row, col)
      local win = hs.window.focusedWindow(); if not win then return end
      win:setFrame(rectFor(win:screen(), row, col), 0)
      last[win:id()] = {row = row, col = col}
    end

    -- 誤差 2px 以内なら同じ矩形とみなす
    local function near(a, b)
      return math.abs(a.x - b.x) < 2 and math.abs(a.y - b.y) < 2
         and math.abs(a.w - b.w) < 2 and math.abs(a.h - b.h) < 2
    end

    -- 画面を左から順に並べ、右隣（右端なら左端）を返す
    local function nextScreen(cur)
      local list = hs.screen.allScreens()
      table.sort(list, function(a, b) return a:frame().x < b:frame().x end)
      for i, s in ipairs(list) do
        if s:id() == cur:id() then return list[i % #list + 1] end
      end
    end

    local function sendToNext()
      local win = hs.window.focusedWindow(); if not win then return end
      local src = win:screen()
      local dst = nextScreen(src)
      if not dst or dst:id() == src:id() then return end
      local p = last[win:id()]
      if p and near(win:frame(), rectFor(src, p.row, p.col)) then
        win:setFrame(rectFor(dst, p.row, p.col), 0)   -- 同じ区画にスナップし直す
      else
        win:moveToScreen(dst, false, true, 0)         -- 相対位置のまま移動
      end
    end

    local KEYS = {
      u = {"top", "left"},    i = {"top", nil},        o = {"top", "right"},
      j = {nil, "left"},      k = {nil, nil},          l = {nil, "right"},
      m = {"bottom", "left"}, [","] = {"bottom", nil}, ["."] = {"bottom", "right"},
    }

    -- ---- 待ち受けモード ----------------------------------------
    local MSG = "U I O / J K L / M , .   7: 次のモニタ   Space/Enter: 確定   Esc: 取消"
    local mode = hs.hotkey.modal.new(LEADER_MODS, LEADER_KEY)
    local timer, alertId, origWin, origFrame

    local function showHint(screen)
      if alertId then hs.alert.closeSpecific(alertId) end
      alertId = hs.alert.show(MSG, {}, screen or hs.screen.mainScreen(), TIMEOUT)
    end

    local function restartTimer()
      if timer then timer:stop() end
      timer = hs.timer.doAfter(TIMEOUT, function() mode:exit() end)
    end

    function mode:entered()
      origWin = hs.window.focusedWindow()
      origFrame = origWin and origWin:frame()
      showHint(); restartTimer()
    end

    function mode:exited()
      if timer then timer:stop(); timer = nil end
      if alertId then hs.alert.closeSpecific(alertId); alertId = nil end
      origWin, origFrame = nil, nil
    end

    -- スナップ：実行して終了
    for key, p in pairs(KEYS) do
      mode:bind({}, key, function() mode:exit(); place(p[1], p[2]) end)
    end

    -- 次のモニタへ：待ち受けを継続
    mode:bind({}, LEADER_KEY, function()
      sendToNext()
      local w = hs.window.focusedWindow()
      showHint(w and w:screen()); restartTimer()
    end)

    -- 確定：スナップせずに終了（Enter のほか、親指で押せる Space でも可）
    mode:bind({}, "return",   function() mode:exit() end)
    mode:bind({}, "padenter", function() mode:exit() end)
    mode:bind({}, "space",    function() mode:exit() end)

    -- 取り消し：元の位置に戻して終了
    mode:bind({}, "escape", function()
      if origWin and origFrame then origWin:setFrame(origFrame, 0) end
      mode:exit()
    end)

    hs.alert.show("ウインドウスナップ設定を読み込みました")

## その他

作業は Claude に手伝ってもらった。BetterTouchTool のカスタムスナップ領域でもできそうだったが、同じ機種の2台を区別できるか分からなかったので、Hammerspoon にした。

- [Hammerspoon](https://www.hammerspoon.org/)
- [Mac のキーボードショートカット - Apple サポート](https://support.apple.com/ja-jp/102650) - 競合の確認に使った
- [Accessibility renamed for macOS 27 Golden Gate · keymanapp/keyman](https://github.com/keymanapp/keyman/issues/16703) - 許可の項目名が変わった件
