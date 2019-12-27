# Visual Studio Code Meetup #1 の参加メモ


別記事でも書いたけど、2019/12/18 に [Visual Studio Code Meetup #1](https://vscode.connpass.com/event/155068/) に参加しました。  
日々の vs code 生活に役立ちそうな情報があったので備忘録として記録しておきます。  
WorkSpace に追加したフォルダを削除するコマンドほしい...

## 基本機能
基本機能では、次の 2 つが気になりました。

- Task
- Snippets

### Task

task を登録して自動実行できる機能。build などの定型作業を登録しておくことで、容易に task を実行できる。
早速 hugo のコンテンツ生成や git push とかの作業を登録してみたが快適。

Integrate with External Tools via Tasks  
https://code.visualstudio.com/docs/editor/tasks

### Snippets

エディタの補完をユーザが定義できる機能。メール文面とかの snippets 登録しておくと便利な気がする。

Snippets in Visual Studio Code  
https://code.visualstudio.com/docs/editor/userdefinedsnippets

## 拡張機能

良さげな拡張機能がいくつかあったので早速インストールしてみた。

### GitLens

git リポジトリの管理を超絶分かりやすくしてくれる  
https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens

### Git History

git のリポジトリ履歴を見やすく表示してくれる  
https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory

### GitHub Pull Requests

github 公式の Pull Requests 拡張機能。  
https://marketplace.visualstudio.com/items?itemName=GitHub.vscode-pull-request-github

### Settings Sync

vs code の設定ファイルを gist 経由で同期する。  
https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync

### Rainbow CSV  

csvファイルをみやすく表示してくれる。  
https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv

## コマンドオプション

ターミナルから code コマンドを使って, vscode を操作することができるが、結構便利そうなオプションがあった。

### -g / --goto file:line[:character]

ファイル、行数、何文字目かを指定してファイルを開く。

例：指定したファイルの9行目、40文字を active にしてファイルを開く

```sh
$ code -g logstats.log:9:40
```

### -d / --diff file file

ファイルの diff を表示する

例：

```sh
$ code -d logstats.log logstats2.log
```

### -a / --add　folder

指定したファルダをアクティブウィンドウのワークスペースに追加

例：

```sh
code -a .
```

### -n / --new-window

新規ウィンドウで開く。*.code-workspace を指定した場合は workspcae が開く。

```sh
code logstats.log -n
```

### -r / --reuse-window

現在のウィンドウを再利用してファイルを開く。*.code-workspace を指定した場合は現在のウィンドウで指定した workspcae が開く。

```sh
code logstats.log -r
```

### -w / --wait

指定したファイルが閉じられたタイミングで code コマンドの応答が返る。

```sh
code logstats.log -w
```

### -h / --help

ヘルプの表示

```sh
$ code -h
Visual Studio Code 1.41.0

Usage: code [options][paths...]

To read from stdin, append '-' (e.g. 'ps aux | grep code | code -')

Options
  -d --diff <file> <file>           Compare two files with each other.
  -a --add <folder>                 Add folder(s) to the last active window.
  -g --goto <file:line[:character]> Open a file at the path on the specified line and character position.
  -n --new-window                   Force to open a new window.
  -r --reuse-window                 Force to open a file or folder in an already opened window.
  -w --wait                         Wait for the files to be closed before returning.
  --locale <locale>                 The locale to use (e.g. en-US or zh-TW).
  --user-data-dir <dir>             Specifies the directory that user data is kept in. Can be used to open multiple distinct instances of Code.
  -v --version                      Print version.
  -h --help                         Print usage.
  --telemetry                       Shows all telemetry events which VS code collects.
  --folder-uri <uri>                Opens a window with given folder uri(s)
  --file-uri <uri>                  Opens a window with given file uri(s)

Extensions Management
  --extensions-dir <dir>                            Set the root path for extensions.
  --list-extensions                                 List the installed extensions.
  --show-versions                                   Show versions of installed extensions, when using --list-extension.
  --category                                        Filters installed extensions by provided category, when using --list-extension.
  --install-extension <extension-id | path-to-vsix> Installs or updates the extension. Use `--force` argument to avoid prompts.
  --uninstall-extension <extension-id>              Uninstalls an extension.
  --enable-proposed-api <extension-id>              Enables proposed API features for extensions. Can receive one or more extension IDs to enable individually.

Troubleshooting
  --verbose                          Print verbose output (implies --wait).
  --log <level>                      Log level to use. Default is 'info'. Allowed values are 'critical', 'error', 'warn', 'info', 'debug', 'trace', 'off'.
  -s --status                        Print process usage and diagnostics information.
  --prof-startup                     Run CPU profiler during startup
  --disable-extensions               Disable all installed extensions.
  --disable-extension <extension-id> Disable an extension.
  --inspect-extensions <port>        Allow debugging and profiling of extensions. Check the developer tools for the connection URI.
  --inspect-brk-extensions <port>    Allow debugging and profiling of extensions with the extension host being paused after start. Check the developer tools for the connection URI.
  --disable-gpu                      Disable GPU hardware acceleration.
  --max-memory                       Max memory size for a window (in Mbytes).

```