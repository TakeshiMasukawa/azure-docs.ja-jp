---
title: Azure PowerShell から IoT Central を管理する | Microsoft Docs
description: Azure PowerShell から IoT Central を管理します。
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 01/14/2019
ms.topic: conceptual
manager: philmea
ms.openlocfilehash: 04726249809656344c8f81164d5deed5af321e71
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355232"
---
# <a name="manage-iot-central-from-azure-powershell"></a>Azure PowerShell から IoT Central を管理する

IoT Central の [[アプリケーション マネージャー]](https://aka.ms/iotcentral) ページから IoT Central アプリケーションを作成して管理する代わりに、[Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) を使用してアプリケーションを管理できます。

## <a name="prerequisites"></a>前提条件

Azure サブスクリプションをお持ちでない場合は、開始する前に [無料アカウント](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) を作成してください。

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

ローカル コンピューターで Azure PowerShell を実行する場合は、「[Install the Azure PowerShell module (Azure PowerShell モジュールのインストール)](https://docs.microsoft.com/powershell/azure/install-az-ps)」を参照してください。 Azure PowerShell をローカルで実行する場合は、この記事のコマンドレットを試す前に、**Connect-AzAccount** コマンドレットを使用して Azure にサインインします。

## <a name="install-the-iot-central-module"></a>IoT Central モジュールのインストール

次のコマンドを実行して、[IoT Central モジュール](https://docs.microsoft.com/powershell/module/az.iotcentral/)がお使いの PowerShell 環境にインストールされていることを確認します。

```PowerShell
Get-InstalledModule -name Az.I*
```

インストール済みモジュールのリストに **Az.IotCentral** が含まれていない場合は、次のコマンドを実行します。

```PowerShell
Install-Module Az.IotCentral
```

## <a name="create-an-application"></a>アプリケーションの作成

[New-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/New-AzIotCentralApp) コマンドレットを使用して、Azure サブスクリプションで IoT Central アプリケーションを作成します。 例: 

```PowerShell
# Create a resource group for the IoT Central application
New-AzResourceGroup -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Location "East US"
```

```PowerShell
# Create an IoT Central application
New-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
  -Name "myiotcentralapp" -Subdomain "mysubdomain" `
  -Sku "S1" -Template "iotc-demo@1.0.0" `
  -DisplayName "My Custom Display Name"
```

このスクリプトでは、まずアプリケーション用に米国東部リージョンにリソース グループを作成します。 次の表は、**New-AzIotCentralApp** コマンドで使用されるパラメーターの説明です。

|パラメーター         |説明 |
|------------------|------------|
|ResourceGroupName |そのアプリケーションを含むリソース グループ。 サブスクリプションにこのリソース グループが既に存在している必要があります。 |
|場所 |既定で、このコマンドレットにはリソース グループの場所が使用されます。 現在、IoT Central アプリケーションは**米国東部**、**米国西部**、**北ヨーロッパ**、または**西ヨーロッパ**のリージョンで作成できます。 |
|Name              |Azure portal 内のアプリケーションの名前。 |
|サブドメイン         |アプリケーションの URL のサブドメイン。 この例では、アプリケーションの URL は https://mysubdomain.azureiotcentral.com です。 |
|SKU               |現在使用できる値は **S1** (Standard レベル) のみです。 「[Azure IoT Central の価格](https://azure.microsoft.com/pricing/details/iot-central/)」を参照してください。 |
|テンプレート          | 使用するアプリケーション テンプレート。 詳細については、後の表を参照してください。 |
|DisplayName       |UI に表示されるアプリケーションの名前。 |

**アプリケーション テンプレート**

|テンプレート名  |説明 |
|---------------|------------|
|iotc-default@1.0.0 |独自のデバイス テンプレートおよびデバイスにデータを入力するための空のアプリケーションを作成します。 |
|iotc-demo@1.0.0    |Refrigerated Vending Machine 用に既に作成したデバイス テンプレートを含むアプリケーションを作成します。 このテンプレートは、Azure IoT Central の調査を開始するために使用します。 |
|iotc-devkit-sample@1.0.0 |MXChip または Raspberry Pi デバイスを接続するための準備ができたデバイス テンプレートを含むアプリケーションを作成します。 このテンプレートは、これらのいずれかのデバイスで実験しているデバイス開発者が使用します。 |

## <a name="view-your-iot-central-applications"></a>IoT Central アプリケーションの表示

[Get-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Get-AzIotCentralApp) コマンドレットを使用して、IoT Central アプリケーションを一覧表示し、メタデータを表示します。

## <a name="modify-an-application"></a>アプリケーションの変更

[Set-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/set-aziotcentralapp) コマンドレットを使用して、IoT Central アプリケーションのメタデータを更新します。 アプリケーションの表示名を変更する場合の例を次に示します。

```PowerShell
Set-AzIotCentralApp -Name "myiotcentralapp" `
  -ResourceGroupName "MyIoTCentralResourceGroup" `
  -DisplayName "My new display name"
```

## <a name="remove-an-application"></a>アプリケーションの削除

[Remove-AzIotCentralApp](https://docs.microsoft.com/powershell/module/az.iotcentral/Remove-AzIotCentralApp) コマンドレットを使用して、IoT Central アプリケーションを削除します。 例: 

```PowerShell
Remove-AzIotCentralApp -ResourceGroupName "MyIoTCentralResourceGroup" `
 -Name "myiotcentralapp"
```

## <a name="next-steps"></a>次の手順

ここでは、Azure PowerShell から Azure IoT Central アプリケーションを管理する方法について説明しました。推奨される次の手順は以下のとおりです。

> [!div class="nextstepaction"]
> [アプリケーションを管理する](howto-administer.md)
