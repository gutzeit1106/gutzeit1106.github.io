# Visual Studio で build の実行ファイルをプロジェクトと別名にする


Visual Studio で build したり、publish(発行) するとデフォルトではプロジェクト名と同じファイル名の実行ファイルが生成される。例えば、ConsoleApp というプロジェクトだと ConsoleApp.exe となる。   
これを任意の名称の実行ファイルに変えたいなと調べたところ、プロジェクトのプロパティからアプリケーション > アセンブリ名を変更することで可能だった。   
例えば、 ConsoleApp というプロジェクトだけども App.exe で生成したい場合は、次のように指定すれば良い。

![build.png](/img/2021/Mar/build.png)
