---
title: インクルード ファイル
description: インクルード ファイル
services: virtual-machines-windows, virtual-machines-linux
author: dlepow
ms.service: multiple
ms.topic: include
ms.date: 10/09/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 6c7a9857f6d6d57dc9e314bcb1ef848a326b7ed2
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "49437010"
---
## <a name="terminology"></a>用語集

Azure Marketplace イメージには、次の属性があります。

* **発行元**: イメージを作成した組織です。 例: Canonical、MicrosoftWindowsServer
* **プラン**: 発行元によって作成された関連するイメージのグループ名です。 例: Ubuntu Server、WindowsServer
* **SKU**: ディストリビューションのメジャー リリースなど、プランのインスタンス。 例: 16.04-LTS、2016-Datacenter
* **バージョン**: イメージの SKU のバージョン番号。 

プログラムで VM をデプロイした場合に Marketplace イメージを識別するには、これらの値をパラメーターとして個別に指定します。 一部のツールはイメージ *URN* を受け入れます。これは、*Publisher*:*Offer*:*Sku*:*Version* のように、これらの値を組み合わせてコロン (:) で区切ったものです。 URN では、バージョン番号を "latest" で置き換えることもできます。この場合、イメージの最新バージョンが選択されます。 

イメージの発行元が追加のライセンスや購入契約を提供する場合は、この契約条件に同意し、プログラムによるデプロイを有効にする必要があります。 プログラムで VM をデプロイする場合は、*購入プラン* パラメーターを指定する必要もあります。 [Marketplace の契約条件でイメージを展開する](#deploy-an-image-with-marketplace-terms)を参照してください。