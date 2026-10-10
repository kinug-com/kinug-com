---
title: "wiki を日付付きの記事一覧に作り替えた"
date: 2026-10-10
description: "主題別の wiki をやめて日付順の記録に切り替えた経緯、日付の推定方法、Jekyll での構成。"
field: programming
tags: [web, git]
redirect_from:
  - "/wiki/2026/wiki-reorganize/"
---

このサイトの構成を見直したのでメモ。

## 経緯

2011年頃に [Gitit]({% post_url 2011-01-01-gitit-setup %}) で wiki を立てて、2019年1月に [GitHub Pages]({% post_url 2019-01-07-github-pages-custom-domain %}) へ移した。移行後も主題別の wiki の形のままにしていたが、ページを数えてみると、2019年の移行以降に更新したのは 50 ページ中 9 ページだけだった。

主題別の wiki は「書いてあることは今も有効」という前提で読まれるが、実際にはそこまで手入れできていない。技術の変化の速さを考えると、知識を集約して最新に保つ場所にするのはそもそも無理があった。

そこで、各ページを「その時点の記録」として扱う形に切り替えた。日付を付けて新しい順に並べ、話題ごとの整理はタグで行う。書いた時点では正しかった、ということだけを約束する形なら、今の更新頻度でも守れる。

## 日付の推定

困ったのは日付。Gitit はページを内部の Git リポジトリで管理していたが、2019年の移行ではページのファイルだけをコピーしたので、編集履歴が残っていなかった。GitHub 側の履歴も 2019-01-06 の一括コピーから始まっている。昔のバックアップも整理してしまっていた。

仕方がないので、次の手がかりから推定した。

- 本文中の年や、リンク先 URL に含まれる日付。リンク先が公開された年なので、書いたのはそれ以降になる。
- Internet Archive に残っていた 2015年10月の Front Page。ここに載っているページは、遅くとも 2015年10月には存在していたことになる。
- 同じく Internet Archive に残っていた Gitit の Recent activity。表示は直近 1 か月ほどだけだが、2 ページの作成日が確定した。
- 端末やソフトの発売時期。

推定した記事は「2012年頃（推定）」のように年だけを表示している。手がかりが少ないものは「2011〜2015年頃」のように幅で書いた。

- [kinug.com/wiki/（2015年10月5日）](https://web.archive.org/web/20151005000637/kinug.com/wiki/) - Internet Archive に残っていた Gitit 時代の Front Page

## 構成

GitHub Pages 標準の Jekyll ビルドのままで組んだ。GitHub Actions も使っていない。年に数回しか更新しないので、放っておいても壊れにくいことを優先した。

最初は Material for MkDocs を検討した。見た目はよかったが、2026年11月で保守が終わるとのことで見送った。

- [End of life on November 5, 2026 · squidfunk/mkdocs-material](https://github.com/squidfunk/mkdocs-material/issues/8523) - Material for MkDocs の保守終了の告知
- [Dependency versions &#124; GitHub Pages](https://pages.github.com/versions/) - GitHub Pages で使える Jekyll とプラグインのバージョン

### 記事

記事は `_posts/` に `年-月-日-名前.md` の形で置く。先頭に次のような設定を書く。

    ---
    title: "Xperia SX SO-05D を root 化してカスタマイズした"
    date: 2012-01-01
    date_label: "2012年頃（推定）"
    updated_label: "2015年9月15日"
    description: "root 化、MVNO SIM でのテザリング、3G 固定、モバイル Suica の不具合。"
    field: hardware
    tags: [android]
    ---

`date_label` を書くと、日付の代わりにその文字列を表示する。正確な日付が分かる記事では省略する。`updated_label` は後から書き足したときの日付。`field` は記事の分野で、舞台映像・音響、映像制作、プログラミング、ハードウェア、ソフトウェアのどれか一つを書く。一覧では分野が色付きで表示され、分野ごとに絞り込める。分野の一覧は `_data/fields.yml`、タグの表示名は `_data/tags.yml` にまとめた。

一覧、年別、タグの各ページは Liquid で記事から生成している。5 年以上前の記事には「当時の記録」の注記が自動で付くようにした。

### 旧 URL の転送

古い URL にリンクしてくれているページがあるかもしれないので、jekyll-redirect-from で転送を入れた。GitHub Pages 標準のプラグインなので、そのまま使える。

    redirect_from:
      - "/wiki/Hardware/Xperia SX SO-05D.html"
      - "/wiki/Tips/Android/Xperia SX SO-05D/"

Gitit 時代の URL は拡張子がないので、そのまま書くと拡張子なしのファイルが出力されてしまう。末尾に `/` を付けると `index.html` になり、転送ページとして動いた。

- [jekyll/jekyll-redirect-from](https://github.com/jekyll/jekyll-redirect-from) - 転送用のプラグイン

### 本文の修正

本文は基本的にそのまま移した。ただ、リンク文字列に `|` が入っている行が表として崩れていたので、`&#124;` に置き換えた。Jekyll の Markdown 処理系（kramdown）が表の区切りと解釈するらしい。

## 消したページの復元

2019年の移行時に整理して消したページが 6 本あった。Git の履歴に残っていたので、内容が少ないものも含めて記事として戻した。

- Viliv S10 Blade に Windows 8 Consumer Preview を入れた記録
- enchantMOON 関連のリンクと開発情報
- ウィルコムのツールとメール設定
- Windows 10 の高 DPI 設定、Windows での Git

最後の 2 本は他のページに統合していたので、統合先から外して独立させた。

## その他

作業は Claude Code に手伝ってもらった。ページの棚卸し、日付の推定、変換スクリプトの作成までまとめて進められたので、手作業よりかなり早く済んだ。

まだ残っていること。

- 古い記事のリンク先は、かなり切れていると思われる。

## 追記

その後、サイト名を kinug/notes に、URL を /wiki/ から /notes/ に変えた。日付順の記録にしたことで、wiki らしさがほとんどなくなったため。古い URL は、Gitit 時代のものも含めて新しい URL へ転送している。

あわせて、記事ごとに分野を一つ付けて一覧で色分けし、トップページを分野ごとに最近の記事を並べる形にした。
