# Docker Cheat Sheet #2 - 表示系

・コンテナの一覧表示 
```sh
dokcer ps // or docker container list 

#※defaultはrunningされたコンテナのみ表示 -a で全表示 
dokcer ps -a
```

・コンテナイメージの一覧表示 
```sh
docker images 
```

・docker hubからのイメージ検索 
```sh
docker search <ImageName> 

#Officalのイメージのみ検索 
docker search --filter is-official=true <ImageName> 
```