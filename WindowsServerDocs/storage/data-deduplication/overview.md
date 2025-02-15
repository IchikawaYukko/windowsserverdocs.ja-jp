---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: データ重複除去の概要
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 1050c63d77db66c8e280ea1bea9503390c5d0bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386301"
---
# <a name="data-deduplication-overview"></a>データ重複除去の概要

> 適用対象:Windows Server 2019、Windows Server 2016、Windows Server (半期チャネル)、 

## <a name="what-is-dedup"></a>データ重複除去とは

データ重複除去は、多くの場合、重複除去と呼ばれ、ストレージコストに対する冗長データの影響を軽減するのに役立つ機能です。 有効にすると、データ重複除去により、ボリューム上の重複している部分が検索され、ボリューム上のデータが検査されて、ボリューム上の空き領域が最適化されます。 ボリューム上のデータセットの重複する部分は、1 回だけ保存され、さらに節減するために (必要に応じて) 圧縮されます。 データ重複除去では、データの忠実性や完全性を損なうことなく冗長性を最適化します。 データ重複除去のしくみの詳細については、「[データ重複除去のしくみ](understand.md#how-does-dedup-work)」 セクション (「[データ重複除去とは](understand.md)」ページ) を参照してください。

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334)には、重要な信頼性の修正を含む、データ重複除去に関する修正のロールアップが含まれています。 windows server 2016 および windows server 2019 でデータ重複除去を使用する場合は、これをインストールすることを強くお勧めします。

## <a name="why-is-dedup-useful"></a>データ重複除去が役に立つのはなぜですか。

記憶域管理者はデータ重複除去を使用して、重複データに関連するコストを削減できます。 大規模なデータセットには **<u>大量の</u>** 重複が存在することが多く、データを格納するコストを増しています。 以下に例を示します。

- ユーザー ファイル共有は同じ、または類似するファイルのコピーを多く含むことがあります。
- 仮想化ゲストは VM 間でほぼ同じであることがあります。
- 毎日のバックアップ スナップショットの内容は、わずかな違いであることがあります。

データ重複除去から得られる領域の節約は、データセットや、ボリューム上のワークロードに依存します。 重複の多いデータセットでは、最大 95% の最適化率、つまり記憶域使用率が 20 分の 1 になることがあります。 以下の表は、各種コンテンツに対して重複除去を行った場合の標準的な削減効果を示しています。

| シナリオ       | Content                                        | 標準的な削減効果 |
|----------------|------------------------------------------------|-----------------------|
| ユーザー ドキュメント | Office ドキュメント、写真、ミュージック、ビデオなど  | 30 ～ 50%                |
| 展開共有 | ソフトウェア バイナリ、cab ファイル、シンボルなど | 70 ～ 80%                |
| 仮想化ライブラリ | ISO、仮想ハード ディスク ファイルなど  | 80 ～ 95%                |
| 一般的なファイル共有 | 上記すべて                           | 50 ～ 60%                |

## <a id="when-can-dedup-be-used"></a>データ重複除去をいつ使用できるか。  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>General ファイルサーバー @ no__t-1<br />
汎用ファイル サーバーは、次に示す任意の種類の共有を含む可能性のある一般的な用途のファイル サーバーです。 <ul>
                    <li>チームの共有</li>
                    <li>ユーザー ホーム フォルダー</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">ワーク フォルダー</a></li>
                    <li>ソフトウェア開発用の共有</li>
                </ul>
汎用ファイル サーバーでは、同じファイルの多くのコピーまたはバージョンを複数のユーザーが所有する傾向があるため、データ重複除去の有力候補です。 ソフトウェア開発用の共有は、多くのバイナリがビルドごとに基本的に変更されていないため、データ重複除去の恩恵を受けることになります。 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">@no__t 0Virtualized デスクトップインフラストラクチャ (VDI) の展開 @ no__t-1<br />
<a href="https://technet.microsoft.com/library/cc725560.aspx">リモート デスクトップ サービス</a>などの VDI サーバーは、組織がユーザーにデスクトップをプロビジョニングするための軽量のオプションになります。 組織がこのようなテクノロジに頼る理由はさまざまです。 <ul>
                    <li><b>アプリケーションの展開</b>:企業全体にアプリケーションを迅速にデプロイできます。 これは、頻繁に更新されるアプリケーション、使用頻度の低いアプリケーション、または管理しにくいアプリケーションがあるときに特に便利です。</li>
                    <li><b>アプリケーションの統合</b>:集中管理された一連の仮想マシンからアプリケーションをインストールして実行すると、クライアントコンピューター上のアプリケーションを更新する必要がなくなります。 このオプションで、アプリケーションにアクセスするために必要なネットワーク帯域幅の量も減少します。</li>
                    <li><b>リモートアクセス</b>:ユーザーは、自宅のコンピューター、キオスク、低電力のハードウェア、Windows 以外のオペレーティングシステムなどのデバイスからエンタープライズアプリケーションにアクセスできます。</li>
                    <li><b>ブランチオフィスアクセス</b>:VDI の展開は、集中管理されたデータストアへのアクセスが必要なブランチオフィスの従業員にとって、アプリケーションのパフォーマンスを向上させることができます。 データ負荷の高いアプリケーションは、低速な接続用に最適化されたクライアント/サーバー プロトコルに対応していないことがあります。</li>
                </ul>
VDI 展開では、ユーザー用にリモート デスクトップを駆動する仮想ハード ディスクが実質的に同じであるためデータ重複除去の有力候補です。 さらに、データ重複除去は、いわゆる「<em>VDI ブート ストーム</em>」を軽減できます。VDI ブート ストームとは、多くのユーザーが 1 日の始まりに同時に自分のデスクトップにログオンして記憶域のパフォーマンスが低下する状況です。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">@no__t 0Backup のターゲット (仮想化されたバックアップアプリケーションなど) @ no__t-1<br />
バックアップ スナップショット間には大幅な重複があるため、<a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a> などのバックアップ アプリケーションはデータ重複除去の有力候補です。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">@no__t の他のワークロード @ no__t<br />
                <a href="install-enable.md#enable-dedup-candidate-workloads" data-raw-source="[Other workloads may also be excellent candidates for Data Deduplication](install-enable.md#enable-dedup-candidate-workloads)">その他のワークロードもデータ重複除去の有力候補になります</a>。
            </td>
        </tr>
    </tbody>
</table>
