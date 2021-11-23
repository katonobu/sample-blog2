# 自治会・町内会向け回覧板を想定ユースケースとしたブログシステム
## 狙いどころ
- 極力運用の手間・コストを最小にして運用できる。
- 近隣で共通化できるデータを共通化する。
- 容易に他の町内会/自治会に展開できる。
- 当面、各世帯毎の「既読」の管理は見送る。
  - 個人情報の扱い、アカウント管理等急に敷居が高くなる。
  - 当面、一方的な情報提供サイトとして運用する。

## 今後の希望的予定
### 想定Monthly ワークフロー
- 地区からのお知らせ、市・区からのお知らせのデータ作成(連合で実施できると手間分散可能)
  - 回覧物をpngで取り込み、Google Driveに入れる
    - まずは1ファイル/面を想定
  - Google DriveのSheetにページ順、タイトル、ファイル名を記載
    - 本当はここ、自動化したい。

- 各自治会別データ作成(各自治会/町内会で実施)
  - Google Docで作成しておく

- ビルド&デプロイ(各自治会/町内会で実施)
  - 各自治会別ドライブにデプロイキック用ボタンを持つGoogle Sheetを準備しておく。
  - ボタンを押すとstatic サイトが生成され、デプロイされる

### 作業優先順位
- Google Docを元にページを生成する機能の実装/課題抽出

### 運用上の課題
- 毎月発生する運用の手間がまだ見えていない。
  - 特にデータの取り込み、タイトル付け等の手間が見えていない。
  - まずはフル手作業から入って、型が見えてきたら少しずつ自動化していく

### 技術的疑問点・課題
- localファイルにpdfを指定してstaticデータを作れるの?
- 月毎・自治会毎情報がまだきれいに分離できていない。
  - 適切なファイル構成の検討/運用
- 運用費用
  - staticファイル動作なので、デプロイ先を毎月変えて、各クラウドサービスの無料枠を毎月渡り歩く運用はありかも。

### 技術情報
- [Google Doc](https://docs.google.com/)をもとにしてページを生成するplugin
  - https://github.com/cedricdelpoux/gatsby-source-google-docs
- Google Driveからファイルをダウンロードしてローカルファイル扱いでページを生成するplugin
  - https://github.com/richseviora/gatsby-plugin-drive


## 開発履歴
### 72c28c3e63f3fbd539168c4f426abd3db343d376 まで
#### 環境
```sh
$ node --version
v16.13.0
$ gatsby --version
Gatsby CLI version: 4.1.3
```
#### まずはサンプルから生成
https://kyabe.net/blog/making-blog-with-gatsbyjs/ を参考に、
```
$ gatsby new blog-sample2 https://github.com/gatsbyjs/gatsby-starter-blog
:
$ cd sample-blog
$ gatsby develop
```
### d29eb7fd5ba932b5c29abc56f985f88b2f0e4e2c の修正
#### モデラ用にカスタム
- ファイル削除
  - src/pages/using-typescript.tsx
  - content/blog/hello-world/index.md
  - content/blog/hello-world/salty_egg.jpg
  - content/blog/my-second-post/index.md
  - content/blog/new-beginnings/index.md
  - src/images/gatsby-icon.png
  - src/images/profile-pic.png
- ファイル新規追加
  - サンプルコンテンツ
    - content/blog/city-information/index.md
    - content/blog/regional-area-information/index.md
    - content/blog/residents-information/index.md
  - アイコン差し替え
    - src/images/ModeraLogo401x300.png
- ファイル修正
  - gatsby-config.js
    - siteMetadataの修正
      - siteMetadata.title
      - siteMetadata.author.name
      - siteMetadata.author.summary
      - siteMetadata.description
      - siteMetadata.siteUrl      
      - siteMetadata.social.twitter
    - gatsby-plugin-manifest optionの修正
      - name
      - short_name
      - icon

  - src/components/bio.js
    - Bio のStaticQueryから`social {twitter}`を削除
    - 変数`social`を削除
    - `StaticImage`の`src`を変更
    - 表示文字列を`author?.summary`のみに変更
  - src/components/layout.js
    - footer表示文字列を変更
  - src/pages/404.js
    - 文言修正
    - TOPページへ戻りやすいように文言中にリンクを追加
  - src/pages/index.js
    - Seo title を修正
    - article の子要素sectionをまるっと削除

### c3800bd9eb59d4a846c329e447f0d99630c3456e の修正
#### GitHub-pageにデプロイできるようにする
- `gh-pages`ブランチを作成
- GitHubのリポジトリの設定ページ - Pagesページから、Sourceをgh-pagesのルートに設定
- gh-pagesをインストール
```sh
$ npm install gh-pages --save-dev
```
- pathPrefix: "/sample-blog2",を追加
```js
module.exports = {
  siteMetadata: {
    :
  },
  pathPrefix: "/sample-blog2",
  plugins: [
    :
  ]
};
```
- package.json にscript.deployエントリを追加
```json
{
  "scripts": {
    "deploy": "gatsby build --prefix-paths && gh-pages -d public"
  }
}
```
- build & デプロイ
```sh
$ npm run deploy
```

### xxxの修正
本ファイルの更新

### pdfの実験
#### jsページに直接リンクを埋め込み
https://www.fixes.pub/program/353141.html

1. sample-blog/static/sample.pdf を追加
1. sample-blog/src/pages/index.js に追加
```js
import sample_pdf from "../../static/sample2.pdf"
：
  return (
    <Layout>
    :
      <a href={sample_pdf} target="_blank" rel="noreferrer">まとめてPDFで見る</a>
    :
    </Layout>
  )
```

#### Markdownからリンクを埋め込み
すでに`gatsby-remark-copy-linked-files`はインストールされていたので、、
- sample-blog/content/blog/residents-information/sample.pdf を追加
- sample-blog/content/blog/residents-information/index.md に下記を追加
```md
- [pdfへのサンプル](sample.pdf)
```