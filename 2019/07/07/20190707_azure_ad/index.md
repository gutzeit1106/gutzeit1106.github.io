# Azure Active Directoryの要点整理(2019/07)


## Azure AD の価格プランによる機能比較
FREE, BASIC, PREMIUM P1, PREMIUM P2のプランがある。<br>
BASICは1ユーザあたり112円/月, PREMIUM P1は672円/月, PREMIUM P2は1008円/月

参考:[Azure Active Directory のプラン機能対応表](https://azure.microsoft.com/ja-jp/pricing/details/active-directory/)


※PREMIUM P1以上が必要な主な機能

 - MFA(Multi-Factor Authentication)の有効化 

 - Azure AD Connect Health

 - 条件付きアクセス 

 - Cloud App Discovery(組織で利用しているクラウドアプリケーションの利用状況の可視化) 


※PREMIUM P2以上が必要な主な機能

 - Identity Protection

 - Privileged Identity Management(PIM)

 - アクセスレビュー

## Azure AD Connect と Connect Health
Azure AD Connectは、オンプレミスとクラウドのハイブリット構成に用いるツールで、Azure AD Connect Healthは、オンプレミスのIDインフラについてログを収集し、統一環境下で監視や正常性を確認することができる機能。<br>

<img src="/img/20190707_azure_Ad/arch.png" align="left"><br clear="left">

Azure AD Connectを用いてオンプレ-クラウド間でのハイブリットを構成する際は適切な認証方式を選択していく。Azure ADがユーザのサインインプロセスを処理するクラウド認証と、別の信頼された認証システム（オンプレのADFSなど）にパスワード検証
プロセスを引継ぐ方式の2種類に大別できる。

### クラウド認証: パスワードハッシュ同期
オンプレミスのADとクラウドベースのAzure AD間でユーザパスワードのハッシュ値を同期する。そのため認証が実施される場所はクラウド上になる。またアカウントの状態は同期が完了するまですぐには適用されない。

### クラウド認証: パススルー認証
公開鍵で暗号化したユーザIDとパスワードをキューにいれ、オンプレミスのエージェントが暗号化情報を取得・復号し、認証情報の検証をおこなう認証形式。オンプレミスにあるパスワード情報がクラウドに保存されることはないことを謳っている。

### フェデレーション認証
Azure ADとフェデレーションを構成したオンプレの認証基盤に認証プロセスを引き継ぐ。オンプレの認証基盤を構築する投資が必要。


## Identity Protection
ユーザー ID に関連して検出された疑わしいアクションに対する自動応答を構成できる。匿名 IP アドレスからのサインインなどのリスクイベントを定義し、これに対応するイベントが発生した場合に多要素認証を要求するといったアクセスポリシーを設定することができる。

## Azure AD Privileged Identity Management(PIM)
組織内のリソース(AzureAD, Azure リソース、O365などの各種 Microsoft Online Service)へのアクセス管理や監視ができるサービス

主な機能

- Azure ADとAzureリソースへのJIT(Just-In-Time)特権アクセスの付与

- 開始日と終了日を使用した期限付きアクセス権のリソースへの付与

- 特権ロールをアクティブ化するために承認を要求する

- ロールをアクティブ化するために多要素認証を強制する

- ユーザがアクティブ化された際の理由を把握する

- 特権ロールがアクティブ化されたときに通知を受ける

- 継続してユーザーにロールが必要であることを確認するためにアクセスレビューを実施する

- 社内監査または外部監査に使用する監査履歴をダウンロードする

※Azure AD Premium P2のライセンスが必要<br>
※アカウント管理者、サービス管理者、共同管理者などの従来のサブスクリプション管理者ロールは扱えない<br>
※アクセスレビューでは、定期的に設定したレビューへレビューをトリガーし、レビューが拒否されたユーザのアクセスを自動的に削除することも可能<br>

参考:
[Azure AD Privileged Identity Management](https://docs.microsoft.com/ja-jp/azure/active-directory/privileged-identity-management/pim-configure)


