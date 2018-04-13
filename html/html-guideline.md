# HTMLのガイドライン

## 規格
### WHATWG Living StandardとHTML5
HTMLは[WHATWG Living Standard](https://html.spec.whatwg.org/)（[日本語訳](https://momdo.github.io/html/)）をベースにします。Chrome、Safari、Firefox、Edgeといった主要なブラウザベンダーはWHATWGにのみ参加しており、W3Cが発行しているHTML5には主要なブラウザベンダは参加していないからです。  
ただし、SVGやWAI-ARIAなどはW3Cが発行しているものなので、部分的にはW3Cに準拠する必要があります。

参考サイト

- [W3CからのWHATWGに関するアナウンスメントについてのメモ - 水底の血](https://momdo.hatenablog.jp/entry/20171212/1513087304)
- [W3Cのは『欠陥フォーク』！？ HTMLスナップショット2016 ── HTML5 Conference 2016セッションレポート](https://html5experts.jp/momdo/21119/)

### バリデーション
HTMLはバリデーションツールに必ず通します。エラーと警告が出ないことを確認してください（運用上の適切な理由がある場合は一部通らなくても問題ありません）。

- [checker.html5.org（WHATWG Living Standard）](https://checker.html5.org/)
- [Validator.nu (X)HTML5 Validator（W3C）](https://html5.validator.nu/)

参考サイト

- [HTMLチェッカーはchecker.html5.orgを推してみたい - 水底の血](https://momdo.hatenablog.jp/entry/20161205/1480941113)

### UTF-8（BOMなし）
WHATWG Living Standardで文字エンコーディングはUTF-8が必須になりました。

> 文字エンコーディング宣言が存在するかどうかにかかわらず、文書のエンコードに使用される実際の文字エンコーディングはUTF-8でなければならない。

また、エンコーディングが違うと差分が正確にとれない、ツールによって文字エンコーディングによって不具合が生じる（GulpではShift_JISのファイルが使えない）といった理由もあり、文字エンコーディングはUTF-8を使います。

参考サイト

- [HTML文書は文字エンコーディングUTF-8でなければなりません - 水底の血](https://momdo.hatenablog.jp/entry/20171008/1507462678)

## フォーマッティング
### ファイル名は半角英数字・ハイフン・アンダースコアのみ
ファイル名に使うのは半角英数字とハイフンとアンダースコアのみとします。

- 半角英数字
- `-` ハイフン
- `_` アンダースコア

これ以外の文字・記号はファイルのやりとりで文字化けしたり、サーバーが対応していない可能性がある、見落としやすい、機種依存文字といった理由で使いません。

### HTMLタグや属性はすべて小文字を使う
HTMLは大文字でも小文字でも同様に解釈されますが、一貫性をもたせるため小文字に統一します。

```
<!-- Bad -->
<A HREF="#general">General</A>
```

```
<!-- Good -->
<a href="#general">General</a>
```

### 属性は半角1スペース
属性や属性値内のスペースは1つとします。

```
<!-- Bad -->
<img  class="foo  bar"  alt="">
```

```
<!-- Good -->
<img class="foo bar" alt="">
```

### クオーテーションはダブルクオーテーションを使う
属性のクオーテーションはダブルクオーテーションに統一します。

```
<!-- Bad -->
<img class='foo' alt=''>
```

```
<!-- Good -->
<img class="foo" alt="">
```

### ディレクトリ名の末尾は`/`にする（index.htmlは省略）
ディレクトリ名の末尾にスラッシュがなくてもブラウザやサーバー側でリダイレクトされるので一般的に問題にはなりませんが、ディレクトリ名の末尾はスラッシュに統一します。  
また、index.htmlも省略することができるのでこれも省略します。

```
<!-- Bad -->
<a href="/foo">Foo</a>
<a href="/index.html">Top</a>
```

```
<!-- Good -->
<a href="/foo/">Foo</a>
<a href="/">Top</a>
```

参考サイト

- [URLの終りに「/」(スラッシュ)は必要?、不要? | 海外SEO情報ブログ](https://www.suzukikenichi.com/blog/do-we-need-a-trailing-slash-at-the-end-of-url/)

### インデントは半角2スペース（タブは使用不可）で適切に使う
インデントは半角2スペースに統一します。

また、`ul`と`li`のような親子関係、`th`と`td`のような兄弟関係にあるタグは改行とインデントを適切に使います。

```
<!-- Bad -->
<ul>
<li></li>
<li></li>
<ul>

<table>
<thead>
<tr>
<th></th>
<td></td>
</tr>
</thead>
</table>
```

```
<!-- Good -->
<ul>
  <li></li>
  <li></li>
<ul>

<table>
  <thead>
    <tr>
      <th></th>
      <td></td>
    </tr>
  </thead>
</table>
```

### パスはルート相対パスを使う
URLや画像などのパス指定はルート相対パスを使います。共通化をするときにパス切れをすることがなくなります。

```
<!-- Bad -->
<a href="../index.html">Top</a>
```

```
<!-- Good -->
<a href="/">Top</a>
```

### 終端スラッシュを省略をしない
終端スラッシュが必要なのはXHTMLだけです。

```
<!-- Bad -->
</br>
<img alt="HTML Best Practices" src="/img/logo.png"/>
```

```
<!-- Good -->
<br>
<img alt="HTML Best Practices" src="/img/logo.png">
```

### 終了タグの省略をしない


```
<!-- Bad -->
<html>
  <body>
    ...
```

```
<!-- Good -->
<html>
  <body>
    ...
  </body>
</html>
```

### エスケープする文字としない文字
UTF-8であれば以下の文字以外はエスケープする必要はありません。

- `&`
- `<`
- `>`
- `"`
- `'`

```
<!-- Bad -->
<h1>The "&" character</h1>
```

```
<!-- Good -->
<h1>The &quot;&amp;&quot; character</h1>
```

### 真偽値をとる属性値は省略する


```
<!-- Bad -->
<audio autoplay="autoplay" src="/audio/theme.mp3">
```

```
<!-- Good -->
<audio autoplay src="/audio/theme.mp3">
```

### コメントを適切に使う
本当にHTMLファイルに残す必要のある情報だけをコメントとして残します。

コメントを単体として使う場合。

```
<!-- コメント -->
```

タグをコメントで囲う場合。

```
<!-- コメント -->
<div></div>
<!-- /コメント -->
```

## ARIA
### 暗黙のARIAセマンティックを尊重する
HTMLタグには「暗黙のARIAセマンティック」が適用されているものがあります。それらを重複して指定することは意味がありません。

```
<!-- Bad -->
<nav role="navigation">
  ...
</nav>

<hr role="separator">
```

```
<!-- Good -->
<nav>
  ...
</nav>

<hr>
```

UAが対応しきれていないrole属性を指定するケースもありますが、必要最小限にしましょう。

参考サイト

- [ARIA in HTML 日本語訳](https://momdo.github.io/html-aria/)

### `aria-*`属性を使用する
状態の変化をUAと通して利用者に伝えたい場合は`aria-*`属性を積極的に使います。

| 属性            | 意味                                                            |
|---------------  |---------------------------------------------------------------- |
| aria-selected   | trueで選択、falseで非選択を示す                                  |
| aria-disabled   | trueで無効、falseで通常を示す                                   |
| aria-hidden     | trueで非表示、falseで表示状態を示す                              |
| aria-expanded   | trueで展開、falseで格納を示す                                   |
| aria-busy       | trueで更新中、falseで通常を示す                                  |
| aria-current    | 表示されているページを視覚的に表すリンクを示す（WAI-ARIA 1.1）   |

## HTMLタグ
### h1はドキュメント内に1つに制限する
WHATWGの仕様ではドキュメント内にh1を複数使っても問題はありません。Googleも複数使ってもよいと名言していますが、UAが`section > h1`のようなアウトラインアルゴリズムに対応していない場合、情報が過不足なく伝えられない恐れがあります。

現状ではh1をドキュメント内に1つに制限するのがベターです。

参考サイト

- [H1タグ「何回でも使っていい」とGoogle発言、これほんと？｜SEO  Packブログ](https://seopack.jp/seoblog/20170414-many-h1s-are-ok/)
- [h1要素は複数回使って良いのか!? HTML5.1に関するW3CとWHATWGの主張の違い - Togetter](https://togetter.com/li/1058977)
- [Frontrend Vol.9 - 春の新人歓迎 マークアップ/アクセシビリティのキホン #frontrend | 覚え書き](https://kidachi.kazuhi.to/blog/archives/039426.html)

### h1以外の見出しはsectionタグで囲む
sectionタグを使うことでセクションを明示的にすることができます。  
以下はsectionタグを使うことでh1とh2に紐づく文章を指定した場合です。Badではpタグがh2に紐づく文章になってしまっています。

```
<!-- Bad -->
<body>
  <h1>タイトル</h1>
  <h2>見出し</h2>
  <p>見出しに関する文章</p>
  <p>タイトルに関する文章</p>
</body>
```

```
<!-- Good -->
<body>
  <h1>タイトル</h1>
  <section>
    <h2>見出し</h2>
    <p>見出しに関する文章</p>
  </section>
  <p>タイトルに関する文章</p>
</body>
```

bodyタグもセクションを明示するタグ（bodyタグはセクショニングルート、sectionタグはセクショニングコンテンツ）なので、`body > section > h1`は誤りですので注意します。

### sectionタグ内の見出しはネストレベルに応じる
仕様としては`section > h1 > section > h1`というマークアップもできますが、UAがアウトラインアルゴリズムを解釈できない場合にすべてがh1として扱われてしまう恐れがあります。

sectionタグのネストレベルに応じて見出しタグも見出しレベルを下げるようにします。

```
<!-- Bad -->
<body>
  <h1>タイトル</h1>
  <section>
    <h1>見出し</h1>
  </section>
</body>
```

```
<!-- Good -->
<body>
  <h1>タイトル</h1>
  <section>
    <h2>見出し</h2>
  </section>
</body>
```

### navタグを効果的に使う
ナビゲーションには`ul`や`ol`タグがよく使われます。`nav`タグで囲うことで、ドキュメント内の主要なナビゲーションであると解釈することができるようになり、UAが`nav`タグに移動するといった機能を利用者に提供することができるようになります。

```
<!-- Bad -->
<ul class="global-nav">
  <li></li>
  <li></li>
</ul>
```

```
<!-- Good -->
<nav class="global-nav">
  <ul>
    <li></li>
    <li></li>
  </ul>
</nav>
```

### ulとolを適切に使う
リストタグには順序に意味のある`ol`タグと順序に意味のない`ul`タグがあります。  
パンくずリストやランキング、料理の手順のような順序に意味がある（順序を変えると意味が変わってしまう）要素の場合は`ol`タグを使います。  
ソーシャルボタンや料理の材料のように順序を変えても意味が変わらない要素の場合は`ul`タグを使います。

```
<!-- Good -->
<p>○○の作り方</p>
<ol>
  <li>△△を〜します。</li>
  <li>□□を〜します。</li>
</ol>
```

```
<!-- Good -->
<p>○○の材料</p>
<ul>
<li>△△</li>
<li>□□</li>
</ul>
```

### mainタグ内にメインコンテンツを入れる
そのドキュメントのメインコンテンツを示すために`main`タグで囲みます。  
タグで囲うことで、ドキュメント内の主要なナビゲーションであると解釈することができるようになり、UAが`main`タグに移動するといった機能を利用者に提供することができるようになります。

注意点は2つあります。

1. グローバルナビゲーションやサイドバー、書作権表記のようなサイト固有の要素を含めない。
2. セクションを明示するタグではないので、`section`や`article`タグのように使わない。

### tableタグの注意点
tableタグにはいくつかのタグを含めることができます。UAが解釈するのが難しいタグの一つですので、なるべく適切なマークアップになるようにします。

- 表の本体部は`tbody`、表の列の見出しを表す行のセットは`thead`、テーブルの列を総括する行のセットは`tfoot`を使う
- セルのヘッダーは`th`、情報を含むセルは`td`タグを使う
- 表の見出しかキャプション（説明）は`caption`タグを使う

### brタグを連続して使わない
brタグは改行をするためのタグです。行間を広げるために連続して使うことを禁止します。  
pタグを使ったり、CSSで`margin`を指定して行間を調整してください。

### strongとemとbを適切に使う
`strong`と`em`と`b`タグは見た目や意味から混同しがちな要素です。

- `strong`タグは重要性を示すタグです。重要性、重大性、または緊急性が高いことを示します。
- `em`タグは強勢（アクセント）を示すタグです。「私は〜が好きです。」といった文章の場合、「私は」にアクセントをつけるか、「〜が」にアクセントをつけるかで文章の意味が変わってきます。
- `b`タグは重要性や強勢を含まないけれど注意を惹きたいことを示すタグです。要約のキーワードやレビューの製品名、太字で表記されるのが通例になっている箇所に使います。

### 画像の切り替えはpictureタグやsrcset属性を使う
画像の切り替えはCSSやJSを使う方法もありますが、HTMLにはネイティブでその機能があります。CSSやJSと違い、画像を重複して読み込むことがないためパフォーマンス面でも優れます。

対応するブラウザによってはpictureタグとsrcset属性に対応していない場合があるので、ポリフィルの[Picturefill](http://scottjehl.github.io/picturefill/)を使うことも検討してください。

### formタグの注意点（適切なtype属性、labelタグで関連付け）

## 属性
### lang属性を指定する
### 属性の指定順
### aタグで別窓で開く場合は`target="_blank"`を指定する
### imgタグにalt属性は必須
### imgタグのwidthとheight属性を省略する
### tableタグの注意点（scope、colspanとrowspan）

## headタグ内
### CSSはheadタグ内、JSはbodyタグの閉じタグ直前
### ファビコンはドキュメントルートに置いて記述をしない
### 必須のメタタグ
#### title
#### description
#### IE compatibility mode
#### viewport
### オプションのメタタグ
#### canonical
#### keywords
### OGP
### 多言語対応
