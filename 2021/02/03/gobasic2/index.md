# Go の基本的なあれこれ #2


### GOROOT
GoSDK の配置場所を定義する環境変数。
Linux だと `GOROOT="/usr/lib/go-x.xx"`、Windows だと `GOROOT=c:\go` が既定で設定されている。
異なるGoバージョンを共存して使用するというようなシナリオがない限り、この変数を変更することは基本的にない。   
$GOPATH と $GOROOT といった Go 環境変数は `go env` で設定値を確認することができる。

### GOPATH
GOPATHは、ワークスペースのルートパスを定義する変数。   
既定では Linux だと `GOPATH="/home/<user>/go` (=`$HOME/go`), Windows だと  `GOPATH=C:\Users\<user>\go` (=`%USERPROFILE%\go`) になる。   
この GOPATH は import 文の解決に利用される。
GOPATH にリストされている各ディレクトリは、以下のような決められた構造を持つ必要がある。


```sh
GOPATH=/home/user/go

    /home/user/go/
        src/
            foo/
                bar/               (go code in package bar)
                    x.go
                quux/              (go code in package main)
                    y.go
        bin/
            quux                   (installed command)
        pkg/
            linux_amd64/
                foo/
                    bar.a          (installed package object)
```

src ディレクトリにはソース コードが格納される。   
pkg ディレクトリには、インストールされたパッケージ オブジェクトが格納される。   
例えば、DIR/src/foo/bar に配置されたソースがある場合、DIR が GOPATH にリストされているディレクトリであれば、パッケージは `foo/bar` としてインポートすることができる。   
bin ディレクトリには、コンパイル済みコマンドが格納される。   
また、`internal` という名前のディレクトリを配置すると、同一ディレクトリ内またはディレクトリ以下のコードからのみインポートのみ可能とすることができる。

GOPATH は変更可能で、各環境での変更方法などの詳細は [SettingGOPATH](https://github.com/golang/go/wiki/SettingGOPATH) に記載がある。

Go 1.13 から go.mod が存在する状態で go get するときに module-aware mode となり、
$GOPATH/pkg/mod の下に依存モジュールが保存されるようになった。

### go install

```
go install [-i] [build flags] [packages]
```

指定されたパッケージをコンパイルして、インストールする。実行ファイルは、GOBIN 環境変数で設定されたディレクトリに配置される。
デフォルトでは、`$GOPATH/bin`　となるが GOPATH 環境変数が設定されていない場合は、`$HOME/go/bin` に配置される。

### go clean

```
go clean [clean flags] [build flags] [packages]
```
buid などで作成された source ディレクトリ内の以下のオブジェクトファイルを削除することができる。

```
_obj/            old object directory, left from Makefiles
_test/           old test directory, left from Makefiles
_testmain.go     old gotest file, left from Makefiles
test.out         old test log, left from Makefiles
build.out        old test log, left from Makefiles
*.[568ao]        object files, left from Makefiles

DIR(.exe)        from go build
DIR.test(.exe)   from go test -c
MAINFILE(.exe)   from go build MAINFILE.go
*.so             from SWIG
```

-i フラグを指定すると、インストール済みバイナリ (`go install` で作成されるもの) が削除される。<br>
-n フラグを指定すると、実際には削除されずに削除されるファイルが出力される。<br>
-r フラグを指定すると、インポートパスで指定されたパッケージのすべての依存関係に対して clean が再帰的に適用される。<br>
-x フラグを指定すると、削除コマンドが出力され、削除が同時に実行される。<br>
-cache フラグを指定すると、`go build`のキャッシュ全体が削除される。<br>
-testcache フラグを指定すると、`go build`のキャッシュ内のテスト結果が全て削除される。<br>
-modcache フラグを指定すると、依存関係のあるソースコードを含む module ダウンロードキャッシュ全体が削除される。

### go fmt

```
go fmt [-n] [-x] [packages]
```

`gofmt -l -w` コマンドを実行し、タブや空白などで Go プログラムのフォーマットを整形してくれる。

### go doc

```
go doc [-u] [-c] [package|[package.]symbol[.methodOrField]]
```

Goのパッケージのドキュメントを参照するためのコマンド


### 参考
[Compile and install packages and dependencies](https://golang.org/cmd/go/#hdr-Compile_and_install_packages_and_dependencies)<br>
[Remove object files and cached files](https://golang.org/cmd/go/#hdr-Remove_object_files_and_cached_files)<br>
[Gofmt (reformat) package sources](https://golang.org/cmd/go/#hdr-Gofmt__reformat__package_sources)<br>