noip2 の設定でちょっと微妙になったのでメモ

# noip2(公式クライアント)の導入

noip の公式ドキュメントは以下の場所にある。

- [How to Install the Linux Dynamic Update Client on Ubuntu](http://www.noip.com/support/knowledgebase/installing-the-linux-dynamic-update-client-on-ubuntu/)

しかし、最近の Ubuntu では起動プロセスに systemd が導入されているとのことで、systemd を使って noip を設定する方法が下記のページに掲載されていた。

- [Ubuntu 16.04 に noip を入れる - Qiita](https://qiita.com/ikeyasu/items/96e2ff8594ae1f8beeab)

# ddclient の導入

上記説明に従えば、systemd 配下で noip2 が起動されるようになるが、２回目以降の更新がうまくいっていないよう。原因は不明だが、インターフェースのIPアドレスが変更されないとアップデートが行われないのかもしれない。noip の無料プランは 30 日間更新がないとホスト名が削除されてしまうので、IP アドレス情報の更新を強制的に実施できるクライアントとして、ddclient を試してみた。

    apt-get install ddclient

インストールしてから下記のページの情報に従って設定ファイルを書き換えると、とりあえず、起動は行われた様子。しばらく様子見。

- [DebianでDDNSを使う（No-IPとddclient） - Qiita](https://qiita.com/juze9/items/745dbbb88ca3e081885b)


（追記）しばらく走らせてみたところ。ddclient 自体は動作している様子だが、noip からのドメイン継続利用の確認メールは定期的に届いている。完全に放置することはできないのかもしれない。




