---
title: マルチサイト有効化のトラブルシューティング
description: このトピックは、「Windows Server 2016 のマルチサイト展開に複数のリモートアクセスサーバーを展開する」の一部です。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc42040d68b8a22dcfc46aa30db3a2a3c3bc060a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367061"
---
# <a name="troubleshooting-enabling-multisite"></a>マルチサイト有効化のトラブルシューティング

>適用先:Windows Server (半期チャネル)、Windows Server 2016

このトピックでは、`Enable-DAMultisite` コマンドに関連する問題のトラブルシューティング情報を示します。 表示されたエラーがマルチサイト有効化に関連するものであるか確認するには、Windows イベント ログでイベント ID 10051 があるか確認してください。  
  
## <a name="user-connectivity-issues"></a>ユーザー接続の問題  
正しくない構成方法でマルチサイトを有効にした場合、ユーザー接続の問題が発生する可能性があります。  
  
**原因**  
  
マルチサイト展開では、Windows 10 および Windows 8 クライアントコンピューターは異なるエントリポイント間をローミングできます。  Windows 7 クライアントコンピューターは、マルチサイト展開の特定のエントリポイントに関連付けられている必要があります。 クライアント コンピューターが適切なセキュリティ グループに属していない場合、誤ったグループ ポリシー設定を受け取る可能性があります。  
  
**ソリューション**  
  
DirectAccess では、すべての Windows 10 および Windows 8 クライアントコンピューターに少なくとも1つのセキュリティグループが必要です。ドメインごとに、すべての Windows 10 コンピューターと Windows 8 コンピューターに1つのセキュリティグループを使用することをお勧めします。 DirectAccess では、各エントリポイントに Windows 7 クライアントコンピューターのセキュリティグループも必要です。 各クライアント コンピューターは、1 つのセキュリティ グループにのみ属している必要があります。 そのため、windows 10 および Windows 8 クライアントのセキュリティグループには、Windows 10 または Windows 8 を実行しているコンピューターのみが含まれていること、および各 Windows 7 クライアントコンピューターが関連するエントリポイントの単一の専用セキュリティグループに属していることを確認してください。windows 10 または Windows 8 クライアントが Windows 7 セキュリティグループに属していない。  
  
**DirectAccess クライアントのセットアップ**ウィザードの **[グループの選択**] ページで、Windows 8 セキュリティグループを構成します。 **マルチサイト展開の有効化**ウィザードの **[クライアントサポート**] ページ、または**エントリポイントの追加**ウィザードの **[クライアントサポート]** ページで、Windows 7 セキュリティグループを構成します。  
  
## <a name="kerberos-proxy-authentication"></a>Kerberos プロキシ認証  
**エラーを受信しました**。 マルチサイト展開では、Kerberos プロキシ認証はサポートされていません。 コンピューター証明書を IPsec ユーザー認証に使用できるようにする必要があります。  
  
**原因**  
  
マルチサイトを有効にする前に、コンピューター証明書認証を有効にする必要があります。  
  
**ソリューション**  
  
コンピューター証明書認証を有効にするには、次の手順を実行します。  
  
1.  リモート アクセス管理コンソールの詳細ウィンドウの **[手順 2 リモート アクセス サーバー]** で、 **[編集]** をクリックします。  
  
2.  **リモート アクセス サーバーのセットアップ** ウィザードの **[認証]** ページで、 **[コンピューターの証明書を使用する]** チェック ボックスをオンにし、展開内で証明書を発行するルート証明機関または中間証明機関を選択します。  
  
Windows PowerShell を使用してコンピューター証明書の認証を有効にするには、@no__t 0 のコマンドレットを使用して、 *Ipsecrootcertificate*パラメーターを指定します。  
  
## <a name="ip-https-certificates"></a>IP-HTTPS 証明書  
**エラーを受信しました**。 DirectAccess サーバーは、自己署名された ip-https 証明書を使用します。 既知の CA から発行された署名入りの証明書を使用するように IP-HTTPS を構成してください。  
  
**原因**  
  
IP-HTTPS 証明書が自己署名されています。 マルチサイト展開で自己署名証明書を使用することはできません。  
  
**ソリューション**  
  
IP-HTTPS 証明書を選択するには、次の手順を実行します。  
  
1.  リモート アクセス管理コンソールの詳細ウィンドウの **[手順 2 リモート アクセス サーバー]** で、 **[編集]** をクリックします。  
  
2.  **リモート アクセス サーバーのセットアップ** ウィザードの **[ネットワーク アダプター]** ページの **[IP-HTTPS 接続の認証に使用する証明書を選択してください]** で、 **[DirectAccess で自動的に作成される自己署名証明書を使用する]** チェック ボックスがオフになっていることを確認し、 **[参照]** をクリックして、信頼できる CA によって発行された証明書を選択します。  
  
## <a name="network-location-server"></a>ネットワーク ロケーション サーバー  
  
-   **問題1**  
  
    **エラーを受信しました**。 DirectAccess は、ネットワークロケーションサーバーの自己署名証明書を使用するように構成されています。 CA から発行された署名入りの証明書を使用するようにネットワーク ロケーション サーバーを構成してください。  
  
    **原因**  
  
    ネットワーク ロケーション サーバーがリモート アクセス サーバー上に展開されていて、自己署名証明書を使用しています。 マルチサイト展開で自己署名証明書を使用することはできません。  
  
    **ソリューション**  
  
    ネットワーク ロケーション サーバーの証明書を選択するには、次の手順を実行します。  
  
    1.  リモート アクセス管理コンソールの詳細ウィンドウの **[手順 3 インフラストラクチャ サーバー]** で、 **[編集]** をクリックします。  
  
    2.  **インフラストラクチャ サーバーのセットアップ** ウィザードの **[ネットワーク ロケーション サーバー]** ページの **[ネットワーク ロケーション サーバーをリモート アクセス サーバー上に展開する]** で、 **[自己署名証明書を使用する]** チェック ボックスがオフになっていることを確認し、 **[参照]** をクリックして、エンタープライズ CA によって発行された証明書を選択します。  
  
-   **問題2**  
  
    **エラーを受信しました**。 ネットワーク負荷分散クラスターまたはマルチサイト展開を展開するには、リモートアクセスサーバーの内部名とは異なるサブジェクト名を使用して、ネットワークロケーションサーバーの証明書を取得します。  
  
    **原因**  
  
    ネットワーク ロケーション サーバーの Web サイト用証明書のサブジェクト名がリモート アクセス サーバーの内部名と同じです。 これは名前解決の問題を引き起こします。  
  
    **ソリューション**  
  
    リモート アクセス サーバーの内部名とは異なるサブジェクト名を持つ証明書を取得します。  
  
    ネットワーク ロケーション サーバーを構成するには、次の手順を実行します。  
  
    1.  リモート アクセス管理コンソールの詳細ウィンドウの **[手順 3 インフラストラクチャ サーバー]** で、 **[編集]** をクリックします。  
  
    2.  **インフラストラクチャ サーバーのセットアップ** ウィザードの **[ネットワーク ロケーション サーバー]** ページの **[ネットワーク ロケーション サーバーをリモート アクセス サーバー上に展開する]** で、 **[参照]** をクリックして、以前に取得した証明書を選択します。 この証明書は、リモート アクセス サーバーの内部名とは異なるサブジェクト名を持っている必要があります。  
  
## <a name="windows-7-client-computers"></a>Windows 7 クライアント コンピューター  
**警告を受信しました**。 マルチサイトを有効にする場合、DirectAccess クライアント用に構成されたセキュリティグループに Windows 7 コンピューターを含めることはできません。 マルチサイト展開で Windows 7 クライアント コンピューターをサポートするには、それらのクライアントを含むセキュリティ グループを各エントリ ポイントに対して選択します。  
  
**原因**  
  
既存の DirectAccess 展開で、Windows 7 クライアントのサポートが有効になりました。  
  
**ソリューション**  
  
DirectAccess では、すべての Windows 8 クライアントコンピューター用に少なくとも1つのセキュリティグループと、各エントリポイントの Windows 7 クライアントコンピューターのセキュリティグループが必要です。 各クライアント コンピューターは、1 つのセキュリティ グループにのみ属している必要があります。 このため、windows 8 クライアントのセキュリティグループには Windows 8 を実行しているコンピューターのみが含まれ、各 Windows 7 クライアントコンピューターは、関連するエントリポイント専用の単一のセキュリティグループに属しており、Windows 8 クライアントは存在しないことを確認する必要があります。Windows 7 セキュリティグループに属している。  
  
## <a name="active-directory-site"></a>Active Directory サイト  
**エラーを受信しました**。 サーバー < server_name > は Active Directory サイトに関連付けられていません。  
  
**原因**  
  
DirectAccess は、Active Directory サイトを特定できませんでした。 Active Directory サイトとサービス コンソールでは、ネットワークのさまざまなサブネットを構成し、各サブネットを関連する Active Directory サイトに関連付けることができます。 このエラーは、リモート アクセス サーバーの IP アドレスがどのサブネットにも属していない場合、または IP アドレスが属しているサブネットが Active Directory サイトで定義されていない場合に発生する可能性があります。  
  
**ソリューション**  
  
リモート アクセス サーバーで `nltest /dsgetsite` コマンドを実行して、この問題が生じているかどうかを確認します。 この問題が生じている場合は、このコマンドから ERROR_NO_SITENAME が返されます。 この問題を解決するには、内部サーバーの IP アドレスを含むサブネットが存在すること、およびそのサブネットが Active Directory サイトで定義されていることを確認します。  
  
## <a name="SaveGPOSettings"></a>サーバーの GPO 設定を保存しています  
**エラーを受信しました**。 リモートアクセス設定を GPO < Gpo 名 > に保存中にエラーが発生しました。  
  
**原因**  
  
接続の問題のため、または registry.pol ファイルに共有違反があるため、サーバー GPO への変更を保存できませんでした。たとえば、別のユーザーがファイルをロックしています。  
  
**ソリューション**  
  
リモート アクセス サーバーとドメイン コントローラー間に接続が存在することを確認します。 接続が存在する場合は、ドメイン コントローラーを調べて registry.pol ファイルが別のユーザーに対してロックされているかどうかを確認し、必要な場合はそのユーザー セッションを終了してファイルのロックを解除します。  
  
## <a name="InternalServerError"></a>内部エラーが発生しました  
**エラーを受信しました**。 内部エラーが発生しました。  
  
**原因**  
  
これはクライアント GPO に予期しないエントリ ポイント テーブルが構成されていることが原因で発生する可能性があります。 管理者が DirectAccess クライアント コマンドレットを使用してクライアント GPO のエントリ ポイント テーブルを編集した場合に発生する可能性があります。  
  
**ソリューション**  
  
すべてのクライアント GPO のエントリ ポイント テーブル構成を確認し、クライアント GPO のさまざまなインスタンスと DirectAccess 構成の間のマルチサイト構成の不整合をすべて解決します。 クライアント GPO の名前を指定して `Get-DaEntryPointTableItem` コマンドレットを実行し、クライアントのエントリ ポイント テーブルを取得します。 `Get-NetIPHttpsConfiguration` コマンドレットを使用して、すべてのエントリ ポイントの IP-HTTPS プロファイルをすべて取得します。  
  
詳細については、「 [Windows PowerShell の DirectAccess クライアントコマンドレット](https://technet.microsoft.com/library/hh848426)」を参照してください。  
  


