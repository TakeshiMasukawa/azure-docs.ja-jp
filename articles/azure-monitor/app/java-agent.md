---
title: Azure Application Insights での Java Web アプリのパフォーマンス監視 | Microsoft Docs
description: Application Insights を使用した Java Web サイトのパフォーマンスおよび利用状況の監視拡張。
services: application-insights
documentationcenter: java
author: mrbullwinkle
manager: carmonm
ms.assetid: 84017a48-1cb3-40c8-aab1-ff68d65e2128
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: mbullwin
ms.openlocfilehash: dbca662f38f13833a4b9e642a4d8f690017d999a
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262134"
---
# <a name="monitor-dependencies-caught-exceptions-and-method-execution-times-in-java-web-apps"></a>Java Web アプリでの依存関係、キャッチされた例外、およびメソッド実行時間の監視


[Java Web アプリを Application Insights][java] でインストルメント化した場合、Java エージェントを使用して、コードを変更することなく、詳細な分析を行うことができます。

* **依存関係:** アプリケーションが他のコンポーネントに対して行った呼び出しについてのデータであり、次のものを含みます。
  * HttpClient、OkHttp、および RestTemplate (Spring) 経由で行われた **REST 呼び出し**がキャプチャされます。
  * Jedis クライアント経由で行われた **Redis** 呼び出しがキャプチャされます。
  * **[JDBC 呼び出し](https://docs.oracle.com/javase/7/docs/technotes/guides/jdbc/)** - MySQL、SQL Server、Oracle DB の各コマンドが自動的にキャプチャされます。 MySQL で呼び出しにかかる時間が 10 秒を超えた場合、エージェントはクエリ プランをレポートします。
* **例外の検出:** コードで処理される例外に関する情報。
* **メソッドの実行時間:** 特定のメソッドの実行にかかる時間に関する情報。

Java エージェントを使用するには、これをサーバーにインストールします。 Web アプリを [Application Insights Java SDK][java] を使用してインストルメント化する必要があります。 

## <a name="install-the-application-insights-agent-for-java"></a>Jave 用の Application Insights エージェントのインストール
1. Java サーバーを実行しているコンピューターで [エージェントをダウンロード](https://github.com/Microsoft/ApplicationInsights-Java/releases/latest)します。 Application Insights の Java SDK コアおよび Web パッケージと同じバージョンの Java エージェントをダウンロードするようにしてください。
2. アプリケーション サーバーのスタートアップ スクリプトを編集し、次の JVM を追加します。
   
    `javaagent:`*エージェント JAR ファイルへの完全パス*
   
    たとえば、Linux マシンの Tomcat で以下を実行します。
   
    `export JAVA_OPTS="$JAVA_OPTS -javaagent:<full path to agent JAR file>"`
3. アプリケーション サーバーを再起動します。

## <a name="configure-the-agent"></a>エージェントの構成
`AI-Agent.xml` という名前のファイルを作成し、エージェントの JAR ファイルの場所と同じフォルダーに配置します。

xml ファイルの内容を設定します。 次の例を編集して、必要に応じて、機能を含めるか省略します。

```XML

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationInsightsAgent>
      <Instrumentation>

        <!-- Collect remote dependency data -->
        <BuiltIn enabled="true">
           <!-- Disable Redis or alter threshold call duration above which arguments are sent.
               Defaults: enabled, 10000 ms -->
           <Jedis enabled="true" thresholdInMS="1000"/>

           <!-- Set SQL query duration above which query plan is reported (MySQL, PostgreSQL). Default is 10000 ms. -->
           <MaxStatementQueryLimitInMS>1000</MaxStatementQueryLimitInMS>
        </BuiltIn>

        <!-- Collect data about caught exceptions
             and method execution times -->

        <Class name="com.myCompany.MyClass">
           <Method name="methodOne"
               reportCaughtExceptions="true"
               reportExecutionTime="true"
               />

           <!-- Report on the particular signature
                void methodTwo(String, int) -->
           <Method name="methodTwo"
              reportExecutionTime="true"
              signature="(Ljava/lang/String;I)V" />
        </Class>

      </Instrumentation>
    </ApplicationInsightsAgent>

```

レポートの例外や、個々のメソッドのメソッド タイミングを有効にする必要があります。

既定では、`reportExecutionTime` は true、`reportCaughtExceptions` は false です。

### <a name="spring-boot-agent-additional-config"></a>Spring Boot エージェントの追加構成

`java -javaagent:/path/to/agent.jar -jar path/to/TestApp.jar`

> [!NOTE]
> AI-Agent.xml とエージェントの jar ファイルは同じフォルダーに含まれている必要があります。 多くの場合、これらはプロジェクトの `/resources` フォルダーに一緒に配置されます。 

#### <a name="enable-w3c-distributed-tracing"></a>W3C 分散トレースを有効にする

AI-Agent.xml に次のコードを追加します。

```xml
<Instrumentation>
        <BuiltIn enabled="true">
            <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
        </BuiltIn>
    </Instrumentation>
```

> [!NOTE]
> 既定では、下位互換性モードが有効になっています。また、enableW3CBackCompat パラメーターは、省略可能で、このモードをオフにするときにのみ使用する必要があります。 

すべてのサービスが W3C プロトコルをサポートする新しいバージョンの SDK に更新されたときにこれが該当することが理想です。 W3C をサポートする新しいバージョンの SDK にできるだけ早く移行することを強くお勧めします。

**受信と送信 (エージェント) の構成が**[両方とも](correlation.md#w3c-distributed-tracing)正確に同じであることを確認してください。

## <a name="view-the-data"></a>データの表示
Application Insights リソースでは、集計されたリモートの依存関係やメソッドの実行時間が [[パフォーマンス] タイル][metrics]に表示されます。

依存関係、例外、メソッドのレポートの個々のインスタンスを検索するには、[[検索]][diagnostic] を開きます。

「[依存関係の問題の診断](../../azure-monitor/app/asp-net-dependencies.md#diagnosis)」を参照してください。

## <a name="questions-problems"></a>疑問がある場合 問題が発生した場合
* データが表示されない場合 [ファイアウォール例外の設定](../../azure-monitor/app/ip-addresses.md)
* [Java のトラブルシューティング](java-troubleshoot.md)

<!--Link references-->

[api]: ../../azure-monitor/app/api-custom-events-metrics.md
[apiexceptions]: ../../azure-monitor/app/api-custom-events-metrics.md#track-exception
[availability]: ../../azure-monitor/app/monitor-web-app-availability.md
[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[eclipse]: app-insights-java-eclipse.md
[java]: java-get-started.md
[javalogs]: java-trace-logs.md
[metrics]: ../../azure-monitor/app/metrics-explorer.md
