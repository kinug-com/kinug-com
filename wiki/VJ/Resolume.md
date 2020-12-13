VJ ソフト Resolume に関するメモ

# AfterEffects / Premiere プラグインのエラーについて

Resolume をインストールする際に、AfterEffects / Premiere から DXV3 コーデックのファイルを書き出すためのプラグインがインストールできる。

しかし、インストールの際に AfterEffects / Premiere / MediaEncoder を起動していると、エラーが発生してしまい、単純に再インストールしても、インストールエラー時のゴミが邪魔をしているようで、エラーが解決できない問題が発生した。

この場合、Resolume を再インストールする前に、以下のフォルダを削除すると、綺麗にプラグインがインストールできた。

    C:\Program Files\Adobe\Common\Plug-ins\7.0\MediaCore\Resolume DXV
