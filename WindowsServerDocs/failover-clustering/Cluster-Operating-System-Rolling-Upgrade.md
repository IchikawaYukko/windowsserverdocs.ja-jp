---
title: Cluster Operating System Rolling Upgrade (クラスター オペレーティング システムのローリング アップグレード)
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: get-started-article
ms.assetid: 6e102c1f-df26-4eaa-bc7a-d0d55d3b82d5
author: jasongerend
ms.author: jgerend
ms.date: 03/27/2018
ms.openlocfilehash: f7d20a099f287d2ee05ae6e908c173e1eb3cfc66
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361844"
---
# <a name="cluster-operating-system-rolling-upgrade"></a>クラスターのオペレーティングシステムのローリングアップグレード

> 適用対象:Windows Server 2019、Windows Server 2016

クラスター OS のローリングアップグレードを使用すると、管理者は Hyper-v またはスケールアウトファイルサーバーワークロードを停止せずに、クラスターノードのオペレーティングシステムをアップグレードできます。 この機能を使用すると、サービス レベル アグリーメント (SLA) でのダウンタイムに対するペナルティを回避できます。

クラスター OS のローリングアップグレードには、次のような利点があります。

- Hyper-v 仮想マシンとスケールアウトファイルサーバー (SOFS) ワークロードを実行しているフェールオーバークラスターは、ダウンタイムなしで (クラスター内のすべてのノードで実行されている) windows server 2012 R2 から Windows Server 2016 (クラスターのすべてのクラスターノードで実行されている) にアップグレードできます。 SQL Server などのその他のクラスターワークロードは、Windows Server 2016 へのフェールオーバーにかかる時間 (通常は5分未満) に使用できなくなります。  
- 追加のハードウェアは必要ありません。 ただし、クラスター OS のローリングアップグレードプロセス中にクラスターの可用性を向上させるために、クラスターノードを小規模クラスターに一時的に追加することができます。  
- クラスターを停止または再起動する必要はありません。  
- 新しいクラスターは必要ありません。 既存のクラスターはアップグレードされます。 また、Active Directory に格納されている既存のクラスターオブジェクトが使用されます。  
- アップグレードプロセスは、お客様が "ポイント対返品" を解除するまで、すべてのクラスターノードが Windows Server 2016 を実行していて、ClusterFunctionalLevel PowerShell コマンドレットが実行されるまで、元に戻すことができます。  
- クラスターは、混合 OS モードでの実行中に、修正プログラムの適用とメンテナンス操作をサポートできます。  
- PowerShell と WMI を使用した自動化がサポートされています。  
- "クラスターのパブリックプロパティ**ClusterFunctionalLevel** " プロパティは、Windows Server 2016 クラスターノード上のクラスターの状態を示します。 このプロパティは、フェールオーバークラスターに属する Windows Server 2016 クラスターノードから PowerShell コマンドレットを使用して照会できます。  
    ```PowerShell
    Get-Cluster | Select ClusterFunctionalLevel  
    ```  

    値が**8**の場合は、クラスターが Windows Server 2012 R2 の機能レベルで実行されていることを示します。 値**9**は、クラスターが Windows Server 2016 の機能レベルで実行されていることを示します。  

このガイドでは、クラスター OS のローリングアップグレードプロセスのさまざまな段階、インストール手順、機能の制限、よく寄せられる質問 (Faq) について説明します。また、Windows Server 2016 の次のクラスター OS ローリングアップグレードシナリオにも適用できます。  
- Hyper-v クラスター  
- スケールアウトファイルサーバークラスター  

次のシナリオは、Windows Server 2016 ではサポートされていません。  
-  仮想ハードディスク (.vhdx ファイル) を共有記憶域として使用するゲストクラスターのクラスター OS ローリングアップグレード  

クラスター OS のローリングアップグレードは、System Center Virtual Machine Manager (SCVMM) 2016 で完全にサポートされています。 SCVMM 2016 を使用している場合は、クラスターのアップグレードとこのドキュメントで説明されている手順の自動化に関するガイダンスについて、「 [VMM で hyper-v ホストクラスターの Windows Server 2016 へのローリングアップグレードを実行](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade?view=sc-vmm-1807)する」を参照してください。  

## <a name="requirements"></a>要件  
クラスター OS のローリングアップグレードプロセスを開始する前に、次の要件を完了してください。

- Windows Server (半期チャネル)、Windows Server 2016、または Windows Server 2012 R2 を実行するフェールオーバークラスターから開始します。
- Windows Server バージョン1709への記憶域スペースダイレクトクラスターのアップグレードはサポートされていません。
- クラスターワークロードが Hyper-v Vm またはスケールアウトファイルサーバーの場合、ダウンタイムなしのアップグレードを予想できます。
- 次のいずれかの方法を使用して、Hyper-v ノードに第2レベルのアドレス指定テーブル (SLAT) をサポートする Cpu があることを確認します。  
        -SLAT に互換性がある @no__t を確認します。WP8 SDK ヒント 01 @ no__t 記事: CPU が SLATs をサポートしているかどうかを確認する2つの方法について説明します。  
        - [Coreinfo v 3.31](https://technet.microsoft.com/sysinternals/cc835722)ツールをダウンロードして、CPU が SLAT をサポートしているかどうかを判断します。

## <a name="cluster-transition-states-during-cluster-os-rolling-upgrade"></a>クラスター OS のローリングアップグレード時のクラスターの移行状態

このセクションでは、クラスター OS のローリングアップグレードを使用して、Windows Server 2016 にアップグレードされる Windows Server 2012 R2 クラスターのさまざまな移行状態について説明します。  

クラスター OS のローリングアップグレードプロセス中にクラスターワークロードを実行したままにするため、windows Server 2012 R2 ノードから windows server 2016 ノードへのクラスターワークロードの移動は、両方のノードが Windows Server 2012 R2 オペレーティングシステムを実行していた場合と同様に機能します。 Windows Server 2016 ノードがクラスターに追加されると、Windows server 2012 R2 互換モードで動作します。 "混合 OS モード" と呼ばれる新しい概念クラスターモードでは、異なるバージョンのノードを同じクラスター内に存在させることができます (図1を参照)。  

クラスター OS のローリングアップグレードの3つの段階を示す @no__t 0Illustration: すべてのノード Windows Server 2012 R2、混合 OS モード、すべてのノード Windows Server 2016 @ no__t-1  
**図 1: クラスターのオペレーティングシステムの状態遷移 @ no__t-0  

Windows server 2016 ノードがクラスターに追加されると、Windows Server 2012 R2 クラスターが混合 OS モードに移行します。 このプロセスは完全に元に戻すことができます。 Windows Server 2016 のノードをクラスターから削除し、Windows Server 2012 R2 ノードをこのモードでクラスターに追加することができます。 ClusterFunctionalLevel PowerShell コマンドレットがクラスターで実行されると、"戻り値がありません" が発生します。 このコマンドレットを成功させるには、すべてのノードが Windows Server 2016 であり、すべてのノードがオンラインである必要があります。  

## <a name="transition-states-of-a-four-node-cluster-while-performing-rolling-os-upgrade"></a>OS のローリングアップグレードの実行中の4ノードクラスターの移行状態

このセクションでは、ノードが Windows Server 2012 R2 から Windows Server 2016 にアップグレードされる共有記憶域を持つクラスターの4つの異なる段階について説明します。  

"ステージ 1" は初期状態であり、Windows Server 2012 R2 クラスターから開始します。  

初期状態を示す @no__t 0Illustration: すべてのノード Windows Server 2012 R2 @ no__t-1  
**図 2:初期状態:Windows Server 2012 R2 フェールオーバークラスター (ステージ 1)**  

"ステージ 2" では、2つのノードが一時停止、ドレイン、削除、再フォーマットされ、Windows Server 2016 と共にインストールされます。  

クラスターが混合 OS モードで表示されている @no__t 0Illustration: 例4ノードクラスター、2つのノードが Windows Server 2016 を実行しており、2つのノードが Windows Server 2012 R2 @ no__t を実行している。  
**図 3:中間の状態:混合 OS モード:Windows Server 2012 R2 および Windows Server 2016 フェールオーバークラスター (ステージ 2)**  

"ステージ 3" では、クラスター内のすべてのノードが Windows Server 2016 にアップグレードされ、クラスターは ClusterFunctionalLevel PowerShell コマンドレットを使用してアップグレードする準備ができています。  

> [!NOTE]  
> この段階では、プロセスを完全に取り消すことができ、Windows Server 2012 R2 ノードをこのクラスターに追加することができます。  

クラスターが Windows Server 2016 に完全にアップグレードされ、ClusterFunctionalLevel コマンドレットを使用してクラスターの機能レベルを Windows Server 2016 @ no__t に上げる準備ができていることを示す @no__t 0Illustration  
**図 4:中間の状態:すべてのノードが Windows Server 2016 にアップグレードされ、ClusterFunctionalLevel (ステージ 3) の準備が完了しました。**  

ClusterFunctionalLevelcmdlet を実行すると、クラスターは "Stage 4" に入ります。この場合、新しい Windows Server 2016 クラスター機能を使用できます。  

クラスターのローリング OS のアップグレードが正常に完了したことを示す @no__t 0Illustrationすべてのノードが Windows Server 2016 にアップグレードされ、クラスターは Windows Server 2016 クラスターの機能レベル @ no__t-1 で実行されています。  
**Figure 5:最終的な状態:Windows Server 2016 フェールオーバークラスター (ステージ 4)**  

## <a name="cluster-os-rolling-upgrade-process"></a>クラスター OS のローリングアップグレードプロセス

このセクションでは、クラスター OS のローリングアップグレードを実行するためのワークフローについて説明します。  

クラスターをアップグレードするためのワークフローを示す @no__t 0Illustration-1  
**Figure 6:クラスター OS のローリングアップグレードプロセスのワークフロー @ no__t  

クラスター OS のローリングアップグレードには、次の手順が含まれます。  

1. 次のように、オペレーティングシステムのアップグレード用にクラスターを準備します。  
    1. クラスター OS のローリングアップグレードでは、クラスターから一度に1つのノードを削除する必要があります。 クラスターノードの1つがオペレーティングシステムのアップグレードのためにクラスターから削除された場合に、HA Sla を維持するのに十分な容量がクラスターにあるかどうかを確認します。 つまり、クラスター OS のローリングアップグレードのプロセス中にクラスターから1つのノードが削除されたときに、ワークロードを別のノードにフェールオーバーする機能が必要ですか。 クラスター OS のローリングアップグレードのためにクラスターから1つのノードが削除されたときに、必要なワークロードを実行するための容量はクラスターにありますか。  
    2. Hyper-v ワークロードの場合は、すべての Windows Server 2016 Hyper-v ホストで、CPU サポート第2レベルのアドレステーブル (SLAT) が使用されていることを確認します。 Windows Server 2016 では、SLAT 対応のマシンのみが Hyper-v の役割を使用できます。  
    3. ワークロードのバックアップが完了していることを確認し、クラスターのバックアップを検討してください。 クラスターにノードを追加しているときにバックアップ操作を停止します。  
    4. [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps)コマンドレットを使用して、すべてのクラスターノードがオンラインになっているかどうかを確認します (図7を参照)。  

        0Screencap は、Start-clusternode コマンドレット @ no__t-1 を実行した結果を表示します。 @no__t  
        **Figure 7:Start-clusternode コマンドレット @ no__t を使用してノードの状態を確認する  

    5. クラスター対応更新 (CAU) を実行している場合は、**クラスター対応**更新 UI または[@no__t 2](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps)コマンドレットを使用して、cau が現在実行されているかどうかを確認します (図8を参照)。 [@No__t-1](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps)コマンドレットを使用して cau を停止します (図9を参照)。クラスター OS のローリングアップグレードプロセス中に、cau によってノードが一時停止およびドレインされないようにします。  

        0Screencap コマンドレット @ no__t-1 の出力を表示する @no__t  
        **Figure 8:[@No__t-1](https://docs.microsoft.com/powershell/module/clusterawareupdating/Get-CauRun?view=win10-ps)コマンドレットを使用して、クラスターに対応する更新プログラムがクラスター上で実行されているかどうかを確認する @ no__t-2  

        0Screencap は、Add-cauclusterrole コマンドレット @ no__t の出力を表示します。 @no__t  
        **Figure 9:[@No__t-1](https://docs.microsoft.com/powershell/module/clusterawareupdating/Disable-CauClusterRole?view=win10-ps)コマンドレット @ no__t を使用してクラスター対応更新の役割を無効にする  

2. クラスター内の各ノードについて、次の手順を実行します。  
    1. クラスターマネージャー UI を使用してノードを選択し、[一時停止] を使用します。ノードをドレインするためのドレインメニューオプション (図10を参照) または[`Suspend-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps)コマンドレットを使用します (図11を参照)。  

        クラスターマネージャー UI @ no__t を使用して役割をドレインする方法を示す @no__t 0Screencap  
        **Figure 10:フェールオーバークラスターマネージャー @ no__t を使用してノードからロールをドレインしています-0  

        @no__t-Start-clusternode コマンドレット @ no__t の出力を表示する0Screencap  
        **Figure 11:[@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Suspend-ClusterNode?view=win10-ps)コマンドレット @ no__t を使用してノードからロールをドレインする  

    2.  クラスターマネージャー UI を使用して、一時停止しているノードをクラスターから**削除**するか、 [@no__t 2](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps)コマンドレットを使用します。  

        @no__t-Start-clusternode コマンドレット @ no__t の出力を表示する0Screencap  
        **Figure 12:[@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Remove-ClusterNode?view=win10-ps)コマンドレット @ no__t を使用して、クラスターからノードを削除します。  

    3.  システムドライブを再フォーマットし、@no__t 0Custom を使用して、ノード上で Windows Server 2016 の "オペレーティングシステムのクリーンインストール" を実行します。Setup.exe の Windows のみ (詳細) @no__t インストール (図13を参照) オプションをインストールします。 @No__t 0Upgrade を選択しないでください。クラスター OS のローリングアップグレードではインプレースアップグレードが推奨されないため、Windows をインストールし、ファイル、設定、およびアプリケーションを @ no__t オプションのままにします。  

        0Screencap-no__t-1 というカスタムインストールオプションが表示されている Windows Server 2016 インストールウィザードの @no__t  
        **Figure 13:Windows Server 2016 @ no__t の利用可能なインストールオプション-0  

    4.  ノードを適切な Active Directory ドメインに追加します。  
    5.  管理者グループに適切なユーザーを追加します。  
    6.  サーバーマネージャー UI または Install-Add-windowsfeature PowerShell コマンドレットを使用して、Hyper-v など、必要なサーバーの役割をインストールします。  

        ```PowerShell
        Install-WindowsFeature -Name Hyper-V  
        ```  

    7.  サーバーマネージャー UI または Install-Add-windowsfeature PowerShell コマンドレットを使用して、フェールオーバークラスタリング機能をインストールします。  

        ```PowerShell
        Install-WindowsFeature -Name Failover-Clustering  
        ```  

    8.  クラスターワークロードに必要な追加機能をインストールします。  
    9. フェールオーバークラスターマネージャー UI を使用して、ネットワークとストレージの接続設定を確認します。  
    10. Windows ファイアウォールが使用されている場合は、ファイアウォール設定がクラスターに適していることを確認します。 たとえば、クラスター対応更新 (CAU) が有効になっているクラスターでは、ファイアウォールの構成が必要になる場合があります。  
    11. Hyper-v ワークロードの場合は、Hyper-v マネージャー UI を使用して、[仮想スイッチマネージャー] ダイアログを起動します (図14を参照)。  

        使用する仮想スイッチの名前が、クラスター内のすべての Hyper-v ホストノードで同一であることを確認してください。  

        @no__t-Hyper-v 仮想スイッチマネージャーダイアログの場所を示す 0Screencap @ no__t-1  
        **Figure 14:仮想スイッチマネージャー @ no__t-0  

    12. Windows Server 2016 ノード (Windows Server 2012 R2 ノードを使用しない) では、フェールオーバークラスターマネージャー (図15を参照) を使用してクラスターに接続します。  

        [クラスターの選択] ダイアログボックスが表示された @no__t 0Screencap  
        **Figure 15:フェールオーバークラスターマネージャー @ no__t を使用してクラスターにノードを追加する  

    13. フェールオーバークラスターマネージャー UI または[`Add-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps)コマンドレットを使用して (図16を参照)、クラスターにノードを追加します。  

        @no__t-Start-clusternode コマンドレット @ no__t の出力を表示する0Screencap  
        **Figure 16:[@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Add-ClusterNode?view=win10-ps)コマンドレット @ no__t を使用してクラスターにノードを追加する  

        > [!NOTE]  
        > 最初の Windows Server 2016 ノードがクラスターに参加すると、クラスターは "混合 OS" モードになり、クラスターコアリソースが Windows Server 2016 ノードに移動されます。 "混合 OS" モードクラスターは、新しいノードが互換性モードで実行され、古いノードと互換性がある完全な機能を備えたクラスターです。 "混合 OS" モードは、クラスターの一時的モードです。 これは永続的なものではなく、4週間以内にクラスターのすべてのノードを更新することが求められています。  

    14. Windows Server 2016 ノードがクラスターに正常に追加された後、クラスター内のワークロードを再調整するために、次のように、クラスターのワークロードの一部を新しく追加したノードに移動できます (オプション)。

        @no__t-Add-clustervirtualmachinerole コマンドレット @ no__t の出力を表示する0Screencap  
        **Figure 17:[@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps)コマンドレット @ no__t を使用したクラスターワークロード (クラスター VM ロール) の移動  

        1. 仮想マシンのライブマイグレーションを実行するには、仮想マシンのフェールオーバークラスターマネージャーまたは[`Move-ClusterVirtualMachineRole`](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterVirtualMachineRole?view=win10-ps)コマンドレット (図17を参照) の**ライブマイグレーション**を使用します。  

            ```PowerShell
            Move-ClusterVirtualMachineRole -Name VM1 -Node robhind-host3  
            ```  

        2. 他のクラスターワークロードには、フェールオーバークラスターマネージャーまたは[@no__t](https://docs.microsoft.com/powershell/module/failoverclusters/Move-ClusterGroup?view=win10-ps)コマンドレットからの**移動**を使用します。  

3. すべてのノードが Windows Server 2016 にアップグレードされ、クラスターに戻された場合、またはその他の Windows Server 2012 R2 ノードが削除された場合は、次の手順を実行します。  

    > [!IMPORTANT]  
    > -   クラスターの機能レベルを更新した後は、Windows Server 2012 R2 の機能レベルに戻ることはできず、Windows Server 2012 R2 のノードをクラスターに追加することはできません。
    > -   [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)コマンドレットが実行されるまで、プロセスは完全に元に戻すことができ、windows Server 2012 R2 ノードをこのクラスターに追加し、windows server 2016 ノードを削除することができます。  
    > -   [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)コマンドレットを実行すると、新しい機能が使用できるようになります。  

    1.  フェールオーバークラスターマネージャー UI または[`Get-ClusterGroup`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps)コマンドレットを使用して、すべてのクラスターロールが想定どおりにクラスター上で実行されていることを確認します。 次の例では、使用可能な記憶域が使用されていません。代わりに CSV が使用されるため、使用可能な記憶域には**オフライン**状態が表示されます (図18を参照)。  

        0Screencap コマンドレット @ no__t-1 の出力を表示する @no__t  
        **Figure 18:[@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterGroup?view=win10-ps)コマンドレット @ no__t を使用して、すべてのクラスターグループ (クラスターの役割) が実行されていることを確認しています  

    2.  すべてのクラスターノードがオンラインであり、 [`Get-ClusterNode`](https://docs.microsoft.com/powershell/module/failoverclusters/Get-ClusterNode?view=win10-ps)コマンドレットを使用して実行されていることを確認します。  
    3.  [@No__t-1](https://technet.microsoft.com/library/mt589702.aspx)コマンドレットを実行します。エラーは返されません (図19を参照)。  

        0Screencap ClusterFunctionalLevel コマンドレット @ no__t-1 の出力を表示する @no__t  
        **Figure 19:PowerShell @ no__t を使用してクラスターの機能レベルを更新する  

    4.  [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)コマンドレットを実行すると、新しい機能を使用できるようになります。  

4. Windows Server 2016-通常のクラスターの更新とバックアップの再開:  

    1. 以前に CAU を実行していた場合は、CAU UI を使用するか、 [`Enable-CauClusterRole`](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)コマンドレットを使用して再起動します (図20を参照)。  

        0Screencap: Add-cauclusterrole @ no__t-1 の出力を表示する @no__t  
        **Figure 20:[@No__t-1](https://docs.microsoft.com/powershell/module/clusterawareupdating/Enable-CauClusterRole?view=win10-ps)コマンドレット @ no__t を使用して、クラスター対応更新の役割を有効にします。  

    2. バックアップ操作を再開します。  

5. Hyper-v Virtual Machines で Windows Server 2016 の機能を有効にして使用します。  

    1. クラスターが Windows Server 2016 の機能レベルにアップグレードされると、Hyper-v Vm のような多くのワークロードに新機能が追加されます。 新しい Hyper-v 機能の一覧を表示します。 「[仮想マシンの移行とアップグレード」を](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/migrating_vms)参照してください。  

    2. クラスター内の各 Hyper-v ホストノードで、 [`Get-VMHostSupportedVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Get-VMHostSupportedVersion?view=win10-ps)コマンドレットを使用して、ホストでサポートされている hyper-v VM 構成のバージョンを表示します。  

        @no__t-VMHostSupportedVersion コマンドレット @ no__t の出力を表示する0Screencap  
        **Figure 21:ホストでサポートされている Hyper-v VM 構成のバージョンを表示する @ no__t-0  

   3. クラスター内の各 Hyper-v ホストノードで、Hyper-v VM 構成のバージョンをアップグレードできます。そのためには、ユーザーの簡単なメンテナンス期間をスケジュールし、仮想マシンをオフにして、 [`Update-VMVersion`](https://docs.microsoft.com/powershell/module/hyper-v/Update-VMVersion?view=win10-ps)コマンドレットを実行します (図22を参照)。 これにより、仮想マシンのバージョンが更新され、新しい Hyper-v 機能が有効になるため、今後の Hyper-v 統合コンポーネント (IC) の更新は不要になります。 このコマンドレットは、VM をホストしている Hyper-v ノードから実行できます。または、`-ComputerName` パラメーターを使用して、VM のバージョンをリモートで更新できます。 この例では、VM1 の構成バージョンを5.0 から7.0 にアップグレードし、運用チェックポイント (アプリケーション整合性バックアップ) やバイナリ VM など、この VM 構成バージョンに関連付けられている多くの新しい Hyper-v 機能を活用しています。構成ファイル。  

       0Screencap は、動作 @ no__t-1 で更新プログラム VMVersion コマンドレットを表示します。 @no__t  
       **Figure 22:更新プログラム VMVersion PowerShell コマンドレット @ no__t を使用して VM バージョンをアップグレードする  

6. 記憶域プールは、[更新プログラム StoragePool](https://docs.microsoft.com/powershell/module/storage/Update-StoragePool?view=win10-ps) PowerShell コマンドレットを使用してアップグレードできます。これはオンライン操作です。  

ここではプライベートクラウドのシナリオを対象としていますが、特にダウンタイムなしでアップグレードできる Hyper-v およびスケールアウトファイルサーバークラスターは、クラスター OS のローリングアップグレードプロセスを任意のクラスターの役割に使用できます。  

## <a name="restrictions--limitations"></a>制限事項と制限事項  
- この機能は、Windows Server 2012 R2 から Windows Server 2016 のバージョンのみに適用されます。 この機能では、windows server 2008、Windows Server 2008 R2、Windows Server 2012 などの以前のバージョンの Windows Server を Windows Server 2016 にアップグレードすることはできません。  
- Windows Server 2016 の各ノードは、再フォーマット/新規インストールのみにする必要があります。 "インプレース" または "アップグレード" のインストールの種類をお勧めしません。  
- Windows server 2016 ノードをクラスターに追加するには、Windows Server 2016 ノードを使用する必要があります。  
- 混合 OS モードのクラスターを管理する場合は、常に Windows Server 2016 を実行している上位レベルのノードから管理タスクを実行します。 ダウンレベルの Windows Server 2012 R2 ノードでは、Windows Server 2016 に対して UI または管理ツールを使用できません。  
- 一部のクラスター機能は混合 OS モード向けに最適化されていないため、クラスターのアップグレードプロセスを迅速に進めることをお勧めします。  
- Windows server 2016 ノードから下位レベルの Windows Server 2012 R2 ノードへのフェールオーバーでは互換性がない可能性があるため、クラスターが混合 OS モードで実行されている場合は、Windows Server 2016 ノードで記憶域を作成またはサイズ変更することは避けてください。  

## <a name="frequently-asked-questions"></a>よく寄せられる質問  
**フェールオーバークラスターを混合 OS モードで実行するにはどのくらいの時間がかかりますか。**  
    お客様は、4週間以内にアップグレードを完了することをお勧めします。 Windows Server 2016 には多くの最適化があります。 Hyper-v とスケールアウトファイルサーバークラスターを正常にアップグレードしました。ダウンタイムは合計4時間未満です。  

**この機能を Windows Server 2012、Windows Server 2008 R2、または Windows Server 2008 に移植しますか?**  
    この機能を以前のバージョンに移植する計画はありません。 クラスター OS のローリングアップグレードは、Windows Server 2012 R2 クラスターを Windows Server 2016 以降にアップグレードするためのビジョンです。  

**クラスター OS のローリングアップグレードプロセスを開始する前に、Windows Server 2012 R2 クラスターにすべてのソフトウェア更新プログラムがインストールされている必要がありますか。**  
    はい。クラスター OS のローリングアップグレードプロセスを開始する前に、すべてのクラスターノードが最新のソフトウェア更新プログラムで更新されていることを確認してください。  

**ノードがオフになっているとき、または一時停止しているときに、 [@no__t 2](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)のコマンドレットを実行できますか。**  
    No. [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)コマンドレットを機能させるには、すべてのクラスターノードがアクティブなメンバーシップである必要があります。  

**Does OS のローリングアップグレードは、クラスターのワークロードに対して機能しますか。SQL Server に対して機能しますか?**  
    はい。クラスター OS のローリングアップグレードは、クラスターのワークロードに対応しています。 ただし、Hyper-v とスケールアウトファイルサーバークラスターではダウンタイムがゼロになります。 その他のほとんどのワークロードでは、フェールオーバー時にダウンタイム (通常は数分) が発生し、クラスター OS のローリングアップグレードプロセス中に少なくとも1回フェールオーバーが必要になります。  

**PowerShell を使用してこのプロセスを自動化できますか。**  
    はい。 PowerShell を使用してクラスター OS のローリングアップグレードを自動化するように設計されています。  

**追加のワークロードとフェールオーバー容量を持つ大規模なクラスターの場合、複数のノードを同時にアップグレードすることはできますか。**  
    可能。 OS をアップグレードするためにクラスターから1つのノードが削除されると、クラスターのフェールオーバー用のノードが1つ少なくなるため、フェールオーバーの容量が減少します。 ワークロードとフェールオーバーの容量が十分にある大規模なクラスターでは、複数のノードを同時にアップグレードできます。 クラスターノードを一時的にクラスターに追加することにより、クラスター OS のローリングアップグレードプロセス中に、ワークロードとフェールオーバーの容量を向上させることができます。  

**[@No__t-2](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)が正常に実行された後にクラスターで問題が検出された場合はどうすればよいですか?**  
    [@No__t-1](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel?view=win10-ps)を実行する前に、システム状態のバックアップを使用してクラスターデータベースをバックアップした場合は、Windows Server 2012 R2 クラスターノードで権限のある復元を実行し、元のクラスターデータベースと構成を復元できます。  

**システムドライブを再フォーマットしてクリーン OS インストールを使用するのではなく、各ノードのインプレースアップグレードを使用できますか。**  
    Windows Server のインプレースアップグレードを使用することはお勧めしませんが、既定のドライバーが使用される場合には機能します。 クラスターノードのインプレースアップグレード中に表示されるすべての警告メッセージを注意してお読みください。  

**Hyper-v クラスターの hyper-v VM で Hyper-v レプリケーションを使用している場合、クラスター OS のローリングアップグレードプロセスの間、レプリケーションはそのまま維持されますか。**  
    はい。 Hyper-v レプリカは、クラスター OS のローリングアップグレードプロセスの間はそのままです。  

**System Center 2016 Virtual Machine Manager (SCVMM) を使用して、クラスター OS のローリングアップグレードプロセスを自動化できますか。**  
    はい。 System Center 2016 の VMM を使用して、クラスター OS のローリングアップグレードプロセスを自動化できます。  

## <a name="see-also"></a>関連項目  
-   [リリース ノート:Windows Server 2016 に関する重要な問題](../get-started/Release-Notes--Important-Issues-in-Windows-Server-2016-Technical-Preview.md)  
-   [Windows Server 2016 の新機能](../get-started/What-s-New-in-windows-server-2016.md)  
-   [Windows Server でのフェールオーバークラスタリングの新機能](whats-new-in-failover-clustering.md)  
