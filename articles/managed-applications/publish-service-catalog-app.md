---
title: Azure サービス カタログ マネージド アプリケーションの作成と発行 | Microsoft Docs
description: 組織のメンバーを対象とする Azure マネージド アプリケーションを作成する方法について説明します。
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: 7e9db85fb91dd0c9a33cc8205bdb30a648dfd38a
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438747"
---
# <a name="create-and-publish-a-managed-application-definition"></a>マネージド アプリケーション定義を作成して発行する

組織のメンバーを対象とする Azure [マネージド アプリケーション](overview.md)を作成し、発行することができます。 たとえば、IT 部門が、組織の基準を満たすマネージ アプリケーションを発行できます。 これらのマネージド アプリケーションは、Azure Marketplace ではなく、サービス カタログを利用して入手できます。

サービス カタログ用のマネージド アプリケーションを発行するには、次の操作を行う必要があります。

* マネージド アプリケーションでデプロイするリソースを定義するテンプレートを作成する。
* マネージド アプリケーションをデプロイするときに、ポータルのユーザー インターフェイス要素を定義する。
* 必要なテンプレート ファイルを含む .zip パッケージを作成する。
* どのユーザー、グループ、またはアプリケーションがユーザーのサブスクリプションのリソース グループにアクセスする必要があるかを決める。
* .zip パッケージを指定して ID アクセスを要求するマネージド アプリケーション定義を作成する。

この記事では、マネージ アプリケーションには、ストレージ アカウントのみ存在します。 これは、マネージ アプリケーションを公開する手順を説明するためです。 詳細な例については、[Azure マネージド アプリケーションのサンプル プロジェクト](sample-projects.md)に関する記事を参照してください。

この記事の PowerShell の例では、Azure PowerShell 6.2 以降が必要です。 必要に応じて、[バージョンを更新](/powershell/azure/azurerm/install-azurerm-ps)してください。

## <a name="create-the-resource-template"></a>リソース テンプレートの作成

各マネージ アプリケーション定義には、**mainTemplate.json** というファイルが含まれています。 この中で、デプロイする Azure リソースを定義します。 テンプレートは、通常の Resource Manager テンプレートと違いはありません。

**mainTemplate.json** というファイルを作成します。 名前は大文字と小文字が区別されます。

次の JSON をファイルに追加します。 このファイルで、ストレージ アカウントを作成するパラメーターを定義し、ストレージ アカウントのプロパティを指定します。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountNamePrefix": {
            "type": "string"
        },
        "storageAccountType": {
            "type": "string"
        },
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "variables": {
        "storageAccountName": "[concat(parameters('storageAccountNamePrefix'), uniqueString(resourceGroup().id))]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            "apiVersion": "2016-01-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('storageAccountType')]"
            },
            "kind": "Storage",
            "properties": {}
        }
    ],
    "outputs": {
        "storageEndpoint": {
            "type": "string",
            "value": "[reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
        }
    }
}
```

mainTemplate.json ファイルを保存します。

## <a name="create-the-user-interface-definition"></a>ユーザー インターフェイス定義を作成する

Azure Portal で **createUiDefinition.json** ファイルを使用して、マネージド アプリケーションを作成するユーザー用のユーザー インターフェイスを生成します。 ユーザーが各パラメーターの入力をどのように提供するかを定義します。 各種のオプション (ドロップダウン リストなど) やテキスト ボックス、パスワード ボックスなどの入力手段を利用することができます。 マネージド アプリケーションの UI 定義ファイルの作成する方法については、「[CreateUiDefinition の基本概念](create-uidefinition-overview.md)」を参照してください。

**createUiDefinition.json** というファイルを作成します。 名前は大文字と小文字が区別されます。

次の JSON をファイルに追加します。

```json
{
    "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
    "handler": "Microsoft.Compute.MultiVm",
    "version": "0.1.2-preview",
    "parameters": {
        "basics": [
            {}
        ],
        "steps": [
            {
                "name": "storageConfig",
                "label": "Storage settings",
                "subLabel": {
                    "preValidation": "Configure the infrastructure settings",
                    "postValidation": "Done"
                },
                "bladeTitle": "Storage settings",
                "elements": [
                    {
                        "name": "storageAccounts",
                        "type": "Microsoft.Storage.MultiStorageAccountCombo",
                        "label": {
                            "prefix": "Storage account name prefix",
                            "type": "Storage account type"
                        },
                        "defaultValue": {
                            "type": "Standard_LRS"
                        },
                        "constraints": {
                            "allowedTypes": [
                                "Premium_LRS",
                                "Standard_LRS",
                                "Standard_GRS"
                            ]
                        }
                    }
                ]
            }
        ],
        "outputs": {
            "storageAccountNamePrefix": "[steps('storageConfig').storageAccounts.prefix]",
            "storageAccountType": "[steps('storageConfig').storageAccounts.type]",
            "location": "[location()]"
        }
    }
}
```

createUiDefinition.json ファイルを保存します。

## <a name="package-the-files"></a>ファイルのパッケージ化

app.zip という .zip ファイルに 2 つのファイルに追加します。 2 つのファイルは .zip ファイルのルートに配置する必要があります。 このファイルをフォルダーに配置すると、マネージ アプリケーション定義を作成するときに、必要なファイルが存在しないことを示すエラーが発生します。 

パッケージは、想定される実行場所からアクセスできる場所にアップロードしてください。 

```powershell
New-AzureRmResourceGroup -Name storageGroup -Location eastus
$storageAccount = New-AzureRmStorageAccount -ResourceGroupName storageGroup `
  -Name "mystorageaccount" `
  -Location eastus `
  -SkuName Standard_LRS `
  -Kind Storage

$ctx = $storageAccount.Context

New-AzureStorageContainer -Name appcontainer -Context $ctx -Permission blob

Set-AzureStorageBlobContent -File "D:\myapplications\app.zip" `
  -Container appcontainer `
  -Blob "app.zip" `
  -Context $ctx 
```

## <a name="create-the-managed-application-definition"></a>マネージド アプリケーション定義の作成

### <a name="create-an-azure-active-directory-user-group-or-application"></a>Azure Active Directory ユーザー グループまたはアプリケーションの作成

次に、顧客に代わってリソースを管理するためのユーザー グループまたはアプリケーションを選択します。 このユーザー グループまたはアプリケーションには、割り当てられているロールに従って、マネージド リソース グループに対するアクセス許可が付与されています。 そのロールには、ロールベースのアクセス制御 (RBAC) の組み込みロール (所有者、共同作成者など) をどれでも使用できます。 リソースを管理するためのアクセス許可は、個々のユーザーに付与することもできますが、ユーザー グループに割り当てるのが一般的です。 新しい Active Directory ユーザー グループの作成する場合は、「[Azure Active Directory でグループを作成し、メンバーを追加する](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)」を参照してください。

リソースの管理に使用するユーザー グループのオブジェクト ID が必要です。 

```powershell
$groupID=(Get-AzureRmADGroup -DisplayName mygroup).Id
```

### <a name="get-the-role-definition-id"></a>ロール定義 ID の取得

次に、ユーザー、ユーザー グループ、アプリケーションへのアクセス権を付与する RBAC の組み込みのロールのロール定義 ID が必要となります。 通常、所有者、共同作成者、閲覧者のいずれかのロールを使います。 次のコマンドは、所有者ロールのロール定義 ID を取得する方法を示しています。

```powershell
$ownerID=(Get-AzureRmRoleDefinition -Name Owner).Id
```

### <a name="create-the-managed-application-definition"></a>マネージド アプリケーション定義の作成

マネージ アプリケーション定義を保存するリソース グループがまだない場合は、ここで作成します。

```powershell
New-AzureRmResourceGroup -Name appDefinitionGroup -Location westcentralus
```

次に、マネージド アプリケーション定義のリソースを作成します。

```powershell
$blob = Get-AzureStorageBlob -Container appcontainer -Blob app.zip -Context $ctx

New-AzureRmManagedApplicationDefinition `
  -Name "ManagedStorage" `
  -Location "westcentralus" `
  -ResourceGroupName appDefinitionGroup `
  -LockLevel ReadOnly `
  -DisplayName "Managed Storage Account" `
  -Description "Managed Azure Storage Account" `
  -Authorization "${groupID}:$ownerID" `
  -PackageFileUri $blob.ICloudBlob.StorageUri.PrimaryUri.AbsoluteUri
```

### <a name="make-sure-users-can-see-your-definition"></a>ユーザーが定義を確認できるようにする

自身がアクセスできるマネージ アプリケーション定義に、組織の他のユーザーもアクセスできることを確認する必要があります。 定義に対して閲覧者以上のロールをユーザーに付与してください。 ユーザーはこのレベルのアクセスをサブスクリプションまたはリソース グループから継承している場合があります。 定義にアクセスできるユーザーを確認し、ユーザーやグループを追加する方法については、「[ロールベースのアクセス制御を使用して Azure サブスクリプション リソースへのアクセスを管理する](../role-based-access-control/role-assignments-portal.md)」を参照してください。

## <a name="next-steps"></a>次の手順

* マネージド アプリケーションを Microsoft Azure Marketplace に発行するには、「[Marketplace の Azure マネージド アプリケーション](publish-marketplace-app.md)」を参照してください。
* マネージ アプリケーション インスタンスをデプロイするには、[Azure portal を通したサービス カタログ アプリのデプロイ](deploy-service-catalog-quickstart.md)に関するページを参照してください。