# Docker Cheat Sheet #1 - 基本系


・help 表示
```sh
docker --help 
```

・Dockerイメージの作成 
```sh
docker build -t ImageName:TagName {DockerFileのあるディレクトリ} 
 
#example
docker build --tag sample/basic:1.0 . 
```
 

・Dockerイメージのリスト表示 
```sh
docker images 
```

・Dokcer コンテナの起動 
```sh
docker run  
#example
docker run -it -d -p 8080:80 --name basic sample/basic:1.0 
```

・Docker コンテナの一覧 
```sh
docker ps 
```
 
・コンテナ内に入る 
```sh
docker exec -it [コンテナ名] /bin/bash 
#example
docker exec -it basic /bin/bash 
```
 

Docker コンテナの停止 
```sh
docker stop 
#example
docker stop basic
```