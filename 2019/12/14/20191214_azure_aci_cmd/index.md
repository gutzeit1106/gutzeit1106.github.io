# Azure Container Instances の起動・停止・再起動 by Azure Powershell


### はじめに
Azure Container Instances を Azure Powershell で操作しようとコマンド一覧を確認していたが、次の通り Get（参照）, New（作成）, Remove（削除）などしか用意されていない。

```powershell
PS Azure:\> gcm *Az*ContainerGroup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Get-AzureRmContainerGroup
Alias           New-AzureRmContainerGroup
Alias           Remove-AzureRmContainerGroup
Cmdlet          Get-AzContainerGroup                               1.0.1      Az.ContainerInstance
Cmdlet          New-AzContainerGroup                               1.0.1      Az.ContainerInstance
Cmdlet          Remove-AzContainerGroup                            1.0.1      Az.ContainerInstance

Azure:/
PS Azure:\> gcm *Az*ContainerInstance*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Get-AzureRmContainerInstanceLog
Cmdlet          Get-AzContainerInstanceLog                         1.0.1      Az.ContainerInstance
```

[REST API](https://docs.microsoft.com/ja-jp/rest/api/container-instances/) では、start、stop、restart も用意されているけど、Azure Powershell のコマンドとしては提供されていないみたい。

どうしようか工夫した結果、[Invoke-AzResourceAction](https://docs.microsoft.com/en-us/powershell/module/az.resources/invoke-azresourceaction?view=azps-3.1.0) で Azure Powershell コマンドからの操作が可能だった。

### コマンド例
Container Instance の起動

```powershell
Invoke-AzResourceAction -ResourceGroupName <YourResourceGroup> -ResourceName <YourACI> -Action Start -ResourceType Microsoft.ContainerInstance/containerGroups -Force
```

Container Instance の停止

```powershell
Invoke-AzResourceAction -ResourceGroupName <YourResourceGroup> -ResourceName <YourACI> -Action Stop -ResourceType Microsoft.ContainerInstance/containerGroups -Force
```

Container Instance の再起動

```powershell
Invoke-AzResourceAction -ResourceGroupName <YourResourceGroup> -ResourceName <YourACI> -Action Restart -ResourceType Microsoft.ContainerInstance/containerGroups -Force
```

### おわり
Invoke-AzResourceAction は便利。これで Automation 経由で ACI を Restart できる。