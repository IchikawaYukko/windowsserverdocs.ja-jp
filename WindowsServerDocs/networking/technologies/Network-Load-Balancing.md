---
title: ネットワーク負荷分散
description: このトピックでは、Windows Server 2016 のネットワーク負荷分散 @no__t 0NLB @ no__t 機能の概要について説明します。 NLB を使用して、単一の仮想クラスターとして 2 つ以上のサーバーを管理することができます。 NLB は、可用性となど、FTP web サイトで使用されるインターネット サーバー アプリケーションのスケーラビリティを向上、ファイアウォール、プロキシ、仮想プライベート ネットワーク \(VPN\), 、およびその他のミッション\-重要なサーバーです。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nlb
ms.topic: article
ms.assetid: 244a4b48-06e5-4796-8750-a50e4f88ac72
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: 4d79b6f29fbe64633bf04604ad586aff3dd86edf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405843"
---
# <a name="network-load-balancing"></a>ネットワーク負荷分散

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

このトピックでは、Windows Server 2016 のネットワーク負荷分散 @no__t 0NLB @ no__t 機能の概要について説明します。 NLB を使用して、単一の仮想クラスターとして 2 つ以上のサーバーを管理することができます。 NLB は、可用性となど、FTP web サイトで使用されるインターネット サーバー アプリケーションのスケーラビリティを向上、ファイアウォール、プロキシ、仮想プライベート ネットワーク \(VPN\), 、およびその他のミッション\-重要なサーバーです。  

> [!NOTE]
> Windows Server 2016 には、新しい Azure 性に優れたソフトウェア ロード バランサーが含まれています。 \(SLB\) ソフトウェア定義されているネットワークのコンポーネントとして \(SDN\) インフラストラクチャです。 使用する NLB ではなく SLB SDN を使用している場合を使用している Windows 以外のワークロード発信ネットワーク アドレス変換を必要な \(NAT\), 、またはレイヤー 3 が必要な \(L3\) または非 TCP ベースの負荷分散します。 SDN 以外の展開の Windows Server 2016 で NLB を使用する続行することができます。 SLB の詳細については、次を参照してください。 [ソフトウェア負荷分散 (SLB) SDN の](../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md)です。

ネットワーク負荷分散 \(NLB\) 機能、TCP を使用して複数のサーバーのトラフィックでは分散\/IP ネットワーク プロトコルです。 1 つの仮想クラスターにアプリケーションを実行している 2 つ以上のコンピューターを組み合わせることで NLB では、信頼性とパフォーマンスの web サーバーとその他のミッション\-重要なサーバーです。  
  
NLB クラスター内のサーバーは *ホスト*と呼ばれ、各ホストでは、サーバー アプリケーションの個別のコピーが実行されます。 送られてきたクライアント要求は NLB によってクラスター内の各ホストに配分されます。 各ホストで処理される負荷は構成可能です。 負荷の増加に対応して、クラスターにホストを動的に追加することもできます。 NLB では指定された 1 つのホストにすべてのトラフィックを送信することも可能です。このホストは *既定のホスト*と呼ばれます。  
  
NLB では、クラスター内のすべてのコンピューターに対し、同じ IP アドレス セットを使ってアドレス指定できます。このとき、各ホスト固有の専用 IP アドレス セットは維持されます。 負荷の\-ホストが失敗したかがオフラインになった、負荷が稼働しているコンピューター間で自動的に再配分ときに、アプリケーションを分散します。 オフラインのコンピューターで準備が整うと、そのコンピューターは透過的にクラスターに戻され、ワークロードの分担に加わるようになります。これにより、クラスター内の他のコンピューターのトラフィックが軽減されます。  
  
## <a name="practical-applications"></a>実際の適用例  
NLB はステートレスなことを確認するのに役立ちますインターネット インフォメーション サービスを実行する web サーバーなどのアプリケーション \(IIS\), 、最小限のダウンタイムで利用できるとはスケーラブルな \(負荷の増加に応じて追加のサーバーを追加することで\)します。 以降のセクションでは、これらのアプリケーションを実行するクラスター化されたサーバーの高可用性、スケーラビリティ、および管理容易性が NLB によってどのようにサポートされるかについて説明します。  
  
### <a name="high-availability"></a>高可用性  
高可用性システムにより、最小のダウンタイムで妥当なサービス レベルを実現します。 NLB を含む高可用性を実現するビルド\-自動的に実行できる機能で。  
  
-   障害が発生するか、オフラインになっているクラスター ホストを検出し、回復します。  
  
-   ホストの追加時または削除時に、ネットワーク負荷を分散します。  
  
-   回復とワークロードの再配分を 10 秒以内に行います。  
  
### <a name="scalability"></a>スケーラビリティ  
スケーラビリティとは、パフォーマンスに対する要求の拡大に合わせてどの程度までコンピューター、サービス、またはアプリケーションを拡張できるかの尺度のことです。 NLB クラスターの場合、スケーラビリティはクラスターの全体的な負荷がクラスターの能力を超えたときに、既存のクラスターにシステムを 1 つ以上段階的に追加して増強できる能力のことです。 スケーラビリティをサポートするために、NLB で次の操作を実行できます。  
  
-   個々 の TCP の NLB クラスター全体で負荷要求のバランスをとる\/IP サービスです。  
  
-   1 つのクラスターで最大 32 台のコンピューターをサポートします。  
  
-   複数のサーバー負荷要求のバランスをとる \(または複数のクライアントから同じクライアントから\) 、クラスター内の複数のホストです。  
  
-   負荷の増大に応じて NLB クラスターにホストを追加できます。その際、クラスターを停止する必要はありません。  
  
-   負荷の減少に応じて、クラスターからホストを削除できます。  
  
-   完全なパイプライン化により、高いパフォーマンスと低いオーバーヘッドを実現します。 パイプライン化により、前の要求に対する応答を待たずに、次の要求を NLB クラスターに送信できます。  
  
### <a name="manageability"></a>管理状態  
管理容易性をサポートするために、NLB で次の操作を実行できます。  
  
-   管理および NLB マネージャーを使用して複数の NLB クラスターと 1 台のコンピューターからクラスター ホストを構成または [Windows PowerShell のネットワーク負荷分散 (NLB) コマンドレット](https://technet.microsoft.com/library/hh801274.aspx)します。
  
-   ポート管理規則を使用して、1 つの IP ポートまたはポート グループに対して負荷分散の動作を指定できます。  
  
-   Web サイトごとに異なるポート規則を定義できます。 負荷の同じセットを使用する場合は、\-変換先の仮想 IP アドレスに基づく、複数のアプリケーションまたは web サイト、ポートの規則のバランスの取れたサーバー \(仮想クラスターを使用して\)します。  

-   省略可能な 1 つを使用して単一のホストにすべてのクライアント要求を直接\-ルールをホストします。 NLB は、特定のアプリケーションを実行しているホストに対して、クライアント要求を転送します。  

-   特定の IP ポートに対する不正ネットワーク アクセスをブロックできます。  

-   インターネット グループ管理プロトコルを有効にする \(IGMP\) スイッチ ポートの混雑を制御するクラスター ホストでサポート \(受信ネットワーク パケットがスイッチ上のすべてのポートに送信される場所\) マルチキャスト モードで動作している場合。  

-   Windows PowerShell コマンドまたはスクリプトにより、リモートで NLB 操作を開始、停止、および制御できます。  

-   Windows のイベント ログを表示して NLB イベントを確認できます。 NLB は、イベント ログで、すべての操作とクラスターへの変更を記録します。  

## <a name="important-functionality"></a>重要な機能  
 
NLB は、標準の Windows Server ネットワーク ドライバー コンポーネントとしてインストールされます。 その操作が意識せずに、TCP\/IP ネットワーク スタックです。 次の図は、一般的な構成における NLB と他のソフトウェア コンポーネント間のリレーションシップを示しています。  
  
![ネットワーク負荷分散と他のソフトウェア コンポーネント](../media/NLB/nlb.jpg)  
  
NLB の主な機能を次に示します。  
  
- 実行するうえでハードウェアの変更は必要ありません。  
  
- 1 台のリモートまたはローカル コンピューターから複数のクラスターおよびすべてのホストを構成、管理できるネットワーク負荷分散ツールが用意されています。  
  
- クライアントが単一の論理インターネット名とクラスター IP アドレスと呼ばれる仮想 IP アドレスを使用してクラスターにアクセスできるように \(コンピューターごとの個別の名前を保持\)します。 NLB では、マルチホーム サーバーで複数の仮想 IP アドレスを使用できます。  
  
> [!NOTE]  
> 仮想クラスターとして Vm を展開するときに、NLB はサーバーを複数の仮想 IP アドレスを使用してマルチホーム化する必要はありません。  
  
- NLB は、複数のネットワーク アダプターにバインドできます。これにより、各ホストで複数の独立クラスターを構成できます。 仮想クラスターは、複数のネットワーク アダプターをサポートすることとは異なります。仮想クラスターでは、単一のネットワーク アダプターで複数のクラスターを構成できます。  
  
- NLB クラスターで実行できるように、サーバー アプリケーションに変更を加える必要はありません。  
  
- 障害が発生したクラスター ホストを再びオンラインに戻す際、そのホストを自動的にクラスターに追加するように構成できます。 追加されたホストは、クライアントからの新しいサーバー要求を処理できます。  
  
-   他のホスト上のクラスター操作を中断することなく、コンピューターをオフラインにして予防メンテナンスを行うことができます。  
  
## <a name="hardware-requirements"></a>ハードウェア要件  
NLB クラスターを実行するためのハードウェア要件を次に示します。  
  
-   クラスター内のすべてのホストが同じサブネットに存在する必要があります。  
  
-   各ホストのネットワーク アダプターの数に制限はありません。ホストごとに異なる数のアダプターを持つこともできます。  
  
-   各クラスター内では、ネットワーク アダプターはすべてマルチキャストまたはユニキャストのどちらか一方であることが必要です。 NLB は、マルチキャストとユニキャストが単一のクラスターに混在する環境をサポートしていません。  
  
-   ユニキャスト モードのクライアントの処理に使用されるネットワーク アダプターを使用する場合\-に\-クラスター トラフィックはメディア アクセス制御の変更をサポートする必要があります \(MAC\) アドレス。  
  
## <a name="software-requirements"></a>ソフトウェア要件  
NLB クラスターを実行するソフトウェアの要件を次に示します。  
  
-   TCP のみ\/IP は、各ホストで NLB が有効であるアダプターで使用できることができます。 その他のプロトコルを追加しないで \(IPX など\) このアダプターにします。  
  
-   クラスター内のサーバーの IP アドレスは、静的なアドレスである必要があります。  
  
> [!NOTE]  
> NLB は動的ホスト構成プロトコルをサポートしていません \(DHCP\)します。 NLB は構成対象の各インターフェイス上の DHCP を無効にします。  
  
## <a name="installation-information"></a>インストール情報  
NLB 用のサーバー マネージャーまたは Windows PowerShell コマンドを使用して、NLB をインストールできます。

ローカルまたはリモート NLB クラスターを管理するためのネットワーク負荷分散ツールをインストールすることもできます。 ツールには、ネットワーク負荷分散マネージャーと NLB Windows PowerShell コマンドが含まれます。

### <a name="installation-with-server-manager"></a>サーバーマネージャーを使用したインストール

サーバー マネージャーで、して追加の役割と機能のウィザードを追加する、 **ネットワーク負荷分散** 機能します。 ウィザードを完了して NLB をインストールすると、コンピューターを再起動する必要はありません。


### <a name="installation-with-windows-powershell"></a>Windows PowerShell を使用したインストール  

NLB をインストールする Windows PowerShell を使用して、実行、次のコマンド管理者特権での Windows PowerShell プロンプトで、コンピューターに NLB をインストールします。

    
    Install-WindowsFeature NLB -IncludeManagementTools
    
インストールが完了したら、コンピューターの再起動は必要ありません。

詳細については、次を参照してください。 [Install-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature?view=win10-ps)します。

### <a name="network-load-balancing-manager"></a>ネットワーク負荷分散マネージャー
サーバー マネージャーでネットワーク負荷分散マネージャーを開くには、 **[ツール]** をクリックし、 **[ネットワーク負荷分散マネージャー]** をクリックします。
  
## <a name="additional-resources"></a>その他の資料  
次の表は、NLB の機能に関する追加情報へのリンクを示します。  
  
|コンテンツの種類|参考資料|  
|----------------|--------------|  
|展開|[ネットワーク負荷分散展開ガイド](https://technet.microsoft.com/library/cc754833(WS.10).aspx) &#124; [ネットワーク負荷分散ターミナル サービスの構成](https://technet.microsoft.com/library/cc771300(v=WS.10).aspx)|  
|操作|[ネットワーク負荷分散クラスターを管理する](https://technet.microsoft.com/library/cc753954(WS.10).aspx) &#124; [ネットワーク負荷分散パラメーターの設定](https://technet.microsoft.com/library/cc731619(WS.10).aspx) &#124; [ネットワーク負荷分散クラスター上のホストを制御します。](https://technet.microsoft.com/library/cc770870(WS.10).aspx)|  
|トラブルシューティング|[ネットワーク負荷分散クラスターのトラブルシューティング](https://technet.microsoft.com/library/cc732592(WS.10).aspx) &#124; [NLB クラスターのイベントとエラー](https://technet.microsoft.com/library/cc731678(WS.10).aspx)|
|ツールと設定|[ネットワーク負荷分散の Windows PowerShell コマンドレット](https://go.microsoft.com/fwlink/p/?LinkId=238123)|
|コミュニティ リソース|[高可用性 \( クラスタリング @ no__t-2 フォーラム](https://go.microsoft.com/fwlink/p/?LinkId=230641)
