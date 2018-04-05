# フォーマット
- 文字コードはUTF-8（BOM無し）。
- 改行コードはLF（本番環境でシェアの高い Linux の改行コードに合わせる）。
- インデントは以下を推奨する（別途、案件ガイドラインや使用フレームワーク等の規約があれば、そちらを優先してよい）。
  - HTML・CSS・JavaScriptと関連するプリプロセッサはスペース2つを使用。
  - PHPはスペース4つを使用（PHP標準のガイドライン PSR のルールを採用）。
- インデントにタブを使用する場合も、タブによるインデント/スペースによるインデントと混在することがないようにする。

上記のフォーマット設定を反映するため、[editorconfig](http://editorconfig.org/)を導入します。主要なテキストエディタは対応していますが初期設定が必要です。対応していないテキストエディタは非推奨とします。

```
root = true

[*]
end_of_line = lf
charset = utf-8
indent_style = space
indent_size = 2
trim_trailing_whitespace = true
insert_final_newline = true

[*.{md,pug}]
trim_trailing_whitespace = false

[*.php]
indent_size = 4
```
