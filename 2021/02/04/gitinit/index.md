# git init の手順


git init など、リポジトリを作成して git 管理し始める時の手順を忘れがちなため備忘録。

### 既存のローカル環境がない場合
github などに既存のリモートリポジトリがない状態から、新規にリモートリポジトリを作成して、ローカルに clone してくる場合。
シンプルで基本的なパターン。

```sh
git clone https://github.com/xxx/xxx.git #リモートリポジトリから clone してくる 
```

ローカルのユーザ設定を確認して設定しておかないと push した時に、想定と違うユーザで commit したことになるので注意が必要。

```sh
git config user.name #ユーザ名の設定確認
git config user.email #ユーザemailの設定確認
```

もし、何も設定されていない、想定と異なるような場合は明示的に指定しておく。

```sh
git config user.name "<UserName>" # github username
git config user.email "<UserEmail>" # github email
```

あとはファイル追加したり、編集して、add, commit, push する。

```sh
git add .
git commim -m "hogefuga"
git push
```

### 既存のローカルディレクトリをリモートリポジトリに同期する場合

同期したいローカルディレクトリに移動して、git init で git リポジトリとして初期化する

```sh
git init
git config user.name "<UserName>" # github username
git config user.email "<UserEmail>" # github email
```

リモートリポジトリの設定をおこなう。

```sh
git remote add origin https://github.com/xxx/xxx.git
git push -u origin main
```

この時、こんな感じで push ができない場合がある。

```sh
$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'https://github.com/xxx/xxx.git'
```

github はデフォルトの branch が master から main に変わったので、ローカルの branch が master だと branch が一致しないと怒られる。
そんな時は、`git branch` で branch 名を確認して、`-m` で main に変更してから push すれば良い。

```
$ git branch
* master
$ git branch -m master main
$ git push -u origin main
```

関連としてリモートリポジトリの設定を確認する場合は、 `git remote -v` を使用する。

```sh
$ git remote -v
origin	https://github.com/xxx/xxx (fetch)
origin	https://github.com/xxx/xxx (push)
```

またリモートリポジトリの設定を削除する場合は、`git remote remove` を使う。もし `git remote add` を間違えたならこれで削除して再度 `add` すれば良い。 

```sh
git remote remove origin
```