---
title: 複数のリージョンへの Studio Web サービスのデプロイ
titleSuffix: Azure Machine Learning Studio
description: 新しい Web サービスを他のリージョンにデプロイ (コピー) する手順。 複数のサブスクリプションやワークスペースがなくても、複数のリージョンに Web サービスを簡単にデプロイできます。
services: machine-learning
ms.service: machine-learning
ms.component: studio
ms.topic: article
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 04/19/2017
---
# <a name="deploy-an-azure-machine-learning-studio-web-service-to-multiple-regions"></a>複数のリージョンに Azure Machine Learning Studio Web サービスをデプロイする

新しい Azure の Web サービスを使用すると、複数のサブスクリプションやワークスペースがなくても、複数のリージョンに Azure Machine Learning Studio Web サービスを簡単にデプロイできます。 

料金は、リージョンに固有なので、Web サービスをデプロイするリージョンごとに課金プランを定義する必要があります。

## <a name="to-create-a-plan-in-another-region"></a>別のリージョンにプランを作成するには
1. [Microsoft Azure Machine Learning Web サービス](https://services.azureml.net/)にサインインします。
2. **[プラン]** メニュー オプションをクリックします。
3. [プラン] 概要ページで **[新規]** をクリックします。
4. **[サブスクリプション]** ドロップダウンで、新しいプランを配置するサブスクリプションを選択します。
5. **[地域]** ドロップダウンで、新しいプランのリージョンを選択します。 選択したリージョンのプラン オプションが、ページの **[Plan Options (プラン オプション)]** セクションに表示されます。
6. **[リソース グループ]** ドロップダウンで、プランのリソース グループを選択します。 リソース グループの詳細については、「[Azure Resource Manager の概要](../../azure-resource-manager/resource-group-overview.md)」をご覧ください。
7. **[プラン名]** にプランの名前を入力します。
8. **[Plan Options (プラン オプション)]** で、新しいプランの課金レベルをクリックします。
9. **Create** をクリックしてください。

## <a name="deploying-the-web-service-to-another-region"></a>別のリージョンに Web サービスをデプロイする
1. **[Web サービス]** メニュー オプションをクリックします。
2. 新しいリージョンにデプロイする Web サービスを選択します。
3. **[コピー]** をクリックします。
4. **[Web サービス名]** に Web サービスの新しい名前を入力します。
5. **[Web service description (Web サービスの説明)]** に Web サービスの説明を入力します。
6. **[サブスクリプション]** ドロップダウンで、新しい Web サービスを配置するサブスクリプションを選択します。
7. **[リソース グループ]** ドロップダウンで、Web サービスのリソース グループを選択します。 リソース グループの詳細については、「[Azure Resource Manager の概要](../../azure-resource-manager/resource-group-overview.md)」をご覧ください。
8. **[地域]** ドロップダウンで、Web サービスをデプロイするリージョンを選択します。
9. **[ストレージ アカウント]** ドロップダウンで、Web サービスを格納するためのストレージ アカウントを選択します。
10. **[Price Plan (価格プラン)]** ドロップダウンで、手順 8 で選択したリージョンのプランを選択します。
11. **[コピー]** をクリックします。

