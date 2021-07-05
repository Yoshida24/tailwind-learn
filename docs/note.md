# ノート

## 最初の依存性のインストールには npm ci を利用する

```dotnetcli
npm ci
```

`npm ci`は`package.lock.json`からバージョン指定でインストールするコマンド。  
インストール時にバージョンが勝手に上がってコマンドが動かなくなってしまうことがないので、初期インストールでは`npm install`ではなく`npm ci`を使うべき。

## npm install はバージョン指定で
下記のようにバージョンを指定する。

```
npm install webpack@4.43.0 webpack-cli@3.3.11 webpack-dev-server@3.11.2
```

バージョンを指定せずインストールしたなどの理由で`package.json`の中にキャレットありの依存性(`^1.0.0`など)がある場合、同ファイルを直接編集し、バージョンの頭についたキャレットを取り除いた方が良い。

**なぜ？**  
バージョン指定なしで`npm install`すると、最新のバージョンのモジュールが入ってしまう。これにより、バージョンのよってスクリプトが動かなくなってしまうことがよくあるから。 
 
> e.g. 具体的なケース  
> - `npm install webpack` で webpack 4.0.0 をインストール
> - `package.json`の`devDependencies`には`webpack: "^4.0.0"`がある状態
> - 他にも`webpack`関連の`webpack-dev-server`などのツールやplugin、presetをインストールし、開発環境を構築。
> - その後他の人が開発に参画し、`npm install`。
この時までにwebpackのバージョンアップがあり、`webpack 5.x`がインストールされる。  
> - `webpack 5.x`向けに`webpack-dev-server`のプラグインが提供されていないため開発サーバーが使えなくなってしまう。他のplugin, preset についても同様。

## @babel/polyfillによるグローバル汚染の確認
グローバル変数に`_babelPolyfill`があれば、`@babel/polyfill`によるグローバル汚染が発生している。

## 参考
- [Github - html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin)
- [Babel7.x時代のpolyfillの設定方法とuseBuiltInsの仕組み](https://aloerina01.github.io/blog/2018-11-29-1#3-%E3%82%B0%E3%83%AD%E3%83%BC%E3%83%90%E3%83%AB%E6%B1%9A%E6%9F%93%E3%81%9B%E3%81%9A%E3%81%ABpolyfill%E3%82%92%E9%81%A9%E7%94%A8%E3%81%99%E3%82%8B%E6%96%B9%E6%B3%95)  
- [Qiita - Babel 7でTypeScriptをトランスパイルしつつ型チェックをする 〜webpack 4 + Babel 7 + TypeScript + TypeScript EsLint + Prettierの開発環境を構築する〜](https://qiita.com/soarflat/items/d583356e46250a529ed5)  
- [Qiita - typescript-eslint で js ファイルと共存する](https://qiita.com/odanado/items/066725b951fd0aacf622)