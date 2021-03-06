---
title: Content Moderator API でのチームおよびサブチームの管理 - Content Moderator
titlesuffix: Azure Cognitive Services
description: Cognitive Services の Content Moderator API でチームとサブチームを使用する方法を説明します。
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: e45e06bdcc69dc88d8c979bfbb5a0974f7284b22
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266231"
---
# <a name="manage-review-teams-and-subteams"></a>レビュー チームとサブチームを管理する

Content Moderator を使用する前に、チームを作成する必要があります。 チームを新規登録し、名前を付けると、このチームは既定のチームになります。 レビュー ツールでは 1 つのチームのみ持つことができます。 ただし、サブチームは複数作成することができます。 サブチームは、エスカレーション チームまたはコンテンツの特定のカテゴリのレビュー専用のチームの作成に役立ちます。 たとえば、成人向けコンテンツをさらにレビューするために別のチームに送信することが必要な場合があります。

このトピックでは、サブチームを作成し、その場でレビューをすばやく割り当てる方法について説明します。 ただし、[ワークフロー](workflows.md)を使用して、特定の条件に基づいてレビューを割り当てることができます。

## <a name="go-to-the-teams-setting"></a>チームの設定に移動する

サブチームの作成を開始するには、[設定] で **[チーム]** オプションを選択します。

![チーム設定](images/0-teams-1.png)

## <a name="create-subteams"></a>サブチームを作成する

既定のチームには、すべての潜在的なレビュー担当者が含まれています。サブチームは、既定のチームのサブセットです。 まだ既定のチームのメンバーになっていないユーザーをサブチームに割り当てることはできないので、既定のチームにレビュー担当者をすぐに追加する必要があります。 [チーム] ページで [招待] をクリックします。

![ユーザーの招待](images/invite-users.png)

### <a name="create-a-subteam"></a>サブチームを作成する
[チーム] ページの [Subteam]\(サブチーム\) セクションまでスクロールします。 [Add Subteam]\(サブチームの追加\) をクリックします。 

![サブチームの追加](images/1-teams-1.png)

### <a name="name-your-subteam"></a>サブチームに名前を付ける
ダイアログにサブチーム名を入力し、[保存] をクリックします。

![サブチーム名](images/1-Teams-2.PNG)

### <a name="assign-members-from-your-default-team"></a>既定のチームからメンバーを割り当てます。
[メンバーの追加] をクリックして、既定のチームから 1 つまたは複数のサブチームにメンバーを割り当てます。 サブチームには既存のユーザーのみを追加することができます。 レビュー ツールに含まれていない新しいユーザーを追加するには、[チーム設定] ページで、[招待] ボタンを使用して、それらのユーザーを招待します。

![サブチーム メンバーの割り当て](images/1-Teams-3.PNG)

## <a name="assign-reviews-to-your-subteams"></a>サブチームにレビューを割り当てる

サブチームを作成し、チーム メンバーを割り当てたら、それらのサブチームへの画像およびテキスト レビューの割り当てを開始できます。 これは、[レビュー] ウィンドウから行います。
個々の画像をサブチームに割り当てる場合は、画像の右上隅にある省略記号をクリックし、[Move to]\(移動先\) を選択して、サブチームを選択します。

![画像のレビューをサブチームに割り当てる](images/3-review-image-subteam-1.png)

## <a name="switch-between-subteams-to-review-assigned-content"></a>サブチーム間を切り替えて割り当てられたコンテンツをレビューする

1 つまたは複数のサブチームのメンバーである場合は、レビュー ツールのダッシュボードからそれらのサブチーム間を切り替えることができます。 サブチームに属している現在保留中のすべてのレビューを表示するには、[Image]\(画像\) タブから [Choose Subteam]\(サブチームの選択\) を選択します。

![サブチーム間の切り替え](images/3-review-image-subteam-2.png)
