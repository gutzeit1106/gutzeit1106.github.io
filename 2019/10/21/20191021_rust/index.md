# MacにRustをインストールしてHelloWorldするまで


Mac OSにRustをインストールして実行するまでのメモです。

### 手順
まずはターミナルで次のコマンドを実行する。（選択肢が出たら、1のdefaultで良いと思う）


```sh 
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```


ターミナルを再起動するか、以下のコマンドを実行してパスを通す。

```sh 
source ~/.bash_profile
```


インストール後はrustcのバージョンを確認する。

```sh 
rustc --version
rustc 1.38.0 (625451e37 2019-09-23)
```

インストールが確認できたら適当なパスにhello.rsという名称でファイルを作り、エディタで内容を次の通りにして保存する。

```rust:hello.rs
fn main() {
    println!("Hello World!");
}
```

rustcでコンパイルするとhelloという実行ファイルが生成される。

```sh 
$ rustc hello.rs
$ ls
hello           hello.rs
```

実行ファイルを実行するとHello Worldと表示されるのでこれで終わり。

```sh
$ ./hello 
Hello World!
```

### おまけ
公式のinstalationは[ここ](https://www.rust-lang.org/tools/install)