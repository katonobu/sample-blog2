# 自治会町内会向け回覧板システムを想定したブログシステム
## 狙いどころ
- 極力手間を省いて運用できる。
- 近隣で共通化できるデータを共通化する。
- 容易に他の町内会/自治会に展開できる。

## 開発履歴
### 初版
#### 環境
```bash
$ node --version
v16.13.0
$ gatsby --version
Gatsby CLI version: 4.1.3
```
#### まずはサンプルから生成
https://kyabe.net/blog/making-blog-with-gatsbyjs/ を参考に、
```
$ gatsby new sample-kairan https://github.com/noahg/gatsby-starter-blog-no-styles
:
$ cd sample-blog
$ gatsby develop
```
#### モデラ用にカスタム

### GitHub-pageにデプロイ
- gh-pagesをインストール

`npm install gh-pages --save-dev`
- pathPrefix: "/sample-blog2",を追加
```
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
```
{
  "scripts": {
    "deploy": "gatsby build --prefix-paths && gh-pages -d public"
  }
}```