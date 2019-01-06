# Token

Ciname 4D は、レンダリング設定の保存ファイル名にトークンと呼ばれる変数を使用できる。

トークンを使用することにより、プロジェクト名、カメラ名、テイク名などを、個別に記述することなく保存パスに加えることが出来る。

ｰ [Use Tokens and Never Name Your Render Files Again | Greyscalegorilla](https://greyscalegorilla.com/tutorials/tokens-name-renders-c4d/?utm_source=Greyscalegorilla+Newsletter&utm_campaign=65a7e4f1c7-EMAIL_CAMPAIGN_WEEKLY_VOICE_COPY_01&utm_medium=email&utm_term=0_025cbe1576-65a7e4f1c7-390849865&mc_cid=65a7e4f1c7&mc_eid=44d1abab1a)

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



