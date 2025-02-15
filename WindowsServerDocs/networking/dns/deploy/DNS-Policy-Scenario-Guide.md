---
title: DNS ポリシー シナリオ ガイド
description: このトピックは、Windows Server 2016 の DNS ポリシーシナリオガイドに含まれています。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8fab127fcd524526a515752871c1a3db92047c46
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406226"
---
# <a name="dns-policy-scenario-guide"></a>DNS ポリシー シナリオ ガイド

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

このガイドは、DNS、ネットワーク、およびシステム管理者が使用することを目的としています。  
  
DNS ポリシーは、Windows Server @ no__t-0 2016 の DNS の新機能です。 このガイドを使用すると、dns ポリシーを使用して、ポリシーで定義したさまざまなパラメーターに基づいて、DNS サーバーが名前解決のクエリをどのように処理するかを制御できます。   
  
このガイドでは、dns ポリシーの概要について説明し、特定の DNS ポリシーシナリオについて説明します。このシナリオでは、プライマリ用の geo ロケーションベースのトラフィック管理を含む、目標を達成するために DNS サーバーの動作を構成する方法について説明しています。セカンダリ DNS サーバー、アプリケーションの高可用性、スプリットブレイン DNS など。  
  
このガイドは次のセクションで構成されます。  
  
- [DNS ポリシーの概要](DNS-Policies-Overview.md)  
- [プライマリサーバーを使用した地理的な場所ベースのトラフィック管理に DNS ポリシーを使用する](primary-geo-location.md)  
- [プライマリとセカンダリのデプロイを使用した地理的な場所ベースのトラフィック管理に DNS ポリシーを使用する](primary-secondary-geo-location.md)  
- [1日の時間に基づくインテリジェント DNS 応答に DNS ポリシーを使用する](dns-tod-intelligent.md)
- [Azure Cloud App Server を使用した時間帯に基づく DNS 応答](dns-tod-azure-cloud-app-server.md)
- [スプリットブレイン DNS 展開に DNS ポリシーを使用する](split-brain-DNS-deployment.md)
- [Active Directory でのスプリットブレイン DNS に DNS ポリシーを使用する](dns-sb-with-ad.md)
- [Dns クエリにフィルターを適用するために DNS ポリシーを使用する](apply-filters-on-dns-queries.md)
- [アプリケーションの負荷分散に DNS ポリシーを使用する](app-lb.md)
- [Geo ロケーションを認識するアプリケーションの負荷分散に DNS ポリシーを使用する](app-lb-geo.md)

