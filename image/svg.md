# SVGのガイドライン

## SVGの作成時に注意すること

### サイズ削減のためにすること
- 「オブジェクト」→「パス」→「単純化」でアンカーポイントの数を最小化する（見た目を損なわない程度に留めます）
- 非表示にしているレイヤーを削除する（非表示レイヤーも書き出されます）
- 1回しか使わないシンボル図形は「シンボルへのリンクの解除」をする
- 「アピアランス」「ブラシ」「ブレンド」「エンベロープ」は分割されてしまうため最低限にする
- 「メッシュグラデーション」「画像ブラシ」「演算」「レイヤー効果」はラスタライズされてしまうため最低限にする（ラスタライズされても表示に問題がないか出力データを確認します）
- 「変形」「回転」「拡大縮小」などを最低限にする（transform属性に変換されます）

### 正常に表示するためにすること
- アートボードの座標は0,0（左上）を起点にする
- 文字データはアウトライン化する（アウトライン化をしないと`<text>`タグとして書き出されるため、環境によって指定したフォントで表示されない場合があります）
- 「線」以外のアピアランスを使わない（うまく書き出されません）
- 同じ図形はシンボル登録してから配置する（`<symbol>`タグと`<use>`タグで自動的にモジュール化してくれます）

### 書き出し設定で注意すること
- 「SVG プロファイル」は「SVG1.1」を指定する（最新版を指定します）
- 「フォント」の「文字」は「SVG」、「サブセット」は「なし（システムフォントを使用）」を指定する
- 「Illustratorの編集機能を保持」のチェックを外す（サイズ削減のため）
- 「CSS プロパティ」は「プレゼンテーション属性」を指定する（CSSで上書きをしやすくするため）
- 「未使用グラフィックスタイルを含める」のチェックを外す（サイズ削減のため）
- 「小数点以下の桁数」は3を指定する（公式で3が最適とされているため）
- 「エンコーディング」は「UTF-8」を指定する
- 「`<tspan>`エレメントの出力を制御」にチェックを入れる（サイズ削減のため）
- 「パス上テキストに エレメントを使用」にチェックを入れる（サイズ削減のため）
- 「レスポンシブ」のチェックを外す（widthとheight属性がないとIEとAndroidで表示が崩れるため）
- 「スライスデータを含める」のチェックを外す（サイズ削減のため）
- 「XMPを含める」のチェックを外す（サイズ削減のため）
- SVG圧縮（SVGZ）をしない（エディタで変更できるようにするため）

## コードの最適化をする
不要なコードは削除してください。Gulpなどのツールで自動的に削除するのを推奨します。  
最適化をすると以下のように変更されます。

```html
<!-- 最適化前 -->
<?xml version="1.0" encoding="utf-8"?>
<!-- Generator: Adobe Illustrator 16.0.0, SVG Export Plug-In . SVG Version: 6.00 Build 0)  -->
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg version="1.1" id="レイヤー1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink"
   x="0px" y="0px" width="600px" height="600px" viewBox="0 0 600 600" enable-background="new 0 0 600 600" xml:space="preserve">
<path fill="#040000" d="M492,300c0,17.695-1.536,32-19.232,32H127.231C109.567,332,108,317.695,108,300c0-17.696,1.567-32,19.231-32
  H472.8C490.464,268,492,282.304,492,300z"/>
</svg>
```

```html
<!-- 最適化後 -->
<svg version="1.1" id="question_x5F_answer" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="600px" height="600px" viewBox="0 0 600 600">
<path fill="#040000" d="M492,300c0,17.695-1.536,32-19.232,32H127.231C109.567,332,108,317.695,108,300c0-17.696,1.567-32,19.231-32
  H472.8C490.464,268,492,282.304,492,300z"/>
</svg>
```

### 不要なコードを削除する
- `<?xml`から始まるXML宣言の`version="1.0"`（バージョンが1.0であれば省略可）
- `<?xml`から始まるXML宣言の`encoding="utf-8"`（UTF-8で書き出している場合は省略可）
- `<?xml`から始まるXML宣言の`standalone="no"`
- `<!-- Generator: Adobe Illustrator`から始まるコメント
- `<!DOCTYPE svg PUBLIC`から始まる文書型宣言（SVG1.1では非推奨）
- `<svg>`タグの`xmlns:a=""`
- `<svg>`タグの`x=""`と`y=""`（両方とも`0(px)`の場合）
- `<svg>`タグの`enable-background=""`
- `<svg>`タグの`xml:space="preserve"`

### 必須の属性を確認する
`<svg>`タグで必須の属性です。表示崩れの原因にもなるので、必ず指定してください。

- `xmlns="http://www.w3.org/2000/svg"`（SVG名前空間宣言）
- `xmlns:xlink="http://www.w3.org/1999/xlink"`（XLink名前空間宣言）
- version属性
- width属性とheight属性（pxは省略可）
- viewBox属性

### gulp-imageminの設定例
gulp-imageminを使用したときに設定例です。

```js
const imagemin = require('gulp-imagemin');
const imageminMozjpeg = require('imagemin-mozjpeg');
const imageminPngquant = require('imagemin-pngquant');

gulp.task('image', () => gulp.src('src/assets/img/**/*.{png,jpg,gif,svg}')
  .pipe(changed('htdocs/assets/img/'))
  .pipe(plumber({
    errorHandler(err) {
      console.log(err.messageFormatted);
      this.emit('end');
    },
  }))
  .pipe(imagemin([
    imageminMozjpeg({
      // 画質
      quality: 70,
    }),
    imageminPngquant({
      // 画質
      quality: 70,
    }),
    imagemin.svgo({
      plugins: [
        // viewBox属性を削除する（widthとheight属性がある場合）。
        // 表示が崩れる原因になるので削除しない。
        { removeViewBox: false },
        // <metadata>を削除する。
        // 追加したmetadataを削除する必要はない。
        { removeMetadata: false },
        // SVGの仕様に含まれていないタグや属性、id属性やvertion属性を削除する。
        // 追加した要素を削除する必要はない。
        { emoveUnknownsAndDefaults: false },
        // コードが短くなる場合だけ<path>に変換する。
        // アニメーションが動作しない可能性があるので変換しない。
        { convertShapeToPath: false },
        // 重複や不要な`<g>`タグを削除する。
        // アニメーションが動作しない可能性があるので変換しない。
        { collapseGroups: false },
        // SVG内に<style>や<script>がなければidを削除する。
        // idにアンカーが貼られていたら削除せずにid名を縮小する。
        // id属性は動作の起点となることがあるため削除しない。
        { cleanupIDs: false },
      ]
    }),
    imagemin.optipng(),
    imagemin.gifsicle(),
  ]))
  .pipe(gulp.dest('htdocs/assets/img/'))
  .pipe(browserSync.reload({ stream: true })));
```

## imgタグでSVGを表示する
pngで表示していたような画像の代わりとしてSVGを使うのがいちばん手軽な方法です。通常の画像と同じように`<img>`タグで指定します。

```html
<img src="image.svg" alt="">
```

使用するSVGファイル内にはwidthとheight、viewBox属性を必ず指定します（IEとAndroid対応）。

### フルードイメージ
`<img>`タグと`<object>`タグをフルードイメージにする場合はCSSで以下のように指定します。

```css
/* 属性値が.svgで終わる要素に適応 */
[src$=".svg"],
[data$=".svg"] {
  width: 100%; /* IE対応 */
  max-width: 100%;
  height: auto;
}
```

### バグフィックス（IEとAndroid）
SVGファイル内でwidthとheight属性が指定されていない場合にIEとAndroidでアスペクト比がおかしくなることがあります。viewBox属性値をもとにwidthとheight属性を追加します。

`preserveAspectRatio="none"`をSVGファイルに指定して直す方法もあります。仕様上、viewBox属性のアスペクト比とwidthとheight属性のアスペクト比が異なる場合、アスペクト比を維持しながらスケーリングし、中央寄せで表示されます。preserveAspectRatio属性はこの挙動を変更するもので、noneと指定することでHTML側で指定したサイズでviewBoxがフィットするようになります。

### フォールバック（IE8）
フォールバックは処理が冗長になるので、IE8に対応するのであればSVGは使わないようにしましょう。

SVG非対応ブラウザ向けのフォールバックにはHTMLでの対応とJavaScriptでの対応の2パターンがあります。

#### objectタグでフォールバックする
HTML側で対応する場合は`<img>`タグではなく、`<object>`タグを使います。`<object>`タグは下記のようにフォールバックを含めた記述ができます。

```html
<object data="image.svg" type="image/svg+xml" width="100" height="100">
　<object data="fallback.png" type="image/png" width="100" height="100">
　</object>
</object>
```

#### JavaScriptでフォールバックする
SVG非対応ブラウザのときには.svgを.pngにJavaScriptで置換します。

```js
if(!window.SVGSVGElement){ //SVG非対応ブラウザの判別
  $('img[src*="svg"]').attr('src', function() {
    return $(this).attr('src').replace('.svg','.png'); //拡張子を置換
  });
}
```

フォールバック画像の作成は[svg2png](https://github.com/domenic/svg2png)などで自動的に生成することができます。

`<a>`タグを`<object>`タグでラップするとリンクがクリックできなくなります（`<img>`タグでは問題ありません）。

```html
<a href="#">
  <object data="image.svg" type="image/svg+xml" width="100" height="100">
　  <object data="fallback.png" type="image/png" width="100" height="100">
　  </object>
  </object>
</a>
```

CSSで以下のように指定するとリンクをクリックできるようになります。

```css
a {
　　display: inline-block; /* もしくは`display:block;` */
}

object {
　　pointer-events: none; /* IE10以下未対応 */
}
```

## background-imageでSVGを表示する
通常の画像と同じくbackground-imageプロパティでSVGを表示させることができます。

```css
.bg {
  background-image: url("image.svg");
}
```

フォールバックをする場合はフォールバック用のpng画像を先に指定します。

```css
.bg {
  background-image: url("fallback.png");
  background-image: url("image.svg"), none;
}
```

使用するSVGファイル内にはwidthとheight、viewBox属性を必ず指定します（IEとAndroid対応）。

## SVGスプライトでSVGを表示する
SVGは`<symbol>`タグと`<use>`タグでコンポーネント化ができます。これをSVGスプライト（svgstore）と呼びます。

`<symbol>`タグは`<svg>`タグの中に記述し、その中にコンポーネント化したい要素を置きます。`<symbol>`タグには固有のid属性を指定しておきます。`<symbol>`要素内に記述したSVGは表示されず、`<use>`タグで参照して表示します。

```html
<svg>
  <symbol id="facebook" viewBox="0 0 110 110">
    <title id="facebook-title">Facebook</title>
    <path d="M55 0C24.6 0 0 24.6 0 55s24.6 55 55 55 55-24.6 55-55S85.4 0 55 0zm14.6 54.8H60v34H45.9v-34h-6.7v-12h6.7V35c0-5.6 2.6-14.2 14.2-14.2h10.5v11.6H63c-1.2 0-3 .6-3 3.3v7.1h10.8l-1.2 12z"></path>
  </symbol>
</svg>
```

`<use>`タグを使ってSVGを呼び出します。role属性やaria-labelledby属性でアクセシブルにしている点に注意してください。

```html
<svg viewBox="0 0 110 110" role="img" aria-labelledby="facebook-title">
  <use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#facebook"></use>
</svg>
```

CSSは以下のように指定すると、余分な余白をなくすことができます。  
正方形のアイコンは`width`と`height`を`1em`にします。長方形の場合は小さいほうを大きいほうの相対値（em）で指定します。最終的なサイズは`font-size`に`em`を指定して調整します。

```css
.Icon {
  display: inline-block;
  vertical-align: middle;
  position: relative;
  top: -0.1em;
  // 幅の方が大きいので、幅を基準（1em）とする。
  width: 1em;
  // 幅に対する相対値をemで指定する。
  // (高さ / 幅) * 1em = 0.8572em
  height: (428.6 / 500) * 1em;
  font-size: 1em;
  fill: currentColor;
}
```

### SVGスプライトのポリフィル「svg4everybody」
IEは`<use>`タグで外部ファイル化したSVGを参照することができません。「[svg4everybody](https://github.com/jonathantneal/svg4everybody)」というポリフィルを使いましょう。CMSやPugなどでインクルードを使う方法もありますが、外部ファイルを参照する方法はどんな環境でも使えるメリットがあります。

npmからパッケージをインストール。

```
npm install svg4everybody
```

svg4everybodyを読み込んだら実行が必要です（下記はES2015の例）。

```js
import svg4everybody from 'svg4everybody';

svg4everybody();
```

### gulp-svg-spriteでSVGスプライトを生成する
SVGスプライトをアイコンごとに手動で作成していると、時間がかかりケアレスミスも起きる可能性があります。「[gulp-svg-sprite](https://github.com/jkphl/gulp-svg-sprite)」を使って自動で生成するようにしましょう。

以下の例では、`src/assets/svg/`に.svgを保存すると、`htdocs/assets/svg/`にsprite.svgというファイル名でSVGスプライトがまとめて生成されます。

```js
const svgSprite = require('gulp-svg-sprite');

gulp.task('svgSprite', () => gulp.src('src/assets/svg/**/*.svg')
  .pipe(plumber({ errorHandler: notify.onError('Error: <%= error.message %>') }))
  .pipe(svgSprite({
    mode: {
      // SVGファイルをsymbol要素としてまとめる。
      symbol: {
        dest: './',
        // 出力するファイル名。
        sprite: 'sprite.svg',
      },
    },
    shape: {
      transform: [{
        svgo: {
          plugins: [
            // `style`属性を削除する。
            { removeStyleElement: true },
            // `fill`属性を削除して、CSSで`fill`の変更ができるようにする。
            { removeAttrs: { attrs: 'fill' } },
          ],
        },
      }],
    },
    svg: {
      // xml宣言を出力する。
      xmlDeclaration: true,
      // DOCTYPE宣言を出力する。
      doctypeDeclaration: false,
    },
  }))
  .pipe(gulp.dest('htdocs/assets/svg/'))
  .pipe(browserSync.reload({ stream: true })));
```

以下のように参照します。

```html
<svg role="img">
  <use xlink:href="/assets/svg/sprite.svg#facebook"></use>
</svg>
```

## svgタグでSVGを表示する
HTML内に`<svg>`タグで記述するインラインSVGは、SVGの機能のすべてを使うことができます。ただし、HTMLファイル内にSVGコードを貼る必要があり、インクルードをするにしても管理がやや複雑になりがちです。`<img>`タグやbackground-imageプロパティ、SVGスプライトで指定するのが適切でない場合に使うようにします。  
以下のような条件に合致する場合に使うといいでしょう。

- 単色ではない
- `<svg>`タグに複数のリンクを設定したい
- `<svg>`タグに複数のテキスト情報を設定したい
- JavaScriptで動的に値を変更したい
- アニメーションさせたい

### svgタグのアクセシビリティ
SVG単体では、`<title>`タグと`<desc>`がスクリーンリーダーで適切に読み上げられないため、WAI-ARIAを使ってアクセシブルにする必要があります。

- `<title>`タグ（タイトル）と`<desc>`タグ（説明）を追加する
- `<title>`タグと`<desc>`タグのid属性値を`aria-labelledby=""`に指定する
- 適切なrole属性（`role="img"`や`role="link"`など）を指定する

```html
<svg>
  <symbol role="img" aria-labelledby="title desc" viewBox="0 0 110 110">
    <title id="title">タイトルのテキスト</title>
    <desc id="desc">説明テキスト</desc>
    <path d="M55 0C24.6 0 0 24.6 0 55s24.6 55 55 55 55-24.6 55-55S85.4 0 55 0zm14.6 54.8H60v34H45.9v-34h-6.7v-12h6.7V35c0-5.6 2.6-14.2 14.2-14.2h10.5v11.6H63c-1.2 0-3 .6-3 3.3v7.1h10.8l-1.2 12z"></path>
  </symbol>
</svg>
```

## SVGにリンクを設定する
インラインSVGにリンクを貼る場合、`<a>`タグで`<svg>`タグを囲んでも動作しません。`<svg>`タグ内の要素を`<a>`タグで囲み、xlink:href属性でリンク先を指定します。

```html
<svg>
  <a xlink:href="#">
    <path></path>
  </a>
  <a xlink:href="#">
    <path></path>
  </a>
</svg>
```

## 参考リンク
- [SVG 1.1 仕様 （第２版） 日本語訳](https://triple-underscore.github.io/SVG11/index.html)
- [SVG | MDN](https://developer.mozilla.org/ja/docs/Web/SVG)
- [svg要素の基本的な使い方まとめ](http://www.h2.dion.ne.jp/~defghi/svgMemo/svgMemo.htm)
- [SVG MANIAX Ver.2 - Mars vanilla](https://www.slideshare.net/ssuser99dc16/mars-svg)
- [HTML5 と SVG で考える、これからの画像アクセシビリティ](https://www.slideshare.net/ssuser99dc16/html5fun-svg-accessibility)
