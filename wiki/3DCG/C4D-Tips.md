# C4D-Tips

# レンダリング時間の短縮について

じっくり時間をかけてレンダリングすれば品質の高い結果が得られるが、現実問題として、品質はそこそこでいいので、早く結果が欲しい場合が多い。そこで、品質の低下を許容範囲に抑えつつ、レンダリング時間を削減する工夫が必要となってくる。

- [レンダリング時間を短縮するいくつかの方法 | isi-work](https://isi-work.com/20160122/fstcg/) - 品質を犠牲にして、レンダリング時間を短縮するコツ
- [レンダリング時間の短縮Tips | MAXON japan](https://www.maxonjapan.jp/archives/2906)


# レンダリング出力のパスを自動化する（Token）

Ciname 4D は、レンダリング設定の保存ファイル名にトークンと呼ばれる変数を使用できる。

トークンを使用することにより、プロジェクト名、カメラ名、テイク名などを、個別に記述することなく保存パスに加えることが出来る。

- [HOW TO TAKE CONTROL OF NAMING IN C4D BY USING TOKENS - LesterBanks](https://lesterbanks.com/2018/11/naming-c4d-using-tokens/)
- [Cinema 4D Tokens - Never Name a Render Again - Greyscalegorilla](https://greyscalegorilla.com/cinema-4d-tokens-never-name-a-render-again/)

# クローナの個々のオブジェクトにマテリアルを固定する

- クローナ全体に貼りたいマテリアルを適用
- クローナ→「オブジェクト」タブ→「テクスチャを固定」チェックボックスにチェックを入れる

# MoGraph エフェクタの動作について

MoGraph エフェクタはエフェクタの種類によってパラメータの適用方法が異なる。適用の方針エフェクタによって大きく異なるので、分かりづらいものがある。

- シェーダ：エフェクタに適用されたマテリアルの「チャンネル」と「テクスチャタグ」を、シェーダエフェクタの「シェーディング」タブで指定、選択したチャンネルの値に応じて、エフェクトパラメータが適用される。
- ステップ：クローナのクローンのＩＤの順にエフェクトが適用される。

# クローナーでクローンされた動画マテリアルの再生タイミングをランダム化する

現在のところ、マルチシェーダを利用した方法が見つかっている。

マルチシェーダは、複数のテクスチャをパラメータ（マテリアルのカラー等）によって切り換えて適用できるので、これを時間軸で変化させることで動画テクスチャを実現できる。マテリアルのカラーやウェイトを Mograph エフェクタで変化させると、マルチシェーダによってテクスチャが変化するので、この性質を利用する。

- [Cinema 4D Random Offset Animated Textures Using Mograph Multishader](https://vimeo.com/21302433)
  - マルチシェーダで各フレームを分離、動画を Time Effector で再生する。
  - さらに、Random Effector で再生スピードをランダムに変更する。

- [Walkthrough : Sequential Time Offset animated textures with the Mograph Multishader](https://vimeo.com/55689488)
  - マルチシェーダ利用

- [Cinema4d Advanced Production Techniques 1](https://vimeo.com/ondemand/c4dapt/90202601)
  - マルチシェーダ利用
  - 再生スピードを変更しないで再生タイミングをランダムに変更する方法について #08 で説明しているらしい。

- [クローナーと動画テクスチャのランダム再生](http://www.tmsmedia.co.jp/phpbb/viewtopic.php?f=4&t=464)
  - マルチシェーダ利用
  - 簡易エフェクタのカラーをキーフレームを打って変更 ＋ マルチシェーダの「カラーの明るさ」を設定　→　動画が再生できる
  - 上記に加えて、カラーに対してランダムエフェクタを適用する

# License Manager の JavaScript エラー「未定義または NULL 参照のプロパティ 'map' は取得できません」の回避方法

このエラーは、他のマシンですでにライセンスがアクティベートされているときに発生するらしい。
他のマシンの Cinema 4D 上でライセンスを開放するか、MyMaxon からライセンスを開放すると、エラーが解消する。


