---
title: Azure CLI のサンプル スクリプト - 負荷分散された仮想マシン スケール セットでの LAMP スタックのデプロイ | Microsoft Docs
description: カスタム スクリプト拡張機能を使用して、LAMP スタックを Azure の負荷分散された仮想マシン スケール セットにデプロイします。
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: cb8815db5e16603ca4578f6d3d943cbef62e49af
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54461147"
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a>負荷分散された仮想マシン スケール セットでの LAMP スタックのデプロイ

この例では、仮想マシン スケール セットを作成し、カスタム スクリプトを実行する拡張機能を適用して、各仮想マシンのスケール セットに LAMP スタックをデプロイします。

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>サンプル スクリプト

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a>接続

このコードを使用して、VM とスケール セットに接続する方法を表示します。

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ 

次のコマンドを実行して、リソース グループ、スケール セット、VM、およびすべての関連リソースを削除します。

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用して、リソース グループ、仮想マシン、可用性セット、ロード バランサー、およびすべての関連リソースを作成します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#az_group_create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az vmss create](https://docs.microsoft.com/cli/azure/vmss#az_vmss_create) | 仮想マシンスケール セットを作成します。 |
| [az network lb rule create](https://docs.microsoft.com/cli/azure/network/lb/rule#az_network_lb_rule_create) | 負荷分散エンドポイントを追加します。 |
| [az vmss extension set](https://docs.microsoft.com/cli/azure/vmss/extension#az_vmss_extension_set) | 仮想マシンのデプロイ時にカスタム スクリプトを実行する拡張機能を作成します。 |
| [az vmss update-instances](https://docs.microsoft.com/cli/azure/vmss#az_vmss_update_instances) | 拡張機能をスケール セットに適用する前にデプロイされた VM インスタンスで、カスタム スクリプトを実行します。 |
| [az vmss scale](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) | VM インスタンスを追加してスケール セットをスケール アップします。 デプロイ時に、カスタム スクリプトでこれらが実行されます。 |
| [az network public-ip list](https://docs.microsoft.com/cli/azure/network/public-ip) | サンプルで作成された VM の IP アドレスを取得します。 |
| [az network lb show](https://docs.microsoft.com/cli/azure/network/lb#az_network_lb_show) | ロード バランサーで使用されるフロントエンド ポートおよびバックエンド ポートを取得します。 |

## <a name="next-steps"></a>次の手順

Azure CLI の詳細については、[Azure CLI のドキュメント](https://docs.microsoft.com/cli/azure)のページをご覧ください。

その他の仮想マシン用の CLI サンプル スクリプトは、[Azure Linux VM のドキュメント](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)のページにあります。
