---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: AD FS 設計計画の実装
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6306b87dd06774bfde5ffc3ff98818d47d0c858f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408378"
---
# <a name="implementing-your-ad-fs-design-plan"></a>AD FS 設計計画の実装

次の環境の条件と要件は、Active Directory フェデレーションサービス (AD FS) \(AD FS @ no__t 設計計画の実装において重要な要素です。  
  
-   **サポートされているパートナー:** 通常、パートナー組織を操作するには AD FS を使用します。 Id フェデレーションを確立するには、パートナーシップを作成する組織を決定します。 ベースライン AD FS の展開が整ったら、パートナーと協力して、パートナーの追加、パートナーの削除、およびパートナー情報の更新を行います。 パートナーシップに対する変更は、さまざまな理由で発生する可能性があります。 たとえば、パートナーがビジネスを大幅に変更した場合、組織が大規模な組織または組織のフェデレーションの一部になった場合、または組織が別の組織によって買収された場合に、AD FS の展開でパートナーシップの更新が必要になることがあります。全社. 複数のドメインから id をフェデレーションするシナリオでは、現在サポートしているドメイン @no__t 0partners @ no__t-1、および潜在的なパートナーを表すすべての追加ドメインを把握しておく必要があります。  
  
-   **サポートされているアプリケーションとサービスの種類:** アプリケーションやサービスによっては、オペレーティングシステムリソースへのアクセスが必要になる場合があります。 管理要件を作成するために AD FS がサポートするアプリケーションとサービスの種類を理解しておくことが重要です。  
  
-   **論理アーキテクチャと物理アーキテクチャの図、またはデプロイトポロジ:** 次のことを把握しておく必要があります。  
  
    -   フェデレーションサーバーが、一連のファームサーバーまたは1台のサーバーで機能するかどうか。  
  
    -   ネットワークでファイアウォールとプロキシを展開します。  
  
    -   リソースの場所と、ユーザーが組織内、組織外、またはその両方からリソースにアクセスしているかどうか。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>このガイドを使用して AD FS 設計を実装する方法  
設計を実装するための次の手順は、各配置タスクをどのように実行するかを決定することです。 このガイドでは、チェックリストを使用して、設計計画を実装するために必要となるさまざまなサーバーやアプリケーションの展開タスクを順を追って進めることができます。 親チェックリストと子チェックリストは、特定の AD FS 設計のタスクを処理する必要がある順序を表すために必要に応じて使用されます。  
  
ガイドのこのセクションでは、次の親チェックリストを使用して、組織の優先 AD FS 設計を実装するための展開タスクに慣れることができます。  
  
-   [チェックリスト:Web SSO 設計の実装](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [チェックリスト:フェデレーション Web SSO 設計の実装](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
