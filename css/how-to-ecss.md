# ECSS
[Enduring CSS](http://ecss.io/)

「Enduring CSS」（以降ECSSと表記）は（ファイルサイズ、UIの数、開発者の数などが）大規模なWebアプリケーションのCSSを合理的に書くためアプローチです。すべてのケースで最適な方法ではないので、[OOCSS](https://github.com/stubbornella/oocss)や[SMACSS](https://smacss.com/ja)、[BEM](https://en.bem.info/)のようなCSS設計の手法や、[Atomic Design](http://atomicdesign.bradfrost.com/)のようなデザインシステム、[CSS Modules](https://github.com/css-modules/css-modules)のようにCSSのスコープを管理できるツールなどのアプローチも検討してください。

このドキュメントはECSSの考え方やアプローチをまとめたものです。意訳や独自に追加した箇所もあります。Webサイトでの使用を想定しています。正確な情報は[公式サイト](http://ecss.io/)を確認してください。

Enduringは、「長続きする、永続的な、不朽の、我慢強い、辛抱強い」という意味を持っています。  
OOCSSのようなオブジェクト指向のCSSアプローチは再利用性やファイルサイズの削減というメリットはありますが、「小さなコンポーネントを組み合わせる複雑さ」「コンポーネントの影響範囲が大きいので変更・削除がしにくい」というデメリットもあります。

ECSSでは複雑で抽象度の高いオブジェクト指向ではなく、複雑さを避けて、管理をしやすく、CSSの寿命を長くすることを目指したアプローチをとります。100を70にするのではなく100は100のままで管理をしたり、大きな塔ではなく中規模のビル群を作っていくイメージです。  
OOCSSが[DRY](https://www.google.co.jp/search?q=Don%27t+repeat+yourself&gws_rd=cr&ei=BkiRWOqJF8uh8QXZzrOAAg)（Don't repeat yourself：繰り返しを避けること）を目指しているのとは対照的に、ECSSでは繰り返しを許容し、適切に分離（decoupling）することを目指しているのが特徴です。

## ECSSで使用される用語の定義
同じ言葉でも違った意味で解釈されてしまうことがあります。これを避けるためにECSSで使用される言葉とその意味を定義します。

- コンテキスト：前後の文脈や状況。
- スコープ：有効になる範囲。
- キーセレクタ：セレクタのいちばん右側にある実際にスタイルが適応されるセレクタ。
- 上書き（オーバーライド）：キーセレクタの値が継承に基づいて意図的に修正される状況。
- 接頭辞：クラス名の先頭につける識別子やベンダープレフィックス。 例：`tp-`, `-webkit-`。
- オーサリングスタイルシート：PostCSSやSassなどのスタイリングルールを作成するファイル。
- CSS：ツールによって生成され、最終的にブラウザで解釈されるCSSファイル。

## ECSSが目標とすること
ECSSは主に、CSSのシンタックスにあるブレース（`{}`）の外側をどのように扱うのかに主眼をおいています。そして、プロパティというブレースの内側のDRYではなく、キーセレクタこそが本当にDRYにすべきものだとしています。
そのため、変数や最低限のmixinなどで共通化はするものの、ブレースの内側をDRYにすることには「見て見ぬ振りをすることが大切」だとも言っています。

### キーセレクタをDRYにする
キーセレクタのDRYとは、コンテキストが違うコンポーネントは既存のコンポーネントと似ていても別のものとして考えること、既存のコンポーネントを（OOCSSのStrucrureやmixinなどで）抽象化したり拡張して表現しないということです。そして、キーセレクタは1つのブレースの中で完結している（DRYである）必要があります。

抽象化したモジュールは変更に対する影響範囲が必然的に大きく、扱いづらくなってしまいます。多少冗長であっても、キーセレクタをDRYにすることで、変更しやすく削除しやすいモジュールにすることが、大規模なプロジェクトを長期間にわたって管理し続けるために大切なことだとしています。

## モジュールと命名規則
他のCSSの手法と同じく、ECSSも特定のスコープでUIを分割します。分割したUIをモジュールとよび、モジュールは以下の要素から構成されます。

- namespace（名前空間）：クラス名の衝突を防ぐための接頭辞で、コンテキストやモジュールをあらわす。
- Module（モジュール）：視覚的に認識できる個別の機能領域のもっとも大きな区分。
- Component（コンポーネント）：Moduleに含まれる機能性を持つ部品。
- ChildNode（子ノード）：Componentに含まれる独立した部品。
- variant（バリアント）：Module内の部品のバリエーション、もしくは部品が動的に変化した状態。

### ECSSとBEM
ECSSの命名規則はBEMの命名規則と似ています。

- ModuleとComponentはBlock
- ChildNodeはElement
- variantはModifier

BEMのBlockとECSSのModuleの違いは粒度の考え方です。  
BlockはModifierによってバリエーションを追加することができますが、Blockの上位にあるDOMにクラスが追加された場合はどう解釈すればいいでしょうか？ECSSでは上位のDOMをModule、内包される要素をComponentのように考えます。  
ECSSはBEMよりも粒度を大きくすることで、様々なケースに対応できるように考えられています。

### ECSSとAtomic Design
ECSSのModuleとComponentの考え方はAtomic DesignのOrganismsとMoleculesの考え方にも似ています。

- ModuleはOrganisms
- ComponentはMolecules

ただし、Atomic Designの場合は小さなものから組み合わせるという考え方ですが、ECSSの場合はいちばん大きなものを探して、その中にはどんな要素が必要なのかを考えていきます。

### 命名規則
モジュールは以下のようなシンタックスで表現されます。

```css
namespace-ModuleName_ChildNode-variant
namespace-ComponentName_ChildNode-variant
```

`namespace`はModule名になることもあります。

```css
modulename-ComponentName_ChildNode-variant
```

namespaceとvariantはローワーキャメルケースで、それ以外はアッパーキャメルケースになります。

ECSSのComponentはModuleの中にある、一定以上の大きさや機能をもったUIのことを指します。例えば、以下のようなものが考えられます。

- ヘッダーやフッターのような共通部分（Structure）のロゴやナビゲーション、検索フォーム（`.st-Logo`, `.st-Nav`, `.st-Search`）。
- トップページ内のaboutエリア、商品リスト、関連リンク（`.tp-About`, `.tp-ProductList`, `.tp-RelatedLink`）。
- `hero`名前空間に属したスタンダートなメインビジュアル（`.hero-Standard`）。
- `doc`名前空間に属したドキュメンテーションエリア（`.doc-Main`）。

クラス名だけでは、それがModuleなのかComponentなのか分からない場合もありますが、ECSSではそれも許容しています。必要なことはnamespaceでコンテキストごとに分割して、その中で重複しないことです。重複しないのであれば、細かな組み立て方は設計者に委ねられます。


### 名前空間（namespace）
CSSは最終的に1つのCSSファイルに結合されます。クラス名の重複などでスタイルの意図せぬ衝突を避けるために、ECSSでは必ず名前空間（namespace）をつけます。

- トップページ → `TopPage` → `.tp-`
- カテゴリートップ → `CategoryTop` → `.ct-`
- ショッピングカート → `ShoppingCart` → `.sc-`

省略は必須ではないので、`.home-`のようにしてもいいでしょう。

名前空間は`.hero-`や`.doc-`といったModuleをあらわすこともあります。例えば`hero`名前空間を持っているシンプルなModule（メインビジュアル）です。

```css
/* `hero`名前空間を持つComponent */
.hero-Standard {}
.hero-Title {}
.hero-Strap {}
```

上記のような名前ではコンポーネントの粒度が大きすぎると感じるのであれば、`.hero-Title`ではなく`.hero-Standard_Title`のようにChildNodeとしてスコープを作ると重複をより避けることができます。もしくは、`.tp-Hero`のようにトップページの`Hero`コンポーネントという意味合いを持たせてもいいでしょう。

```css
/* `hero`名前空間を持つComponent */
.hero-Standard {}
.hero-Title {}
.hero-Strap {}

/* `hero`名前空間を持つComponentとChildNode */
.hero-Standard {}
.hero-Standard_Title {}
.hero-Standard_Strap {}

/* `tp`名前空間を持つComponentとChildNode */
.tp-Hero {}
.tp-Hero_Title {}
.tp-Hero_Strap {}
```

いずれの場合も、名前空間というスコープを作ることでModuleやComponentなどの名前をつけやすくすることができます。

名前空間が重複しないように、コンテキストやモジュール数の多い案件では注意が必要です。例えば、コンテキストは省略をして、Moduleは省略をしないルールにするのもいいかもしれません。

#### サイト共通の構造的なModule
ヘッダーやフッターのようなサイト共通で使われる構造的なModuleは「Structure」という意味の`.st-`名前空間が紹介されています。

- グローバルヘッダー → `.st-Header`
- グローバルフッター → `.st-Footer`
- サイト共通のグリッドレイアウト：`.st-Grid`, `.st-Grid_Item`

#### モジュールのバージョン管理
ECSSではModuleは（例えばトップページやカテゴリートップページのような）特定のコンテキストを持っています。コンテキストをスコープとしているので、Moduleは簡単に削除することができます。

例えばトップページの改修があったとき、`.tp-`（TopPage）という名前空間を持ったModuleは迷わず削除することができます。改修後のModuleは同じ`.tp-`名前空間を使ってもいいですし、`.tp2-`（TopPage2）という名前空間に変更してもいいでしょう。

### サイト共通の汎用的なModule
サイト内で使われる汎用的なModuleは「SiteWide」という意味の`.sw-`名前空間が紹介されています。Atomic DesignのAtomsのように小さな部品のようなもので、例えばボタンやリストのようなモジュールです。

- サイト共通のボタン：`.sw-Button`
- サイト共通の順序つきリスト：`.sw-OrderedList`

### layout名前空間
トップページは1カラム、詳細ページは2カラムのように、ページによって大枠のレイアウトが大きく変わる場合があります。

- サイトトップページ
- カテゴリートップページ
- カテゴリー詳細ページ
- 採用情報ページ
- 検索結果ページ
- サイトマップページ

このようなレイアウトを管理する`layout-`名前空間を作ってもいいでしょう。

- サイトトップページ：`.layout-Home`
- カテゴリートップページ：`.layout-Top`
- カテゴリー詳細ページ：`.layout-Detail`

### 汎用的なModuleのメリット・デメリット
ECSSでは汎用的なModuleは極力作るべきではないとされているため、メリットとデメリットをよく見定める必要があります。

汎用的なModuleを作るメリットは

- デザインの再利用ができる（工数の削減、よく似ているデザインを防ぐ）
- 小さなModuleごとにテストができる
- 同じルールセットを減らすことができる

汎用的なModuleを作るデメリットは

- 影響範囲が大きく変更・削除しにくい
- 異なるnamespaceが混ざるので複雑になる（mixinなどで抽象化しても同じ）

サイト共通の見た目であることと、実装として同じコードであることは違います。今は同じでも、改修によって例外的なデザインになる可能性もあります。  
例外を含んでいないか、サイト共通のコードとして実装してもいいのかを合意形成してから共通化する必要があります。  
実装時には、以下のようなことについて注意するといいでしょう。

- 見た目をあらわした名前にしない
- できるだけコンテキストを含んだ名前にする
- できるだけ最低限のスタイルを持たせる
- 充分にテストをしてから使う
- スタイルガイドなどで一覧化できるようにする

## ディレクトリ構造
ECSSではコンテキストをスコープとしてファイルを管理します。スコープごとに必要なものを分割することで、見つけやすく、削除しやすくします。  
スコープごとにまとめられたフォルダをアセットと呼びます。

ECSSはWebアプリケーションを想定しているため、CSSとJSを並列に管理することを推奨していますが、このドキュメントではWebサイトを想定しているため以下のようなファイル構成を提案します。

```
root or assets
├── css
│   ├── font/
│   ├── base/
│   │   ├── variable/
│   │   ├── function/
│   │   ├── mixin/
│   │   ├── _normalize.scss
│   │   ├── _base.scss
│   │   └── _Icon.scss
│   ├── SiteWide/
│   │   ├── _Button.scss
│   │   └── _Embed.scss
│   ├── Structure/
│   │   ├── _Grid.scss
│   │   ├── _Footer.scss
│   │   └── _ContentsFooter.scss
│   ├── namespace/
│   │   ├── layout/
│   │   │   ├── _HomePage.scss
│   │   │   ├── _Top.scss
│   │   │   └── _Detail.scss
│   │   ├── news/
│   │   ├── product/
│   │   ├── results/
│   │   ├── search/
│   │   └── sitemap/
│   └── site.scss
├── img/
│   ├── common/
│   │   ├── Header/
│   │   ├── Footer/
│   │   └── SideBanner
│   └── products/
└── js
    ├── jquery-2.2.0.min.js
    ├── common/
    │   └── smooth-scroll.js
    └── namespace/
        ├── SiteWide/
        ├── js/
        │   └── smoothScroll.js
        └── product/
```

ディレクトリは以下のようなクラス名になります。

- /src/namespace/Structure/Module1：`.st-Module1`
- /src/namespace/SiteWide/Module1：`.sw-Module1`
- /src/namespace/TopPage/Module1：`.tp-Module1`
- /src/namespace/CategoryTop/Module1：`.ct-Module1`


例えば、`.st-Module1`の中には以下のようなクラス名が含まれることになります（逆に`.st-Module2`は含まれません）。

- `.st-Module1`
- `.st-Module1_ChildNode`
- `.st-Module1-variant`

namespaceディレクトリ内を見ることで、名前空間が重複していないかをいつでも確認することができます。

CSSファイルはglobパターンを使って一括でインポートします。[ecss-postcss-shell](https://github.com/benfrain/ecss-postcss-shell/blob/master/gulpfile.js)では[postcss-import](https://github.com/postcss/postcss-import)を使った例が紹介されています。

```js
// Create the styles
gulp.task("styles", ["lint-styles"], function () {
    var processors = [
        postcssImport({glob: true}),
        mixins,
        simpleVars,
        colorFunction(),
        nested,
        autoprefixer({ browsers: ["last 2 version", "safari 5", "opera 12.1", "ios 6", "android 2.3"] }),
        cssnano
    ];
    return gulp.src("preCSS/styles.css")
    // start Sourcemaps
    .pipe(sourcemaps.init())
    // We always want PostCSS to run
    .pipe(postcss(processors).on("error", gutil.log))
    // Write a source map into the CSS at this point
    .pipe(sourcemaps.write())
    // Set the destination for the CSS file
    .pipe(gulp.dest("./build"))
    // If in DEV environment, notify user that styles have been compiled
    .pipe(notify("Yo Mofo, check dem styles!!!")); 
});
```

CSSは以下のようにしてインポートします。namespaceディレクトリ内のModuleは依存関係を持たないようにする必要があります。

```scss
@import "base/variable/**/*.scss";
@import "base/function/**/*.scss";
@import "base/mixin/**/*.scss";
@import "base/_normalize.scss";
@import "base/_base.scss";
@import "base/_Icon.scss";

@import "SiteWide/**/*.scss";
@import "Structure/**/*.scss";
@import "namespace/**/*.scss";
```

## Moduleの状態変化
Moduleの状態変化は[WAI-ARIA](https://github.com/momdo/momdo.github.io/wiki#aria%E9%96%A2%E9%80%A3)を使うことが推奨されています。

| 属性            | 意味                                                            |
|---------------  |---------------------------------------------------------------- |
| aria-selected   | trueで選択、falseで非選択を示す                                  |
| aria-disabled   | trueで無効、falseで通常を示す                                   |
| aria-hidden     | trueで非表示、falseで表示状態を示す                              |
| aria-expanded   | trueで展開、falseで格納を示す                                   |
| aria-busy       | trueで更新中、falseで通常を示す                                  |
| aria-current    | 表示されているページを視覚的に表すリンクを示す（WAI-ARIA 1.1）   |

`.is-active`のようなクラスを追加するよりも名前のバラつきが起こりにくいことや、標準化された支援技術によってアクセシブルになることが期待できます。

WAI-ARIAを使わない場合は`.is-`のような名前空間でなく、variantを使うことを勧めています。

```html
<!-- Allow -->
<button class="co-Button is-Selected">Old Skool Button</button>

<!-- Good -->
<button class="co-Button co-Button-selected">Old Skool Button</button>
```

## CMSから生成されたコンテンツ
CMSでコンテンツを作成するときに、すべての要素にクラスを指定することが難しいことがあります。そういった場合には無理にクラスを指定せずに、Module内の要素セレクタに対してスタイルを指定することも許容されています。

```css
.st-Main {
  h1 {
    /* Styles for h1 */
  }
  p {
    /* Styles for p */
  }
  ul {
    /* Styles for ul */
  }
  li {
    /* Styles for li */
  }
}
```

クラス指定のない要素に限定するのもいいでしょう。

```css
.st-Main {
  h1:not([class]) {}
  p:not([class]) {}
  ul:not([class]) {}
  li:not([class]) {}
}
```

## ツール
ECSSではCSSプリプロセッサとLintの使用を推奨しています。どのCSSプリプロセッサやタスクランナーを使うのかといったことは、プロジェクトの要件によっても違いますし、時期によっても最適なものは違います（ECSS本ではPostCSSとGulp、Stylelintを使用）。  
必要な機能は以下の通りです。

- 変数：サイズや色を取得したり、定数化することによるヒューマンエラーの軽減。
- mixins：フォントファミリーなどの特定の設定のマクロのようなもの。
- アンパサンド記号：（`&`）でキーセレクタを参照する。
- loop：長いルールセット（例えば、100種類の異なる色のヘッダー）のループを書く機能。
- partials：オーサリングスタイルシートを特定のスコープごとに分割。
- Lint：適切でないコードをデプロイするのを防ぐ。
- 積極的なMinify：コードの最適化。
- Autoprefixer：正確にベンダープレフィックスを付与し、オーサリングスタイルシートにベンダープレフィックスが存在しないようにする。

### Stylelint
PostCSSではStylelintを使います。`.stylelintrc`には以下のように設定します。

```js
// Config for linting
module.exports = { 
    "rules": {
        "block-no-empty": true,
        "color-no-invalid-hex": true,
        "declaration-colon-space-after": "always",
        "declaration-colon-space-before": "never",
        "function-comma-space-after": "always",
        "function-url-quotes": "double",
        "media-feature-colon-space-after": "always",
        "media-feature-colon-space-before": "never",
        "media-feature-name-no-vendor-prefix": true,
        "max-empty-lines": 2,
        "number-leading-zero": "never",
        "number-no-trailing-zeros": true,
        "property-no-vendor-prefix": true,
        "declaration-block-no-duplicate-properties": true,
        "block-no-single-line": true,
        "declaration-block-trailing-semicolon": "always",
        "selector-list-comma-newline-after": "always-multi-line",
        "selector-no-id": true,
        "string-quotes": "double",
        "value-no-vendor-prefix": true,
        "function-linear-gradient-no-nonstandard-direction": true,
        "selector-no-universal": true,
        "declaration-block-no-shorthand-property-overrides": true,
        "indentation": 4,
        "selector-max-specificity": "0,2,0"
    } 
}
```

'selector-max-specificity'という「セレクタが持つことができるセレクタ特異性の最大レベルを制御する」ルールもあります。その他のECSSプロジェクトに有効なルールは以下の通りです。

- オーバーライドとメディア照会のみをネストすることができます（親（`&`）セレクターを使用しないネストを防止します）。
- キーセレクタがECSS命名規則に合っていることを確認してください（これを行うには、Stylelintに ['selector-class-pattern'](https://stylelint.io/?/src/rules/selector-class-pattern/README.md)ルールが追加されました）。
- キーセレクタが複合化されないようにします（例：`.ip-Selector.ip-Selector2 {}`）。
- キーセレクタが特異であることを確認します（例：`.ip-Thing`ではなく`.a-Parent .ip-Thing {}`）。


## 健全なスタイルシートの十戒
1. すべてのキーセレクタは"Single Source of truth"であること
2. 入れ子にしてはいけない（メディアクエリ、上書き、本当に必要であると感じない限りは）
3. IDセレクタを使用しない（必要だと感じていても）
4. オーサリングスタイルシートにベンダープレフィックスを書いてはいけない
5. サイズや色、z-indexに変数を使用する
6. モバイルファーストで書く（max-widthを避ける）
7. mixinを控えめに使用する（extendを避ける）
8. すべてのマジックナンバーとブラウザハックに対してコメントを書く
9. インラインイメージを使わない
10. 複雑なCSSを書かず、同じように動くシンプルなCSSを書く

### 1. すべてのキーセレクタは"Single Source of truth"であること
キーセレクタ（実際にスタイルが適応されるセレクタ）に対するスタイルの指定は1つのブレース内で完結するようにします。重複をなくして管理をしやすくするため、アセットを削除しやすくするのと同じようにキーセレクタも削除しやすくするためです。

ブレース内は常にキーセレクタを参照した処理をおこないます。

```css
.key-Selector {
  width: 100%;

  @media (min-width: $M) {
    width: 50%;
  }

  .an-Override_Selector & {
    color: $color-grey-33;
  }
}
```

ネストをしたときのパターンとして、キーセレクタを`&`で参照したり、メディアクエリで上書きする場合などが考えられます。  
許容されるネストは3つです。

1.標準的な上書き  
この結果、`ip-HomeCallouts`のクラスを持つ要素の内部にある場合、`ip-Carousel`のフォントサイズが大きくなります。

```css
.ip-Carousel {
  font-size: $text13;

  .ip-HomeCallouts & {
    font-size: $text15;
  }
}
```

2.要素にクラスを追加してオーバーライドする  
`.ip-ClassificationHeader`を追加された`.ip-Carousel`の背景色は変更されます。

```css
.ip-Carousel {
  background-color: $color-green;
  
  &.ip-ClassificationHeader {
    background-color: $color-grey-a7;
  }
}
```

3.メディアクエリによる上書き  
横幅は50%に上書きされます。

```css
.key-Selector {
  width: 100%;
  
  @media (min-width: $M) {
    width: 50%;
  }
}
```

いずれもキーセレクタはブレース内でカプセル化されます。他のキーセレクタが混ざり込むこともありません。

### 2. 入れ子にしてはいけない（メディアクエリ、上書き、本当に必要であると感じない限りは）
子要素をネストすることによって別のセレクタを生成することを禁止します。  
以下の例では、`.key-Selector`の中で`.key-Selector_Item`を生成してしまい、`.key-Selector_Item`に関する情報が重複する可能性があります。

```css
/* Good */
.key-Selector {}
.key-Selector_Item {}

/* Bad キーセレクタが2箇所に存在する */
.key-Selector {

  &_Item {

  }
}
.key-Selector_Item {}
```

子要素をネストすることによって詳細度を上げることも禁止します。  
キーセレクタである`.key-Selector`と子要素である`.key-Selector_Item`が結合されることで詳細度が上がり、ルールが複雑になってしまいます。

```css
/* Good */
.key-Selector {}
.key-Selector_Item {}

/* Bad */
.key-Selector {

  .key-Selector_Item {

  }
}
```

オーサリングスタイルシートのセレクタに`h1.yes-This_Selector`のような他のセレクタを追加する必要もありません。  
以下のようなデメリットがあります。

- 望ましくない特異性（詳細度の上昇）を作り出してしまう
- より具体的なセレクタにする（詳細度を上げる）必要があるため、維持するのが難しくなります
- 生成されたCSSのファイルサイズが不必要に大きくなります
- 要素タイプの場合、ルールを特定の要素や構造に結びつけてしまいます

### 3. IDセレクタを使用しない（必要だと感じていても）
ECSSではIDセレクタの使用を禁止しています。IDセレクタには以下のようなデメリットがあります。

- クラスセレクタよりも具体的で上書きを難しくしてしまう
- ページ内で一度しか使えない

どうしてもIDを使わなければいけない場合は、属性セレクタを使ってクラスセレクタと同じ詳細度にします。

```css
[id="Thing"] {
  /* Property/Values Here */
}
```

### 4. オーサリングスタイルシートにベンダープレフィックスを書いてはいけない
PostCSSのAutoprefixerによって必要なベンダープレフィックスを自動で付与することができます。わざわざ手書きをしたり、mixinで用意する必要はありません。

ただし、以下のような独自実装のプロパティは手動で追加する必要があります。

```css
.ui-ScrollPanel {
  -webkit-overflow-scrolling: touch;
}

.ui-Component {
  &::-webkit-scrollbar {
    -webkit-appearance: none;
  }
}
```

### 5. サイズや色、z-indexに変数を使用する
抽象化はなるべく避けますが、サイズ・色・z-indexは変数として管理することを推奨しています。

サイズと色を変数化することで微妙に異なる値に気づくことができるため、一貫性を保つことができます。
z-indexに関してはスタッキングコンテキストを完全にカバーすることはできませんが、全体のバランスを見て数値を決めるのに役立ちます。

### 6. モバイルファーストで書く（max-widthを避ける）
最小のビューポートを基準にすることで、スタイルの継承を促し、必要なときにだけ、最小限のスタイルの上書きをすることができます。

基本的にはmax-widthを使うことはありませんが、中間の範囲にだけスタイルを当てたいときに使うことがあります。

```css
.med-Video {
  position: relative;
  background-color: $color-black;
  font-size: $text13;
  line-height: $text15;

  /* Between medium and large sizes we want to bump the text up */
  @media (min-width: $M) and (max-width: $L) {
    font-size: $text15;
    line-height: $text18;
  }
}
```

### 7. mixinを控えめに使用する（extendを避ける）
mixinを使った抽象化はなるべく避けてください。有用なケースとしては以下のようなものがあります。

- テキストの切り捨て（@mixin Truncate）
- iOSスタイルの慣性スクロールパネル
- さまざまなブラウザに適した数の擬似セレクタ
- 複雑なフォントスタック

単純なフォントスタックは変数で定義します。それ以外の（例えば`font-family`以外のプロパティを持っている）複雑なフォントスタックはmixinで定義するほうが合理的です。

プロジェクトのmixinは10個以下になるようにします。それより多い場合は不必要に抽象化されている可能性があります。

Sassの@extendディレクティブは、あるセレクタのスタイルを別のセレクタに継承することができます。ファイルサイズの削減が期待できますが、オーサリングの時点では予測ができない処理をしてしまうため、デバッグが難しくなります。

Minify時はmixinよりもextendのほうがファイルサイズの削減ができますが、Gzipをするとそれほど変わらなくなります。extendで得られる（Gzipではわずかな）ファイルサイズの削減よりも、mixinによる保守性を選択します。

### 8. すべてのマジックナンバーとブラウザハックに対してコメントを書く
ピクセルベースのまだ変数として定義されていない値を入力するときには注意が必要です。オーサリングスタイルシートにマジックナンバー（第三者は数字の意味が分からないが正常に動作すること）を入力するときは、上の行にコメントを追加して「なぜ必要なのか」を説明してください。今は余分に思えるかもしれませんが、他の人や1か月後の自分が見たときのことを考えましょう。

```css
.med-Video {
  position: relative;
  background-color: $color-black;
  font-size: $text13;
  line-height: $text15;
  /* 絶対配置されたアイコンに対応するためには、上記のスペースが必要です */
  margin-top: 20px;
}
```

特定のブラウザに対するハックを書くときは`/ * HHHack：* /`をコメントに追加します。

```css
.med-Video {
  background-color: $color-black;
  font-size: $text13;
  line-height: $text15;
  /*HHHack Windows Phone 8.1をフルサイズで表示させるために必要, reference ticket SMP-234 */
  width: 100%;
}
```

この種のハックは可能な限りルールの最下位に記述します。

### 9. インラインイメージを使わない
インラインイメージはHTTP環境ではHTTPリクエストを減らすことができるメリットがあります。ただし、オーサリングスタイルシートにインラインアセットを展開することは避けます。そのアセットが何であるかを把握しにくいからです。

代わりにツールを使ってインラインイメージに変換しましょう。例えば[postcss-assets](https://github.com/borodean/postcss-assets)です。

```css
.rr-Outfit {
  min-height: $size-quadruple;
  background-image: inline("/path/to-image/relevant-image-name.png");
}
```

### 10. 複雑なCSSを書かず、同じように動くシンプルなCSSを書く
できるだけ多くの人が理解できるように、できるだけシンプルなCSSを書きましょう。ループ、mixin、関数を書くのは最小限にします。  
原則として、ルールのバリエーションが10未満の場合は手で書きます。30以上のスプライト画像であればツールを使っていきましょう。

シンプルさはレイアウトにも適応されます。DOM構造が同じであれば、よりサポートされているレイアウトアルゴリズムを使いますが、DOM構造をよりシンプルにできるのであれば、その方法も検討しましょう。

## 参考リンク
- [Enduring CSSの設計思想 - ECSSが目指す設計 | CodeGrid](https://app.codegrid.net/entry/2016-ecss-1)
- [抽象化を避けるCSS設計方法論「Enduring CSS」 第1回 | HTML5Experts.jp](https://html5experts.jp/takazudo/21946/)
- [PostCSSでECSSビルド環境構築 - PostCSSの基本的な使い方 | CodeGrid](https://app.codegrid.net/entry/2016-ecss-build-1)