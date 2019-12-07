# ターミナルから Visual Studio Code を呼び出す方法


Mac や SSH で接続した Linux サーバ上でターミナルから操作している時に、既存ファイルをエディタで編集したり、ファイル名を指定して新規にファイルを作成、内容の編集をしたいみたいなことは日常的にあると思います。  
そんな時、今までは vi や emacs を使いがちだったのですが、Visual Studio Code で開いて編集する方法をメモしておきます。
もちろん Visual Studio  Code のワークスペースにフォルダ追加していけば同じことはできますが、ワークスペースがごちゃごちゃするし、定期的に編集するコードとかじゃなく滅多に編集しない os やミドルウェアの設定ファイルとかだと、どこにファイルあったっけなぁとターミナルで探しながら作業することが多いので、そんな時ターミナルからコマンドで開いて編集できると便利です。

1. Visual Studio Code を開いている状態で Command Palette (command + shift + p) を開きます
2. Shell Command : Install 'code' command in PATH を選択します

![20191205_vscode_terminal](/img/20191205_vscode_terminal/ShellCommand.png)

  
上の手順で、ターミナルで code コマンドから VS code を起動させることが可能です。  
特に使っていて良い点は、Visual Studio Code の Remote Deployment でリモートサーバに SSH 接続している際に、ファイルを編集する時です。
Remote Deployment でも　code コマンドを有効化しておけば、Desktop を有効化していない Linux でも  VS Code のターミナルから code <FileName> で VS Code でファイル編集ができます。