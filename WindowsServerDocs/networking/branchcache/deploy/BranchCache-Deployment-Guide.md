---
title: BranchCache 展開ガイド
description: このトピックは、BranchCache 展開ガイドの Windows Server 2016、ブランチ オフィスに WAN 帯域幅使用量を最適化するために分散され、ホスト型キャッシュ モードで BranchCache を展開する方法を示しますの一部
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 3830b356-36d3-44f9-a1d7-990ff3e57403
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 14eb9e5b4d5a28a64d3cfa0d27b5294ba7168da9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356730"
---
# <a name="branchcache-deployment-guide"></a>BranchCache 展開ガイド

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

このガイドでは、Windows Server 2016 で BranchCache を展開する方法について説明します。  
  
このトピックに加えて、このガイドには次のセクションが含まれています。  
  
-   [BranchCache 設計の選択](../../branchcache/plan/Choosing-a-BranchCache-Design.md)  
  
-   [BranchCache を展開する](../../branchcache/deploy/Deploy-BranchCache.md)  
  
## <a name="branchcache-deployment-overview"></a>BranchCache の展開の概要

BranchCache は、Windows Server 2016、Windows Server @ no__t-0 2012 R2、Windows Server @ no__t-1 2012、Windows Server @ no__t 2008 R2、および関連する Windows クライアントに搭載されているワイドエリアネットワーク (WAN) 帯域幅最適化テクノロジです。オペレーティングシステム。  
  
WAN の帯域幅を最適化するため、BranchCache ではメイン オフィスのコンテンツ サーバーからコンテンツがコピーされ、ブランチ オフィスの場所にキャッシュされます。これにより、ブランチ オフィスのクライアント コンピューターは WAN を経由せずにローカルにコンテンツにアクセスできます。  
  
ブランチオフィスでは、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2 の BranchCache 機能を実行しているサーバーにコンテンツがキャッシュされます。また、ブランチオフィスでサーバーが使用できない場合は、コンテンツが cac になります。Windows 10 @ no__t-0、Windows @ no__t-1 8.1、windows 8、または Windows 7 @ no__t を実行しているクライアントコンピューターでの hed。  
  
クライアントコンピューターがメインオフィスまたはクラウドデータセンターのコンテンツを要求して受信し、コンテンツがブランチオフィスにキャッシュされた後、同じブランチオフィスにある他のコンピューターは、コンテンツサーバーに接続するのではなく、コンテンツをローカルに取得できます。WAN リンク。  
  
**BranchCache を展開する利点**  
  
BranchCache は、ブランチオフィスの場所にファイル、web、およびアプリケーションのコンテンツをキャッシュすることで、低速 WAN 接続を介してコンテンツにアクセスするのではなく、クライアントコンピューターがローカルエリアネットワーク (LAN) を使用してデータにアクセスできるようにします。  
  
BranchCache は、WAN トラフィックと、ブランチオフィスのユーザーがネットワーク上のファイルを開くために必要な時間の両方を削減します。  BranchCache は常に最新のデータをユーザーに提供し、ホスト型キャッシュサーバーとクライアントコンピューターのキャッシュを暗号化することによってコンテンツのセキュリティを保護します。  
  
### <a name="what-this-guide-provides"></a>このガイドの内容  
この展開ガイドでは、次のモードで BranchCache を展開できます。  
  
-   分散キャッシュモード。 このモードでは、ブランチオフィスのクライアントコンピューターは、メインオフィスまたはクラウドのコンテンツサーバーからコンテンツをダウンロードし、同じブランチオフィスにある他のコンピューターのコンテンツをキャッシュします。 分散キャッシュモードでは、ブランチオフィスのサーバーコンピューターは必要ありません。  
  
-   ホスト型キャッシュモード。 このモードでは、ブランチオフィスのクライアントコンピューターがメインオフィスまたはクラウドのコンテンツサーバーからコンテンツをダウンロードし、ホスト型キャッシュサーバーがクライアントからコンテンツを取得します。 ホスト型キャッシュサーバーは、他のクライアントコンピューターのコンテンツをキャッシュします。  
  
このガイドでは、3種類のコンテンツサーバーを展開する方法についても説明します。 コンテンツサーバーには、ブランチオフィスのクライアントコンピューターによってダウンロードされるソースコンテンツが含まれています。また、いずれかのモードで BranchCache を展開するには、1つまたは複数のコンテンツサーバーが必要です。 コンテンツサーバーの種類は次のとおりです。  
  
-   **Web サーバーベースのコンテンツサーバー**。 これらのコンテンツサーバーは、HTTP および HTTPS プロトコルを使用して、BranchCache クライアントコンピューターにコンテンツを送信します。 これらのコンテンツサーバーは、branchcache 機能がインストールされている Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2 バージョンを実行している必要があります。  
  
-   **BITS ベースのアプリケーションサーバー**。 これらのコンテンツサーバーは、バックグラウンドインテリジェント転送サービス (BITS) を使用して BranchCache クライアントコンピューターにコンテンツを送信します。 これらのコンテンツサーバーは、branchcache 機能がインストールされている Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2 バージョンを実行している必要があります。  
  
-   **ファイルサーバーベースのコンテンツサーバー**。 これらのコンテンツサーバーは、BranchCache をサポートし、ファイルサービスサーバーの役割がインストールされている windows server 2016、Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2 バージョンを実行している必要があります。 また、ファイルサービスサーバー役割の**ネットワークファイル用 BranchCache**役割サービスをインストールして構成する必要があります。 これらのコンテンツサーバーは、サーバーメッセージブロック (SMB) プロトコルを使用して、BranchCache クライアントコンピューターにコンテンツを送信します。  
  
詳細については、「[オペレーティングシステムのバージョン (BranchCache](https://technet.microsoft.com/windows-server-docs/networking/branchcache/branchcache#a-namebkmkosaoperating-system-versions-for-branchcache))」を参照してください。  
  
### <a name="branchcache-deployment-requirements"></a>BranchCache の展開要件

このガイドを使用して BranchCache を展開するための要件を次に示します。  
  
-   BranchCache 機能を提供するには、**ファイルサーバーと Web コンテンツサーバー**で次のオペレーティングシステムのいずれかを実行している必要があります。Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2。 Windows 8 以降のクライアントでは、Windows Server 2008 R2 を実行しているコンテンツサーバーにアクセスするときに BranchCache のメリットが引き続き表示されますが、windows server 2016、Windows Server 2012 で新しいチャンキングとハッシュテクノロジを利用することはできません。R2 および Windows Server 2012。  
  
-   最新の展開モデルを使用し、Windows Server 2012 で導入されたチャンキングとハッシュの機能強化を利用するには、**クライアントコンピューター**で windows 10、Windows 8.1、または windows 8 を実行している必要があります。  
  
-   **ホスト型キャッシュサーバー**では、windows server 2016、windows Server 2012 R2、または windows server 2012 が実行されている必要があります。これにより、このドキュメントで説明する展開の機能強化とスケールの機能が利用できます。  ホスト型キャッシュサーバーとして構成されているいずれかのオペレーティングシステムを実行しているコンピューターは、引き続き Windows 7 を実行しているクライアントコンピューターにサービスを提供できますが、そのためには、トランスポート層セキュリティ (TLS) に適した証明書を備えている必要があります。) を参照してください。詳細については、「Windows Server 2008 R2 および Windows 7 [BranchCache の展開ガイド](https://technet.microsoft.com/library/ee649232.aspx)」を参照してください。  
  
-   グループポリシーとホスト型キャッシュの自動検出を利用するには、 **Active Directory ドメイン**が必要ですが、ドメインは BranchCache を使用する必要はありません。  Windows PowerShell を使用して、個々のコンピューターを構成できます。 また、新しい BranchCache グループポリシー設定を利用するために、ドメインコントローラーで Windows Server 2012 以降を実行している必要はありません。BranchCache 管理用テンプレートは、以前のオペレーティングシステムを実行しているドメインコントローラーにインポートできます。また、Windows 10、Windows Server 2016、Windows 8.1 を実行している他のコンピューターでリモートからグループポリシーオブジェクトを作成することもできます。Windows Server 2012 R2、Windows 8、または Windows Server 2012。

-   **Active Directory サイト**は、自動的に検出されるホスト型キャッシュサーバーの範囲を制限するために使用されます。  ホスト型キャッシュサーバーを自動的に検出するには、クライアントとサーバーの両方のコンピューターが同じサイトに属している必要があります。 BranchCache は、クライアントとサーバーへの影響が最小限に抑えられるように設計されており、それぞれのオペレーティングシステムを実行するために必要な追加のハードウェア要件を課していません。  

**BranchCache の履歴とドキュメント**

BranchCache は、windows 7 @ no__t-0 および Windows Server @ no__t 2008 R2 で初めて導入され、Windows Server 2012、Windows 8、およびそれ以降のオペレーティングシステムで改善されました。

> [!NOTE]
> Windows Server 2016 以外のオペレーティングシステムで BranchCache を展開する場合は、次のドキュメントリソースを参照してください。
> 
> - Windows 8、Windows 8.1、Windows Server 2012、および Windows Server 2012 R2 の BranchCache の詳細については、「 [branchcache の概要](https://technet.microsoft.com/library/hh831696.aspx)」を参照してください。  
> - Windows 7 と Windows Server 2008 R2 の BranchCache の詳細については、「 [Windows server 2008 r2 の branchcache](https://technet.microsoft.com/library/dd996634.aspx)」を参照してください。  
  


