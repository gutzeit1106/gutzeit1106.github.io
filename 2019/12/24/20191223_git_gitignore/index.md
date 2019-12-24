# git 管理から特定のファイル、ディレクトリを除外する(.gitignore)


秘匿性のあるファイルやキャッシュなどの一時ファイルなど git リポジトリのバージョン管理から特定のファイルやフォルダを除外したいときには .gitignore ファイルを作成します。  
Mac だと, finder のメタ情報を持つ .DS_store がディレクトリの至るところに作成されるので, git を使う場合、除外設定しておかないと面倒です。


### 基本ルール
最も基本的な設定方法は, リポジトリのルートの直下に 1 つの .gitignore ファイルを配置することです。これによってリポジトリ配下のディレクトリに再帰的に .gitignore の設定が適用されます。  
サブディレクトリにも .gitignore を配置することも可能です。サブディレクトリに配置された .gitignore は、配置したディレクトリ以下にのみ適用され、また上位の .gitignore のルールは優先度は低くなり、同じルールがあった場合はより深い階層のルールが優先されます。

### 文法
原則は以下の通りです。

- 空白行, # で開始される行は無視されます
- .gitignore が配置されたパスから再帰的に下位ディレクトリに適応されます
- /からパターンを記載することで再帰性を無効にしたパターンを指定できます
- パターンの最後に/を置くことでディレクトリを指定することができます
- ! で開始することで gitignore の設定を無視します

また、正規表現のような指定も可能です。

- アスタリスク（*）: 0 個以上の文字
- [abc] : 括弧内の任意の文字
- ? : 単一の文字
- [0-9] : 0 から 9 までのいづれかの文字
- **  : 0 個以上のファイルあるいはディレクトリ

具体的なサンプルも記載します。

```sh
#拡張子 .a の全てのファイルを除外
*.a

#上の設定の例外として lib.a は管理に含める。
!lib.a

#カレントディレクトリの TODO のみ除外。サブディレクトリ配下の TODO は除外されない
/TODO

#ディレクトリ build 配下の全てのファイルを除外
build/

#例えば、doc/notes.txt を除外するが, doc/server/arch.txt は除外しない
doc/*.txt

#doc/配下の全ての .pdf ファイルを除外する doc/a.pdf, doc/a/a.pdf なども含む
doc/**/*.pdf
```

### 参考ファイル
自分で文法を理解することも大切だと思いますが、既に出来合いのものを利用することもできます。  
[gitignore.io](https://www.gitignore.io/)では、プログラミング言語などから一般的な .gitignore ファイルを生成することができて非常に便利です。  

また、github も主な .gitignore ファイルをコレクションしています。

github/gitignore   
https://github.com/github/gitignore

もっと小規模なものだと、octocat くんが、汎用的な.gitignore設定のサンプルをgistで公開してくれているので、とりあえずこれを設定してみるのも良いかと思います。

<script src="https://gist.github.com/octocat/9257657.js"></script>

### 補足
特定のリポジトリだけでなく、ユーザが全ての状況で git 管理から除外する設定を行いたい場合もあります。(global)
この場合は ~/.gitignore_global に .gitignore ファイルを作成して次のコマンドを実行します。

```sh
$ git config --global core.excludesfile ~/.gitignore_global
```
また、 .gitigonore 自体をリポジトリ管理したくない場合は、リポジトリ配下にある .git/info/exclude にルールを追加します。

### 関連
Git  
https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository#_ignoring  
https://git-scm.com/docs/gitignore  

Github Help - Ignoring files  
https://help.github.com/en/github/using-git/ignoring-files