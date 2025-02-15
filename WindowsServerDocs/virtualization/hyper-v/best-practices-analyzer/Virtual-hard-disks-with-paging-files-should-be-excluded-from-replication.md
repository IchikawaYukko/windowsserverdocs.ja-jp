---
title: 仮想ハード_ディスクのページング ファイルとは、レプリケーションから除外する必要があります。
description: このベストプラクティスアナライザールールのテキストのオンラインバージョン。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 215e265d69af1b384d3461c627558ff0a59c8e91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364554"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>仮想ハード_ディスクのページング ファイルとは、レプリケーションから除外する必要があります。

>適用先:Windows Server 2016

ベストプラクティスとスキャンの詳細については、「[ベストプラクティスアナライザースキャンの実行」および「スキャン結果の管理](https://go.microsoft.com/fwlink/p/?LinkID=223177)」を参照してください。  
  
|プロパティ|詳細|  
|-|-|  
|**オペレーティング システム**|Windows Server 2016|  
|**製品/機能**|Hyper-V|  
|**順**|[情報]|  
|**カテゴリ**|構成|  
  
次のセクションでは、斜体は、この問題のためのベスト プラクティス アナライザー ツールで表示される UI テキストを示します。  
  
## <a name="issue"></a>問題  
*ページングファイルはレプリケーションに参加しないようにする必要がありますが、ディスクは除外されていません。*  
  
## <a name="impact"></a>影響  
@no__t 0Paging ファイルを使用すると、大量の入出力アクティビティが発生し、レプリケーションに参加するために必要なリソースが不必要に増えることがあります。これは、次の仮想マシンに影響します: *  
  
@no__t-仮想マシンの > の一覧  
  
## <a name="resolution"></a>解決方法  
*If ページングファイル用に別の仮想ハードディスクを作成します (まだ作成していない場合)。初期レプリケーションは既に完了して場合、は、HYPER-V マネージャーを使用して、レプリケーションを削除します。次に、レプリケーションをもう一度構成し、ページングファイルを含むバーチャルハードディスクをレプリケーションから除外します。*  
  


