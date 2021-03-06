---
title: Azure Log Analytics クエリ内の文字列の操作 | Microsoft Docs
description: この記事では、Log Analytics で Analytics ポータルを使用してクエリを作成する方法のチュートリアルを提供します。
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 24f22d659ccfb6923ad2a038e12454716b2c5445
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263885"
---
# <a name="working-with-json-and-data-structures-in-log-analytics-queries"></a>Log Analytics クエリ内の JSON とデータ構造の操作

> [!NOTE]
> このレッスンを完了する前に、[Analytics ポータルの概要](get-started-portal.md)および[クエリの概要](get-started-queries.md)に関するチュートリアルを完了する必要があります。

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

入れ子になったオブジェクトは、配列やキーと値のペアのマップ内に他のオブジェクトを含むオブジェクトです。 これらのオブジェクトは JSON 文字列として表されます。 この記事では、データを取得し、入れ子になったオブジェクトを分析するために、JSON を使用する方法を説明します。

## <a name="working-with-json-strings"></a>JSON 文字列の使用
既知のパスの特定の JSON 要素にアクセスするには、`extractjson` を使用します。 この関数では、次の規則を使用するパス式が必要です。

- ルート フォルダーを参照するための _$_
- 次の例に示すように、インデックスおよび要素を参照するために、角かっこまたはドット表記を使用します。


インデックスには角かっこを使用し、要素を区切るにはドットを使用します:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report
| extend status = extractjson("$.hosts[0].status", hosts_report)
```

これは角かっこ表記のみを使用していますが、同じ結果になります:

```Kusto
let hosts_report='{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}';
print hosts_report 
| extend status = extractjson("$['hosts'][0]['status']", hosts_report)
```

要素が 1 つしかない場合は、ドット表記のみを使用できます:

```Kusto
let hosts_report='{"location":"North_DC", "status":"running", "rate":5}';
print hosts_report 
| extend status = hosts_report.status
```


## <a name="working-with-objects"></a>オブジェクトの操作

### <a name="parsejson"></a>parsejson
json 構造内の複数の要素にアクセスするには、動的オブジェクトとして構造にアクセスするのが簡単です。 テキスト データを動的オブジェクトにキャストするには、`parsejson` を使用します。 いったん動的な型に変換すれば、追加の関数を使用してデータを分析できるようになります。

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend status0=hosts_object.hosts[0].status, rate1=hosts_object.hosts[1].rate
```



### <a name="arraylength"></a>arraylength
配列内の要素の数を数えるには、`arraylength` を使用します:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| extend hosts_num=arraylength(hosts_object.hosts)
```

### <a name="mvexpand"></a>mvexpand
オブジェクトのプロパティを個別の行に分割するには、`mvexpand` を使用します。

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| mvexpand hosts_object.hosts[0]
```

![mvexpand](media/json-data-structures/mvexpand.png)

### <a name="buildschema"></a>buildschema
オブジェクトのすべての値に対応するスキーマを取得するには、`buildschema` を使用します:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"location":"South_DC", "status":"stopped", "rate":3}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```

出力は JSON 形式のスキーマです:
```json
{
    "hosts":
    {
        "indexer":
        {
            "location": "string",
            "rate": "int",
            "status": "string"
        }
    }
}
```
この出力には、オブジェクト フィールドの名前と、それに対応するデータ型が記述されています。 

入れ子になったオブジェクトは、次の例のように、さまざまなスキーマがある場合があります:

```Kusto
let hosts_object = parsejson('{"hosts": [{"location":"North_DC", "status":"running", "rate":5},{"status":"stopped", "rate":"3", "range":100}]}');
print hosts_object 
| summarize buildschema(hosts_object)
```


![スキーマの作成](media/json-data-structures/buildschema.png)

## <a name="next-steps"></a>次の手順
Log Analytics クエリ言語の使用については、他のレッスンをご覧ください。

- [文字列操作](string-operations.md)
- [日付と時刻の操作](datetime-operations.md)
- [集計関数](aggregations.md)
- [高度な集計](advanced-aggregations.md)
- [詳細クエリ書き込み](advanced-query-writing.md)
- [結合](joins.md)
- [グラフ](charts.md)