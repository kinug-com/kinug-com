# vue.js の基本

## vue.js の読み込み

CDN を利用する方法が一般的
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>

## ルートのテンプレートを作成

マウント対象の要素にIDを指定
>  <div id="app">
>  </div>

Vue のオプションにマウントする要素のIDを指定
>  var app = new Vue({
>    el: '#app'
>  })

## データバインディング

Vue の data オプションにプロパティを設定

> var spp = new Vue({
>   el: '#app',
>   data: {
>     message: 'Hello, vue.js!'
>   }
> })

テンプレートにマスタッシュ構文でプロパティを指定

> <div id="app">
> <p>
> {{ message }}
> </p>
> </div>

# ディレクティブ

v- で始まる特別なアトリビュートのこと。

## v-bind
要素のアトリビュートにプロパティを指定
> <input type="text" v-bind:value="message" />

## v-if
要素の表示、非表示を切り替える（非表示の場合、要素そのものが消える）
> <p v-if="toggle">hoge</p>

## v-show
要素の表示、非表示を切り替える（css の display プロパティが変わる）
> <p v-if="toggle">hoge</p>

## v-for
オブジェクトの繰り返し
> colors : ['red','green','blue']
> <ul>
>   <li v-for="color in colors">{{ color }}</li>
> </ul>

> user : {
>   firstName : 'Taro',
>   lastName : 'Yamada',
>   age : 28
> }  
> <ul>
>   <li v-for="(value, key) in user">{{ key }} : {{ value }}</li>
> </ul>

## v-on
イベント処理
> <button v-on:click="onlick">
> Click!
> </button>
>
> methods: {
>   onclick: function() {
>     ・・・・・・
>   }
> }

## v-model
双方向データバインディング
要素のアトリビュートにバインドするdataオブジェクトのプロパティ名を指定すると、
dataオブジェクト経由で要素の値を同期できる。
> <input type="text" v-model="message" />
> <input type="text" v-model="message" />

# コンポーネント
ページを構成するUI部品

コンポーネントの定義
> Vue.component('hello-component',{
> 	template:'<p>Helllo</p>'
> })

コンポーネントの使用
> <hello-component></hello-component>
