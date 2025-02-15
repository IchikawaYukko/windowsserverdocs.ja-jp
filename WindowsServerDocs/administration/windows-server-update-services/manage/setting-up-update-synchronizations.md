---
title: 更新プログラムの同期を設定する
description: Windows Server Update Service (WSUS) のトピック-更新プログラムの同期をセットアップして構成する方法
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4559016388f9b0d765c8e4d76f76fa7ef0a7f0f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361602"
---
# <a name="setting-up-update-synchronizations"></a>更新プログラムの同期を設定する

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

同期中には、WSUS サーバーは、更新ソースから更新プログラム (メタデータとファイルの更新) をダウンロードします。 一緒にダウンロード新しい製品の分類、カテゴリ、存在する場合。 WSUS サーバーを初めて同期すると、すべての同期オプションを構成したときに指定した更新プログラムがダウンロードされます。 初回同期後は、WSUS サーバーは、更新プログラムを更新ソースと既存の更新と期限切れのメタデータ内の変更履歴からの更新のみをダウンロードします。

初めて WSUS サーバーの更新プログラムのダウンロード時間がかかる場合があります。 複数の WSUS サーバーを設定する場合、その更新プログラムを他の WSUS サーバーのコンテンツ ディレクトリにコピーを 1 つの WSUS サーバー上のすべての更新プログラムをダウンロードしてをある程度、プロセスを短縮できます。

1 つの WSUS サーバーのコンテンツ ディレクトリからコンテンツをコピーするには別です。 コンテンツ ディレクトリの場所は、WSUS のポスト インストール手順を実行するときに指定します。 Wsusutil.exe ツールを使用すると、1 つの WSUS サーバーから更新プログラムのメタデータをファイルにエクスポートします。 そのファイルをインポートして、他の WSUS サーバーにすることができます。

## <a name="setting-up-update-synchronizations"></a>更新プログラムの同期を設定する
**オプション** ページは、WSUS 管理コンソールで、WSUS サーバーが更新プログラムを同期する方法をカスタマイズするための集約型アクセス ポイント。 自動的に同期する更新を指定する、サーバーが更新プログラム、接続の設定、および同期スケジュールを取得します。 構成ウィザードを使用することもできます。、 **オプション** ページを構成またはをいつでも、WSUS サーバーを再構成します。

### <a name="synchronizing-update-by-product-and-classification"></a>製品と分類の更新を同期します。
WSUS サーバーは、指定した製品または製品ファミリ (Windows、Windows Server 2008、Datacenter edition など) と分類 (重要な更新プログラムやセキュリティ更新プログラムなど) に基づいて更新プログラムをダウンロードします。 最初の同期時に、WSUS サーバーによって、指定したカテゴリで利用可能なすべての更新プログラムがダウンロードされます。 以降の同期で、WSUS server のダウンロード最新の更新プログラムのみ (または、WSUS サーバーで既に使用可能な更新プログラムへの変更) をカテゴリ別が指定されました。

更新プログラムの製品と分類を指定することができます、 **オプション** ] ページの [ **製品と分類**します。 階層では、製品ファミリによってグループ化、製品が一覧表示されます。 Windows を選択する場合は、その製品階層の下にあるすべての製品に自動的に選択します。 親のチェック ボックスをオンには、将来のバージョンだけでなく、その下にあるすべての項目を選択します。 子のチェックボックスをオンにしても、親のチェックボックスはオンになりません。 製品がすべての Windows 製品と分類の既定の設定は重要なの既定の設定とセキュリティ更新プログラムです。

WSUS サーバーがレプリカ モードで実行されている場合に、このタスクを実行できなきます。 レプリカモードの詳細については、「 [WSUS レプリカモードの実行](running-wsus-replica-mode.md)」および [step 1:WSUS の展開を準備する @ no__t-0

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>更新プログラムの製品との同期の分類を指定するには

1.  WSUS 管理コンソールで 、 **オプション** ノードです。

2.  をクリックして **製品と分類**, 、順にクリックし、 **製品**  タブをクリックします。

3.  製品または製品ファミリが、WSUS を更新し、をクリックする チェック ボックスをオンに **OK**します。

4.  **分類**  タブで、WSUS サーバーをクリックして、同期する更新プログラムの分類のチェック ボックスをオン **OK**します。

> [!NOTE]
> 同じ方法では、製品または分類を削除できます。 WSUS サーバーでは、オフになっている製品の新しい更新プログラムの同期を停止します。 ただし、それらを消去する前に、これらの製品の同期された更新プログラムは、WSUS サーバー上に残りますされ、利用可能な表示されます。
> 
> これらの製品を削除するには、「[更新プログラムの操作](updates-operations.md)」に記載されている更新プログラムを拒否し、[サーバークリーンアップウィザード](the-server-cleanup-wizard.md)を使用して削除します。

### <a name="synchronizing-updates-by-language"></a>言語によって更新を同期します。
WSUS サーバーには、指定された言語に基づく更新がダウンロードされます。 すべてが利用可能な言語の更新プログラムを同期するか、一部の言語を指定できます。 WSUS サーバーの階層があり、さまざまな言語の更新プログラムをダウンロードする必要がある場合は、上流のサーバーで必要なすべての言語を指定したことを確認します。 ダウン ストリーム サーバーでは、アップ ストリーム サーバーで指定されている言語のサブセットを指定できます。

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>Microsoft Update カタログから更新を同期します。
Microsoft Update カタログサイトからの更新プログラムの同期の詳細については、以下を参照してください。[WSUS とカタログサイト](wsus-and-the-catalog-site.md)。

## <a name="configuring-proxy-server-settings"></a>プロキシ サーバー設定の構成
上流のサーバーまたは Microsoft Update との同期中にプロキシ サーバーを使用するように WSUS サーバーを構成することができます。 この設定は、WSUS サーバーの同期の実行時にのみ適用されます。 既定では、WSUS サーバーをアップ ストリーム サーバーまたは Microsoft Update に直接接続する試みます。

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>同期にプロキシ サーバーを指定するには

1.  WSUS 管理コンソールで  **オプション**, 、クリックして **更新元およびプロキシ サーバー**します。

2.  **プロキシ サーバー** ] タブで [、 **を同期するときにプロキシ サーバーを使用して** チェック ボックスをオンし、サーバー名を入力し、プロキシ サーバーのポート番号。

    > [!NOTE]
    > プロキシ サーバーが構成されている同じポート番号を持つ WSUS の構成を使用します。

    -   特定のユーザー資格情報を使用してプロキシサーバーに接続する場合は、[**ユーザーの資格情報を使用してプロキシサーバーに接続**する] チェックボックスをオンにし、対応するボックスにユーザーのユーザー名、ドメイン、およびパスワードを入力します。

    -   プロキシサーバーに接続しているユーザーの基本認証を有効にする場合は、 **[基本認証を許可する (クリアテキストでパスワードを送信する)]** チェックボックスをオンにします。

3.  **[OK]** をクリックします。

    > [!NOTE]
    > WSUS を開始すると、すべてのネットワーク トラフィックが、ために、Microsoft update に直接接続されている WSUS サーバーで Windows ファイアウォールを構成する必要はありません。

## <a name="configuring-the-update-source"></a>更新プログラムのソースを構成します。
更新プログラムのソースとは、WSUS サーバーの更新の取得元となる場所とメタデータを更新します。 更新プログラムのソースが Microsoft Update または別の WSUS サーバー (更新プログラムのソースがアップ ストリーム サーバーと、サーバーがダウン ストリーム サーバーとして機能する WSUS サーバー) であることを指定することができます。

WSUS サーバーが更新ソースと同期する方法をカスタマイズするためのオプションを以下に示します。

-   同期のためのカスタム ポートを指定できます。 ポートの構成の詳細については、「[Step 3:」を参照してください。Wsus の展開ガイドの「Configure WSUS @ no__t-0」を参照してください。

-   WSUS サーバー間での更新情報のセキュリティで保護された同期には、セキュリティで保護されたソケット レイヤー (SSL) を使用できます。 SSL の使用に関する詳細については、"3.5 のセクションを参照してください。 Secure Sockets Layer プロトコルを使用して WSUS をセキュリティで保護する [Step 3:Wsus の展開ガイドの「Configure WSUS @ no__t-0」を参照してください。

## <a name="synchronizing-manually-or-automatically"></a>手動または自動で同期します。
WSUS サーバーを手動で同期するか、自動的に同期するには、その時刻を指定します。

#### <a name="to-manually-synchronize-the-wsus-server"></a>WSUS サーバーを手動で同期させる

1.  WSUS 管理コンソールで  **オプション**, 、クリックして **同期スケジュール**します。

2.  をクリックして **を手動で同期**, 、順にクリック **OK**です。

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>自動同期スケジュールを設定するには

1.  WSUS 管理コンソールで  **オプション**, 、クリックして **同期スケジュール**します。

2.  クリックして **を自動的に同期**します。

3.  **最初の同期**, 、同期を毎日を開始する時刻を選択します。

4.  1**日の同期**については、毎日実行する同期の数を選択します。 たとえば、する場合は 4 つの同期を開始、毎日午前 3 時にの同期が午前 3 時まで、午前 9時 00分午後 3 時と午後 9時 00分 1 日です。 (ランダムな時間のオフセットが、スケジュールされた同期にかかる時間を間隔に、Microsoft Update サーバー接続するために追加されることに注意してください)。

5.  **[OK]** をクリックします。

#### <a name="to-synchronize-your-wsus-server-immediately"></a>WSUS サーバーをすぐに同期するには

1.  WSUS 管理コンソールでは、上位のサーバー ノードを選択します。

2.  **概要** ウィンドウで、 **同期状態**, 、 をクリックして **を今すぐ同期**です。

> [!NOTE]
> 同期はダウンストリームサーバーによって開始されます。
