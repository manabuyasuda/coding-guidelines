# コーディングガイドライン

## 目次

- [CSS](css/README.md)
  - [CSSのガイドライン](css/css-guideline.md)
  - [ECSS](css/how-to-ecss.md)
  - [CSSクラス名リスト](css/css-naming-list.md)
- [画像](image/README.md)
  - [画像のガイドライン](image/image-guideline.md)

## フォーマット
- 文字コードは UTF-8（BOM無し）
- 改行コードはLF（本番環境でシェアの高い Linux の改行コードに合わせる）
- インデントは以下を推奨する（別途、案件ガイドラインや使用フレームワーク等の規約があれば、そちらを優先してよい）
- HTML/CSS/JavaScript
  - スペース2つを使用 （Google/GitHub等の主要ガイドラインと同じルールを採用）
- PHPファイル
  - スペース4つを使用 （PHP標準のガイドライン PSR のルールを採用）
- インデントにタブを使用する場合も、タブによるインデント/スペースによるインデントと混在することがないようにする。

いずれの場合も、フォーマット設定は[editorconfig](http://editorconfig.org/)等のツールを使用し、自動的に整形されることが望ましい。

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
