---
title: Azure で Windows VM イメージを選択する | Microsoft Docs
description: Azure PowerShell を使用して、Marketplace VM イメージの発行元、プラン、SKU、バージョンを判別します。
services: virtual-machines-windows
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/08/2018
ms.author: danlep
ms.openlocfilehash: ff4ccdf28be9d28798fff0e9f66bbb2c860166b7
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54424542"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Azure PowerShell を使用して Azure Marketplace で Windows VM イメージを検索する

この記事では、Azure PowerShell を使用して Azure Marketplace 内で VM イメージを検索する方法について説明します。 次に、PowerShell、Resource Manager テンプレート、またはその他のツールを使用して、VM をプログラムによって作成する際、Marketplace イメージを指定できます。

また、[Azure Marketplace](https://azuremarketplace.microsoft.com/) のストアフロント、[Azure portal](https://portal.azure.com)、または [Azure CLI](../linux/cli-ps-findimage.md) を使用して、使用できるイメージとオファーを参照することもできます。 

最新の [Azure PowerShell モジュール](/powershell/azure/azurerm/install-azurerm-ps)がインストールおよび構成されていることを確認してください。

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>一般的に使用される Windows イメージの表
| 発行元 | プラン | SKU |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-with-Containers |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008-R2-SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2017-WS2016 |Enterprise |
| MicrosoftRServer |RServer-WS2016 |Enterprise |

## <a name="navigate-the-images"></a>イメージの移動

特定の場所にあるイメージを検索するための 1 つの方法として、シーケンス内で [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher)、[Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer)、および [Get-AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) コマンドレットを順に実行します。

1. イメージの発行元を一覧表示する。
2. 指定された発行元について、そのプランを一覧表示する。
3. 指定されたプランについて、その SKU を一覧表示する。

その後、選択された SKU について [Get-AzureRMVMImage](/powershell/module/azurerm.compute/get-azurermvmimage) を実行して、デプロイするバージョンを一覧表示します。

1. 発行元を一覧表示します。

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
    ```

2. 選択した発行元の名前を入力して、プランを一覧表示します。

    ```powershell
    $pubName="<publisher>"
    Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
    ```

3. 選択したプランの名前を入力して、SKU を一覧表示します。

    ```powershell
    $offerName="<offer>"
    Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
    ```
    
4. 選択した SKU の名前を入力して、イメージのバージョンを取得します。

    ```powershell
    $skuName="<SKU>"
    Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
`Get-AzureRMVMImage` コマンドの出力から、バージョン イメージを選択して新しい仮想マシンをデプロイできます。

次の例は、コマンドとその出力の完全な順序を示しています。

```powershell
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

出力の一部を次に示します。

```
PublisherName
-------------
...
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

*MicrosoftWindowsServer* が発行元の場合:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

出力:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

*WindowsServer* プランの場合:

```powershell
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

出力:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
```

次に、*2016-Datacenter* SKU の場合:

```powershell
$skuName="2016-Datacenter"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Sku $skuName | Select Version
```

これで、選択した発行元、プラン、SKU、およびバージョンを URN (: で区切られた値) へと結合することができます。 [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) コマンドレットで VM を作成するときに、この URN を `--image` パラメーターを使用して渡します。 必要に応じて、URN のバージョン番号を "latest" に置き換えて、最新バージョンのイメージを取得できます。

Resource Manager テンプレートを使って VM をデプロイする場合は、`imageReference` プロパティでイメージ パラメーターを個別に設定します。 [テンプレート リファレンス](/azure/templates/microsoft.compute/virtualmachines)をご覧ください。

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>プランのプロパティの表示

イメージの購入プラン情報を表示するには、`Get-AzureRMVMImage` コマンドレットを実行します。 出力内の `PurchasePlan` プロパティが `null` ではない場合、イメージには、プログラムによるデプロイの前に同意しなければならない使用条件があります。  

たとえば、*Windows Server 2016 Datacenter* イメージに追加条項がないのは、`PurchasePlan` 情報が `null` であるためです。

```powershell
$version = "2016.127.20170406"
Get-AzureRMVMImage -Location $locName -Publisher $pubName -Offer $offerName -Skus $skuName -Version $version
```

出力:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/
                   Versions/2016.127.20170406
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2016-Datacenter
Version          : 2016.127.20170406
FilterExpression :
Name             : 2016.127.20170406
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

下の例は、*Data Science Virtual Machine - Windows 2016* イメージの同様のコマンドを示しています。次の `PurchasePlan` プロパティが表示されます: `name`、`product`、および `publisher`。 イメージによっては、`promotion code` プロパティもあります。 このイメージをデプロイするには、次のセクションを参照して使用条件に同意し、プログラムによるデプロイを有効にします。

```powershell
Get-AzureRMVMImage -Location "westus" -Publisher "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

出力:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMIma
                   ge/Offers/windows-data-science-vm/Skus/windows2016/Versions/0.2.02
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 0.2.02
FilterExpression :
Name             : 0.2.02
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>使用条件への同意

ライセンス条項を表示するには、[Get-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/get-azurermmarketplaceterms) コマンドレットを使用し、購入プラン パラメーターを渡します。 出力には、Marketplace イメージの使用条件へのリンクと、以前その使用条件に同意したかどうかが表示されます。 パラメーターの値は全小文字で入力してください。

```powershell
Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

出力:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 2/23/2018 7:43:00 PM
```

[Set-AzureRmMarketplaceterms](/powershell/module/azurerm.marketplaceordering/set-azurermmarketplaceterms) コマンドレットを使用して、使用条件に同意するか、または拒否します。 使用条件に同意する必要があるのは、イメージのサブスクリプションごとに 1 回だけです。 パラメーターの値は全小文字で入力してください。 

```powershell
$agreementTerms=Get-AzureRmMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzureRmMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

出力:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```

### <a name="deploy-using-purchase-plan-parameters"></a>購入プラン パラメーターを使用したデプロイ

イメージの使用条件に同意したら、そのサブスクリプション内で VM をデプロイできます。 次のスニペットに示すように、[Set-AzureRmVMPlan](/powershell/module/azurerm.compute/set-azurermvmplan) コマンドレットを使用して、VM オブジェクトの Marketplace プラン情報を設定します。 VM のネットワーク設定を作成し、デプロイを完了するための完全なスクリプトについては、[PowerShell スクリプトの例](powershell-samples.md)をご覧ください。

```powershell
...

$vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzureRmVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzureRmVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "0.2.02"

$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
その後、VM 構成とネットワーク構成オブジェクトを `New-AzureRmVM` コマンドレットに渡します。

## <a name="next-steps"></a>次の手順
基本イメージ情報を使用し、`New-AzureRmVM` コマンドレットを使って仮想マシンをすばやく作成するには、「[PowerShell で Windows 仮想マシンを作成する](quick-create-powershell.md)」をご覧ください。

[完全に構成された仮想マシンを作成する](../scripts/virtual-machines-windows-powershell-sample-create-vm.md)には、PowerShell スクリプトの例をご覧ください。


