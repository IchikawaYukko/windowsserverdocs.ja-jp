---
title: 運用環境でサーバーワークロードを実行する仮想マシンでチェックポイントを使用しない
description: このベストプラクティスアナライザールールのテキストのオンラインバージョン。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f2486093e31143b7493665d3d1254f7034ad1415
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365220"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>運用環境でサーバーワークロードを実行する仮想マシンでチェックポイントを使用しない

>適用先:Windows Server 2016


  
*ベストプラクティスとスキャンの詳細については、「* [ベストプラクティスアナライザー](https://go.microsoft.com/fwlink/?LinkId=122786)」を参照してください。  
  
|プロパティ|詳細|  
|-|-|  
|**オペレーティング システム**|Windows Server 2016|  
|**製品/機能**|Hyper-V|  
|**順**|警告|  
|**カテゴリ**|操作|  

次のセクションでは、斜体は、この問題のためのベスト プラクティス アナライザー ツールで表示される UI テキストを示します。

> [!NOTE]  
> Windows Server 2012 R2 では、仮想マシンのスナップショットは、System Center の仮想マシンの管理に使用される用語と一致するように、Hyper-v マネージャーの仮想マシンのチェックポイントに名前が変更されました。 詳細については、「[チェックポイントとスナップショットの概要](https://technet.microsoft.com/library/dn818483.aspx)」を参照してください。  
  
## <a name="issue"></a>問題  
  
*1つ以上のチェックポイントを持つバーチャルマシンが見つかりました。*  
  
## <a name="impact"></a>影響  
  
@no__t の空き領域が、チェックポイントファイルが格納されている物理ディスク上で実行される場合があります。このような場合、物理記憶域に対して追加のディスク操作を実行できるはされません。物理記憶域に依存しているすべての仮想マシンが影響を受ける可能性があります。 *  
  
物理ディスク領域が不足している場合、チェックポイントまたは仮想ハードディスクがそのディスクに格納されている実行中の仮想マシンは、自動的に一時停止される可能性があります。 Hyper-v マネージャーは、これらの仮想マシンの状態を "一時停止-重大" として表示します。  
  
## <a name="resolution"></a>解決方法  
  
@no__t-仮想マシンが運用環境でサーバーワークロードを実行する場合は、仮想マシンをオフラインにし、Hyper-v マネージャーを使用してチェックポイントを適用または削除します。チェックポイントを削除するには、バーチャルマシンをシャットダウンしてプロセスを完了する必要があります。 *  
  
> [!NOTE]  
> 標準チェックポイントの代わりに、運用チェックポイントを使用できるようになりました。 詳細について[は、「標準チェックポイントまたは運用チェックポイントの選択](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)」を参照してください。  
  


