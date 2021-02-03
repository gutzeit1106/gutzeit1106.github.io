# Go Modules の基本的なこと 


Modules は Go の依存モジュール管理ツール。   
Go言語 1.11 から標準で使用でき、依存モジュールの自動検知、バージョン固定、バージョンアップ検知などができる。
依存モジュールは go.mod と go.sum というファイルで管理されるため、このファイルを git で管理し、プログラムの依存モジュールの情報を明示することができる。   
node.js でいうところの npm、python でいうところの pip、.NET でいうところの nuget 的なもの。

### 使い方
基本的な使い方としては、プロジェクトのルートディレクトリで以下の手順を実行する。

1. `go mod init` で初期化
2. `go build` で go プログラムをビルドする
3. `import`などで定義された module が自動インストールされる

他にも、<br>
`go list -m all` で現在の依存モジュールをリストしたり、<br>
`go get <modulename>` で依存モジュールの追加やアップグレードをしたり、<br> 
`go mod tidy` で不要な依存モジュールを削除できたりする。

### go.mod と go.sum
実際に`go mod init` して `go build` した後に生成された、go.mod と go.sum の例。   
Go で Life Game をつくろうとしていたのだが、`github.com/gdamore/tcell` を import していたので依存モジュールが追加されていることがわかる。

- go.mod
```
module github.com/y10e/go-life

go 1.13

require github.com/gdamore/tcell v1.4.0 // indirect
```

- go.sum
```
github.com/gdamore/encoding v1.0.0 h1:+7OoQ1Bc6eTm5niUzBa0Ctsh6JbMW6Ra+YNuAtDBdko=
github.com/gdamore/encoding v1.0.0/go.mod h1:alR0ol34c49FCSBLjhosxzcPHQbf2trDkoo5dl+VrEg=
github.com/gdamore/tcell v1.4.0 h1:vUnHwJRvcPQa3tzi+0QI4U9JINXYJlOz9yiaiPQ2wMU=
github.com/gdamore/tcell v1.4.0/go.mod h1:vxEiSDZdW3L+Uhjii9c3375IlDmR05bzxY404ZVSMo0=
github.com/lucasb-eyer/go-colorful v1.0.3 h1:QIbQXiugsb+q10B+MI+7DI1oQLdmnep86tWFlaaUAac=
github.com/lucasb-eyer/go-colorful v1.0.3/go.mod h1:R4dSotOR9KMtayYi1e77YzuveK+i7ruzyGqttikkLy0=
github.com/mattn/go-runewidth v0.0.7 h1:Ei8KR0497xHyKJPAv59M1dkC+rOZCMBJ+t3fZ+twI54=
github.com/mattn/go-runewidth v0.0.7/go.mod h1:H031xJmbD/WCDINGzjvQ9THkh0rPKHF+m2gUSrubnMI=
golang.org/x/sys v0.0.0-20190626150813-e07cf5db2756/go.mod h1:h1NjWce9XRLGQEsW7wpKNCjG9DtNlClVuFLEZdDNbEs=
golang.org/x/text v0.3.0 h1:g61tztE5qeGQ89tm6NTjjM9VPIm088od1l6aSorWRWg=
golang.org/x/text v0.3.0/go.mod h1:NqM8EUOU14njkJ3fqMW+pc6Ldnwhi/IjpwHt7yyuwOQ=
```

### 現在の依存モジュールをリスト
上記の状態で `go list -m all` すると依存関係のあるモジュールがリストアップされる。

```sh
$ go list -m all
github.com/y10e/go-life
github.com/gdamore/encoding v1.0.0
github.com/gdamore/tcell v1.4.0
github.com/lucasb-eyer/go-colorful v1.0.3
github.com/mattn/go-runewidth v0.0.7
golang.org/x/sys v0.0.0-20190626150813-e07cf5db2756
golang.org/x/text v0.3.0
```

### 依存モジュールの追加
試しに glog を `go get` してみる。

```sh
$ go get github.com/golang/glog
go: finding github.com/golang/glog latest
go: downloading github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b
go: extracting github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b
```

このとき、`go list -m all` や go.mod, go.sum をみると glog が追加されているのがわかる。

```sh
$ go list -m all
github.com/y10e/go-life
github.com/gdamore/encoding v1.0.0
github.com/gdamore/tcell v1.4.0
github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b
github.com/lucasb-eyer/go-colorful v1.0.3
github.com/mattn/go-runewidth v0.0.7
golang.org/x/sys v0.0.0-20190626150813-e07cf5db2756
golang.org/x/text v0.3.0
```

- go.mod
```
module github.com/y10e/go-life

go 1.13

require (
	github.com/gdamore/tcell v1.4.0 // indirect
	github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b // indirect
)
```

- go.sum
```sh
github.com/gdamore/encoding v1.0.0 h1:+7OoQ1Bc6eTm5niUzBa0Ctsh6JbMW6Ra+YNuAtDBdko=
github.com/gdamore/encoding v1.0.0/go.mod h1:alR0ol34c49FCSBLjhosxzcPHQbf2trDkoo5dl+VrEg=
github.com/gdamore/tcell v1.4.0 h1:vUnHwJRvcPQa3tzi+0QI4U9JINXYJlOz9yiaiPQ2wMU=
github.com/gdamore/tcell v1.4.0/go.mod h1:vxEiSDZdW3L+Uhjii9c3375IlDmR05bzxY404ZVSMo0=
github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b h1:VKtxabqXZkF25pY9ekfRL6a582T4P37/31XEstQ5p58=
github.com/golang/glog v0.0.0-20160126235308-23def4e6c14b/go.mod h1:SBH7ygxi8pfUlaOkMMuAQtPIUF8ecWP5IEl/CR7VP2Q=
github.com/lucasb-eyer/go-colorful v1.0.3 h1:QIbQXiugsb+q10B+MI+7DI1oQLdmnep86tWFlaaUAac=
github.com/lucasb-eyer/go-colorful v1.0.3/go.mod h1:R4dSotOR9KMtayYi1e77YzuveK+i7ruzyGqttikkLy0=
github.com/mattn/go-runewidth v0.0.7 h1:Ei8KR0497xHyKJPAv59M1dkC+rOZCMBJ+t3fZ+twI54=
github.com/mattn/go-runewidth v0.0.7/go.mod h1:H031xJmbD/WCDINGzjvQ9THkh0rPKHF+m2gUSrubnMI=
golang.org/x/sys v0.0.0-20190626150813-e07cf5db2756/go.mod h1:h1NjWce9XRLGQEsW7wpKNCjG9DtNlClVuFLEZdDNbEs=
golang.org/x/text v0.3.0 h1:g61tztE5qeGQ89tm6NTjjM9VPIm088od1l6aSorWRWg=
golang.org/x/text v0.3.0/go.mod h1:NqM8EUOU14njkJ3fqMW+pc6Ldnwhi/IjpwHt7yyuwOQ=
```

### 使用していないモジュールを削除
`go mod tidy` で使用していないモジュールを削除する。glog が削除されていることがわかる。

```sh
$ go mod tidy
$ go list -m all
github.com/y10e/go-life
github.com/gdamore/encoding v1.0.0
github.com/gdamore/tcell v1.4.0
github.com/lucasb-eyer/go-colorful v1.0.3
github.com/mattn/go-runewidth v0.0.7
golang.org/x/sys v0.0.0-20190626150813-e07cf5db2756
golang.org/x/text v0.3.0
```

### 参考
[Command go](https://golang.org/cmd/go/#hdr-Compile_and_run_Go_program)