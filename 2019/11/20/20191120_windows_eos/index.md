#  Windows 7、Windows Server 2008/R2 の延長セキュリティ更新プログラムを入手する方法


Windows Server 2008 および 2008 R2 の延長サポートは、2020 年 1 月 14 日に終了しますが、オンプレミス環境で追加の料金を支払うことで 3 年間の延長セキュリティ更新プログラム(ESU)を利用できるオプションが昨年公開されました。また、Windows Server 2008 および 2008 R2のワークロードを Azure に移行すると、この ESU を無償で利用できるということで話題になりました。

このオプションについての概要や全体的な Azure への移行などについては、[SQL Server 2008 および Windows Server 2008 のサポートを延長するオプションについて](https://blogs.technet.microsoft.com/mssvrpmj/2018/12/13/new-options-for-sql-server-2008-and-windows-server-2008-end-of-support/)などで説明がありましたが、オンプレミスや Azure への移行後にどのようにして ESU を入手するかについては長らく公式情報の公開待ちのステータスになっておりました。

ですが、先日公式に ESU の入手手順と条件についての公開情報に更新がありました。

対象の Windows デバイスの延長セキュリティ更新プログラムを入手する方法
https://blogs.windows.com/japan/2019/10/25/how-to-get-extended-security-updates-for-eligible-windows/#pWeZBtTMXeUqUKAp.97

原文（英語）；How to get Extended Security Updates for eligible Windows devices
https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/How-to-get-Extended-Security-Updates-for-eligible-Windows/ba-p/917807

前半はオンプレミスでESUを入手する手順についてですが、Azure あるいは Azure Stack　に関する記述は以下となります。

>Azure Virtual Machines (VM)、Windows Virtual Desktop の Windows 7 ESU、Azure 上で Windows 7/Windows Server 2008/Windows Server 2008 R2 の Bring your own image (イメージ持ち込み) を利用する場合には、追加の ESU キーを展開する必要はありません。オンプレミス デバイスと同様に、これらのデバイスにも KB4516033 および KB4519972 に記載されている SSU (またはそれ以降の SSU/ロールアップ) をインストールする必要があります。パッチ適用済みの Windows 7 イメージとパッチ適用済みの Windows Server 2008 R2 SP1 イメージは Azure Marketplace から入手できます。Azure Stack VM または Azure VMware Solution の場合、オンプレミス デバイスと同じ手順を踏む必要があります。

>You do not need to deploy an additional ESU key for Azure virtual machines (VMs), Windows 7 ESU with Windows Virtual Desktop, or for bring-your-own images on Azure for Windows 7, Windows Server 2008, and Windows Server 2008 R2. Like on-premises devices, these devices will also require the installation of the SSUs and monthly rollups outlined in the prerequisites section above. A pre-patched Windows 7 image and a pre-patched Windows Server 2008 R2 SP1 image are available from the Azure Marketplace. Azure Stack VMs or Azure VMware solutions should follow the same process as on-premises devices.

オンプレミスから Azure に移行したWindows Server 2008/Windows Server 2008 R2 については、KB4516033 および KB4519972 に記載されている SSU (またはそれ以降の SSU/ロールアップ) をインストールしておく必要があります。

Windows Server 2008 と 2008 R2　の延長セキュリティ更新プログラムについては、FAQも用意されています。

拡張セキュリティ更新プログラムのよくある質問 (FAQ)<br>
https://www.microsoft.com/ja-jp/cloud-platform/extended-security-updates

Extended Security Updates frequently asked questions<br>
https://www.microsoft.com/en-us/cloud-platform/extended-security-updates

ESUは、Windows Update や WSUSなどの通常のパッチ管理ソリューションを通じて提供されるので、上記の前提条件を満たすことで、Windows Update や WSUS を通じて無償で Azure VM へ適用することができるようです。

>How and when will Microsoft deliver Extended Security Updates?<br>
>Customers can deploy the new ESU Key and any pre-requisite servicing stack updates to the applicable machines, then continue with their current update/servicing strategy to deploy Extended Security Updates through Windows Update, Windows Server Update Services (WSUS), or whatever patch management solution the customer prefers. This is also the process that customers will need to follow for Azure Stack.

