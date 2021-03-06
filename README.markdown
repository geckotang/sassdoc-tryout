jsDocみたいなものをsassでもやりたかったので、[sassdoc](https://github.com/eoneill/sassdoc)とやらを使ってみるサンプル

## Setup

``` sh
bundle install --without=production --path vendor/bundle
```

## Build sassDoc

``srcディレクトリ``以下にあるscssファイルをもとに、``docsディレクトリ``にドキュメントを作成します。

ドキュメントのタイトルは``俺の考えた最強のドキュメント``とします。

```
bundle exec sassdoc src -d docs -n '俺の考えた最強のドキュメント'
```

### ディレクトリ構成

```
┣docs
┗src
　┗ style.scss
```

## 気づいたこと

### ドキュメント用のテンプレート

- docs/tmpl/*.tmpl

### 生成されたドキュメントの見方

- file://...で表示するとうまくいかない。
- ``docs/js/lib/sassdoc.js``がscssファイルをパースして、docs/sassdoc.jsonを生成している。
- そのjsonを↑が$.getJSONしているので、localhostなりのサーバー上で見る必要があります。

### scss記法にで書くと…

``@usage``を使って使用方法を明示したいが、sass記法で想定されているので、scss記法の``@include``だとうまくパースされない。

なので大文字の＠を使うかsass記法で書く。

``` scss
// @usage:
// .link.disabled{
//  @include pointer-events(none);
// }
```
↓うまくいかないので大文字に…（当たり前か…

``` scss
// @usage:
// .link.disabled{
//  ＠pointer-events(none);
// }
```

さすがに大文字だと気持ち悪いので…sass記法で書くとドキュメントには@includeで表示される。

``` scss
// @usage:
// .link.disabled{
//  =pointer-events(none);
// }
```

``` css
.link.disabled{
@include pointer-events(none);
}
```

