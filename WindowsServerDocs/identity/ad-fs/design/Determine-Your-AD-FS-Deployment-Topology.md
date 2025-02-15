---
ms.assetid: f67b0bc9-e5af-4891-9da0-d9be539af42d
title: AD FS 展開トポロジの決定
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b9128dded44e83acc63cef6785a1949e614cf6a7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408116"
---
# <a name="determine-your-ad-fs-deployment-topology"></a>AD FS 展開トポロジの決定

Active Directory フェデレーションサービス (AD FS) \(AD FS @ no__t の展開を計画するための最初の手順は、組織の \(SSO @ no__t のニーズに対して、シングルサインオンを満たすための適切な展開トポロジを決定することです。 このセクションのトピックでは、AD FS で使用できるさまざまな展開トポロジについて説明します。 また、特定のビジネスのニーズに最適なトポロジを選択できるよう、各展開トポロジに関連する利点と制約事項についても説明します。  
  
この展開トポロジに関するトピックを読む前に、まず次の表に示したタスクを記載された順に実行することをお勧めします。  
  
|推奨されるタスク|説明|参照|  
|--------------------|---------------|-------------|  
|フェデレーションサーバーファーム内の他のフェデレーションサーバーに AD FS データを格納し、レプリケートする方法を確認します。|AD FS 構成データベースに保存されている基になるデータの目的と使用可能なレプリケーション方法について理解します。 このトピックでは、構成データベースの概念を紹介し、2 種類のデータベースWindows Internal Database \(WID @ no__t-1 および Microsoft SQL Server。|[AD FS 構成データベースの役割](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)|  
|組織で展開する AD FS 構成データベースの種類を選択する。|AD FS 構成データベースとして WID または SQL Server のどちらかを使用することに関連するさまざまな利点および制約事項と、データベースでサポートされるさまざまな利用シナリオを確認します。|[AD FS 展開トポロジに関する考慮事項](AD-FS-Deployment-Topology-Considerations.md)|  
  
> [!NOTE]  
> 基本的な冗長性と負荷分散を実装し、必要に応じてフェデレーションサービス @no__t をスケーリングするオプション @ no__t を実装するには、すべての運用環境でフェデレーションサーバーファームごとに少なくとも2台のフェデレーションサーバーを展開することをお勧めします。使用するデータベースの種類。  
  
前述の表の内容を確認したら、このセクションの次のトピックに進んでください。  
  
-   [WID を使用するスタンドアロン フェデレーション サーバー](Stand-Alone-Federation-Server-Using-WID.md)  
  
-   [WID を使用するフェデレーション サーバー ファーム](Federation-Server-Farm-Using-WID-2012.md)  
  
-   [WID とプロキシを使用するフェデレーション サーバー ファーム](Federation-Server-Farm-Using-WID-and-Proxies-2012.md)  
  
-   [SQL Server を使用するフェデレーション サーバー ファーム](Federation-Server-Farm-Using-SQL-Server-2012.md)  
  
AD FS 展開トポロジの選択が完了したら、トピック「 [AD FS サーバー容量の計画](Planning-for-AD-FS-Server-Capacity.md)」を参照して、このトポロジをサポートするために展開する必要があるサーバーの推奨数を決定することをお勧めします。  
  
## <a name="see-also"></a>関連項目
[Windows Server 2012 での AD FS 設計ガイド](AD-FS-Design-Guide-in-Windows-Server-2012.md)

