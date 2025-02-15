---
ms.assetid: e7f9e518-2d5d-4a0d-9147-34e1304f42ac
title: チェックリスト-AD FS 1.x からの要求を使用するように AD FS を構成する
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1c952abc0bca5eadbfc14f3eda54d05826d50294
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408485"
---
# <a name="checklist-configuring-ad-fs--to-consume-claims-from-ad-fs-1x"></a>チェックリスト:AD FS 1.x からの要求を使用するように AD FS を構成する

  
## <a name="checklist-configuring-ad-fs-to-consume-claims-from-adfs1x"></a>チェックリスト:AD FS 1.x からの要求を使用するように AD FS を構成する  
このチェックリストには、Windows Server 2012 の Active Directory フェデレーションサービス (AD FS) \(AD FS @ no__t-1 フェデレーションサービスを構成するために必要なタスクが含まれており、AD FS 1 によって送信された要求を使用します。*x*フェデレーションサービス。  
  
> [!NOTE]  
> このチェックリストのタスクは順番に実行してください。 参照リンクによって手順に移動した場合は、このチェックリストの残りのタスクを進めることができるように、その手順の作業が完了したらこのトピックに戻ります。  
  
![consume @ no__t チェックリストから要求を使用します。AD FS 1.x @ no__t からの要求を使用するように AD FS を構成しています  
  
||タスク|参照|  
|-|--------|-------------|  
|![AD FS からの要求を使用する](media/icon_checkboxo.gif)|Windows Server 2012 と以前のバージョンの AD FS の AD FS 間の相互運用性を計画し、Name ID 要求の種類の詳細について説明します。|@no__t は、AD FS 1.x との](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[相互運用性のために AD FS 計画](https://technet.microsoft.com/library/ff678040.aspx)の要求を使用します。|  
|![AD FS からの要求を使用する](media/icon_checkboxo.gif)|以前のバージョンの AD FS と相互運用するには、まず AD FS フェデレーションサービスで要求プロバイダー信頼を作成する必要があります。 **注:** AD FS 1 を持つ信頼を作成することはできません。*x*フェデレーションサービスフェデレーションメタデータを使用します。<br /><br />右側のリンクにある手順を使用して信頼を設定する場合は、要求プロバイダー信頼の追加ウィザードで次の操作を行って、AD FS 1 と相互運用するようにこの信頼を設定する必要があります。*x*フェデレーションサービス:<br /><br />1. **[データソースの選択]** ページで、 **[証明書利用者の信頼に関するデータを手動で入力]** する を選択します。<br />2. **[プロファイルの選択]** ページで、 **[AD FS 1.0 および1.1 プロファイル]** を選択します。<br />3. **[URL の構成]** ページの **[WS @ No__t-2FEDERATION パッシブ URL]** に、AD FS 1 で定義されている**フェデレーションサービスエンドポイント URL**を入力します。パートナーの*x*フェデレーションサービス。<br />4。 **[識別子の構成]** ページの **[要求プロバイダー信頼の識別子]** に、AD FS 1 で定義されている**フェデレーションサービス URI**を入力します。パートナーの*x*フェデレーションサービス。|@no__t は AD FS 要求](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[プロバイダー信頼を手動で作成し](../../ad-fs/operations/Create-a-Claims-Provider-Trust.md)ます。|  
|![AD FS からの要求を使用する](media/icon_checkboxo.gif)|先ほど作成した要求プロバイダー信頼では、AD FS 1.x フェデレーションサービスから受信した要求を取得し、それらをパススルー、フィルター処理、または名前 ID 要求の種類に変換する要求規則を作成する必要があります。<br /><br />名前 ID 要求の種類が渡され、フィルター処理または変換された場合、Windows Server 2012 の AD FS フェデレーションサービスによって認識および使用できるように、別の規則または規則の入力として使用できます。|@no__t は AD FS からの要求を使用して](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[AD FS 1. x 互換の要求を送信する規則を作成します](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)|  
|![AD FS からの要求を使用する](media/icon_checkboxo.gif)|AD FS 1 の管理者に連絡してください。*x*フェデレーションサービス、AD FS 1 の管理者がいます。*x*フェデレーションサービス新しいリソースパートナーの信頼を設定します。 また、管理者に対して、フェデレーションサービスプロパティ @ no__t、フェデレーションサービスエンドポイント URL、およびエクスポートされたトークン @ no__t-2signing 証明書 @no__t ファイルのフェデレーションサービス @no__t URI を指定し、公開キーのみ @ no__t を指定します。 信頼を設定するには、管理者がこれらの項目を必要とします。|N\/A|  
  

