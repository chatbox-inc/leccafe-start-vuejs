# Vue.js の環境構築

Vue.js は JavaScript の アプリケーションフレームワークです。

Vue.js を用いることで、JavaScript を利用した複雑な Web アプリケーションを簡単に構築することができます。

Vue.js は プログレッシブフレームワークというコンセプトで、
小規模な Webサイトから、大規模な Web システムまで幅広く適用可能なように設計されており、
簡単な記述フローを学ぶことで、 Web 制作全般に適用可能な知識をみにつけることが可能になっています。

日本語化されたドキュメントが整備されているため、
英語が苦手な方でも気軽に学習を始めることができるのも人気のポイントです。

[Vue\.js](https://jp.vuejs.org/index.html)

まずは、Vue.js を用いてアプリケーションを構築するための環境開発を進めていきましょう。

## 準備

Vue.js は Node 製のビルドツールを用いて利用されるのが一般的です。

最新版の Node.js は以下のURLからダウンロードすることが可能です。

https://nodejs.org/ja/

Node.js がインストールできたら、以下のコマンドをターミナル上で実行して、
適切なバージョンの Node.js がインストールできている事を確認してみましょう。

```bash
$ node -v
```

Vue.js の環境構築には `vue-cli` と呼ばれるツールを用いたセットアップが便利です。

`vue-cli` は以下のコマンドを実行してインストールすることができます。

```bash 
$ npm install -g @vue/cli
```

::: tip
上記のコマンド実行時に `EACCESS` のエラーが表示された方は
コマンドの頭に `sudo ` をつけて実行するとエラーを出さずにインストールが可能です。
:::

::: tip
上記の `sudo` での対応方法より、スマートな本来の構成については、
[こちら](https://qiita.com/okoysm/items/ced3c3de30af1035242d) の記事を参考にして下さい。
:::

`vue-cli` がインストールできているかを確認するには以下のコマンドを実行します。

```bash 
$ vue --version
```

本ドキュメントでは vue-cli version 3 以降を前提で進めています。

## vue-cli を用いた環境構築

`vue-cli` がインストールされた環境では、以下のコマンドを実行して
Vue.js のプロジェクトを作成する事ができます。

```bash
$ vue create my-app
```

`my-app` の部分はフォルダ名になります。

コマンド実行途中に選択子が表示されますが、デフォルトのままでOKです。

コマンドが完了すると、`my-app` フォルダが出来上がっているので、
移動して、好みのエディタで開いてみましょう。

```bash 
$ cd my-app
```

生成されたファイルは以下のような形になっているはずです。

```
├── node_modules       ## 環境構築に必要な Node モジュール群
├── public             ## Web に公開する画像や HTMLなどの格納先
├── src                ## JavaScript の格納先
├── babel.config.js    ## babel の設定ファイル
├── package-lock.json  ## Node モジュールの管理に必要なファイル
└── package.json       ## Node モジュールの管理に必要なファイル
```

生成されたプロジェクトは `babel` や `webpack` を用いて開発を進めることが可能な
フロントエンドプロジェクトの構成となっています。

- `babel` 最新のJavaScript 文法を複数のブラウザで実行可能に変換するためのモジュール
- `webpack` babel などの変換処理を自動で行うためのモジュール

プロジェクトが生成された後は、`npm run serve` コマンドを実行することで
開発用サーバを立ち上げて開発を始めることができます。

```bash
$ npm run serve
``` 

`http://localhost:8080` にアクセスすると Vue.js のロゴが表示されるはずです。

例えば、`src/App.vue` を編集することで、リアルタイムにHTMLの変化が反映されることを確認してみましょう。

`npm run serve` で立ち上げた 開発サーバは Ctr + C で解除できます。

## プロジェクトのビルド

生成されたプロジェクトを Web で公開するには、 `npm run build` コマンドを使用します。

```bash
$ npm run build
```

src フォルダ内に格納された JavaScript などは、
Webpack でビルドしてからブラウザ上で実行されることを想定されているため、
実際のサーバ上でサイトを公開する際には、コマンドを用いたビルドを行う必要があります。

上記のコマンドを実行すると、 `dist` フォルダが作成されます。

この `dist` フォルダをサーバ上にUPすることでWebサイトを公開することができるようになっています。

## 検証ツールの追加

Vue.js を利用したプロジェクトを進める際に、便利な検証ツールがあります。

Chrome ストアからインストールすることができるため、是非導入してみましょう。

https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd?hl=ja

上記の拡張機能をインストールして、エントリーファイルで以下の記述を加えると、
検証ツール起動時に 「Elements」「Console」「Network」などのタブと並んで
「Vue」のタブを確認することができます。

```js
Vue.config.devtools = true
```

「Vue」タブでは、Vue.js 内部の変数の値などをブラウザで実行しながら確認することができます。

## vue cli の活用

プロジェクトの ルートに `vue.config.js` を設置して変更を記述することで、
プロジェクトに様々な設定を加えることができます。

### ページの追加

`vue.config.js` に `pages` セクションを追加することで、
ページで利用する起動ファイルや、テンプレートとして利用する HTML ファイル、dist フォルダに出力するファイル名などを
変更する事ができます。

```js
module.exports = {
  // ...
  pages: {
    index: {
      entry: 'src/main.js',
      template: 'public/index.html',
      filename: 'index.html',
      title: 'Index Page',
    },
  },
  // ...
}
```

- `entry` 必須。 Vue.js のエントリーファイル(main.js) を指定します。
- `template` テンプレートとして利用する HTMLファイル。 デフォルトは `public/index.html`
- `filename` `dist` フォルダに出力する際の名前。 省略時には キー名が利用されます。
- `title` ページのタイトルを指定することが可能です。

::: tip 
`vue.config.js` で各ページのタイトル要素を変更する場合、
テンプレート内では `<title><%= htmlWebpackPlugin.options.title %></title>` のようにタイトルタグを記述する必要があります。
:::

vue cli で作成するプロジェクトで複数のページが必要な場合、`pages` に複数のページ情報を記述して
複数のページ構成を出力することも可能です。

設定で単純にエントリーファイルを指定するだけなら、以下のように省略形式で記述することも可能です。

```js
module.exports = {
  // ...
  pages: {
      // ...
      "event/spring": "src/event/spring/main.js",
      "event/summer": "src/event/summer/main.js",
  },
  // ...
}
```
