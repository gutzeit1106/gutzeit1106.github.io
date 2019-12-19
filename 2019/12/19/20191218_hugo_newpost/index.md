# hugo new 実行時に生成されるデフォルトテンプレートをカスタマイズする


### はじめに
hugo で、ブログ post を追加する時は、次のコマンドで新しい markdown ファイルを生成することができる。

```sh
hugo new posts/20191218_hugo_newpost.md
```

この時、デフォルトだとこんな感じのシンプルな内容が含まれるが、これを任意にカスタマイズする方法を考える。

```markdown
---
title: "hugo new 時に生成されるデフォルトテンプレートをカスタマイズする"
date: 2019-12-19T18:05:54+09:00
draft: true
---
```

### 方法
hugo 公式に詳細な説明があった。   

 Archetypes   
 https://gohugo.io/content-management/archetypes/
    

次の 4 つのファイルを上から参照し、最初に見つかったファイルの設定に応じて新しい md ファイルが生成される。

1. archetypes/posts.md
2. archetypes/default.md
3. themes/my-theme/archetypes/posts.md
4. themes/my-theme/archetypes/default.md

というわけで 1 の archetypes/posts.md に好みのテンプレートを設定してみる。

### 適用
post するときの設定に加えて、markdown のチートシートも記載しておく。これで生成される md に最初から cheet cheet が含まれるので新しい記事を書こうとするたびに忘れた markdown 記法を調べる時間が削減できる。

```md
---
title: "xxx"
date: {{ .Date }}
draft: true
categories: [""]
tags: ["","",""]
Authors: "Yusuke Yoneda"
comment: true
toc: true
autoCollapseToc: false
math: true
---

### 1. はじめに
xxxx

### x. Markdown CheetSheet

#### Text Format

_Italic（斜体）_
*Italic（斜体）*

__Emphasis（強調）__
**Emphasis（強調）**

~~Strikethrough（取り消し線）~~

<details><summary>これは詳細表示の例です。</summary>詳細をこっちに書きます。</details>

This is `inline`.

### List
* text
    * test
    * test

- text
    - test
    - test

1. text
1. test
    1. test

#### Horizontal rules
* * *
***
*****
- - -
---------------------------------------

#### Blockquotes（引用）
> This is Blockquotes

#### Links（参照）
[yonehub blog](https://yonehub.y10e.com/)

#### Images（画像）
![sample](/img/sample/sample.png)

#### Tables（表）
| id     | name    | date       |
| ------ | ------- | ---------- |
| 1      | test    | 2019-01-01 |
| 2      | test    | 2019-01-02 |
| 3      | test    | 2019-01-03 |
```

ちなみに 上の markdown が実際に html として適用されると以下の感じになる。

### x. Markdown CheetSheet

#### Text Format

_Italic（斜体）_
*Italic（斜体）*

__Emphasis（強調）__
**Emphasis（強調）**

~~Strikethrough（取り消し線）~~

<details><summary>これは詳細表示の例です。</summary>詳細をこっちに書きます。</details>

This is `inline`.

### List
* text
    * test
    * test

- text
    - test
    - test

1. text
1. test
    1. test

#### Horizontal rules
* * *
***
*****
- - -
---------------------------------------

#### Blockquotes（引用）
> This is Blockquotes

#### Links（参照）
[yonehub blog](https://yonehub.y10e.com/)

#### Images（画像）
![sample](/img/sample/sample.png)

#### Tables（表）
| id     | name    | date       |
| ------ | ------- | ---------- |
| 1      | test    | 2019-01-01 |
| 2      | test    | 2019-01-02 |
| 3      | test    | 2019-01-03 |