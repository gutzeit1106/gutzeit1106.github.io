# Github pages + Hugoでブログ構築


クラウドのVM上でWordpressで公開していたブログをgithub pages + Hugoに移行してみた。
Hugoは静的サイトジェネレータの一つで、Go言語で作られています。MarkDown等の記述性の高い形式で書かれたファイルを指定したデザインテーマでHTML + CSSのWEBコンテンツに変換してくれます。

## Why GitHub Pages?
1. 第一の理由はコストです。クラウドとはいえ Web Server の稼働代金も1000円/月程度かかるので無料で使えるgithub pagesに移行を考えました。

2. カスタムドメインが使用可能。ブログのドメインは変えたくなかったのですが、github pagesはカスタムドメインに対応しています。

3. Httpsが無料で使える。github pagesはでLet's EncryptのSSL証明書が自動発行されるのでカスタムドメインでhttpsが無料で使えます。[Custom domains on GitHub Pages gain support for HTTPS](https://github.blog/2018-05-01-github-pages-custom-domains-https/)

## Why Hugo?
1. Themeのデザインが自分好みのものが多かった。移行に際して、python ベースのpelicanも試してみましたがHugoの方が自分好みのものが多かったので主に使用することに決めました。

2. 記事生成が早い。pelicanと比べてもGoベースのHugoはサクサク動いてくれる感があります。ストレスフリーに作るためにも重要な要素です。

## 手順

1. gitHub上に必要なリポジトリを作成しておく（作業用とgithub.ioの公開用2つ必要になります）

2. Hugoコマンドを利用してblogのコンテンツを生成する

3. 作業用リポジトリにある/publicをgithub.io用の公開リポジトリのサブモジュールとして設定する

4. リポジトリへのpushを自動化する

### 1. GitHub上に必要なリポジトリを作成しておく
github pagesでhugoを運用していくために次の２つのリポジトリをgithub上に用意します。

1. github pagesとして公開するファイルを配置するリポジトリ　
2. 公開ファイルにhugoで変換する前に生コンテンツ（マークダウンなど）を配置するリポジトリ

1のリポジトリについては、github pagesの機能を使うためのgithubのご作法なので、リポジトリ名は指定の形式　username.github.io で作る必要があります。
サイトのURLはデフォルトで https://username.github.io となりますが、カスタムドメインが設定可能なので独自ドメインをつけたい場合は、リポジトリのSettings > Custom domainで指定できます。
2のリポジトリは元コンテンツファイル群を配置するためのリポジトリです。私はprivateリポジトリとして運用しています。
2つのリポジトリを作成したら、2つ目のリポジトリをローカルにcloneします。

```sh 
git clone https://github.com/xxx/xxx.git
```

### 2. Hugoコマンドを利用してblogのコンテンツを生成する
まずはHugoを作業端末にインストールしましょう。MacであればHomebrewでインストールできます。（WindowsはChocolateyでインストールすることができるようです）

```sh 
brew install hugo
```

インストールしたあとは、[HUGO - Quick Start](https://gohugo.io/getting-started/quick-start/)に従って進めていきます。
hugo new site　SiteName でコンテンツの雛形が作成できまるので、cloneしたフォルダの直下で実行しましょう。

```sh 
hugo new site newblog
$ tree
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```
主に編集するフォルダは、上記の中でcontent, theme, config.tomlになると思います。中でもcontentは、公開記事の元となるmarkdownを配置するので一番ファイル作成や編集の頻度が高いと思います。

blog自体の雛形が作られたので、ここからはコンテンツの作成や設定の作業です。
こだわり始めるといくらでもできるので、最低限の３つに絞って記載します。

a.サイトデザイン（theme）の適用

b.ブログ記事の作成

c.設定ファイルの編集（config.toml）

<br>

### a.サイトデザイン（theme）の適用
Hugoでは数多くのテーマが公開されています。
[Hugo Themes](https://themes.gohugo.io/tags/simple/)から、好みのテーマを探してきます。(Demoページなども用意されていて素晴らしい！)
テーマが決まったら上で作成したthemesディレクトリに関連ファイルをcloneしてきます。

```sh
cd themes
git clone https://github.com/Tazeg/hugo-blog-jeffprod.git 
ls
hugo-blog-jeffprod
```

### b.ブログ記事の作成
一旦、config.tomlと同階層まで移動して、hugo new post/new.mdコマンドで記事の元となるファイルを作成します。

```sh
hugo new post/new.md
```

content配下にpost/new.mdが生成されるので、このファイルをエディタで編集していきます。

```sh
---
title: "New"　#タイトル
date: 2019-10-22T18:40:33+09:00　#記事の公開日時
draft: true　#下書きか否か。falseで記事として公開されます。
---
```

一般的に使用するいくつかの項目を足すと次のようになります。

```sh
---
title: "New"
date: 2019-10-22T18:40:33+09:00
draft: false
categories: ["Tech"]
tags: ["git","hugo","go"]
Authors: "Hoge Fuga"
---
```

本文はその下にMarkdown形式で書いていきます。この時、次のコマンドでローカルサーバを起動させておくと記事の修正が逐次反映されてブラウザで確認できるので便利です。
(-tでthemeを指定するのでテーマを試してみたいときにも便利です。)

```sh
hugo server -D -t hugo-blog-jeffprod
```

### c.設定ファイルの編集（config.toml）
使用するテーマに寄って変わる部分があるので、テーマのページなどを参照して追記する必要があります。

```sh
baseURL = "https://xxx.github.io//"
languageCode = "ja-jp" 
title = "Hugo New Blog"
```

内容が固まったらhugo -t で公開ファイルを/public配下に生成します

```sh
hugo -t hugo-blog-jeffprod
```

### 3.作業用リポジトリにある/publicをgithub.io用の公開リポジトリのサブモジュールとして設定する
/public配下には、cssやhtmlなどの公開用ファイルが生成されますが、本来これは github pagesの公開用リポジトリに配置される必要があります。
手動作業で生成したファイルをgithub pagesの公開用リポジトリにpushしていくこともできますが、面倒すぎるので、/public配下をサブモジュールとして別のリポジトリと紐づけておきます。

```sh
git submodule add -b master git@github.com:UserName/UserName.github.io.git public
```

### 4. リポジトリへのpushを自動化する
公開ファイルを生成して、github リポジトリにpushしていくことでブログを更新することになりますが面倒なのでスクリプト化しておきます。
themeなどは環境によって変えると良いとおもいます。

```sh
#!/bin/bash

# 公開ファイルの生成
hugo -t hugo-blog-jeffprod

cd public
git add .

msg="building mysite `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# サブモジュールサイトへのPush
git push origin master

#作業用リポジトリへのpush
cd ..
git add .
git commit -m "$msg"
git push
```

```sh
$ ./deploy.sh
```

デプロイ後に"https：//UserName.github.io/"にアクセスすると自身のサイトが確認できるはずです。

