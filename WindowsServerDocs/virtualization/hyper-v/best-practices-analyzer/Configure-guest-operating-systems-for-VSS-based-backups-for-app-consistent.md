---
title: VSS ベースのバックアップ用のゲストオペレーティングシステムを構成して、Hyper-v レプリカのアプリケーション整合性スナップショットを有効にする
description: このベストプラクティスアナライザールールのテキストのオンラインバージョン。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364960"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>VSS ベースのバックアップ用のゲストオペレーティングシステムを構成して、Hyper-v レプリカのアプリケーション整合性スナップショットを有効にする

>適用先:Windows Server 2016

ベストプラクティスとスキャンの詳細については、「[ベストプラクティスアナライザースキャンの実行」および「スキャン結果の管理](https://go.microsoft.com/fwlink/p/?LinkID=223177)」を参照してください。  
  
|プロパティ|詳細|  
|-|-|  
|**オペレーティング システム**|Windows Server 2016|  
|**製品/機能**|Hyper-V|  
|**順**|Error|  
|**カテゴリ**|構成|  
  
次のセクションでは、斜体は、この問題のためのベスト プラクティス アナライザー ツールで表示される UI テキストを示します。  
  
## <a name="issue"></a>問題  
*アプリケーション整合性スナップショットを使用するには、レプリケーションに参加している仮想マシンのゲストオペレーティングシステムで、ボリュームシャドウコピーサービス (VSS) が有効になっていて構成されている必要があります。*  
  
## <a name="impact"></a>影響  
@no__t-hyper-v は、レプリケーション構成でアプリケーション整合性スナップショットが指定されている場合でも、VSS が構成されていない限り、それらを使用しません。これは、次の仮想マシンに影響します: *  
  
@no__t-仮想マシンの > の一覧  
  
## <a name="resolution"></a>解決方法  
*仮想マシンに統合サービスをインストールするには、仮想マシン接続を使用します。*  
  


