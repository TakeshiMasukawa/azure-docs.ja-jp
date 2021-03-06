---
title: Azure SQL Database の仮想コアベースのリソース制限 - 単一データベース | Microsoft Docs
description: このページでは、Azure SQL Database の単一データベースに対するいくつかの一般的な仮想コアベースのリソース制限について説明します。
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 01/09/2019
ms.openlocfilehash: 894922a80ab874e5304ef441571e03ef559a34b0
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54215424"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Azure SQL Database の単一データベースに対する仮想コアベースの購入モデルの制限

この記事では、仮想コアベースの購入モデルを使用した、Azure SQL Database の単一データベースに対する詳細なリソース制限について説明します。

論理サーバー上の 1 つのデータベースに対する DTU ベースの購入モデルの制限については、[論理サーバー上のリソース制限の概要](sql-database-resource-limits-logical-server.md)に関する記事をご覧ください。

> [!IMPORTANT]
> 場合によっては、未使用領域を再利用できるようにデータベースを縮小する必要があります。 詳細については、「[Manage file space in Azure SQL Database](sql-database-file-space-management.md)」(Azure SQL Database でファイル領域を管理する) を参照してください。

[Azure portal](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases)、[Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases)、[PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases)、[Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases)、または [REST API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases) を使って、単一のデータベースにサービス レベル、コンピューティング サイズ、ストレージ量を設定できます。

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>General Purpose サービス レベル:ストレージ サイズとコンピューティング サイズ

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>General Purpose サービス レベル:第 4 世代コンピューティング プラットフォーム (パート 1)

|コンピューティング サイズ|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|H/W の世代|4|4|4|4|4|4|
|仮想コア|1|2|3|4|5|6|
|メモリ (GB)|7|14|21|28|35|42|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (GB)|1024|1024|1024|1536|1536|1536|
|最大ログ サイズ (GB)|307|307|307|461|461|461|
|TempDB のサイズ (GB)|32|64|96|128|160|192|
|ストレージの種類|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|500|1,000|1500|2000|2500|3000|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1,000|1200|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|1|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|000
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>General Purpose サービス レベル:第 4 世代コンピューティング プラットフォーム (パート 2)

|コンピューティング サイズ|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|H/W の世代|4|4|4|4|4|4|
|仮想コア|7|8|9|10|16|24|
|メモリ (GB)|49|56|63|70|112|168|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (GB)|1536|3072|3072|3072|4096|4096|
|最大ログ サイズ (GB)|461|922|922|922|1229|1229|
|TempDB のサイズ (GB)|224|256|288|320|384|384|
|ストレージの種類|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)
|ターゲットの IOPS (64 KB)|3500|4000|4500|5000|7000|7000|
|最大同時実行ワーカー (要求) 数|1400|1600|1800|2000|3200|4800|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|1|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>General Purpose サービス レベル:第 5 世代コンピューティング プラットフォーム (パート 1)

|コンピューティング サイズ|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W の世代|5|5|5|5|5|5|5|
|仮想コア|2|4|6|8|10|12|14|
|メモリ (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (GB)|1024|1024|1024|1536|1536|1536|1536|
|最大ログ サイズ (GB)|307|307|307|461|461|461|461|
|TempDB のサイズ (GB)|64|128|192|256|320|384|384|
|ストレージの種類|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|500|1,000|1500|2000|2500|3000|3500|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1,000|1200|1400|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|30000|
|レプリカの数|1|1|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>General Purpose サービス レベル:第 5 世代コンピューティング プラットフォーム (パート 2)

|コンピューティング サイズ|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|H/W の世代|5|5|5|5|5|5|5|
|仮想コア|16|18|20|24|32|40|80|
|メモリ (GB)|81.6|91.8|102|122.4|163.2|204|408|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (GB)|3072|3072|3072|4096|4096|4096|4096|
|最大ログ サイズ (GB)|922|922|922|1229|1229|1229|1229|
|TempDB のサイズ (GB)|384|384|384|384|384|384|384|
|ストレージの種類|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|Premium (リモート) ストレージ|
|IO 待機時間 (概算)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|5 ～ 7 ミリ秒 (書き込み)<br>5 ～ 10 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|4000|4500|5000|6000|7000|7000|7000|
|最大同時実行ワーカー (要求) 数|1600|1800|2000|2400|3200|4000|8000|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|30000|
|レプリカの数|1|1|1|1|1|1|1|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>Business Critical サービス レベル:ストレージ サイズとコンピューティング サイズ

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>Business Critical サービス レベル:第 4 世代コンピューティング プラットフォーム (パート 1)

|コンピューティング サイズ|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W の世代|4|4|4|4|4|4|
|仮想コア|1|2|3|4|5|6|
|メモリ (GB)|7|14|21|28|35|42|
|列ストアをサポート|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|インメモリ OLTP ストレージ (GB)|1|2|3|4|5|6|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|最大データ サイズ (GB)|1024|1024|1024|1024|1024|1024|
|最大ログ サイズ (GB)|307|307|307|307|307|307|
|TempDB のサイズ (GB)|32|64|96|128|160|192|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|5000|10000|15000|20000|25000|30000|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1,000|1200|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|[はい]|はい|はい|はい|はい|[はい]|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>Business Critical サービス レベル:第 4 世代コンピューティング プラットフォーム (パート 2)

|コンピューティング サイズ|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W の世代|4|4|4|4|4|4|
|仮想コア|7|8|9|10|16|24|
|メモリ (GB)|49|56|63|70|112|168|
|列ストアをサポート|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|インメモリ OLTP ストレージ (GB)|7|8|9.5|11|20|36|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|最大データ サイズ (GB)|1024|1024|1024|1024|1024|1024|
|最大ログ サイズ (GB)|307|307|307|307|307|307|
|TempDB のサイズ (GB)|224|256|288|320|384|384|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|35000|40000|45000|50000|80000|120000|
|最大同時実行ワーカー (要求) 数|1400|1600|1800|2000|3200|4800|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|[はい]|はい|はい|はい|はい|[はい]|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>Business Critical サービス レベル:第 5 世代コンピューティング プラットフォーム (パート 1)

|コンピューティング サイズ|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|H/W の世代|5|5|5|5|5|5|5|
|仮想コア|2|4|6|8|10|12|14|
|メモリ (GB)|11|22|33|44|55|66|77|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|1.571|3.142|4.713|6.284|8.655|11.026|13.397|
|最大データ サイズ (GB)|1024|1024|1024|1536|1536|1536|1536|
|最大ログ サイズ (GB)|307|307|307|461|461|461|461|
|TempDB のサイズ (GB)|64|128|192|256|320|384|384|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|5000|10000|15000|20000|25000|30000|35000|
|最大同時実行ワーカー (要求) 数|200|400|600|800|1,000|1200|1400|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|4|4|
|マルチ AZ|[はい]|はい|はい|はい|はい|はい|[はい]|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>Business Critical サービス レベル:第 5 世代コンピューティング プラットフォーム (パート 2)

|コンピューティング サイズ|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |
|H/W の世代|5|5|5|5|5|5|5|
|仮想コア|16|18|20|24|32|40|80|
|メモリ (GB)|88|99|110|132|176|220|440|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|15.768|18.139|20.51|25.252|37.936|52.22|131.64|
|最大データ サイズ (GB)|3072|3072|3072|4096|4096|4096|4096|
|最大ログ サイズ (GB)|922|922|922|1229|1229|1229|1229|
|TempDB のサイズ (GB)|384|384|384|384|384|384|384|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|IO 待機時間 (概算)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|1 ～ 2 ミリ秒 (書き込み)<br>1 ～ 2 ミリ秒 (読み取り)|
|ターゲットの IOPS (64 KB)|40000|45000|50000|60000|80000|100000|200000|
|最大同時実行ワーカー (要求) 数|1600|1800|2000|2400|3200|4000|8000|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|30000|
|レプリカの数|4|4|4|4|4|4|
|マルチ AZ|[はい]|はい|はい|はい|はい|[はい]|
|読み取りスケールアウト|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|含まれるバックアップ ストレージ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|1X DB サイズ|

## <a name="hyperscale-service-tier-preview"></a>ハイパースケール サービス レベル (プレビュー)

### <a name="generation-4-compute-platform-storage-sizes-and-compute-sizes"></a>第 4 世代コンピューティング プラットフォーム:ストレージ サイズとコンピューティング サイズ

|パフォーマンス レベル|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|H/W の世代|4|4|4|4|4|4|
|仮想コア|1|2|4|8|16|24|
|メモリ (GB)|7|14|28|56|112|168|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100 |
|最大ログ サイズ (TB)|1 |1 |1 |1 |1 |1 |
|TempDB のサイズ (GB)|32|64|128|256|384|384|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|ターゲットの IOPS (64 KB)|未定|未定|未定|未定|未定|未定|
|IO 待機時間 (概算)|未定|未定|未定|未定|未定|未定|
|最大同時実行ワーカー (要求) 数|200|400|800|1600|3200|4800|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|
|レプリカの数|2|2|2|2|2|2|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ|7|7|7|7|7|7|
|||

### <a name="generation-5-compute-platform"></a>第 5 世代コンピューティング プラットフォーム

|パフォーマンス レベル|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W の世代|5|5|5|5|5|5|5|5|
|仮想コア|2|4|8|16|24|32|40|80|
|メモリ (GB)|10.2|20.4|40.8|81.6|122.4|163.2|204|408|
|列ストアをサポート|はい|はい|はい|はい|はい|はい|はい|はい|
|インメモリ OLTP ストレージ (GB)|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|最大データ サイズ (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|最大ログ サイズ (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|TempDB のサイズ (GB)|64|128|256|384|384|384|384|384|
|ストレージの種類|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|ローカル SSD|
|ターゲットの IOPS (64 KB)|未定|未定|未定|未定|未定|未定|未定|未定|
|IO 待機時間 (概算)|未定|未定|未定|未定|未定|未定|未定|未定|
|最大同時実行ワーカー (要求) 数|200|400|800|1600|2400|3200|4000|8000|
|許可される最大セッション数|30000|30000|30000|30000|30000|30000|30000|30000|
|レプリカの数|2|2|2|2|2|2|2|2|
|マルチ AZ|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|該当なし|
|読み取りスケールアウト|はい|はい|はい|はい|はい|はい|はい|はい|
|含まれるバックアップ ストレージ (プレビューの制限)|7|7|7|7|7|7|7|7|
|||

## <a name="next-steps"></a>次の手順

- よく寄せられる質問の回答については、「[SQL Database に関する FAQ](sql-database-faq.md)」を参照してください。
- Azure の一般的な制限については、「[Azure サブスクリプションとサービスの制限、クォータ、制約](../azure-subscription-service-limits.md)」をご覧ください。
