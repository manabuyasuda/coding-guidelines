# JavaScriptのガイドライン
「Airbnb JavaScript Style Guide」に則ります。  
スター数も多く、ルールの説明も含めてドキュメント化されています。このガイドラインをベースに、プロジェクトによって変更を加えてください。

- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)（[日本語訳](https://github.com/mitsuruog/javascript-style-guide)）
- [Airbnb JavaScript Style Guide es5](https://github.com/airbnb/javascript/tree/es5-deprecated/es5)（[日本語訳](https://github.com/mitsuruog/javascript-style-guide/blob/master/es5/README.md)）

[Vue.js](https://jp.vuejs.org/)のように、JavaScriptのフレームワークにスタイルガイドがある場合は、そのルールも遵守してください。

- [スタイルガイド — Vue.js](https://jp.vuejs.org/v2/style-guide/)

## Lint
「[ESLint](https://eslint.org/)」を利用して、ガイドラインを守っているのか自動で検証します。

- [eslint-config-airbnb-base](https://www.npmjs.com/package/eslint-config-airbnb-base)
- [eslint-plugin-vue](https://www.npmjs.com/package/eslint-plugin-vue)

## Format
フォーマットは「[Prettier](https://prettier.io/)」のルールを優先します。  
Prettierには変更できる設定がほとんど用意されていないので、フォーマットの一貫性を保ちやすくします。

ESLintと併用するにはパッケージが必要です。

- [eslint-config-prettier](https://www.npmjs.com/package/eslint-config-prettier)
- [eslint-plugin-prettier](https://www.npmjs.com/package/eslint-plugin-prettier)
