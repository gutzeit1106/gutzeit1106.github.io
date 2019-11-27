# GitのCommitユーザを修正する方法


githubのcontributionsを見ていたら確かにリポジトリ更新したはずなのに草が生えてなかった。 contributions Activityを見ると、なぜか違うユーザでcommitしたことになっている。 どうやら、メインのPCはMacなのだがたまにサブのWindowsからおこなったcommitが別ユーザで実施したことになっているらしい。そこでgitのcommitユーザの確認と過去のcommitユーザの修正方法についてまとめておく。 

#CommitterとAuthorの違い 
まず過去のcommitのlogを見るにはgit logを使う。 

```sh 
$ git log --pretty=fuller 
commit xxxxx085dd658593ca4c42f81ea636134171cec7 (HEAD -> master, origin/master) 
Author:     abc <abc@xxx.com> 
AuthorDate: Fri Feb 1 00:22:06 2019 +0900 
Commit:     abc <abc@xxx.com> 
CommitDate: Fri Feb 1 00:22:06 2019 +0900
``` 

このとき, Authorは、オリジナルのCode作成者,Committerはコミットをした人になる。 
gitは、rebaseやcommit --amendを用いてhistoryを変更できる場合がある。なので元の作成者を分けて管理しているらしい。 

#Author と Committer 変更方法 
commitする際に記録される　AuthorとCommitterはローカルリポジトリの .git/config に保存されている。この設定は次で確認できる 

```sh 
git config --list 
``` 

なお個別の項目は次のように表示する 

```sh 
git config user.name 
git config user.email 
``` 

ここで次のように user.name と user.email　を変更する。 

```sh 
git config user.name "hogehoge" 
git config user.email hoge@fuga.com 
``` 

そうすると以後のcommitでAuthorとCommitterが変更される 

```sh 
$ git log --pretty=full 
commit xxed4a4c145fb1bc624f34331354795760168ab9 (HEAD -> master, origin/master) 
Author: hogehoge <hoge@fuga.com> 
Commit: hogehoge <hoge@fuga.com> 
``` 

#過去のCommitのAuthorとCommitterを変更する方法 
過去の全てのcommitについて変更したい場合以下を実行する 

```sh
 git filter-branch -f --env-filter \ 
  "GIT_AUTHOR_NAME='new'; \ 
   GIT_AUTHOR_EMAIL='new@example.com'; \ 
   GIT_COMMITTER_NAME='new'; \ 
   GIT_COMMITTER_EMAIL='new@example.com';" \ 
  HEAD 
``` 

変更が反映されていることを確認 

```sh 
$ git log --pretty=full 
commit f2ff63e0090750f06c2a9115960220e0a21b9bb9 (HEAD -> master) 
Author: new <new@example.com> 
Commit: new <new@example.com> 
``` 

githubなどのリモートリポジトリへの反映はこの状態で強制的にpushする。 
```sh 
git push -f 
```

#補足
特定条件のcommitのみ修正したい場合は、次のように条件を追記する。 
```sh
git filter-branch --commit-filter '  
  if [ "$GIT_COMMITTER_EMAIL" = "new@example.com" ]; 
  then 
    GIT_COMMITTER_NAME="new2"; 
    GIT_AUTHOR_NAME="new2"; 
GIT_COMMITTER_EMAIL="new2@example.com"; 
GIT_AUTHOR_EMAIL="new2@example.com"; 
git commit-tree "$@"; 
else 
git commit-tree "$@"; 
fi'  HEAD 
``` 