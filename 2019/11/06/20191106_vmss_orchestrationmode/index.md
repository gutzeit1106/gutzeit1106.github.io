# VM ScalesetのOrchestration Modeについて


Microsoft Ignite 2019で触れられていた、VM Scaleset の[Orchestration mode](https://docs.microsoft.com/en-us/azure/virtual-machine-scale-sets/orchestration-modes)というプレビュー機能について調べてみました。
要約すると、VM Scalesetに通常の仮想マシンを追加できる機能でした。

・2種類のOrchestration(ScaleSetVM or VM)を選択できる（VMSS作成時のみ）

・ScaleSetVMモードは以前のVMSS。VMSSの雛形にそったVMインスタンスをスケールさせるので、基本的に同一構成のVMインスタンスを管理する

・VMモードは通常のVMをVM Scalesetに所属させることができる。そのため異なる仮想マシンサイズや構成（NICやディスク数なども含む）を混在させて管理することができる
　
なお、VMモードで、通常の仮想マシンをスケールセットに追加する場合は、仮想マシンを新規作成する時のみ可能のようです。
VMモードの追加できるのは通常の仮想マシンになるので、Azure Backup、Azure Site Recoveryなどの個別の仮想マシンにて適用可能な機能は、そのまま利用することができるようです。
最終的には、VMモードでオートスケールなども対応するのかなと思いますが、プレビューの現段階だとサポートされないようです。今の段階だと通常の仮想マシンを可用性セットで構築するのとの差があんまり感じられないので、おそらく今後は仮想マシンスケールセット特有の機能がVMモードでも使えるようになっていくのかなと予想されます。

Azure Portalからだと、VMモードでScalesetを作成することはできましたが、VMをScalesetに参加させることはまだできないようでした。
調べたところAzure CLIは、2019/11/04のリリース　[Version 2.0.76](https://github.com/MicrosoftDocs/azure-docs-cli/blob/master/docs-ref-conceptual/release-notes-azure-cli.md#november-4-2019)から対応しているようで、現在の動作を確認するにはCLIを使用するのが勝手がよさそうです。  
こんな感じで作成することができます。


```sh
# 1.orchestration-mode:VMのVMSSを作成
az vmss create -g Scaleset -n orchestrationModeVMSS --orchestration-mode VM

# 2. orchestration-mode:VMに仮想マシンを追加※仮想マシン新規作成時のみ可能
az vm create `
 -g Scaleset -n orchestrationModeVM001 `
 --vmss /subscriptions/xxxx-xxxx-xxxx-xxxx-xxxx/resourceGroups/Scaleset/providers/Microsoft.Compute/virtualMachineScaleSets/ orchestrationModeVMSS `
 --image ubuntuLTS
```

作成したVMの定義（json）を見ると、properties.virtualMachineScaleSetが追加されているのがわかります。

```json
   "virtualMachineScaleSet": {
          "id": "/subscriptions/527xxxx-xxxx-xxxx-xxxx-xxxx/resourceGroups/Scaleset/providers/Microsoft.Compute/virtualMachineScaleSets/orchestrationModeVMSS"
        },
```

REST-APIはversion 2019-03-01 から対応しています。

[Virtual Machines - Create Or Update](https://docs.microsoft.com/ja-jp/rest/api/compute/virtualmachines/createorupdate#request-body)

VMSSは様々な Azure サービスのベースになっているので、Orchestration modeを利用するサービスも出てくるのかもしれませんね。
