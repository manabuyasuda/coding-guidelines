# CSS クラス名リスト
名前をつけることは難しいですが、とても重要なことです。

CSSには設計思想が必要ですが、実践するにあたり、名前と機能の意味がとおり、名前のつけ方にブレがないようにするべきです。

このドキュメントでは、CSSでよく使われる単語を分類し、意味や機能を短くまとめています。  
ただし、見た目やUIの機能をクラス名にするのではなく、デザインの意図やそのコンポーネントの役割をクラス名にすることを推奨します。


## 形容詞
名詞の性質や状態を修飾する品詞。「〜の」「〜な」。

- `main` - 主要な。
- `primary` - 主要な。
- `secondary` - 補助的な・第二の。
- `tertiary` - 第三の。
- `quaternary` - 第四の。
- `common` - 共通の。
- `global` - 全体的な。
- `local` - 局所的な。
- `general` - 一般的な。
- `brand` - ブランドの。
- `site` - サイトの。


## 分類
サイトのページやカテゴリとして使える名詞。

- `about` - 〜について。
- `work` - 仕事・任務。
- `product` - 製品。
- `service` - サービス。
- `news` - お知らせ・近況。
- `event` - 行事・出来事。
- `history` - 歴史・沿革。
- `archive` - 保存・保管・記録。
- `concept` - 構想・概念・考え。
- `recommend` - おすすめ・推奨。
- `table of contents` - 目次。[検索する](http://bit.ly/2Nn7KNu)
  - `toc` - `Table of contents`の略語。
- `index` - 索引・指標。
- `contact` - 問い合わせ・連絡。
- `inquiry` - 問い合わせ・質問・調査。
- `access` - 交通手段。
- `shop` - 店舗。
- `related` - 関連のある。
- `privacy` - 個人情報の利用・保護の方針。
- `faq` - `qanda`の類語、Frequently asked questions（よくある質問）の略。[検索する](http://bit.ly/2NjMKH8)
  - `qanda` - Question and answer（質問と回答）の略。[検索する](http://bit.ly/2NL4JmJ)
- `input` - フォームの入力画面。
- `confirm`- フォームの確認画面。
- `finish` - フォームの完了画面。
- `search-result` - 検索結果画面。[検索する](http://bit.ly/2MQ8on6)
- `cart` - 購入するアイテムを一時的に保存する画面。[検索する](http://bit.ly/2Nf4u6A)
- `checkout` - 保存していたアイテムを購入する画面。[検索する](http://bit.ly/2NfzYJJ)


## Block
BEMのBlockで使われるような名詞。

- `section` - 区分・区画。
- `content` - 文書の内容。
- `article` - 記事。
- `post` - 投稿。
- `top` - 頂上・上部。
- `home`- トップページ。
- `sidebar` - 補足記事。
- `wrapper` - 内包する。
  - `wrap` - `wrapper`の略語。
  - `container` - `wrapper`の類語。容器・入れ物。`wrapper`はレイアウト的に、`container`は意味的に使うことが多い。
- `group` - 集まり。
- `area` - 特定の場所・範囲。
- `emphasis` - 強調・重視。
- `catch` - 興味を惹く・関心をつかむ。
- `note` - 注釈。
- `description` - 概要。
  - `desc` - `description`の略語。
- `introduction` - 前置き・導入。
  - `intro` - `introduction`の略語。
- `announce` - お知らせ。
- `information` - 情報。
  - `info` - `information`の略語。
- `action` - Call To Action。重要度の高い。
- `more` - もっと多くの。
- `feature` - 主要なもの。
- `detail` - 詳細・細部。
- `summary` - 概要・要約。


## Element
BEMのElementで使われるような名詞や形容詞など。

- `inner` - 内側の。
- `outer` - 外側の。
- `body` - 主要部分。
- `head` - 上部。
- `foot` - 下部。
- `heading` - 見出し・表題。
- `title` - 表題・題名。
- `lead` - 見出しの補足・記事の要約。
- `list` - 一覧・表。
- `menu` - 一覧・表。
- `item` - （表やデータなどの）項目。
- `unit` - 1つの単位・1セット。
- `column` - 縦列。
  - `col` - `column`の略語。
- `text` - 本文。
- `caption` - 画像・図表の補足文。
- `thumbnail` - 縮小した画像。
  - `thumb` - `thumbnail`の略語。
- `avatar` - 人の顔を示す画像。
- `feature` - 特徴を補足する画像。
- `tel` - 電話番号。
- `address` - 住所。
- `date` - 日付。
- `time` - 日時。
- `category` - 分類・部類。
  - `cat` - `category`の略語。
- `tag` - 識別子。
- `next` - 次の。
- `previous` - 前の。
  - `prev` - `previous`の略語。
- `mask` - 覆い隠す。
- `overlay` - かぶせる・上書きする。
- `delimiter` - アイテムの範囲や境界線を示すインターフェイス。
  - `separator` - `delimiter`の類語で混ぜないように分離する目的で使います。
  - `divider` - `delimiter`の類語でグルーピングするように分割する目的で使います。


## Modifier
BEMのModifierで使われるような名詞や形容詞など。

- `success` - 成功。
- `alert` - 注意・警戒。
- `attention` - 配慮・気配り。
- `error` - 間違い・失敗。
- `caution` - 用心・警告。
- `warning` - 警告・予告。
- `danger` - 危険・驚異。
- `tiny` - とても小さい。
  - `xs` - `tiny`の類語でExtra small（smallよりさらに小さい）こと。
- `small` - 小さい。
- `medium` - 中間。
- `large` - 大きい。
- `huge` - とても大きい。
  - `xl` - `huge`の類語でExtra large（特大・超大型）のこと。
- `reverse` - 反転する。
- `push` - 前方に押す。
- `pull` - 自分の方に引く。
- `offset` - 相殺する・埋めあわせる。
- `left`- 左側の。
- `center`  - 真ん中。
- `right` - 右側の。
- `top` - 上部。
- `middle` - 真ん中。
- `bottom` - 下部。
- `round` - 角丸。
- `circle` - 円形。


## State
状態を示す動詞や形容詞など。

- `show` - 見せる。
- `hide` - 隠す。
- `open` - 開く。
- `close` - 閉じる。
- `current` - 現在の。
- `active` - 活動中・有効な。
- `disabled` - 無効になっている。


## UI
利用者に情報を提供するためのインターフェイス。

### Text
- `link` - アンカーテキスト。[検索する](http://bit.ly/2PBVgyJ)
- `label` - 分類する・項目名。[検索する](http://bit.ly/2MKvfjM)
- `tag` - 符号・識別子。[検索する](http://bit.ly/2PHB2Uz)
- `badge` - 残数を示す数値。[検索する](http://bit.ly/2NPftR6)
- `copyright` - 著作権表示。[検索する](http://bit.ly/2PDvOc7)
- `tooltip` - マウスオーバー時に補足情報を表示するインターフェイス。[検索する](http://bit.ly/2NQR6CC)
- `button` - オン・オフの選択に使うインターフェイス。[検索する](http://bit.ly/2PEzhY2)
  - `btn` - `button`の略語。

### Image
- `image` - 画像。[検索する](http://bit.ly/2NUydP6)
  - `img` - `image`の略語。
- `icon` - 対象の内容や機能などを小さな絵柄で表したもの。[検索する](http://bit.ly/2NUqS20)
- `loading` - 読み込み中であることを示すインターフェイス。[検索する](http://bit.ly/2NPfJj2)
- `logo` - 社名や製品名などを図案化・装飾化した文字・文字列。[検索する](http://bit.ly/2PG6l1X)
- `map` - 地図。[検索する](http://bit.ly/2NO6K1P)
- `chart` - 理解しやすいような方法で情報を示すリストやグラフのこと。[検索する](http://bit.ly/2NNDb0f)
  - `graph` - `chart`の類語で視覚表現のための手段のこと。[検索する](http://bit.ly/2PyyUOK)
- `hero` - キービジュアルになる大型の画像。[検索する](http://bit.ly/2PyyZC2)
- `banner` - （主に宣伝用の）画像をともなうリンク。[検索する](http://bit.ly/2PxbB7K)
- `carousel` - 画像などのコンテンツを上下左右にスライドさせて切り替えるインターフェイス。[検索する](http://bit.ly/2PFowVA)
  - `slider` - `carousel`の類語。[検索する](http://bit.ly/2PEzCKi)
  - `ticker` - `carousel`の類語で自動でアイテムを左右に流しながら表示する。ユーザーは基本的にコントロールできない。[検索する](http://bit.ly/2Ngw2Zn)
  - `lightbox` - `carousel`の類語で、モーダルのjQueryプラグインのこと。主な用途はサムネイル画像をクリックしてモーダルを開き画像を大きく見せること。[検索する](http://bit.ly/2NOFd08)

### Navigation
- `navigation` - 情報へ誘導するリンク。[検索する](http://bit.ly/2PxbGZ6)
  - `nav` - `navigation`の略語。
  - `global-navigation` - ほとんどの画面で表示されている主要なナビゲーション。[検索する](http://bit.ly/2PCOfO6)
  - `local-navigation` - あるカテゴリ内専用のナビゲーション。グローバルナビゲーションの中や下に配置する場合や、サイドバーに配置する場合がある。[検索する](http://bit.ly/2PCSNUF)
- `mega-menu` - 深い階層のカテゴリやページへのナビゲーション。ドロップダウン（クリックやマウスオーバー）で階層を表示していく。カテゴリやページ数の多いサイトのグローバルナビゲーションに使われることが多い。[検索する](http://bit.ly/2PDHCvc)
- `breadcrumb` - パンくずリスト。トップページから現在ページまでの階層構造を示したリンク。[検索する](http://bit.ly/2NO4p71)
  - `topicpath` - `breadcrumb`の類語。[検索する](http://bit.ly/2NPpXA9)
- `springboard` - 同じサイズのリンクを2×2や3×3のように配置した同じ重要度を持つ並列なナビゲーション。[検索する](http://bit.ly/2NPCN1e)
- `list-menu` - 縦に並んだリスト型のリンクで、階層構造をもったナビゲーション。[検索する](http://bit.ly/2PEAC10)
- `dashboard` - グラフやステータスなどを一つの画面にまとめたインターフェイス。[検索する](http://bit.ly/2NRu2Uf)
- `pagination` - 昇順にしたページ番号付きのナビゲーション。[検索する](http://bit.ly/2MNWJ8p)
- `linear-navigation` - その画面の前後に移動するためのナビゲーション。`pagination`との違いはページ指定ができないこと。[検索する](http://bit.ly/2Njru4i)
- `back-to-top` - ページトップに戻るためのページ内リンク。[検索する](http://bit.ly/2MNXCOh)
- `tab` - 書類などのインデックスタブを模した、画面内のコンテンツ表示を切り替えるインターフェイス。[検索する](http://bit.ly/2MMfNnw)
  - `tabbar` - おもに画面下部に固定されたタブで、画面全体を切り替えるインターフェイス。[検索する](http://bit.ly/2MMg4qy)
  - `segmented-control` - 機能的には`tab`と同じで、見た目がタブではなくボタンである点が違う。[検索する](http://bit.ly/2MNlKjW)
- `off-canvas` - 表示領域外から画面の大半を覆い隠したり（オーバーレイ）、ずらすようにスライドしながら表示するナビゲーション。[検索する](http://bit.ly/2MPQaC3)
  - `side-drawer` - `off-canvas`の類語。drawerは「引き出し」の意味。[検索する](http://bit.ly/2NnTnIN)
- `toggle-menu` - 同一の操作で二つの状態を交互に切り換えるインターフェイスで、操作対象になるボタンを基準にナビゲーションを表示することが多い。[検索する](http://bit.ly/2MQ7NBS)
- `sitemap` - サイト内のすべてのページへのリンクをリスト化したもの。[検索する](http://bit.ly/2MKDDzM)
- `sns` - ソーシャルネットワーキングサービス。[検索する](http://bit.ly/2NTFxuI)

### Form
- `form` - 送信フォーム。
- `login` - ユーザー認証をするためのフォーム。[検索する](http://bit.ly/2NhhkkB)
  - `signin` - `login`の類語。[検索する](http://bit.ly/2MLurLu)
  - `logout` - `signout` - ユーザー認証を解除すること。[検索する](http://bit.ly/2NfzuTV)
- `registration` - ユーザー登録をするためのフォーム。[検索する](http://bit.ly/2NjsjKq)
  - `signup` - `registration`の類語。[検索する](http://bit.ly/2MMPKMF)
- `step-navigation` - 複数画面にわたるフォームの順序や、現在地を示したナビゲーション。[検索する](http://bit.ly/2NgEPKR)
- `search-box` - キーワード検索するためのフォームエリア。[検索する](http://bit.ly/2NeUla1)
- ` text-field` - `input[type="text"]`のような利用者が編集する改行なしのテキストフィールド。[検索する](http://bit.ly/2NixGtn)
- `textarea` - `textarea`のような利用者が編集する複数行のテキストフィールド。[検索する](http://bit.ly/2NnU057)
- `checkbox` - `input[type="checkbox"]`のような1つ以上の項目を選択するインターフェイス。[検索する](http://bit.ly/2MK2LGZ)
- `radio` - `input[type="radio"]`のような1つの項目を選択するインターフェイス。[検索する](http://bit.ly/2MQInE4)
- `select` - `select`のような1つの項目を選択するインターフェイス。ラジオボタンと違い、（`dropdown`のように）項目が最初は隠れているのが特徴。[検索する](http://bit.ly/2MK2RhP)
- `submit` - 送信ボタン。[検索する](http://bit.ly/2NfJ1dU)
- `reset` - リセット（取り消し）ボタン。[検索する](http://bit.ly/2MNDz2j)
- `modify` - 修正ボタン。[検索する](http://bit.ly/2MLd1i5)

### Other
- `cards` - トランプのような積み重なったカードのメタファーをもつインターフェイス。[検索する](http://bit.ly/2PG6Zwp)
- `dropdown` - 複数の項目を表示して、1つの項目を選択するインターフェイス。[検索する](http://bit.ly/2NdBtbz)
  - `pulldown` - `dropdown`の類語。[検索する](http://bit.ly/2NixwCh)
- `accordion` - 折りたたまれたコンテンツを選択することで表示させるインターフェイス。[検索する](http://bit.ly/2MNCTdh)
- `scroll-tab` - 表示領域よりも横に長いナビゲーションで、左右にスクロールすることで非表示部分を見ることができるインターフェイス。[検索する](http://bit.ly/2Nf7GiC)
- `dialog` - （主に）注意や警告を知らせるために使用されるメッセージで、ユーザーに操作を要求するのが特徴。[検索する](http://bit.ly/2PCSitL)
  - `toast` - `dialog`の類語で、`dialog`との違いは時間が経つと自動的に消えること。[検索する](http://bit.ly/2PAIEYE)
  - `popup-window` - `dialog`の類語で、リンク先を別の小さなウィンドウで表示するインターフェイス。[検索する](http://bit.ly/2PDw1Mr)
- `toolbar` - ユーザーが利用できるツールやアクションをまとめたインターフェイス。[検索する](http://bit.ly/2NgEB6t)
- `comment` - 記事に対する反応。[検索する](http://bit.ly/2MOoDkC)
- `table` - テーブル・図表。[検索する](http://bit.ly/2NdBJY5)
- `timeline` - チャットや年表のように時系列に並べたリスト。[検索する](http://bit.ly/2MOoJbY)
