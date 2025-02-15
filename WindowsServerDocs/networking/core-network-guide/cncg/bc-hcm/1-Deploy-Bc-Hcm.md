---
title: BranchCache ホスト型キャッシュ モードを展開する
description: このガイドでは、Windows Server 2016 と Windows 10 を実行するコンピューターに、ホスト型キャッシュモードで BranchCache を展開する手順について説明します。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 49e74132dba2909b7e5b639c95ef50064cf23e8c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356382"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>BranchCache ホスト型キャッシュ モードを展開する

>適用対象:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server 2016 コアネットワークガイドでは、完全に機能するネットワークに必要なコアコンポーネントを計画および展開する手順について説明し、新しいフォレストに新しい Active Directory @ no__t ドメインを追加します。

このガイドでは、クライアントコンピューターが Windows @ no__t-1 10 Windows 8.1 を実行している読み取り @ no__t-0Only ドメインコントローラーを使用して、1つまたは複数のブランチオフィスにホスト型キャッシュモードで BranchCache を展開する方法について説明します。、、または Windows 8、およびはドメインに参加しています。

>[!IMPORTANT]
>Windows Server 2008 R2 を実行している BranchCache ホスト型キャッシュサーバーを展開する場合、または既に展開している場合は、このガイドを使用しないでください。 このガイドでは、Windows Server @ no__t-0 2016、Windows Server 2012 R2、または Windows Server 2012 を実行しているホスト型キャッシュサーバーを使用して、ホスト型キャッシュモードを展開する手順について説明します。

このガイドは次のセクションで構成されます。

- [このガイドを使用するための前提条件](#bkmk_pre)

- [このガイドについて](#bkmk_about)

- [このガイドで説明されていないもの](#bkmk_not)

- [テクノロジの概要](#bkmk_tech)

- [BranchCache ホスト型キャッシュモードの展開の概要](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache ホスト型キャッシュモードの展開計画](3-Bc-Hcm-Plan.md)

- [BranchCache ホスト型キャッシュモードの展開](4-Bc-Hcm-Deployment.md)

- [その他のリソース](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>このガイドを使用するための前提条件

これは、Windows Server 2016 コアネットワークガイドの必携ガイドです。 このガイドに従ってホスト型キャッシュ モードで BranchCache をデプロイするには、先に次のことを行う必要があります。

- コア ネットワーク ガイドを使用してメイン オフィスにコア ネットワークをデプロイするか、コア ネットワーク ガイドで説明されているテクノロジが既にネットワーク上に正しくインストールされて機能している必要があります。 これらのテクノロジには、TCP @ no__t-0IP v4、DHCP、Active Directory Domain Services \( AD DS @ no__t、DNS などがあります。

    > [!NOTE]
    > Windows Server 2016[コアネットワークガイド](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)は、windows Server 2016 テクニカルライブラリから入手できます。  

- メインオフィスまたはクラウドデータセンターで、windows Server 2016、Windows Server 2012 R2、または Windows Server 2012 を実行する BranchCache コンテンツサーバーを展開します。 BranchCache コンテンツサーバーを展開する方法の詳細については、「[その他のリソース](11-Bc-Hcm-additional-resources.md)」を参照してください。

- 仮想プライベートネットワーク \(VPN @ no__t、DirectAccess、またはその他の接続方法を使用して、ブランチオフィス、メインオフィス、必要に応じてクラウドリソースの間で、ワイドエリアネットワーク @no__t 0WAN @ no__t 接続を確立します。

- 次のオペレーティングシステムのいずれかを実行しているブランチオフィスのクライアントコンピューターを展開します。これにより、バックグラウンドインテリジェント転送サービス (BITS)、ハイパーテキスト転送プロトコル (HTTP)、およびサーバーメッセージブロック (SMB) をサポートする BranchCache が提供されます.
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

> [!NOTE]
> 次のオペレーティングシステムでは、BranchCache は HTTP および SMB 機能をサポートしていませんが、BranchCache BITS 機能はサポートしています。
>     - Windows 10 Pro、BITS サポートのみ
>     - Windows 8.1 Pro、BITS サポートのみ
>     - Windows 8 Pro、BITS サポートのみ

## <a name="bkmk_about"></a>このガイドについて

このガイドは、Windows Server 2016 コアネットワークガイドまたは Windows Server 2012 コアネットワークガイドに記載されている手順に従ってコアネットワークを展開するネットワーク管理者またはシステム管理者向けに設計されています。コアネットワークガイドに含まれるテクノロジには、Active Directory Domain Services \(AD DS @ no__t、Domain Name Service \(DNS @ no__t、動的ホスト構成プロトコル \(DHCP @ no__t-5、TCP @ no__t-6IP v4 などがあります。

このデプロイ シナリオで使用される各テクノロジについては、それぞれの設計ガイドやデプロイ ガイドを参照することをお勧めします。 これらのガイドは、このデプロイ シナリオが組織のネットワークに必要なサービスおよび構成を提供するかどうかを判断するのに役立ちます。

## <a name="bkmk_not"></a>このガイドで提供されていないもの

このガイドでは、BranchCache のモードと機能に関する情報など、BranchCache の概念に関する情報は提供しません。  

このガイドでは、WAN 接続またはブランチ オフィス内の他のテクノロジ (DHCP、RODC、VPN サーバーなど) をデプロイする方法についての情報は提供しません。

さらに、このガイドではホスト型キャッシュ サーバーをデプロイするときに使用するハードウェアについてのガイダンスは提供しません。 ホスト型キャッシュ サーバーで他のサービスやアプリケーションを実行することもできますが、ワークロード、ハードウェアの機能、およびブランチ オフィスの規模に基づいて、特定のコンピューターに BranchCache ホスト型キャッシュ サーバーをインストールするかどうか、およびキャッシュに割り当てるディスク領域の量を、判断する必要があります。  
このガイドでは、Windows 7 を実行しているコンピューターを構成する手順については説明しません。 ブランチオフィスで Windows 7 を実行しているクライアントコンピューターがある場合は、Windows 10、Windows 8.1、および Windows 8 を実行しているクライアントコンピューターの場合とは異なる手順を使用して構成する必要があります。
  
さらに、Windows 7 を実行しているコンピューターがある場合は、クライアントコンピューターが信頼する証明機関によって発行されたサーバー証明書を使用して、ホスト型キャッシュサーバーを構成する必要があります。 \( すべてのクライアントコンピューターで Windows 10、Windows 8.1、または Windows 8 が実行されている場合は、サーバー証明書を使用してホスト型キャッシュサーバーを構成する必要はありません。 \) 
> [!IMPORTANT]
> ホスト型キャッシュサーバーで Windows Server 2008 R2 が実行されている場合は、このガイドではなく Windows Server 2008 R2 [Branchcache 展開ガイド](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx)を使用して、ホスト型キャッシュモードで branchcache を展開します。 このガイドで説明されているグループポリシー設定を、windows 7 から windows 10 に対して windows のバージョンを実行しているすべての BranchCache クライアントに適用します。 このガイドの手順を使用して、Windows Server 2008 R2 を実行しているコンピューターを構成することはできません。

## <a name="bkmk_tech"></a>テクノロジの概要

この必携ガイドでインストールして構成する必要のあるテクノロジは、BranchCache だけです。 Web サーバーやファイル サーバーなどのコンテンツ サーバーで Windows PowerShell の BranchCache コマンドを実行する必要がありますが、それ以外の変更や構成をコンテンツ サーバーで行う必要はありません。 さらに、Windows Server 2016、Windows Server 2012 R2、または Windows Server 2012 で AD DS を実行しているドメインコントローラーでグループポリシーを使用して、クライアントコンピューターを構成する必要があります。

### <a name="branchcache"></a>BranchCache

BranchCache は、windows Server 2016 および Windows 10 オペレーティングシステムの一部のエディション、および Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8 の一部のエディションに組み込まれているワイドエリアネットワーク (WAN) 帯域幅最適化テクノロジです。、Windows Server 2008 R2、および Windows 7。

ユーザーがリモートサーバー上のコンテンツにアクセスするときに WAN 帯域幅を最適化するために、BranchCache はクライアントから要求されたコンテンツをメインオフィスまたはホストされているクラウドコンテンツサーバーからダウンロードし、ブランチオフィスの場所にキャッシュして、他のクライアントコンピューターにWAN 経由ではなく、ローカルで同じコンテンツにアクセスするブランチオフィス。

ホスト型キャッシュ モードで BranchCache をデプロイするときは、ブランチ オフィスのクライアント コンピューターをホスト型キャッシュ モード クライアントとして構成した後、ホスト型キャッシュ サーバーをブランチ オフィスにデプロイする必要があります。 このガイドでは、Web およびファイルサーバー @ no__t ベースのコンテンツサーバーから事前ハッシュされ、事前に読み込まれたコンテンツを使用して、ホスト型キャッシュサーバーをデプロイする方法を示します。

### <a name="group-policy"></a>グループ ポリシー

グループポリシー Windows Server 2016、Windows Server 2012 R2、および Windows Server 2012 は、1つまたは複数の必要な構成またはポリシー設定を、Active Directory 環境内の対象ユーザーとコンピューターのセットに適用するために使用されるインフラストラクチャです。 

このインフラストラクチャは、グループポリシーエンジンと、対象のクライアントコンピューターのポリシー設定の読み取りを行う、\(CSEs @ no__t の複数のクライアント @ no__t 拡張機能で構成されています。

このシナリオでのグループ ポリシーは、BranchCache ホスト型キャッシュ モードでドメイン メンバー クライアント コンピューターを構成するために使用されます。

このガイドを続行するには、「 [BranchCache ホスト型キャッシュモードの展開の概要](2-Bc-Hcm-Deploy-Overview.md)」を参照してください。
