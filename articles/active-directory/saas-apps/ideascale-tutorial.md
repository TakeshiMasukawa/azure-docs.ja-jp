---
title: チュートリアル:Azure Active Directory と IdeaScale の統合 | Microsoft Docs
description: Azure Active Directory と IdeaScale の間でシングル サインオンを構成する方法について説明します。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: e16dda6b-fdf9-43cc-9bbb-a523f085a8af
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: ca5cac1888fe7e126d6bdc8bd4a2e9bc192f4d41
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/23/2019
ms.locfileid: "54810059"
---
# <a name="tutorial-azure-active-directory-integration-with-ideascale"></a>チュートリアル:Azure Active Directory と IdeaScale の統合

このチュートリアルでは、IdeaScale と Azure Active Directory (Azure AD) を統合する方法について説明します。

IdeaScale と Azure AD の統合には、次の利点があります。

- IdeaScale にアクセスする Azure AD ユーザーを制御できます
- ユーザーが自分の Azure AD アカウントで自動的に IdeaScale にサインオン (シングル サインオン) できるように、設定が可能です
- 1 つの中央サイト (Azure Portal) でアカウントを管理できます

SaaS アプリと Azure AD の統合の詳細については、「[Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)」をご覧ください。

## <a name="prerequisites"></a>前提条件

Azure AD と IdeaScale の統合を構成するには、次のものが必要です。

- Azure AD サブスクリプション
- IdeaScale でのシングル サインオンが有効なサブスクリプション

> [!NOTE]
> このチュートリアルの手順をテストする場合、運用環境を使用しないことをお勧めします。

このチュートリアルの手順をテストするには、次の推奨事項に従ってください。

- 必要な場合を除き、運用環境は使用しないでください。
- Azure AD の評価環境がない場合は、 [こちら](https://azure.microsoft.com/pricing/free-trial/)から 1 か月の評価版を入手できます。

## <a name="scenario-description"></a>シナリオの説明
このチュートリアルでは、テスト環境で Azure AD のシングル サインオンをテストします。 このチュートリアルで説明するシナリオは、主に次の 2 つの要素で構成されています。

1. ギャラリーからの IdeaScale の追加
1. Azure AD シングル サインオンの構成とテスト

## <a name="adding-ideascale-from-the-gallery"></a>ギャラリーからの IdeaScale の追加
Azure AD への IdeaScale の統合を構成するには、ギャラリーから管理対象 SaaS アプリの一覧に IdeaScale を追加する必要があります。

**ギャラリーから IdeaScale を追加するには、次の手順に従います。**

1. **[Azure Portal](https://portal.azure.com)** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。 

    ![Active Directory][1]

1. **[エンタープライズ アプリケーション]** に移動します。 次に、**[すべてのアプリケーション]** に移動します。

    ![[アプリケーション]][2]
    
1. 新しいアプリケーションを追加するには、ダイアログの上部にある **[新しいアプリケーション]** をクリックします。

    ![[アプリケーション]][3]

1. 検索ボックスに「**IdeaScale**」と入力します。

    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/tutorial_ideascale_search.png)

1. 結果ウィンドウで **IdeaScale** を選択し、**[追加]** をクリックして、アプリケーションを追加します。

    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/tutorial_ideascale_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成とテスト
このセクションでは、"Britta Simon" というテスト ユーザーに基づいて、IdeaScale で Azure AD のシングル サインオンを構成し、テストします。

シングル サインオンを機能させるには、Azure AD ユーザーに対応する IdeaScale ユーザーが Azure AD で認識されている必要があります。 言い換えると、Azure AD ユーザーと IdeaScale の関連ユーザーの間で、リンク関係が確立されている必要があります。

IdeaScale で、Azure AD の **[ユーザー名]** の値を **[Username]** の値として割り当ててリンク関係を確立します。

IdeaScale で Azure AD のシングル サインオンを構成してテストするには、次の構成要素を完了する必要があります。

1. **[Azure AD シングル サインオンの構成](#configuring-azure-ad-single-sign-on)** - ユーザーがこの機能を使用できるようにします。
1. **[Azure AD のテスト ユーザーの作成](#creating-an-azure-ad-test-user)** - Britta Simon で Azure AD のシングル サインオンをテストします。
1. **[IdeaScale テスト ユーザーの作成](#creating-an-ideascale-test-user)** - Azure AD でのユーザーにリンクされた、IdeaScale での Britta Simon の対応するユーザーを作成します。
1. **[Azure AD テスト ユーザーの割り当て](#assigning-the-azure-ad-test-user)** - Britta Simon が Azure AD のシングル サインオンを使用できるようにします。
1. **[シングル サインオンのテスト](#testing-single-sign-on)** - 構成が機能するかどうかを確認します。

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD シングル サインオンの構成

このセクションでは、Azure Portal で Azure AD のシングル サインオンを有効にして、IdeaScale アプリケーションでシングル サインオンを構成します。

**IdeaScale で Azure AD シングル サインオンを構成するには、次の手順に従います。**

1. Azure Portal の **IdeaScale** アプリケーション統合ページで、**[シングル サインオン]** をクリックします。

    ![Configure single sign-on][4]

1. **[シングル サインオン]** ダイアログで、**[モード]** として **[SAML ベースのサインオン]** を選択し、シングル サインオンを有効にします。
 
    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_ideascale_samlbase.png)

1. **[IdeaScale のドメインと URL]** セクションで、次の手順を実行します。

    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_ideascale_url.png)

    a. **[サインオン URL]** ボックスに、`https://<companyname>.ideascale.com` のパターンを使用して URL を入力します。

    b. **[識別子]** ボックスに、次の形式で URL を入力します。
    | |
    |--|
    | `http://<companyname>.ideascale.com`  |
    | `https://<companyname>.ideascale.com` |

    > [!NOTE] 
    > これらは実際の値ではありません。 実際のサインオン URL と識別子でこれらの値を更新してください。 これらの値を取得するには、[IdeaScale クライアント サポート チーム](https://support.ideascale.com/)に問い合わせてください。 
 
1. **[SAML 署名証明書]** セクションで、**[Metadata XML (メタデータ XML)]** をクリックし、コンピューターにメタデータ ファイルを保存します。

    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_ideascale_certificate.png) 

1. **[保存]** ボタンをクリックします。

    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_general_400.png)

1. **[IdeaScale 構成]** セクションで、**[IdeaScale の構成]** をクリックして、**[サインオンの構成]** ウィンドウを開きます。 **[クイック リファレンス]** セクションから、**サインアウト URL と SAML エンティティ ID** をコピーします。

    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_ideascale_configure.png) 

1. 別の Web ブラウザーのウィンドウで、IdeaScale 企業サイトに管理者としてログインします。

1. **[コミュニティの設定]** に移動します。
   
    ![Community Settings](./media/ideascale-tutorial/ic790847.png "Community Settings")

1. **[セキュリティ] \> [シングル サインオン設定]** の順にクリックします。
   
    ![Single Signon Settings](./media/ideascale-tutorial/ic790848.png "Single Signon Settings")

1. **[シングル サインオンのタイプ]** で **[SAML 2.0]** を選択します。
   
    ![Single Signon Type](./media/ideascale-tutorial/ic790849.png "Single Signon Type")

1. **[シングル サインオンの設定]** ダイアログで、次の手順を実行します。
   
    ![Single Signon Settings](./media/ideascale-tutorial/ic790850.png "Single Signon Settings")
   
    a. **[SAML IdP Entity ID]\(SAML IdP エンティティ ID\)** ボックスに、Azure Portal からコピーした **SAML エンティティ ID** の値を貼り付けます。

    b. Azure Portal からダウンロードしたメタデータ ファイルの内容をコピーし、 **[SAML IdP Metadata]\(SAML IdP メタデータ\)** ボックスに貼り付けます。

    c. **[Logout Success URL]\(ログアウト成功 URL\)** ボックスに、Azure Portal からコピーした**サインアウト URL** の値を貼り付けます。

    d. **[変更を保存]** をクリックします。

> [!TIP]
> アプリのセットアップ中、[Azure Portal](https://portal.azure.com) 内で上記の手順の簡易版を確認できるようになりました。  **[Active Directory] の [エンタープライズ アプリケーション]** セクションからこのアプリを追加した後、**[シングル サインオン]** タブをクリックし、一番下の **[構成]** セクションから組み込みドキュメントにアクセスするだけです。 埋め込みドキュメント機能の詳細については、[Azure AD の埋め込みドキュメント]( https://go.microsoft.com/fwlink/?linkid=845985)に関するページを参照してください。
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD のテスト ユーザーの作成
このセクションの目的は、Azure Portal で Britta Simon というテスト ユーザーを作成することです。

![Azure AD ユーザーの作成][100]

**Azure AD でテスト ユーザーを作成するには、次の手順に従います。**

1. **Azure Portal** の左側のナビゲーション ウィンドウで、**[Azure Active Directory]** アイコンをクリックします。

    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/create_aaduser_01.png) 

1. **[ユーザーとグループ]** に移動し、**[すべてのユーザー]** をクリックして、ユーザーの一覧を表示します。
    
    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/create_aaduser_02.png) 

1. ダイアログの上部にある **[追加]** をクリックして、**[ユーザー]** ダイアログを開きます。
 
    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/create_aaduser_03.png) 

1. **[ユーザー]** ダイアログ ページで、次の手順を実行します。
 
    ![Azure AD のテスト ユーザーの作成](./media/ideascale-tutorial/create_aaduser_04.png) 

    a. **[名前]** ボックスに「**BrittaSimon**」と入力します。

    b. **[ユーザー名]** ボックスに BrittaSimon の**電子メール アドレス**を入力します。

    c. **[パスワードを表示]** を選択し、**[パスワード]** の値をメモします。

    d. **Create** をクリックしてください。
 
### <a name="creating-an-ideascale-test-user"></a>IdeaScale テスト ユーザーの作成

Azure AD ユーザーが IdeaScale にログインできるようにするには、そのユーザーを IdeaScale にプロビジョニングする必要があります。 IdeaScale の場合、プロビジョニングは手動で行います。

**ユーザー プロビジョニングを構成するには、次の手順に従います。**

1. **IdeaScale** 企業サイトに管理者としてログインします。

1. **[コミュニティの設定]** に移動します。
   
    ![Community Settings](./media/ideascale-tutorial/ic790847.png "Community Settings")

1. **[基本設定] \> [メンバ管理]** の順にクリックします。

1. **[Add Member]** をクリックします。
   
    ![Member Management](./media/ideascale-tutorial/ic790852.png "Member Management")

1. [新しいメンバーの追加] セクションで、次の手順を実行します。
   
    ![Add New Member](./media/ideascale-tutorial/ic790853.png "Add New Member")
   
    a. **[電子メール アドレス]** ボックスに、プロビジョニングする有効な AAD アカウントの電子メール アドレスを入力します。
   
    b. **[変更を保存]** をクリックします。 
   
    >[!NOTE]
    >Azure Active Directory のアカウント所有者には、そのアカウントがアクティブになる前に、アカウント確認用のリンクを含む電子メールが送信されます。
      
>[!NOTE]
>IdeaScale から提供されている他の IdeaScale ユーザー アカウント作成ツールまたは API を使用して、AAD ユーザー アカウントをプロビジョニングできます。
 

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD テスト ユーザーの割り当て

このセクションでは、Britta Simon に IdeaScale へのアクセスを許可することで、このユーザーが Azure シングル サインオンを使用できるようにします。

![ユーザーの割り当て][200] 

**IdeaScale に Britta Simon を割り当てるには、次の手順に従います。**

1. Azure Portal でアプリケーション ビューを開き、ディレクトリ ビューに移動します。次に、**[エンタープライズ アプリケーション]** に移動し、**[すべてのアプリケーション]** をクリックします。

    ![ユーザーの割り当て][201] 

1. アプリケーションの一覧で **[IdeaScale]** を選択します。

    ![Configure single sign-on](./media/ideascale-tutorial/tutorial_ideascale_app.png) 

1. 左側のメニューで **[ユーザーとグループ]** をクリックします。

    ![ユーザーの割り当て][202] 

1. **[追加]** ボタンをクリックします。 次に、**[割り当ての追加]** ダイアログで **[ユーザーとグループ]** を選択します。

    ![ユーザーの割り当て][203]

1. **[ユーザーとグループ]** ダイアログで、ユーザーの一覧から **[Britta Simon]** を選択します。

1. **[ユーザーとグループ]** ダイアログで **[選択]** をクリックします。

1. **[割り当ての追加]** ダイアログで **[割り当て]** ボタンをクリックします。
    
### <a name="testing-single-sign-on"></a>シングル サインオンのテスト


このセクションの目的は、アクセス パネルを使用して Azure AD のシングル サインオン構成をテストすることです。

アクセス パネルで [IdeaScale] タイルをクリックすると、自動的に IdeaScale アプリケーションにサインオンします。

## <a name="additional-resources"></a>その他のリソース

* [SaaS アプリと Azure Active Directory を統合する方法に関するチュートリアルの一覧](tutorial-list.md)
* [Azure Active Directory のアプリケーション アクセスとシングル サインオンとは](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ideascale-tutorial/tutorial_general_01.png
[2]: ./media/ideascale-tutorial/tutorial_general_02.png
[3]: ./media/ideascale-tutorial/tutorial_general_03.png
[4]: ./media/ideascale-tutorial/tutorial_general_04.png

[100]: ./media/ideascale-tutorial/tutorial_general_100.png

[200]: ./media/ideascale-tutorial/tutorial_general_200.png
[201]: ./media/ideascale-tutorial/tutorial_general_201.png
[202]: ./media/ideascale-tutorial/tutorial_general_202.png
[203]: ./media/ideascale-tutorial/tutorial_general_203.png

