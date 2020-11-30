# Event Viewer の履歴を一括削除する方法

Event Viewer で Event log を開いて確認していると履歴が累積していく。   
そんなとき Event Viewer  からだと一つづつしか履歴が消せずにストレスがたまる。   
そんなときには以下の方法で履歴が削除できる。

```
del /s /q %programdata%\microsoft\eventv~1\extern~1
```

あるいは以下のフォルダのxmlをまとめて削除するのでもOK。ただし管理者権限が必要

```
C:\ProgramData\Microsoft\Event Viewer\ExternalLogs
```