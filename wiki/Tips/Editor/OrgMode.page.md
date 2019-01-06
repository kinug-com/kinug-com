OrgMode は Emacs 上でノート・TODO リストの管理などを行うモードで、シンプルなテキストベースのアウトライナーを基本としていて、可搬性の高さが売りらしい。

# セットアップ

デフォルトでも org-mode は使えるようだが、最新版を入れると新しい機能が使えるらしい。

    $ sudo aptitude install org-mode

以下のように設定した

    (require 'org-install)
    (add-to-list 'auto-mode-alist '("\\.org\\'" . org-mode))
    (add-hook 'org-mode-hook 'turn-on-font-lock) ; not needed when global-font-lock-mode is on
    (global-set-key "\C-cl" 'org-store-link)
    (global-set-key "\C-cc" 'org-capture)
    (global-set-key "\C-ca" 'org-agenda)
    (global-set-key "\C-cb" 'org-iswitchb)
    
    (setq org-log-done 'time)            ; DONE時にタイムスタンプ
    (setq org-use-fast-todo-selection t) ; TODO項目の入力補助
    (setq org-export-latex-classes nil)
    
    ;; ローカルのデータ用ディレクトリ
    (setq org-directory "~/org")
    
    ;; TODO項目
    (setq org-todo-keywords
     '((sequence "TODO(t)" "STARTED(s)" "WAITING(w)" "|" "DONE(x)" "CANCEL(c)")
       (sequence "APPT(a)"                           "|" "DONE(x)" "CANCEL(c)")))

# MobileOrg

MobileOrg を利用すると、org-mode のファイルを Android/iOS を含む複数のデバイス間で同期できる。

.emacs にて同期用のディレクトリやファイル名の設定を行う

    ;; 同期用ディレクトリ
    (setq org-mobile-directory "~/public_html/dav/MobileOrg")
    
    ;; 同期するファイル
    (setq org-agenda-files `("~/org/todo.org"))
    
    ;; MobileOrgで新規作成したファイルの保存先
    (setq org-mobile-inbox-for-pull "~/org/flagged.org")
    
* Android アプリの MobileOrgNG を WebDAV で同期できるか試してみたが、先に DropBox で同期したのちに、同じファイルを DAV で同期させなければ上手くいかなかった。
* Apache の mod_dav を用いた場合ファイルの所有者やパーミッションが上手く設定できないため、エラーが出たら手動で修正する必要がある（モバイル→サーバの場合のみ）。
* Android 版 MobileOrgNG の場合、設定したタグを上手く認識しない（バグ？）
* Android 版の MobileOrg アプリは ssh 経由での同期（鍵認証可）をサポートしたので、設定はこれが楽。

# カスタマイズ

* [org-mode+MobileOrg(+Dropbox)でGTD始めました。オレオレ改造をごっそり公開](http://d.hatena.ne.jp/holidays-l/20110719/p1)

