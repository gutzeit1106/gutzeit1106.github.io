# Go の基本的なあれこれ #1


### package
Go では、全てのプログラムがいづれかのパッケージに所属している。パッケージは NameSpace 的なもの。
原則として以下のルールが存在する。

- 1つのファイルに複数のパッケージを設定しない
- 1つのディレクトリに複数のパッケージを配置しない
- package main にある main 関数がプログラムのエントリーポイントとなる（処理の開始点）

```go:hello.go
package main

import "fmt"

func main() {
  fmt.Printf("hello world!")
}
```

### import
Go プログラムで読み込む必要のあるパッケージを宣言する。Go では未使用のパッケージはコンパイル時にエラーとなる。
スキップする場合は、パッケージ名の左に _ をつけることができる。

```go
import "fmt"
```

```go
import (
  "fmt"
  "time"
)
```

### プログラムの実行
`go run` もしくは　`go build` で実行ファイルをコンパイルしてから、実行ファイルを使って実行することができる。

#### go run
`go run hoge.go` とするだけで指定された名前の go プログラムから main パッケージをコンパイルして実行することができる。
go プログラムは .go 拡張子のファイルと定義されている。

```
go run [build flags] [-exec xprog] package [arguments...]
```

ただし、`go build`　とは異なり、既定でカレントディレクトリ配下のファイルを全て読み込むわけではない。
そのため main package が複数ファイル存在するときなどに、main関数のファイルだけを指定してもコンパイルエラーとなる。
その場合、明示的に複数ファイルを指定するか、* で複数ファイルを指定したり . (カレントディレクトリ) を指定する。

```
go run hoge.go fuga.go
```

あるいは、

```
go run *.go
```

あるいは、

```
go run .
```


#### go build
`go build` は指定されたパッケージについて依存関係も含めてコンパイルをする。なお、オプション無し `go build` を実行した場合は、
main パッケージを読み込み、main 関数があるファイルを特定し、そのファイル名を使ったバイナリ実行ファイルが生成される。

```
go build [-o output] [-i] [build flags] [packages]
```

### 参考

[Compile and run Go program: go run](https://golang.org/cmd/go/#hdr-Compile_and_run_Go_program)<br>
[Compile packages and dependencies](https://golang.org/cmd/go/#hdr-Compile_packages_and_dependencies)<br>
[Command go](https://godoc.org/github.com/gophersjp/go/src/cmd/go)<br>