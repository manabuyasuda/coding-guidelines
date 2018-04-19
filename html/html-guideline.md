# HTMLのガイドライン

## 規格
### WHATWG Living StandardとHTML5
HTMLは[WHATWG Living Standard](https://html.spec.whatwg.org/)（[日本語訳](https://momdo.github.io/html/)）をベースにします。Chrome、Safari、Firefox、Edgeといった主要なブラウザベンダーはWHATWGにのみ参加しており、W3Cが発行しているHTML5には主要なブラウザベンダは参加していないからです。  
ただし、SVGやWAI-ARIAなどはW3Cが発行しているものなので、部分的にはW3Cに準拠する必要があります。

参考サイト

- [W3CからのWHATWGに関するアナウンスメントについてのメモ - 水底の血](https://momdo.hatenablog.jp/entry/20171212/1513087304)
- [W3Cのは『欠陥フォーク』！？ HTMLスナップショット2016 ── HTML5 Conference 2016セッションレポート](https://html5experts.jp/momdo/21119/)

### バリデーション
HTMLはバリデーションツールに必ず通します。エラーと警告が出ないことを確認してください。

- [checker.html5.org（WHATWG Living Standard）](https://checker.html5.org/)
- [Validator.nu (X)HTML5 Validator（W3C）](https://html5.validator.nu/)

ただし、運用上の適切な理由がある場合は運用ルールを優先してください。

参考サイト

- [HTMLチェッカーはchecker.html5.orgを推してみたい - 水底の血](https://momdo.hatenablog.jp/entry/20161205/1480941113)

### UTF-8（BOMなし）
WHATWG Living Standardでは文字エンコーディングはUTF-8が必須になりました。

> 文字エンコーディング宣言が存在するかどうかにかかわらず、文書のエンコードに使用される実際の文字エンコーディングはUTF-8でなければならない。

また、エンコーディングが違うと差分が正確にとれなくなる、ツールによって不具合が生じる（例えばGulpではShift_JISのファイルが使えない）こともあり、文字エンコーディングはUTF-8に統一します。

参考サイト

- [HTML文書は文字エンコーディングUTF-8でなければなりません - 水底の血](https://momdo.hatenablog.jp/entry/20171008/1507462678)

## フォーマッティング
### ファイル名は半角英数字・ハイフン・アンダースコアのみ
ファイル名に使うのは半角英数字とハイフンとアンダースコアのみとします。

- 半角英数字： `abcdefghijklmnopqrstuvwxyz0123456789`
- ハイフン： `-` 
- アンダースコア： `_`

これ以外の文字・記号はファイルのやりとりで文字化けしたり、サーバーやツールが対応していない可能性がある、見落としやすい、機種依存文字といった理由で使いません。

### HTMLタグや属性はすべて小文字を使う
HTMLのタグや属性は大文字でも小文字でも同様に解釈されますが、一貫性をもたせるため小文字に統一します。

```html
<!-- Bad -->
<A HREF="#top">Top</A>
```

```html
<!-- Good -->
<a href="#top">Top</a>
```

### 属性は半角1スペース
属性や属性値内のスペースは1つとします。

```html
<!-- Bad -->
<img  class="foo  bar"  alt="">
```

```html
<!-- Good -->
<img class="foo bar" alt="">
```

### クオートはダブルクオーテーションを使う
属性のクオートはダブルクオーテーションに統一します。

```html
<!-- Bad -->
<img class='foo' alt=''>
```

```html
<!-- Good -->
<img class="foo" alt="">
```

### ディレクトリ名の末尾は`/`にする（`index.html`は省略）
ディレクトリ名の末尾にスラッシュがなくてもブラウザやサーバー側でリダイレクトされるので一般的に問題にはなりませんが、ディレクトリ名の末尾はスラッシュに統一します。  
また、`index.html`も省略することができるのでこれも省略します。

```html
<!-- Bad -->
<a href="/foo">Foo</a>
<a href="/index.html">Top</a>
```

```html
<!-- Good -->
<a href="/foo/">Foo</a>
<a href="/">Top</a>
```

参考サイト

- [URLの終りに「/」(スラッシュ)は必要?、不要? | 海外SEO情報ブログ](https://www.suzukikenichi.com/blog/do-we-need-a-trailing-slash-at-the-end-of-url/)

### インデントは半角2スペースで適切に使う
インデントは半角2スペースに統一します。

また、ulとliタグのような親子関係、thとtdタグのような兄弟関係にあるタグは改行とインデントを適切に使います。

```html
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

```html
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

### ルート相対パスを使う
URLや画像などのパス指定はルート相対パスに統一します。外部ファイル化してインクルードする際にパス切れが起きなくなります。

```html
<!-- Bad -->
<a href="../index.html">Top</a>
```

```html
<!-- Good -->
<a href="/">Top</a>
```

### 終端スラッシュを省略をする
終端スラッシュが必要なのはXHTMLだけです。

```html
<!-- Bad -->
</br>
<img alt="" src=""/>
```

```html
<!-- Good -->
<br>
<img alt="" src="">
```

### 終了タグの省略をしない
ほとんどの人にとっては読みにくいです。一般的なルールで書きましょう。

```html
<!-- Bad -->
<html>
  <body>
```

```html
<!-- Good -->
<html>
  <body>
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

```html
<!-- Bad -->
<small>&copy;︎ All rights reserved.</small>
```

```html
<!-- Good -->
<small>©︎ All rights reserved.</small>
```

### 真偽値をとる属性値は省略する
省略したほうが把握しやすくなります。

```html
<!-- Bad -->
<button type="button" disabled="disabled"></button>
```

```html
<!-- Good -->
<button type="button" disabled></button>
```

### コメントを適切に使う
本当にHTMLファイルに残す必要のある情報だけをコメントとして残します。classやid属性にしたり、ドキュメントに残すことを検討してください。

コメントを単体として使う場合。

```html
<!-- コメント -->
```

2つのコメントで囲う場合。

```html
<!-- コメント -->
<div></div>
<!-- /コメント -->
```

## ARIA
### 暗黙のARIAセマンティックを尊重する
HTMLタグには「暗黙のARIAセマンティック」が適用されているものがあります。それらを重複して指定することは意味がありません。

```html
<!-- Bad -->
<nav role="navigation"></nav>

<hr role="separator">
```

```html
<!-- Good -->
<nav></nav>

<hr>
```

UAが対応しきれていないrole属性を指定するケースもありますが、必要最小限にしましょう。

参考サイト

- [ARIA in HTML 日本語訳](https://momdo.github.io/html-aria/)

### aria-*属性を使用する
状態の変化はaria-*属性を積極的に使います。UAを通して利用者に状態が変化していることを伝えることができるようになります。

| 属性            | 意味                                                            |
|---------------  |---------------------------------------------------------------- |
| `aria-selected`   | trueで選択、falseで非選択を示す                                  |
| `aria-disabled`   | trueで無効、falseで通常を示す                                   |
| `aria-hidden`     | trueで非表示、falseで表示状態を示す                              |
| `aria-expanded`   | trueで展開、falseで格納を示す                                   |
| `aria-busy`       | trueで更新中、falseで通常を示す                                  |
| `aria-current`    | 表示されているページを視覚的に表すリンクを示す（WAI-ARIA 1.1）   |

`.is-open`のようなクラス名を使うよりもセマンティックで、CSSのセレクタとしても使えます。

## HTMLタグ
### h1はドキュメント内に1つに制限する
WHATWGの仕様ではドキュメント内にh1を複数使っても問題はありません。Googleも複数使ってもよいと名言していますが、UAが`section > h1`のようなアウトラインアルゴリズムに対応していない場合、すべてがh1だと解釈されてしまう恐れがあります。

現状ではh1をドキュメント内に1つに制限するのがベターです。

参考サイト

- [H1タグ「何回でも使っていい」とGoogle発言、これほんと？｜SEO  Packブログ](https://seopack.jp/seoblog/20170414-many-h1s-are-ok/)
- [h1要素は複数回使って良いのか!? HTML5.1に関するW3CとWHATWGの主張の違い - Togetter](https://togetter.com/li/1058977)
- [Frontrend Vol.9 - 春の新人歓迎 マークアップ/アクセシビリティのキホン #frontrend | 覚え書き](https://kidachi.kazuhi.to/blog/archives/039426.html)

### h1以外の見出しはsectionタグで囲む
sectionタグを使うことでセクションを明示にすることができます。  
以下はsectionタグを使うことでh1とh2にそれぞれ紐づく文章を指定した場合です。Badではpタグが2つともh2に紐づく文章になってしまいますが、Goodではh1とh2にそれぞれ文章を紐づけることができます。

```html
<!-- Bad -->
<body>
  <h1>タイトル</h1>
  <h2>見出し</h2>
  <p>見出しに関する文章</p>
  <p>タイトルに関する文章</p>
</body>
```

```html
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

bodyタグもセクションを明示するタグ（bodyタグはセクショニングルート、sectionタグはセクショニングコンテンツ）なので、`body > section > h1`は誤りです。

### sectionタグ内の見出しはネストレベルに応じる
仕様としては`section > h1 > section > h1`というマークアップもできますが、UAがアウトラインアルゴリズムを解釈できない場合にすべてがh1として解釈されてしまう恐れがあります。

sectionタグのネストレベルに応じて見出しタグも見出しレベルを下げるようにします。

```html
<!-- Bad -->
<body>
  <h1>タイトル</h1>
  <section>
    <h1>見出し</h1>
  </section>
</body>
```

```html
<!-- Good -->
<body>
  <h1>タイトル</h1>
  <section>
    <h2>見出し</h2>
  </section>
</body>
```

### navタグを効果的に使う
navタグで囲うことで、ドキュメント内の主要なナビゲーションであると解釈することができるようになり、UAがnavタグに移動するといった機能を利用者に提供することができるようになります。

```html
<!-- Bad -->
<ul class="global-nav">
  <li></li>
  <li></li>
</ul>
```

```html
<!-- Good -->
<nav class="global-nav">
  <ul>
    <li></li>
    <li></li>
  </ul>
</nav>
```

### ulとolを適切に使う
リストタグには順序に意味のあるolタグと順序に意味のないulタグがあります。  
パンくずリストやランキング、料理の手順のような順序に意味がある（順序を変えると意味が変わってしまう）内容の場合はolタグを使います。  
ソーシャルボタンや料理の材料のように順序を変えても意味が変わらない内容の場合はulタグを使います。

```html
<!-- Good -->
<p>○○の作り方</p>
<ol>
  <li>△△を〜します。</li>
  <li>□□を〜します。</li>
</ol>
```

```html
<!-- Good -->
<p>○○の材料</p>
<ul>
  <li>△△</li>
  <li>□□</li>
</ul>
```

### メインコンテンツはmainタグ内に入れる
そのドキュメントのメインコンテンツはmainタグで囲みます。  
mainタグで囲うことで、ドキュメント内の主要なコンテンツであると解釈することができるようになり、UAがmainタグに移動するといった機能を利用者に提供することができるようになります。

注意点が2つあります。

1. グローバルナビゲーションやサイドバー、書作権表記のようなサイト固有の要素を含めないこと。
2. セクションを明示するタグではないので、sectionやarticleタグのように使わないこと。

```html
<!-- Good -->
<header></header>

<main>
  <h1></h1>
  <section>
    <h2></h2>
  </section>
</main>

<footer></footer>
```

### tableタグの注意点
tableタグにはいくつかのタグを含めることができます。UAが解釈するのが難しいタグの一つですので、なるべく適切なマークアップになるようにします。

- 表の本体部はtbodyタグ、表の列の見出しを表す行のセットはtheadタグ、テーブルの列を総括する行のセットはtfootタグを使う
- セルのヘッダーはthタグ、情報を含むセルはtdタグを使う
- 表の見出しか説明はcaptionタグを使う

```html
<!-- Good -->
<table>
  <caption>表の見出しかキャプション</caption>
  <thead>
    <tr>
      <th>セルのヘッダー</th>
      <th>セルのヘッダー</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>セルのデータ</td>
      <td>セルのデータ</td>
    </tr>
  </tbody>
</table>
```

### brタグを連続して使わない
brタグは改行をするためのタグです。行間を広げるために連続して使うことを禁止します。  
pタグを使ったり、CSSでmarginを指定して行間を調整してください。

```html
<!-- Bad -->
<p>
  <br>
  <br>
</p>
```

```html
<!-- Good -->
<p></p>
```

### strongとemとbを適切に使う
strongとemとbタグは見た目や意味から混同しがちな要素です。コンテンツの作者でないと判断が難しいことも多いので、strongタグだけを使うようにするのがベターです。

- strongタグは重要性を示すタグです。重要性、重大性、または緊急性が高いことを示します。
- emタグは強勢（アクセント）を示すタグです。「私は〜が好きです。」といった文章の場合、「私は」にアクセントをつけるか、「〜が」にアクセントをつけるかで文章の意味が変わってきます。
- bタグは重要性や強勢を含まないけれど注意を惹きたいことを示すタグです。要約のキーワードやレビューの製品名、太字で表記されるのが通例になっている箇所に使います。

```html
<!-- Good -->
<p><strong>30%OFFのため、たいへんお買い得です！</strong></p>
<p><strong>30%OFFのため</em>、たいへんお買い得です！</p>
<p>今回紹介するのは、<b>◯◯</b>という商品です。</p>
```

### 画像の切り替えはpictureタグやsrcset属性を使う
画像の切り替えはCSSやJSを使う方法もありますが、HTMLにはネイティブでその機能があります。CSSやJSと違い、画像を重複して読み込むことがないためパフォーマンス面でも優れます。

対応するブラウザによってはpictureタグとsrcset属性に対応していない場合があるので、ポリフィルの[Picturefill](http://scottjehl.github.io/picturefill/)を使うことも検討してください。

### formタグの注意点
inputタグにはid属性を、labelタグにはfor属性を指定して紐付けてください。

inputタグには適切なtype属性を指定してください。

- `type="text"`： 一行テキストボックスを作成する
- `type="search"`： 検索テキストの入力欄を作成す
- `type="tel"`： 電話番号の入力欄を作成する
- `type="email"`： メールアドレスの入力欄を作成する
- `type="password"`： パスワード入力欄を作成する
- `type="number"`： 数値の入力欄を作成する
- `type="radio"`： ラジオボタンを作成する
- `type="checkbox`： チェックボックスを作成する

ブラウザの自動入力機能をオンにするためにautocomplete属性に該当する項目があれば指定してください。[HTML Standard](https://momdo.github.io/html/forms.html#autofilling-form-controls:-the-autocomplete-attribute)を参照。

- `autocomplete="name"`： 姓名（フルネーム）
- `autocomplete="email"`： メールアドレス
- `autocomplete="current-password"`： 現在のパスワード
- `autocomplete="new-password"`： 新しいパスワード
- `autocomplete="postal-code"`： 郵便番号
- `autocomplete="street-address"`： 住所
- `autocomplete="address-level1"`： 都道府県
- `autocomplete="address-level2"`： 市区町村
- `autocomplete="address-line1"`： 番地・マンション名（1行目）
- `autocomplete="address-line2"`： 番地・マンション名（2行目）
- `autocomplete="tel"`： 郵便番号
- `autocomplete="organization"`： 会社名

## 属性
### lang属性を指定する
Googleは言語指定にhreflang属性を見ていますが、lang属性はそれ以外にもメリットの大きい属性ですので指定しておきます。  
例えば、言語によってフォントサイズを変えたり表示する記号を変更する、lang属性によって音声読み上げのイントネーションを変えるといったことができます。

```html
<!-- Bad -->
<html>
</html>
```

```html
<!-- Good -->
<html lang="ja">
</html>
```

```html
<!-- Good -->
<ul>
  <li>
    <a href="/" lang="ja">日本語</a>
  </li>
  <li>
    <a href="/english/" lang="en">English</a>
  </li>
  <li>
    <a href="/chines/" lang="zh-cn">中文</a>
  </li>
</ul>
```

言語コードの一覧。

```
langList = [
  ["en","英語"],
  ["fr","フランス語"],
  ["de","ドイツ語"],
  ["it","イタリア語"],
  ["nl","オランダ語"],
  ["ru","ロシア語"],
  ["es","スペイン語"],
  ["ja","日本語"],
  ["zh","中国語"],
  ["zh-CN","中国語(簡体字)"],
  ["zh-TW","中国語(繁体字)"],
  ["ko","韓国語"],
  ["el","ギリシャ語"],
  ["af","アフリカーンス語"],
  ["sq","アルバニア語"],
  ["ar","アラビア語"],
  ["be","ベラルーシ語"],
  ["bg","ブルガリア語"],
  ["ca","カタロニア語"],
  ["hr","クロアチア語"],
  ["cs","チェコ語"],
  ["da","デンマーク語"],
  ["et","エストニア語"],
  ["tl","フィリピノ語"],
  ["fi","フィンランド語"],
  ["gl","ガリシア語"],
  ["iw","ヘブライ語"],
  ["hi","ヒンディー語"],
  ["hu","ハンガリー語"],
  ["is","アイスランド語"],
  ["id","インドネシア語"],
  ["ga","アイルランド語"],
  ["lv","ラトビア語"],
  ["lt","リトアニア語"],
  ["mk","マケドニア語"],
  ["ms","マレー語"],
  ["mt","マルタ語"],
  ["no","ノルウェー語"],
  ["fa","ペルシア語"],
  ["pl","ポーランド語"],
  ["pt-PT","ポルトガル語"],
  ["ro","ルーマニア語"],
  ["sr","セルビア語"],
  ["sk","スロバキア語"],
  ["sl","スロベニア語"],
  ["sw","スワヒリ後"],
  ["sv","スウェーデン語"],
  ["tl","タガログ語"],
  ["th","タイ語"],
  ["tr","トルコ語"],
  ["uk","ウクライナ語"],
  ["vi","ベトナム語"],
  ["cy","ウェールズ語"],
  ["yi","イディッシュ語"]
];
```

---

中国語の言語コードの指定方法が変わったようです（ソース：[W3C日本語訳](https://www.w3.org/International/articles/language-tags/index.ja)）。

- 中国語簡体字サイト： `lang="zh-cmn-Hans"`
- 中国語繁体字サイト： `lang="zh-cmn-Hant"`

`zh`は中国語、`cmn`は公用語、`Hans`は簡体字、`Hant`は繁体字です。

参考サイト

- [中国語ウェブサイトの言語コード指定（lang属性） : 海外向けWebマーケティングのエクスポート・ジャパン](https://www.export-japan.co.jp/blog/%E4%B8%AD%E5%9B%BD%E8%AA%9E%E3%82%A6%E3%82%A7%E3%83%96%E3%82%B5%E3%82%A4%E3%83%88%E3%81%AE%E8%A8%80%E8%AA%9E%E3%82%B3%E3%83%BC%E3%83%89%E6%8C%87%E5%AE%9A%EF%BC%88lang%E5%B1%9E%E6%80%A7%EF%BC%89/)
- [中国語ウェブサイトの言語コード指定（lang属性） | 中国Web情報マガジン](http://www.china-webby.com/%E4%B8%AD%E5%9B%BD%E8%AA%9E%E3%82%A6%E3%82%A7%E3%83%96%E3%82%B5%E3%82%A4%E3%83%88%E3%81%AE%E8%A8%80%E8%AA%9E%E3%82%B3%E3%83%BC%E3%83%89%E6%8C%87%E5%AE%9A%EF%BC%88lang%E5%B1%9E%E6%80%A7%EF%BC%89/)

### 属性の指定順
可読性を向上するため属性は以下の順番で指定します。

1. `class`
2. `id`, `name`
3. `data-*`
4. `src`, `for`, `type`, `href`, `value`
5. `title`, `alt`
6. `role`, `aria-*`

### `target="_blank"`を指定しない
ユーザビリティとセキュリティの面から`target="_blank"`は使用しません。

ユーザビリティの面では以下のような考え方があります。

- 利用者に選択権を与えたほうが自己帰属感（自分で操作している感覚）を与えられる
- 別窓にすると「戻る」が使えない（iOS Safariでは遷移先の1ページ目だけは「戻るが使えます」）
- 別窓になったと気づかず混乱させる恐れがある（別窓アイコンをつけるなどの対応が必要）
- スマホでは「戻る」「進む」のほうが使い勝手がいい

セキュリティ面では以下のような考えがあります。

- `target="_blank"`で開いたリンク元ページを、JavaScriptを仕込んだリンク先で操作することができる
- これを防ぐには`rel="noopener noreferrer"`を指定する必要がある

サイトのレギュレーションによっては別窓にする場合もあるので、上記のメリットデメリットを考慮したうえで使用します。

### imgタグにalt属性は必須
imgタグのalt属性は仕様では省略が可能です。ただし、音声読み上げの際にsrc属性値を読み上げてしまう恐れがあります。  
alt属性値は任意ですが、alt属性自体は必ず指定するようにします。

```html
<!-- Bad -->
<img src="">
```

```html
<!-- Good -->
<img src="" alt="">
```

### imgタグのwidthとheight属性を省略する
imgタグのwidthとheight属性を指定することでパフォーマンスがよくなると一般的に言われていますが、最近の主要なブラウザではほとんど影響がないとも言われています。  
メンテナンスの面でも省略をしたほうが管理しやすくなります。

```html
<!-- Bad -->
<img src="" width="" height="" alt="">
```

```html
<!-- Good -->
<img src="" alt="">
```

参考サイト

- [強調,引用,グループ化,画像などの要素 -- ごく簡単なHTMLの説明](https://www.kanzaki.com/docs/html/htminfo14.html#S14)

### tableタグの注意点（scope、colspanとrowspan）
tableタグにはいくつかの属性を含めることができます。UAが解釈するのが難しいタグの一つですので、なるべく適切なマークアップになるようにします。  
また、なるべく結合をせずに簡潔な表にすることも検討してください。

- scope属性： thタグが関連するセルを指定します
  - `scope="row"`： 見出しはその行に属するすべてのセルに関連します。
  - `scope="col"`： 見出しはその列に属するすべてのセルに関連します。
  - `scope="rowgroup"`： 見出しは行グループに属し、その中のすべてのセルに関連します。これらのセルはtableタグのdir属性の値によって、見出しの右又は左に配置されます。
  - `scope="colgroup"`： 見出しは行グループに属し、その中のすべてのセルに関連します。これらのセルはtableタグのdir属性の値によって、見出しの右又は左に配置されます。
- colspan属性： 複数のセルを横方向（列）に結合します。
- rowspan属性： 複数のセルを縦方向（行）に結合します。

## headタグ内
### CSSはheadタグ内、JSはbodyタグの閉じタグ直前で読み込む
レンダリング速度を上げるために、CSSはheadタグ内に、JSはbodyタグの閉じタグ直前で読み込みます。

```html
<!-- Good -->
<head>
  <link rel="stylesheet" href="/assets/css/site.css">
</head>

<body>

  <script src="/assets/js/site.js"></script>
</body>
```

ただし、IE9の対応が不要な場合はdefer属性を指定してheadタグ内でJSを読み込みます。bodyの閉じタグ直前で読み込んだ場合と同じ処理になり、さらにHTMLの読み込みとJSファイルの読み込みを同時にできるのでパフォーマンスの向上が見込めます。複数読み込んでいる場合は読み込んだ順で実行されます。

```html
<!-- Good -->
<head>
  <link rel="stylesheet" href="/assets/css/site.css">
  <script src="/assets/js/site.js" defer></script>
</head>

<body>
</body>
```

### ファビコンはheadタグ内で読み込まない
ほとんどのブラウザではheadタグ内で読み込まなくても、/favicon.icoを非同期で取得します。ルートディレクトリのファビコンを使用するのであれば、指定する必要はありません。

```html
<!-- Bad -->
<link href="/favicon.ico" rel="icon" type="image/vnd.microsoft.icon">
```

### 必須のメタタグ
#### title

```html
<!-- Good -->
<head>
  <title>ページ名 | サイト名</title>
</head>
```

#### description

```html
<!-- Good -->
<head>
  <meta name="description" content="サイトの概要">
</head>
```

#### IE compatibility mode
ドキュメントモードをEdgeに統一します。古いIEでも崩れないようにします。

```html
<!-- Good -->
<head>
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
</head>
```

#### viewport
表示幅を画面幅と同じにします。`user-scalable=no`（拡大禁止）は指定しません。

```html
<!-- Good -->
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
```

### オプションのメタタグ
#### canonical
http(s)からはじまる絶対パスで指定してURLの正規化をします。  
SEOに有効ですが、URLの指定を間違えてしまうと適切にインデックスされなくなってしまうので慎重に指定してください（自動化を推奨）。

```html
<!-- Good -->
<head>
  <link rel="canonical" href="https://example.com/">
</head>
```

参考リンク

- [canonicalとは〜URLの正規化でSEOのマイナス評価を避けよう｜ferret [フェレット]](https://ferret-plus.com/1809)

#### keywords
keywordsは過去にスパムとして使用されてしまったためGoogleは評価していません。指定するのも間違いではありませんが、工数もかかるので優先順位を考慮するようにしてください。

```html
<!-- Good -->
<head>
  <meta name="keywords" content="サイトのキーワード1, サイトのキーワード2">
</head>
```

参考リンク

- [meta keywordsの書き方とSEOで不要な理由](https://seolaboratory.jp/34998/)

### OGP
TwitterやFacebookなどのSNS用のOGPは以下のように指定します。

- headタグとog:typeの`website`はホームページのみ、それ以外のページは`article`を指定する
- 必須項目はog:title、og:type、og:url、og:image。
- fb:adminsよりfb:app_idを優先して指定する

```html
<head prefix="og: http://ogp.me/ns# fb: http://ogp.me/ns/fb# website: http://ogp.me/ns/website#">
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>ページ名 | サイト名</title>
  <meta name="description" content="サイトの概要">
  <meta name="keywords" content="サイトのキーワード1, サイトのキーワード2">

  <link rel="stylesheet" href="/assets/css/site.css">

  <link rel="canonical" href="https://example.com/">

  <!-- 必須 -->
  <meta property="og:title" content="ページ名"><!-- 省略するとtitleタグを反映する -->
  <meta property="og:type" content="website"><!-- 省略すると初期値のwebsiteになる -->
  <meta property="og:image" content="https://example.com/ogp.jpg"><!-- 絶対パスで指定する -->
  <meta property="og:url" content="https://example.com/"><!-- 絶対パスで指定する -->

  <!-- 任意 -->
  <meta property="og:site_name" content="サイト名">
  <meta property="og:description" content="サイトの概要">
  <meta property="og:locale" content="ja">
  <meta property="fb:app_id" content="App-ID"><!-- サイトのアカウントを作る場合 -->
  <meta property="fb:admins" content="adminID"><!-- 個人のアカウントを使う場合 -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:site" content="@SiteAccount">
</head>
```

以下のページでOGPの確認ができます。

- [Card Validator | Twitter Developers](https://cards-dev.twitter.com/validator)
- [Open Graph Debugger](https://developers.facebook.com/tools/debug/)

### 多言語対応
同じドメイン内で、ディレクトリによって国や言語を振り分けている場合、alternate属性値とhreflang属性を指定して紐付けます。

```html
<link rel="alternate" hreflang="ja" href="https://www.example.com/">
<link rel="alternate" hreflang="en" href="https://www.example.com/en/">
<link rel="alternate" hreflang="zh-cn" href="https://www.example.com/zh-cn/">
<link rel="alternate" hreflang="zh-tw" href="https://www.example.com/zh-tw/">
<link rel="alternate" hreflang="ko" href="https://www.example.com/ko/">
<link rel="alternate" hreflang="th" href="https://www.example.com/th/">
<link rel="alternate" hreflang="vi" href="https://www.example.com/vi/">
<link rel="alternate" hreflang="ru" href="https://www.example.com/ru/">
```

参考サイト

- [言語や地域の URL に hreflang を使用する - Search Console ヘルプ](https://support.google.com/webmasters/answer/189077?hl=ja)
