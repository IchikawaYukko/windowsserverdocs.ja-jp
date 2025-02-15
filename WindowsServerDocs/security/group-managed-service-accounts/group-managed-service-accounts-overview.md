---
title: Group Managed Service Accounts Overview
description: Windows Server のセキュリティ
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cef0693c-f861-48a7-a1c0-8d1bc06143ce
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 4e7f46739dd8def6ffc34c6cc50210c0e6999c79
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403745"
---
# <a name="group-managed-service-accounts-overview"></a>Group Managed Service Accounts Overview

>適用先:Windows Server (半期チャネル)、Windows Server 2016

IT 担当者向けのこのトピックでは、実際のアプリケーション、Microsoft の実装の変更点、およびハードウェアとソフトウェアの要件について説明することで、グループの管理されたサービスアカウントについて説明します。


## <a name="BKMK_OVER"></a>機能の説明
スタンドアロンの管理されたサービスアカウント (sMSA) は、パスワードの自動管理、簡略化されたサービスプリンシパル名 (SPN) の管理、および他の管理者に管理を委任する機能を提供する、管理されたドメインアカウントです。 この種類の管理されたサービスアカウント (MSA) は、Windows Server 2008 R2 と Windows 7 で導入されました。

グループの管理されたサービスアカウント (gMSA) は、ドメイン内で同じ機能を提供しますが、その機能を複数のサーバーに拡張することもできます。 ネットワーク負荷分散ソリューションなど、サーバーファームでホストされているサービスに接続する場合、相互認証をサポートする認証プロトコルでは、サービスのすべてのインスタンスが同じプリンシパルを使用する必要があります。 GMSA がサービスプリンシパルとして使用されている場合、Windows オペレーティングシステムは、管理者に依存してパスワードを管理するのではなく、アカウントのパスワードを管理します。

-0kdssvc @ no__t-1 という Microsoft キー配布 @no__t サービスは、最新のキーまたは特定のキーを、Active Directory アカウントのキー識別子を使用して安全に取得するためのメカニズムを提供します。 キー配布サービスは、アカウントのキーの作成に使用されるシークレットを共有します。 これらのキーは定期的に変更されます。 GMSA の場合、ドメインコントローラーは、gMSA の他の属性に加えて、キー配布サービスによって提供されるキーのパスワードを計算します。  メンバーホストは、ドメインコントローラーに接続することにより、現在と以前のパスワード値を取得できます。

## <a name="BKMK_APP"></a>実用的なアプリケーション
gMSAs は、サーバーファームまたはネットワーク Load Balancer の背後にあるシステムで実行されているサービスに対して単一の id ソリューションを提供します。 GMSA ソリューションを提供することで、新しい gMSA プリンシパル用にサービスを構成でき、パスワード管理が Windows によって処理されます。

GMSA、サービス、またはサービス管理者は、サービスインスタンス間のパスワード同期を管理する必要はありません。 GMSA は、長時間にわたってオフラインになっているホストと、サービスのすべてのインスタンスのメンバーホストの管理をサポートしています。 つまり、既存のクライアント コンピューターが接続先のサービス インスタンスを把握しなくても認証できる単一 ID がサポートされたサーバー ファームを展開できます。

フェールオーバー クラスターでは、gMSA をサポートしていません。 ただし、クラスター サービスの上位で実行されるサービスは、それが Windows サービス、アプリケーション プール、またはスケジュールされたタスクである場合、あるいは gMSA/sMSA をネイティブにサポートしている場合、gMSA または sMSA を使用できます。

## <a name="BKMK_SOFT"></a>ソフトウェア要件

GMSAs の管理に使用される Windows PowerShell コマンドを実行するには、64 @ no__t-0bit アーキテクチャが必要です。

管理されたサービス アカウントは、Kerberos でサポートされる暗号化の種類によって決まります。Kerberos を使用するサーバーに対してクライアント コンピューターが認証されると、DC とサーバーの両方でサポートされる暗号化で保護される Kerberos サービスが DC で作成されます。 DC は、アカウントの no__t-0SupportedEncryptionTypes 属性を使用して、サーバーでサポートされる暗号化を判断します。属性がない場合は、クライアントコンピューターがより強力な暗号化の種類をサポートしていないことを前提としています。 ホストが RC4 をサポートしないように構成されている場合、認証は常に失敗します。 このため、AES は常に MSA 用に明示的に構成する必要があります。

> [!NOTE]
> Windows Server 2008 R2 以降、既定では、DES が無効になっています。 サポートされる暗号化の種類の詳細については、「 [Kerberos 認証の変更点](https://technet.microsoft.com/library/dd560670(WS.10).aspx)」を参照してください。

gMSAs は、windows Server 2012 より前の Windows オペレーティングシステムには適用されません。

## <a name="server-manager-information"></a>サーバー マネージャー情報
サーバーマネージャーまたは Install @ no__t-0WindowsFeature コマンドレットを使用して MSA と gMSA を実装するために必要な構成手順はありません。

## <a name="BKMK_LINKS"></a>関連項目
次の表に、管理されたサービス アカウントとグループの管理されたサービス アカウントに関連する追加リソースへのリンクを掲載します。

|コンテンツの種類|参考資料|
|--------|-------|
|**製品評価**|[管理されたサービスアカウントの新機能](what-s-new-for-managed-service-accounts.md)<br /><br />[Windows 7 および Windows Server 2008 R2 の管理されたサービスアカウントのドキュメント](https://technet.microsoft.com/library/ff641731(v=ws.10).aspx)<br /><br />[サービスアカウント手順 @ no__t-1by @ no__t-2Step Guide](https://technet.microsoft.com/library/dd548356(v=ws.10).aspx)|
|**計画**|現在は入手不能|
|**展開**|現在は入手不能|
|**運用**|[Active Directory の管理されたサービスアカウント](https://technet.microsoft.com/library/dd378925(v=ws.10).aspx)|
|**トラブルシューティング**|現在は入手不能|
|**用**|[グループの管理されたサービス アカウントの概要](getting-started-with-group-managed-service-accounts.md)|
|**ツールと設定**|[Active Directory Domain Services の管理されたサービスアカウント](https://technet.microsoft.com/library/dd378925(v=WS.10).aspx)|
|**コミュニティ リソース**|@no__t 0 で管理されるサービスアカウント:@ No__t の理解、実装、ベストプラクティス、およびトラブルシューティング|
|**関連テクノロジ**|[Active Directory Domain Services の概要](active-directory-domain-services-overview.md)|


