---
title: '場所と接続プロバイダー: Azure ExpressRoute | Microsoft Docs'
description: この記事では、サービスが提供されている場所と Azure リージョンに接続する方法の詳細について説明します。 場所ごとにソートされています。
services: expressroute
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
ms.assetid: feb67da3-5abc-4acb-bad4-f78e3c541ded
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/19/2018
ms.author: pareshmu
ms.openlocfilehash: 6f57a63938ff09008099faccbc599f21995f2557
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232857"
---
# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute パートナーとピアリングの場所

> [!div class="op_single_selector"]
> * [プロバイダー別の場所](expressroute-locations.md)
> * [場所別のプロバイダー](expressroute-locations-providers.md)


この記事の表では、ExpressRoute 接続プロバイダー、ExpressRoute の地理的範囲、ExpressRoute でサポートされる Microsoft クラウド サービス、ExpressRoute システム インテグレーター (SI) に関する情報を提供します。

## <a name="partners"></a>ExpressRoute 接続プロバイダー
ExpressRoute は、すべての Azure のリージョンと場所でサポートされています。 次のマップは Azure のリージョンと ExpressRoute の場所を一覧しています。 ExpressRoute の場所は、Microsoft がいくつかのサービス プロバイダーと接続している場所を示します。

![場所のマップ][0]

地理的リージョン内の少なくとも 1 つの ExpressRoute の場所に接続している場合は、その地理的リージョン内のすべてのリージョンの Azure サービスにアクセスできます。 

### <a name="azure-regions-to-expressroute-locations-within-a-geopolitical-region"></a>地理的リージョン内の Azure リージョンと ExpressRoute の場所の対応
次の表は、地理的リージョン、ExpressRoute の場所、Azure リージョンをまとめたものです。

| **地理的リージョン** | **ゾーン** | **Azure リージョン** | **ExpressRoute の場所** |
| --- | --- | --- | --- |
| **オーストラリア政府** | 1 | オーストラリア中部、オーストラリア中部 2 |キャンベラ、キャンベラ2 |
| **ヨーロッパ** | 1 |フランス中部、フランス南部、北ヨーロッパ、西ヨーロッパ、英国西部、英国南部 |アムステルダム、アムステルダム 2、ダブリン、ロンドン、マルセイユ、ニューポート (ウェールズ)、パリ |
| **北米** | 1 |米国東部、米国西部、米国東部 2、米国西部 2、米国中部、米国中南部、米国中北部、米国中西部、カナダ中部、カナダ東部 |アトランタ、シカゴ、ダラス、デンバー、ラスベガス、ロサンゼルス、マイアミ、ニューヨーク、サンアントニオ、シアトル、シリコン バレー、ワシントン DC、モントリオール、ケベックシティ、トロント |
| **アジア** | 2 |東アジア、東南アジア |香港特別行政区、クアラルンプール、シンガポール、シンガポール 2 |
| **オーストラリア** | 2 |オーストラリア東部、オーストラリア南東部 |メルボルン、シドニー | 
| **インド** | 2 |インド西部、インド中部、インド南部 |チェンナイ、チェンナイ 2、ムンバイ、ムンバイ 2 |
| **日本** | 2 |西日本、東日本 |大阪、東京 |
| **韓国** | 2 |韓国中部、韓国南部 |釜山、ソウル|
| **南アフリカ** | 3 |[南アフリカ西部+、南アフリカ北部+](https://blogs.microsoft.com/blog/2017/05/18/microsoft-deliver-microsoft-cloud-datacenters-africa/) |ケープタウン、ヨハネスブルグ |
| **南アメリカ** | 3 |ブラジル南部 |サンパウロ |

 **+** は近日対応予定を表します


### <a name="regions-and-geopolitical-boundaries-for-national-clouds"></a>各国のクラウドのリージョンと地理的境界
次の表は、国内のクラウドのリージョンと地政学的境界に関する情報をまとめたものです。

| **地理的リージョン** | **Azure リージョン** | **ExpressRoute の場所** |
| --- | --- | --- |
| **米国政府のクラウド** |米国政府アリゾナ、US Gov アイオワ、米国政府テキサス、米国政府バージニア、US DoD 中部、US DoD 東部  |シカゴ、ダラス、ニューヨーク、フェニックス、サンアントニオ、シアトル、シリコン バレー、ワシントン DC |
| **中国東部** |中国東部、中国東部 2 |上海、上海 2 |
| **中国北部** |中国北部、中国北部 2 |北京、北京 2 |
| **ドイツ** |ドイツ中部、ドイツ東部 |ベルリン、フランクフルト |

標準の ExpressRoute SKU では、地理的リージョン間の接続はサポートされていません。 グローバル接続に対応するには、ExpressRoute Premium アドオンを有効にする必要があります。 国内のクラウド環境には接続できません。 国内のクラウド環境に接続する必要がある場合、接続プロバイダーにご相談ください。

## <a name="locations"></a>接続プロバイダーの場所

次の表に、接続する場所と、各場所のサービス プロバイダーを示します。 サービス プロバイダーと、そのサービス プロバイダーがサービスを提供できる場所を確認する場合は、[サービス プロバイダー別の場所](expressroute-locations.md#locations)に関するページを参照してください。 


### <a name="production-azure"></a>運用 Azure
| **場所** | **ピアリングの場所の所有者** | **サービス プロバイダー** |
| --- | --- | --- |
| **アムステルダム** | Equinix | Aryaka Networks、AT&T NetBond、British Telecom、Colt、Equinix、euNetworks、GÉANT、InterCloud、Interxion、KPN、IX Reach、Level 3 Communications、Megaport、NTT Communications、Orange、Tata Communications、TeleCity Group、Telefonica、Telenor、Telia Carrier、Verizon、Zayo |
| **アムステルダム 2** | Interxion | Interxion |
| **アトランタ** | Equinix | Equinix、Megaport |
| **釜山** |LG CNS | LG CNS |
| **キャンベラ** | CDC | CDC |
| **キャンベラ2** | CDC | CDC |
| **ケープタウン** | Teraco | Internet Solutions - Cloud Connect、Liquid Telecom、Teraco |
| **チェンナイ** | Tata Communications | Global CloudXchange (GCX)、SIFY、Tata Communications |
| **チェンナイ 2** | Airtel | Airtel |
| **シカゴ** | Equinix | Aryaka Networks、AT&T NetBond、Cologix、Comcast、Coresite、Equinix、Internet2、Level 3 Communications、Megaport、PacketFabric、PCCW Global Limited、Sprint、Telia Carrier、Verizon、Zayo |
| **ダラス** | Equinix | Aryaka Networks、AT&T NetBond、Cologix、Equinix、Internet2、Level 3 Communications、Megaport、Neutrona Networks、Telmex Uninet+、Telia Carrier、Verizon、Zayo|
| **デンバー** | CoreSite | CoreSite、Megaport |
| **ダブリン** | Equinix | Colt、eir、Equinix、Interxion、Megaport |
| **香港特別行政区** | Equinix | Aryaka Networks、British Telecom、China Telecom Global、Equinix、Megaport、NTT Communications、Orange、PCCW Global Limited、Tata Communications、Verizon |
| **ヨハネスブルグ** | Teraco | Internet Solutions - Cloud Connect、Liquid Telecom、Teraco |
| **クアラルンプール** | TIME dotCom | TIME dotCom |
| **ラスベガス** | Switch | CenturyLink Cloud Connect、Megaport |
| **ロンドン** | Equinix | AT&T NetBond、British Telecom、Colt、Equinix、InterCloud、Internet Solutions - Cloud Connect、Interxion、Jisc、Level 3 Communications、Megaport、MTN、NTT Communications、Orange、PCCW Global Limited、Tata Communications、Telehouse - KDDI、Telenor、Telia Carrier、Verizon、Vodafone、Zayo |
| **ロサンゼルス** | CoreSite | CoreSite、Equinix、Megaport、NTT、Zayo |
| **マルセイユ** |Interxion | Interxion |
| **メルボルン** | NextDC | AARNet、Equinix、Megaport、NEXTDC、Optus+、Telstra Corporation |
| **マイアミ** | Equinix | C3ntro+、Equinix、Megaport、Neutrona Networks |
| **モントリオール** | Cologix | Bell Canada、Cologix、Telus、Zayo |
| **ムンバイ** | Tata Communications | Global CloudXchange (GCX)、Sify、Tata Communications、Vodafone Idea |
| **ムンバイ 2** | Airtel | Airtel、Sify、Vodafone Idea |
| **ニューヨーク** | Equinix | CenturyLink Cloud Connect、Coresite、Equinix、InterCloud、Megaport、Zayo |
| **ニューポート (ウェールズ)** | Next Generation Data | Level 3 Communications、Next Generation Data |
| **大阪** | Equinix | Equinix、Internet Initiative Japan Inc. - IIJ、NTT Communications、NTT SmartConnect、Softbank |
| **パリ** | Interxion | Colt、Intercloud、Interxion、Equinix、Orange |
| **ケベック シティ** | 4Degrees | Bell Canada、Megaport |
| **サンアントニオ** | CyrusOne | CenturyLink Cloud Connect、Megaport |
| **サンパウロ** | Equinix | Aryaka Networks、Ascenty Data Centers、British Telecom、Equinix、Level 3 Communications、Neutrona Networks、Orange、Tata Communications、Telefonica、UOLDIVEO |
| **シアトル** | Equinix | Aryaka Networks、Equinix、Level 3 Communications、Megaport、Zayo |
| **ソウル** | KINX | KINX、LG CNS、Sejong Telecom |
| **シリコン バレー** | Equinix | Aryaka Networks、AT&T NetBond、British Telecom、CenturyLink Cloud Connect、Comcast、Coresite、Equinix、InterCloud、IXReach、PacketFabric、Level 3 Communications、Megaport、Orange、Sprint、Tata Communications、Verizon、Zayo Group |
| **シンガポール** | Equinix | Aryaka Networks、AT&T NetBond、British Telecom、Epsilon Global Communications、Equinix、InterCloud、Level 3 Communications、Megaport、NTT Communications、Orange、SingTel、Tata Communications、Telstra Corporation、Verizon、Vodafone |
| **シンガポール 2** | Global Switch | Megaport、SingTel |
| **シドニー** | Equinix | AARNet、AT&T NetBond、British Telecom、Equinix、Megaport、NEXTDC、NTT Communications、Optus、Orange、Telstra Corporation、Verizon |
| **東京** | Equinix | Aryaka Networks、AT&T NetBond、British Telecom、CenturyLink Cloud Connect、Colt、Equinix、Internet Initiative Japan Inc. - IIJ、NTT Communications、NTT EAST、Softbank、Verizon |
| **トロント** | Cologix | AT&T NetBond、Bell Canada、CenturyLink Cloud Connect、Cologix、Equinix、Megaport、Telus、Zayo |
| **ワシントン DC** | Equinix | Aryaka Networks、AT&T NetBond、British Telecom、Cologix、Comcast、Coresite、Equinix、Internet2、InterCloud、Level 3 Communications、Megaport、NTT Communications、Orange、PacketFabric、Sprint、Tata Communications、Telia Carrier、Verizon、Zayo |

 **+** は近日対応予定を表します

### <a name="national-cloud-environments"></a>国内のクラウド環境

### <a name="us-government-cloud"></a>米国政府のクラウド
| **場所** | **サービス プロバイダー** |
| --- | --- |
| **シカゴ** |AT&T NetBond、Equinix、Level 3 Communications、Verizon |
| **ダラス** |Equinix、Megaport、Verizon |
| **ニューヨーク** |Equinix、CenturyLink Cloud Connect、Verizon |
| **Phoenix** | CenturyLink Cloud Connect |
| **サンアントニオ** | Megaport |
| **シリコン バレー** | Equinix、Level 3 Communications、Verizon |
| **シアトル** | Equinix、Megaport |
| **ワシントン DC** |AT&T NetBond、Equinix、Level 3 Communications、Verizon |

### <a name="china"></a>中国
| **場所** | **サービス プロバイダー** |
| --- | --- |
| **北京** |China Telecom |
| **北京 2** | GDS |
| **上海** |China Telecom |
| **上海 2** | GDS |

詳細については、 [中国の ExpressRoute](http://www.windowsazure.cn/home/features/expressroute/)

### <a name="germany"></a>ドイツ
| **場所** | **サービス プロバイダー** |
| --- | --- |
| **ベルリン** |e-shelter、Megaport+、T-Systems |
| **フランクフルト** |Colt、Equinix、Interxion |

## <a name="c1partners"></a>Exchange プロバイダー経由の接続
接続プロバイダーが上記のセクションの一覧にない場合でも、接続を作成できます。

* 接続プロバイダーが上の表に記載されているいずれかの Exchange に接続されているかどうかをその接続プロバイダーに確認します。 次のリンクから、Exchange プロバイダーが提供するサービスの詳細情報を収集できます。 一部の接続プロバイダーは既にイーサネット Exchange に接続されています。
  * [Cologix](http://www.cologix.com/)
  * [CoreSite](http://www.coresite.com/)
  * [Equinix Cloud Exchange](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
  * [InterXion](http://www.interxion.com/)
  * [NextDC](http://www.nextdc.com/)
  * [Megaport](https://www.megaport.com/services/microsoft-expressroute/)
  * [PacketFabric](https://www.packetfabric.com/packetcor/microsoft-azure/)
  
* その接続プロバイダーに、選択したピアリングの場所までネットワークを拡張してもらいます。
  * 単一障害点がないように、接続プロバイダーが可用性の高い方法で接続を拡張していることを確認します。
* Microsoft に接続するために、接続プロバイダーとしてその Exchange で ExpressRoute 回線を要求します。
  * 「 [ExpressRoute 回線の作成](expressroute-howto-circuit-classic.md) 」の手順に従い、接続を構築します。

## <a name="c1partners"></a>その他のサービス プロバイダー経由の接続
| **場所** | **Exchange** | **接続プロバイダー** |
| --- | --- | --- |
| **アムステルダム** | Equinix、Telecity | BICS、Eurofiber、Fastweb S.p.A、Gulf Bridge International、MainOne、Nianet、Post、Proximus、Telecom Italia Sparkle、Telia |
| **ケープタウン** | Teraco | MTN |
| **シカゴ** | Equinix | Lightower、Windstream |
| **ダラス** | Equinix、Megaport | Axtel、C3ntro Telecom、Cox Business、Data Foundry、Transtelco |
| **フランクフルト** | Telecity | BICS、Nianet、QSC AG |
| **香港特別行政区** | Equinix | Macroview Telecom |
| **ヨハネスブルグ** | Teraco | MTN |
| **ロンドン** | Equinix、euNetworks、Telecity | Bezeq International Ltd.、Epsilon、Exponential E、HSO、NexGen Networks、Tamares Telecom、Zain |
| **ロサンゼルス** | Equinix |Transtelco |
| **マドリッド** | Level3 | Zertia |
| **モントリオール** | Cologix、Equinix | Airgate Technologies. Inc、Cogeco Peer 1、Rogers、Zirro |
| **ニューヨーク** |Equinix、Megaport | Altice Business、Lightower、Webair |
| **シアトル** |Equinix | Alaska Communications |
| **シリコン バレー** |Equinix | Cox Business、Windstream |
| **シンガポール** |Equinix |1CLOUDSTAR、BICS、Epsilon Telecommunications Limited、LGA Telecom、United Information Highway (UIH) |
| **スラウ** | Equinix | HSO|
| **シドニー** | Megaport | Macquarie Telecom Group|
| **東京** | Equinix | アルテリア・ネットワークス株式会社、株式会社ブロードバンドタワー |
| **トロント** | Equinix | Airgate Technologies. Inc、Cogeco Peer 1、IVedha Inc、Rogers、Thinktel、Zirro|
| **ワシントン DC** |Equinix | Altice Business、BICS、Gtt Communications Inc、Epsilon、Lightower、Masergy、Windstream |

## <a name="expressroute-system-integrators"></a>ExpressRoute システム インテグレーター
ネットワークの規模によっては、ニーズに合わせてプライベート接続を有効にするのは難しい場合があります。 次の表のいずれかのシステム インテグレーターを使用すると、ExpressRoute の利用開始に役立ちます。

| **大陸** | **システム インテグレーター** |
| --- | --- |
| **アジア** |Avanade Inc.、OneAs1a |
| **オーストラリア** | Ensyst、IT Consultancy、MOQdigital、Vigilant.IT |
| **ヨーロッパ** |Avanade Inc.、Altogee、Bright Skies GmbH、Inframon、MSG Services、New Signature、Nelite、Orange Networks、sol-tec |
| **北米** |Avanade Inc.、Equinix Professional Services、FlexManage、Lightstream、Perficient、Presidio |
| **南アメリカ** |Avanade Inc.、Venha Pra Nuvem |
## <a name="next-steps"></a>次の手順
* ExpressRoute の詳細については、「 [ExpressRoute のFAQ](expressroute-faqs.md)」をご覧ください。
* すべての前提条件を満たしていることを確認します。 「 [Azure ExpressRoute の前提条件](expressroute-prerequisites.md)」を参照してください。

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "場所のマップ"
