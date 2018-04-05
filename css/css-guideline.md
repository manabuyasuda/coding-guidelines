# CSSのガイドライン

- ファイル・ディレクトリ構成
  - baseレイヤー
  - namespaceレイヤー
- 名前空間（namespace）
- 命名規則
  - パーシャルファイル
  - variantの指定方法
  - 状態変化
  - サフィックス（接尾辞）
  - キーセレクタ
- フォーマッティング
  - インポート
  - フォーマットとプロパティの宣言順
  - セレクタ
  - プロパティ・ルールセット

## ファイル・ディレクトリ構成
ファイル構成は[Enduring CSS](http://ecss.io/)（ECSS）をベースにします。ECSSの詳しい説明は[こちら](how-to-ecss.md)。

```
.
├── _print.scss
├── base/
│   ├── _base.scss
│   ├── _normalize.scss
│   ├── function/
│   ├── mixin/
│   │   ├── _Icon.scss
│   │   ├── _mq-up.scss
│   │   └── namespace/
│   │       ├── layout/
│   │       └── sitewide/
│   └── variable/
│       └── _global.scss
├── namespace/
│   ├── company/
│   ├── csr/
│   ├── faq/
│   ├── home/
│   ├── inquiry/
│   ├── ir/
│   ├── layout/
│   ├── news/
│   ├── products/
│   ├── recruit/
│   ├── results/
│   ├── sitemap/
│   ├── sitewide/
│   ├── structure/
│   └── wisywig/
└── site.scss
```

すべてのファイルはsite.cssとして統合します。  
大きくは2つレイヤーにわかれます。

- base
- namespace

### baseレイヤー
baseレイヤーは変数や関数をまとめた子レイヤーと、要素セレクタのデフォルトスタイルやNormarize.cssなどのベースになるスタイルを
指定します。

#### base/_normarize.scss
Normalize.cssです。一般的なリセットCSSは`outline`プロパティを削除してしまうなどのデメリットもあります。必要最小限の平準化だけをするNormalize.cssを使用します。

#### base/_base.scss
_base.scssは`html`タグの`box-sizing: border-box;`や`font-size`の指定などをしています。モジュールを作成するときの上書きが頻発しないように必要最小限のスタイルを指定します。

#### base/variable/
サイト共通の変数を格納します。例えば以下のようなものです。

- ブレイクポイント
- 横幅の最大値
- フォントのサイズや種類
- アニメーションの速度や種類

#### base/function/
サイト共通の関数を格納します。例えば、pxをremに変換する関数です。

#### base/mixin/
サイト共通のmixinを格納します。例えば以下のようなものです。

- メディアクエリを管理
- ウェブフォントの読み込み
- アイコンを作成
- namespaceモジュールのもとになるスタイル

サイト全体で使えるmixinはbase/mixin/_mq-up.scssのように保存します。  
namespaceモジュールの雛形になるmixinはbase/mixin/namespace/sitewide/_button.scssのように保存します。

### namespaceレイヤー
モジュールはnamespaceディレクトリ内に名前空間ごとに格納します。  
例えば以下のようなものです。

- namespace/sitewide/ ：サイト共通で場所を選ばない比較的小さなモジュール
- namespace/structure/ ：サイト共通で使う場所がある程度固定されている構造的なモジュール
- namespace/layout/ ：モジュールを配置・レイアウトするためのレイアウトモジュール
- namespace/home/ ：ホームページでだけ使用されるモジュール

## 名前空間（namespace）
namespaceモジュールにはECSSの考えをベースに名前空間（namespace）をつけます。  
sitewideとstructureとlayout以外は基本的に省略をしません。

- namespace/sitewide/ ：`.sw-`
- namespace/structure/ ：`.st-`
- namespace/layout/ ：`.l-`
- namespace/home/ ：`.home-`

見た目が似ているかだけではなく、情報設計とビジュアルデザインも含めて共通化するのが適切なのか認識をあわせてから共通化をします。  
もし、今後別の見た目や仕様になりそうなのであれば、名前空間をわけて別のモジュールとして管理します。

## 命名規則
命名規則はECSSの考えをベースにします。

```
namespace-ModuleName_ChildNode-variant
namespace-ComponentName_ChildNode-variant
```

基本的には以下のような関係性になります。

```
ModuleName > ComponentName > ChildNode
```

ただし、名前からはModuleNameなのかComponentNameなのかは判断できないことも多いのでそれほど気にする必要はありません。

### パーシャルファイル
名前空間でディレクトリをわけて、その中にModuleとComponentごとにパーシャルファイルを作ります。ChildNodeとvariantはそれに紐づくパーシャルファイルに記述します。

```scss
// namespace/sitewide/_Button.scss
.sw-Button {}
.sw-Button-primary {}
```

```scss
// namespace/home/_Intro.scss
.home-Intro {}
.home-Intro_Heading {}
```

```scss
// namespace/home/_News.scss
.home-News {}
.home-News_Heading {}
```

### variantの指定方法
variantはChildNodeに指定してもいいですが、HTMLでの管理がしにくい場合はModuleNameやComponentNameに指定することも検討してください。

```scss
.namespace-ModuleName_ChildNode {
  ...
}

// ChildNodeをChildNode-variantで上書きする
.namespace-ModuleName_ChildNode-variant {
  ...
}
```

```scss
.namespace-ModuleName_ChildNode {
  ...

  // ChildNodeをModuleName-variantで上書きする
  .namespace-ModuleName-variant & {
    ...
  }
}
```

### 状態変化
クリック時などの状態変化があった場合は以下の優先度でvariantを指定してください。

1. `aria-*`などの標準的なセレクタ
2. variant

#### `aria-*`などの標準的なセレクタ
`aria-expanded`や`aria-hidden`のような属性を使用している場合は、CSSのセレクタにも使用してください。

```scss
.namespace-ModuleName_ChildNode {
  .namespace-ModuleName[aria-expanded="true"] & {
    ...
  }
}
```

#### variant
`aria-*`などの属性がつけられない場合は、variantを指定してください。

```scss
.namespace-ModuleName_ChildNode {
  .namespace-ModuleName-expanded & {
    ...
  }
}
```

### サフィックス（接尾辞）
ブレイクポイントで変化するクラス名はサフィックスであらわします。  
サフィックスで使用するキーワードは変数のキーを使います。

```scss
$breakpoint-up: (
  'sm': 'print, screen and (min-width: 375px)',
  'md': 'print, screen and (min-width: 768px)',
  'lg': 'print, screen and (min-width: 1024px)',
  'xl': 'print, screen and (min-width: 1440px)',
) !default;
```

```scss
@include mq-up(md) {
  .l-Grid_Item {
    &-1of12Md { width: percentage(1 / 12)};
    &-2of12Md { width: percentage(2 / 12)};
    &-3of12Md { width: percentage(3 / 12)};
    &-4of12Md { width: percentage(4 / 12)};
    &-5of12Md { width: percentage(5 / 12)};
    &-6of12Md { width: percentage(6 / 12)};
    &-7of12Md { width: percentage(7 / 12)};
    &-8of12Md { width: percentage(8 / 12)};
    &-9of12Md { width: percentage(9 / 12)};
    &-10of12Md { width: percentage(10 / 12)};
    &-11of12Md { width: percentage(11 / 12)};
    &-12of12Md { width: percentage(12 / 12)};
  }
}
```

### キーセレクタ
あるセレクタのルールセット（`{}`）の中に、他のキーセレクタ（実際に適応されるセレクタ）が入らないようにしてください。

`.ns-Foo`の中に`.ns-Bar`が入っているのでNG。

```scss
.ns-Foo {
  ...

  & .ns-Bar {
    ...
  }
}
```

キーセレクターは`.ns-Bar`なので、`.ns-Bar`のルールセットの中に指定します。

```scss
.ns-Bar {
  ...

  .ns-Foo & {
    ...
  }
}
```

また、以下のようにルールセット内に他のキーセレクターを生成するのもNGです。

```scss
.ns-Foo {
  ...

  &_Bar {
    ...
  }
}
```

1つのルールセットの中でキーセレクターを閉じ込めるようにします。

```scss
.ns-Foo {
  ...
}

.ns-Foo_Bar {
  ...
}
```

## フォーマッティング
### インポート
Sassの`@import`でパーシャルファイルをインポートするときは、Globパターンでインポートして、コメントに使用している名前空間の説明を載せます。

```scss
@charset "UTF-8";

@import "base/variable/**/*.scss";
@import "base/function/**/*.scss";
@import "base/mixin/**/*.scss";
@import "base/_normalize.scss";
@import "base/_base.scss";

/**
 * .sw- (SiteWide)...サイト共通の汎用的なModule（リストやボタンなどの場所を選ばないもの）
 * .st- (Structure)...サイト共通の構造的なModule（ヘッダーやフッターのような場所が固定されるもの）
 * .l- (Layout)...コンテンツ内の余白やレイアウト専用のModule
 * .wisywig- (WISYWIG)...WISYWIG（ウィジウィグ）で入力した要素に対するスタイル
 * .home- (HomePage)...ホームページ（サイトトップページ）
 * .top- (TopPage)...カテゴリートップページ
 * .sub- (SubPage)...カテゴリー下層ページ
 * .products- (Products)...製品情報
 * .news- (News)...ニュース
 * .company- (Company)...会社案内・企業情報
 * .recruit- (Recruit)...採用情報ページ
 * .csr- (CorporateSocialResponsibility)...企業の社会的責任
 * .faq- (FrequentlyAskedQuestions)...よくある質問
 * .inquiry- (Inquiry)...お問い合わせ
 * .ir- (InvestorRelations)...投資家向け情報
 * .results- (Results)...検索結果ページ
 * .sitemap- (Sitemap)...サイトマップページ
 */
@import "namespace/**/*.scss";

@import "_print.scss";
```

### フォーマットとプロパティの宣言順
CSSの構文はセレクタとブレース、プロパティと値で構成されます。このルールセットに読みやすさを確保したフォーマット（書式）をルール化します。

#### ルールセットのフォーマット
ルールセットの基本的なフォーマットは以下の通りです。

- 複数のセレクタは別の行に（関連性があって1行が長くなり過ぎなければ同じ行に指定してもいい）
- 最後のセレクタとオープニングブレースの間にスペースを1つ
- それぞれの宣言は別々の行に
- ルールセット内に空行は入れない
- プロパティの前にスペースを2つ
- コロン（`:`）と値の間にスペースを1つ
- クロージングブレースは独立した行に
- 各ルールセットの間に空行を1つ
- 関数はカンマ（`,`）と引数の間にスペースを1つ
- ローカル変数は最初に定義
- extendはローカル変数の次に指定
- mixinはextendの次に指定
- contentを使用しているmixinは最後に指定
- 演算は常に括弧（`()`）で囲む
- 括弧内のカンマ（`,`）と値の間にスペースを1つ

```scss
// Good
.ns-Foo, .ns-Foo--Bar,
.ns-Baz {
  $_padding: 1em;
  @extend %base-unit;
  @include _clearfix;
  display: block;
  margin-right: auto;
  margin-left: auto;
  padding-right: $_padding;
  padding-left: $_padding;
  backgrouond-color: rgba(0, 0, 0, 0.7);
  @include _mq-up(md) {
    padding-right: ($_padding * 2);
    padding-left: ($_padding * 2);
  }
}

// Bad
.ns-Foo,
.ns-Foo--Bar, .ns-Baz{
    @include _mq-up_(md) {
        padding-right: $_padding * 2;
        padding-left: $_padding * 2;
    }
    display: block;margin-right: auto;
    backgrouond-color: rgba(0,0,0,0.7);
    margin-left:auto;
    @extend %base-unit;
    @include _clearfix;
    $_padding: 1em;}
```

#### 単一行ルールセットのフォーマット
原則的には複数行でルールセットを記述しますが、汎用クラスなどの単一のスタイルの場合は1行で記述することもできます。

単一行のフォーマットは以下の通りです。

- 最後のセレクタとオープニングブレースの間にスペースを1つ
- オープニングブレースとプロパティの間にスペースを1つ
- コロン（`:`）と値の間にスペースを1つ
- セミコロン（`;`）とクロージングブレースの間にスペースを1つ
- 各ルールセットの間には空行を入れない

```scss
// Good
.u-text-right { text-align: right !important; }
.u-text-center { text-align: center !important; }
.u-text-left { text-align: left !important; }
```

#### プロパティの宣言順
ルールセット内の宣言は重要なプロパティから書き始め、その機能ごとに固めて記述することで意図を素早く理解できるようにします。プロパティの宣言順は以下のようなルールで記述します。

- ボックスモデルの種類や表示方法を示すプロパティ（`box-sizing`, `display`, `visibility`, `float`など）
- 位置情報に関するプロパティ（`position`, `z-index`など）
- ボックスモデルのサイズに関するプロパティ（`width`, `height`, `margin`, `padding`, `border`など）
- フォント関連のプロパティ（`font-size`, `line-height`など）
- 色に関するプロパティ（`color`, `background-color`など）
- それ以外

アルファベット順には記述しません。プロパティをアルファベット順で書いていくのは難しいと思いますし読みにくいとも思います。例えば`position`プロパティで位置の指定をする場合に`top`と`left`が離れてしまいます。

```scss
// Good
.foo {
  display: block;
  position: absolute; // 親要素に対して、
  top: 0;
  left: 0; // 左上を基準にする。
  width: 100%;
  margin: 0;
  padding: 0;
  font-size: 0.75em;
  color: #000;
  background-color: #fff;
}

// Bad
.foo {
  background-color: #fff;
  color: #000;
  display: block;
  font-size: 0.75em;
  left: 0; // どこかにpositionがある？
  margin: 0;
  padding: 0;
  position: absolute; // positionがあった、親要素を基準にするのか
  top: 0; // 上にあわせて、他の値はなんだっただろう？
  width: 100%;
}
```

#### 個別指定プロパティの宣言順
`margin`や`position`のように上下左右に個別に指定できるプロパティは時計回りで記述をすることで規則性をもたせます。

```scss
// Good
.foo {
  margin-top: auto;
  margin-right: auto;
  margin-bottom: auto;
  margin-left: auto;
}

// Bad
.foo {
  margin-left: auto;
  margin-right: auto;
  margin-top: auto;
  margin-bottom: auto;
}
```

#### カラーコードは短縮する
colorプロパティなどで色を指定する時は可読性をあげるために、可能な場合は短縮します。

```scss
// Good
.foo { color: #ddd; }

// Bad
.foo { color: #dddddd; }
```

#### 文字列には引用符（ダブルクォート）をつける
contentプロパティやURLの指定、属性セレクタの指定をする時にはダブルクウォートを使用します。

```scss
// Good
.foo {
  content: "this is content";
  background: url("logo.png");
}
input[type="submit"] {}

// Bad
.foo {
  content: 'this is content';
  background: url(logo.png);
}
input[type=submit] {}
input[type='submit'] {}
```

#### 0に単位をつけない
値が`0`の場合は`px`や`%`といった単位は必要がないため指定しません。

```scss
// Good
.foo { margin: 0; }

// Bad
.foo { margin: 0px; }
```

ただし、角度（`deg`,`grad`,`rad`,`turn`）や時間（`s`,`ms`）では単位の省略をすることができないので注意します。

#### すべて小文字で記述する
プロパティとプロパティ値は大文字でも小文字でも同様に扱われますが、小文字に統一します。

```scss
// Good
.foo { color: #fff; }

// Bad
.foo { COLOR: #FFF; }
```

#### ショートハンドはなるべく避ける
`font-size`や`margin`などのショートハンドプロパティの使用は必要がなければ避けます。ショートハンドプロパティに渡さなかったプロパティに初期値が指定されてしまい、思わぬスタイルが当たってしまう恐れがあります。

必要なプロパティにだけ指定するほうがコードの目的も分かりやすくなります。

```scss
// Good
.foo {
  // 中央配置にする。
  margin-right: auto;
  margin-left: auto;
}

// Bad
// 上下を0に指定する必要がない。
.foo { margin: 0 auto; }
```

#### 変数にはdefaultフラグを指定する
変数を定義するときには`!default`フラグも指定します。同じ変数名の場合、先に定義した変数が適応されるというシンプルなルールになるので管理がしやすくなります。

```scss
// Good
$unit-base: 1em !default;

// Bad
$unit-base: 1em;
```

### セレクタ
#### インライン記述を使用しない
HTMLのstyle属性にスタイルを記述することは禁止します。HTMLには見た目に関する情報を極力書かないようにします。クラスを振ったほうが柔軟にスタイルを変更することができます。JavaScriptを使う場合もstyle属性を使いません。

```html
<!-- Bad -->
<div style="color:red; background-color: black;"></div>
```

#### HTMLの構造に依存させない
要素セレクタに対してスタイルを指定すると、構造が変更されたときにスタイルが崩れてしまう恐れがあります。

```scss
// Bad
article h2 {}
```

クラスセレクタであれば、指定している要素に関係なくスタイルが適応されるので、使い回しがしやすくなります。ただし、要素（例えば`h2`と`h3`）によってデフォルトのスタイルがちがうことがあるので、スタイルが崩れないようにリセットしておく必要がある場合があります。

```scss
// Good
sw-Heading2 {}
```

クラスセレクタに対して要素セレクタを連結させるのも意味がありません。

```scss
// Good
..ns-Foo {}

// Bad
h2.ns-Foo {}
```

特定の要素で指定してほしいのであれば、コメントやクラス名でそれを示します。

```scss
// Good
/* ulタグかolタグに指定してください。 */
.ns-Foo {}
```

#### IDセレクタを使用しない
CSSセレクタにIDセレクタは使用しません。ページ内リンクやJavaScriptのフックとしてだけ使います。

#### メディアクエリをまとめて管理しない
メディアクエリをまとめて指定しません。そのセレクタがどのように変化していくのかを把握しにくくしてしまいます。

```scss
// Good
.foo {}

@media (min-width: 768px) {
  .foo {}
}

// Bad
@media (min-width: 768px) {
  .foo {}
  .bar {}
  .baz {}
}
```

#### セレクタを短くする
セレクタは必ずしもHTML構造に沿う必要はありません。場合によってはセレクタを短くすることで詳細度を低く抑えることができます。

```scss
// Good
ul a {}
th {}

// Bad
ul li a {}
table th {}
```

#### divやspanをセレクタに使用しない
`div`や`span`は使用頻度が高く、パターンも一定ではありません。セレクタに指定すると意図しないところにスタイルが当たってしまう恐れがあります。`div`と`span`にはスタイルを指定せずに、クラスを振って指定します。

```scss
// Bad
div {}
span {}
.foo div {}
.foo span {}
```

### プロパティ・ルールセット
#### !importantを多用しない
`!important`の使用は禁止しませんが、多用するべきではありません。例えばシングルクラスのヘルパークラスを確実に適応されるために指定したり、外部ライブラリがJavaScriptでstyle属性を指定してしまうのを上書きするためといった場合に限って使用します。

#### スタイルをリセットしない
スタイルのリセットとは、`0`や`none`などで値を上書きすることです。スタイルのリセットが多いときは、要素にスタイルを持たせすぎていたり、早く指定しすぎている恐れがあります。

```scss
// Bad
.foo {
  border: 1px solid #ddd;
}

// ある要素内ではボーダーが邪魔になるためリセット
.ns-Bar .ns-Foo {
  border: none;
}
```

variantで必要なときにだけ指定するとリセットが起きにくくなります。

```scss
// Good
.ns-Foo {}
.ns-Foo-bordered {
  border: 1px solid #ddd;
}
```

#### 要素セレクタに装飾的なスタイルを指定しない
`h1`や`p`といったセマンティックな要素セレクタには多くのスタイル（特に装飾的なスタイル）を指定しません。必ずあとでスタイルをリセットすることになります。

```scss
// Bad
h2 {
  padding-bottom: 0.5em;
  border-bottom: 1px solid #888;
}
```

要素セレクタにはブラウザのデフォルトスタイルシートやリセットCSSで指定されるような最低限のスタイルにとどめておきます。

```scss
// Good
h2 {
  font-size: 2rem;
  line-height: 1.2;
  margin-top: 0;
  margin-bottom: 24px;
}
```

#### 高さを指定しない
レスポンシブでは横幅も高さも変化します。高さを指定していると余分な余白ができてしまったり、逆にコンテンツが隠れてしまう恐れがあります。

```scss
// Bad
.ns-Foo {
  height: 1000px;
}
```

#### 固定の幅を持たせない
コンテンツは基本的に親要素に対して横幅100%の表示になるようにします。横幅を固定すると、横幅が足りなかったり、はみ出してしまう恐れがあります。

幅を変更するときは、コンテナブロックに`max-width`を指定する、`width`のヘルパークラスを指定する、variantでサイズを指定するなどの方法があります。

```scss
// Good
.sw-Button {}

.l-Wrapper {
  max-width: 1200px;
}

.sw-Button-full {
  width: 100%;
}

// Bad
.sw-Button {
  width: 300px;
}
```

