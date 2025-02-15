---
ms.assetid: 02580b2f-a339-4470-947c-d700b2d55f3f
title: フェデレーション サーバー ファームを作成するのに適した状況
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fcfc7d640d3688bf0e23557af9bd56082418ef37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358945"
---
# <a name="when-to-create-a-federation-server-farm"></a>フェデレーション サーバー ファームを作成するのに適した状況

より大規模な AD FS デプロイを使用していて、フォールトトレランスを提供したり、@ no__t-2balancing を組織のフェデレーションにロードしたりする場合は、Active Directory フェデレーションサービス (AD FS) @no__t 0AD FS @ でフェデレーションサーバーファームを作成することを検討してください。処理. 同じネットワーク内に2台以上のフェデレーションサーバーを作成し、それぞれを同じフェデレーションサービスを使用するように構成し、各サーバーのトークン @ no__t-0signing 証明書の公開キーを AD FS Management snap @ no__t に追加します。フェデレーションサーバーファーム。  
  
AD FS フェデレーションサーバー構成ウィザードを使用して、フェデレーションサーバーファームを作成するか、既存のファームに追加のフェデレーションサーバーをインストールすることができます。 詳細については、「 [When to Create a Federation Server](When-to-Create-a-Federation-Server.md)」を参照してください。  
  
> [!NOTE]  
> AD FS フェデレーションサーバー構成ウィザードを使用して**新しいフェデレーションサーバーファーム**を作成するオプションを選択した場合、ウィザードは Active Directory の証明書を共有するためのコンテナーオブジェクト \( を作成しようとします。 そのため、最初に、Active Directory においてコンテナー オブジェクトを作成するのに十分なアクセス許可を持つアカウントを使用してコンピューターにログオンし、フェデレーション サーバー ロールを設定することが重要です。  
  
フェデレーションサーバーをファームとしてグループ化するには、まず、1つの完全修飾ドメイン名に到着した要求がサーバーファーム内のさまざまなフェデレーションサーバーにルーティングされるように、そのサーバーをクラスター化しておく必要があります。1つの完全修飾ドメイン名 \(FQDN @ no__t-1 を指定します。 サーバークラスターを作成するには、ネットワーク負荷分散 \(NLB @ no__t-1 を企業ネットワーク内に展開します。 このガイドでは、ファーム内の各フェデレーションサーバーをクラスター化するために NLB が適切に構成されていることを前提としています。  
  
Microsoft NLB テクノロジを使用してクラスターの FQDN を構成する方法の詳細については、「[クラスターパラメーターの指定](https://go.microsoft.com/fwlink/?LinkID=74651)」を参照してください。  
  
## <a name="best-practices-for-deploying-a-federation-server-farm"></a>フェデレーション サーバー ファームのデプロイのベスト プラクティス  
運用環境にフェデレーションサーバーを展開する場合は、次のベストプラクティスに従うことをお勧めします。  
  
-   同時に複数のフェデレーションサーバーを展開する場合、またはファームにサーバーをさらに追加することがわかっている場合は、ファーム内の既存のフェデレーションサーバーのサーバーイメージを作成してから、cr が必要になったときにそのイメージからインストールすることを検討してください。追加のフェデレーションサーバーを迅速にします。  
  
    > [!NOTE]  
    > 追加のフェデレーションサーバーを展開するためにサーバーイメージの方法を使用する場合、@no__t のチェックリストのタスクを実行する必要はありません。新しいサーバーをファームに追加するたびに、フェデレーションサーバー @ no__t-0 を設定します。  
  
-   NLB または他の何らかのクラスタリング方式を使用して、多くのフェデレーションサーバーコンピューターに単一の IP アドレスを割り当てることができます。  
  
-   ファーム内の各フェデレーションサーバーに対して静的 IP アドレスを予約し、ドメインネームシステム \(DNS @ no__t の構成に応じて、動的ホスト構成プロトコル \(DHCP @ no__t-3 に各 IP アドレスの除外を挿入します。 Microsoft の NLB テクノロジでは、NLB クラスターに参加する各サーバーに静的 IP アドレスを割り当てる必要があります。  
  
-   AD FS 構成データベースを SQL データベースに格納する場合は、複数のフェデレーションサーバーから SQL データベースを同時に編集しないでください。  
  
## <a name="configuring-federation-servers-for-a-farm"></a>ファーム用のフェデレーション サーバーの構成  
次の表では、各フェデレーションサーバーがファーム環境に参加できるようにするために完了する必要があるタスクについて説明します。  
  
|タスク|説明|  
|--------|---------------|  
|AD FS 構成データベースを SQL Server に置く場合|フェデレーションサーバーファームは、同じ AD FS 構成データベースを共有する2つ以上のフェデレーションサーバーと、トークン @ no__t-0signing 証明書で構成されます。 構成データベースは、Windows Internal Database または SQL Server データベースに置くことができます。 構成データベースを SQL データベースに格納する場合は、ファームに参加しているすべての新しいフェデレーションサーバーからアクセスできるように、構成データベースにアクセスできることを確認してください。 **注:** ファームのシナリオでは、構成データベースが、そのファームにフェデレーションサーバーとして参加していないコンピューターに配置されていることが重要です。 Microsoft NLB では、ファームに参加しているコンピューターが相互に通信することはできません。 **注:** ファームに参加しているすべてのフェデレーションサーバーでインターネットインフォメーションサービス \(IIS @ no__t @ no__t にある AD FS AppPool の id が、構成データベースに対する読み取りアクセス権を持っていることを確認します。|  
|証明書を取得して共有する|1つのサーバー認証証明書は、公共の証明機関 \(CA @ no__t-1 (VeriSign など) から取得できます。 その後、すべてのフェデレーションサーバーが証明書の同じ秘密キー部分を共有するように、証明書を構成できます。 同じ証明書を共有する方法の詳細については、「@no__t」チェックリストを参照してください。フェデレーションサーバー @ no__t をセットアップしています。 **注:** AD FS Management snap @ no__t-0in は、サービス通信証明書としてフェデレーションサーバーのサーバー認証証明書を参照します。<br /><br />詳細については、「 [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md)」を参照してください。|  
|同じ SQL Server インスタンスを参照する|AD FS 構成データベースを SQL database に格納する場合、新しいフェデレーションサーバーは、ファーム内の他のフェデレーションサーバーで使用されているのと同じ SQL Server インスタンスを参照する必要があります。これにより、新しいサーバーがファームに参加できるようになります。|  
  
## <a name="see-also"></a>関連項目
[Windows Server 2012 での AD FS 設計ガイド](AD-FS-Design-Guide-in-Windows-Server-2012.md)
