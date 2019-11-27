# Docker Cheat Sheet #3 - 削除系コマンド


停止コンテナ、タグ無しイメージ、未使用ボリューム、未使用ネットワーク一括削除
```sh
docker system prune
```

停止コンテナ一括削除
```sh
docker container prune
```

全コンテナ一括削除
```sh
docker rm -f `docker ps -a -q`
```

未使用イメージ一括削除
```sh
docker image prune
```

タグ無しイメージ一括削除
```sh
docker rmi `docker images -f "dangling=true" -q`
```

未使用ボリューム一括削除
```sh
docker volume rm docker volume ls -q
```

未使用ネットワーク一括削除
```sh
docker network prune
```

コンテナを動いているとイメージを削除できないので、本当に全部削除したい時は次の順序で実施する

コンテナを全削除する
```sh
docker ps -aq | xargs docker rm
```

イメージを全削除する
```sh
docker images -aq | xargs docker rmi
```