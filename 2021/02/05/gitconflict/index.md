# git pull で conflict した場合の対処方法 


git pull した時に以下のようなエラーで conflict が発生した場合の対処をまとめる

```
error: Your local changes to the following files would be overwritten by merge:
    Directory/File
Please, commit your changes or stash them before you can merge.
```

### 1. stash でワーキングツリーの内容を退避
コンフリクトを起こしているワーキングツリーの内容を `git stash` で退避する。

```sh
$ git stash
```

stash で退避した内容を確認する。

```sh
$ git stash list
```

この状態で pull すると成功する。

```sh
$ git pull origin main
```

退避した内容を `git stash pop` で戻す。   

```sh
$ git stash pop
```

pull で取り込んだ内容とコンフリクトしている箇所は, `git status` で確認できる。

```sh
$ git status
On branch main
Your branch is up to date with 'origin/main'.

Unmerged paths:
  (use "git restore --staged <file>..." to unstage)
  (use "git add <file>..." to mark resolution)
        both modified:   ***

no changes added to commit (use "git add" and/or "git commit -a")
```

コンフリクトした箇所にはこんな感じで変更前と変更後が表示されるので修正する。

```
<<<<<<< Updated upstream
xxxxx
=======
xxxxx
>>>>>>> Stashed changes
```

修正したら `git add`, `git commit`, `git push` する。


### 2. 強制マージ (リモートリポジトリの状態にする)

1) リモートリポジトリの最新をリモート追跡ブランチ (origin/main) に取り込む。

```sh
$ git fetch origin main
```

2) ワーキングディレクトリをリモート追跡ブランチ (origin/main) に強制的に変更する

```sh
$ git reset --hard origin/main
```

### 参考
[git-resetは結局何を戻すのか](https://qiita.com/fnobi/items/ec036c1b5d7ee5a8517c)

[git reset 解説](https://qiita.com/forest1/items/f7c821565a7a7d64d60f)

[Git のさまざまなツール - リセットコマンド詳説](https://git-scm.com/book/ja/v2/Git-%E3%81%AE%E3%81%95%E3%81%BE%E3%81%96%E3%81%BE%E3%81%AA%E3%83%84%E3%83%BC%E3%83%AB-%E3%83%AA%E3%82%BB%E3%83%83%E3%83%88%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89%E8%A9%B3%E8%AA%AC)