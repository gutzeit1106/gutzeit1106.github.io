# git init の取り消し方法


git init　でリポジトリ作成したけども取り消したい場合の対処
git init したフォルダで .git を削除すればよい。

```
$ ls -a
.               ..              .git              readme.md
$ rm -rf .git
```