---
title: NPS で使われる証明書を管理する
description: このトピックでは、Windows Server 2016 のネットワークポリシーサーバーでサーバー証明書を使用する方法について説明します。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 204a4ef4-9d78-4a62-9940-43cc0e1c39d0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b83f68b52a9cceef779e5204e295bbc9e45e7a14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396196"
---
# <a name="manage-certificates-used-with-nps"></a>NPS で使われる証明書を管理する

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

拡張認証プロトコル @ no__t-0Transport Layer Security @no__t など、証明書ベースの認証方法を展開する場合は、保護された拡張認証プロトコル @ no__t-4Transport Layer Security @no__ を使用します。t-5PEAP @ no__t-6TLS @ no__t-7、および PEAP @ no__t-8Microsoft チャレンジハンドシェイク認証プロトコルバージョン 2 \(MS @ no__t-10CHAP v2 @ no__t-11。すべての NPSs にサーバー証明書を登録する必要があります。 サーバー証明書には次のものが必要です。

- 「 [PEAP および EAP 要件の証明書テンプレートを構成する](nps-manage-cert-requirements.md)」の説明に従って、サーバー証明書の最小要件を満たします。

- クライアントコンピューターによって信頼されている証明機関 @no__t 0CA @ no__t によって発行されます。 CA は、現在のユーザーおよびローカルコンピューターの信頼されたルート証明機関の証明書ストアに証明書が存在する場合に信頼されます。

次の手順は、信頼されたルート CA が Verisign などのサードパーティの CA であるか、またはを使用して公開キー基盤 \(PKI @ no__t-1 を展開した CA である展開で、NPS 証明書を管理する際に役立ち Active Directory証明書サービス \(AD CS @ no__t。

## <a name="change-the-cached-tls-handle-expiry"></a>キャッシュされた TLS ハンドルの有効期限の変更

EAP @ no__t-0TLS、PEAP @ no__t-1TLS、および PEAP @ no__t-2 ミリ秒 @ no__t-3CHAP v2 の初期認証プロセスでは、接続しているクライアントの TLS 接続プロパティの一部が NPS によってキャッシュされます。 クライアントは、NPS の TLS 接続プロパティの一部もキャッシュします。

これらの TLS 接続プロパティの個々のコレクションは、TLS ハンドルと呼ばれます。

クライアントコンピューターは複数の認証子の TLS ハンドルをキャッシュでき、NPSs は多くのクライアントコンピューターの TLS ハンドルをキャッシュできます。

クライアントとサーバーでキャッシュされた TLS ハンドルを使用すると、再認証プロセスをより迅速に実行できます。 たとえば、ワイヤレスコンピューターが NPS で再認証されると、NPS はワイヤレスクライアントの TLS ハンドルを調べて、クライアント接続が再接続であることを迅速に判断できます。 NPS は、完全な認証を実行せずに接続を承認します。

また、クライアントは NPS の TLS ハンドルを調べ、再接続であると判断し、サーバー認証を実行する必要はありません。

Windows 10 および Windows Server 2016 を実行しているコンピューターでは、既定の TLS ハンドルの有効期限は10時間です。

場合によっては、TLS ハンドルの有効期限を増減することがあります。

たとえば、管理者によってユーザーの証明書が失効し、証明書の有効期限が切れている場合に、TLS ハンドルの有効期限を短縮することができます。 このシナリオでは、NPS に有効期限が切れていないキャッシュされた TLS ハンドルがある場合でも、ユーザーはネットワークに接続できます。 TLS ハンドルの有効期限を短くすると、証明書が失効しているユーザーの再接続を防ぐことができます。

>[!NOTE]
>このシナリオに最適な解決策は、Active Directory でユーザーアカウントを無効にするか、ネットワークポリシーでネットワークに接続するためのアクセス許可が付与されている Active Directory グループからユーザーアカウントを削除することです。 ただし、レプリケーションの遅延が原因で、すべてのドメインコントローラーに対するこれらの変更の反映が遅延する場合もあります。 

## <a name="configure-the-tls-handle-expiry-time-on-client-computers"></a>クライアントコンピューターでの TLS ハンドルの有効期限の構成

この手順を使用して、クライアントコンピューターが NPS の TLS ハンドルをキャッシュする時間の長さを変更できます。 NPS が正常に認証されると、クライアントコンピューターは、NPS の TLS 接続プロパティを TLS ハンドルとしてキャッシュします。 TLS ハンドルの既定の期間は、10時間 \(36000000 ミリ秒 @ no__t-1 です。 TLS ハンドルの有効期限を増減するには、次の手順に従ってください。

メンバーシップ **管理者**, 、または同等の権限は、この手順を実行するために必要な最小値。

>[!IMPORTANT]
>この手順は、クライアントコンピューターではなく、NPS で実行する必要があります。

### <a name="to-configure-the-tls-handle-expiry-time-on-client-computers"></a>クライアントコンピューターで TLS ハンドルの有効期限を構成するには

1. NPS で、レジストリエディターを開きます。

2. レジストリキー **HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**に移動します。

3. **[編集]** メニューの **[新規作成]** をクリックし、 **[キー]** をクリックします。

4. 「 **Clientcachetime**」と入力し、enter キーを押します。

5. **[Clientcachetime]** を右クリックし、 **[新規]** をクリックして、 **[DWORD (32 ビット) 値]** をクリックします。

6. Nps による認証が最初に成功した後に、クライアントコンピューターが NPS の TLS ハンドルをキャッシュする時間を、ミリ秒単位で入力します。

## <a name="configure-the-tls-handle-expiry-time-on-npss"></a>NPSs で TLS ハンドルの有効期限を構成する

次の手順に従い、NPSs がクライアントコンピューターの TLS ハンドルをキャッシュする時間の長さを変更します。 アクセスクライアントが正常に認証された後、NPSs はクライアントコンピューターの TLS 接続プロパティを TLS ハンドルとしてキャッシュします。 TLS ハンドルの既定の期間は、10時間 \(36000000 ミリ秒 @ no__t-1 です。 TLS ハンドルの有効期限を増減するには、次の手順に従ってください。

メンバーシップ **管理者**, 、または同等の権限は、この手順を実行するために必要な最小値。

>[!IMPORTANT]
>この手順は、クライアントコンピューターではなく、NPS で実行する必要があります。

### <a name="to-configure-the-tls-handle-expiry-time-on-npss"></a>NPSs で TLS ハンドルの有効期限を構成するには

1. NPS で、レジストリエディターを開きます。

2. レジストリキー **HKEY @ no__t-1LOCAL @ no__t-2MACHINE\System\CurrentControlSet\Control\SecurityProviders\SCHANNEL**に移動します。

3. **[編集]** メニューの **[新規作成]** をクリックし、 **[キー]** をクリックします。

4. 「 **Servercachetime**」と入力し、enter キーを押します。

5. **[Servercachetime]** を右クリックし、 **[新規]** をクリックして、 **[DWORD (32 ビット) 値]** をクリックします。

6. クライアントによる最初の認証試行が成功した後に、NPSs がクライアントコンピューターの TLS ハンドルをキャッシュする時間をミリ秒単位で入力します。

## <a name="obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>信頼されたルート CA 証明書の SHA-1 ハッシュを取得する

ローカルコンピューターにインストールされている証明書から、信頼されたルート証明機関 (CA) のセキュアハッシュアルゴリズム (SHA-1) ハッシュを取得するには、次の手順を使用します。 グループポリシーを展開するときなど、状況によっては、証明書の SHA-1 ハッシュを使用して証明書を指定する必要があります。

グループポリシーを使用する場合は、EAP または PEAP を使用した相互認証のプロセス中に、クライアントが NPS を認証するために使用する必要がある信頼されたルート CA 証明書を1つ以上指定できます。 クライアントがサーバー証明書を検証するために使用する必要がある信頼されたルート CA 証明書を指定するには、証明書の SHA-1 ハッシュを入力します。

この手順では、証明書の Microsoft 管理コンソール (MMC) スナップインを使用して、信頼されたルート CA 証明書の SHA-1 ハッシュを取得する方法について説明します。 

この手順を完了するには、ローカルコンピューターの**Users**グループのメンバーである必要があります。

### <a name="to-obtain-the-sha-1-hash-of-a-trusted-root-ca-certificate"></a>信頼されたルート CA 証明書の SHA-1 ハッシュを取得するには

1. [実行] ダイアログボックスまたは Windows PowerShell で、「 **mmc**」と入力し、enter キーを押します。 Microsoft 管理コンソール \(MMC @ no__t-1 が開きます。 MMC で、 **[ファイル]** をクリックし、 **[スナップの追加と削除]** をクリックします。 **[スナップインの追加と削除]** ダイアログボックスが表示されます。

2. **[スナップインの追加と削除]** の **[使用できるスナップ]** イン で、 **[証明書]** をダブルクリックします。 証明書スナップインウィザードが開きます。 **[コンピューターアカウント]** をクリックし、 **[次へ]** をクリックします。

3. **[コンピューターの選択**] で、 **[ローカルコンピューター (このコンソールを実行しているコンピューター)]** が選択されていることを確認し、 **[完了]** をクリックして、 **[OK]** をクリックします。

4. 左側のウィンドウで、 **[証明書 (ローカルコンピューター)]** をダブルクリックし、 **[信頼されたルート証明機関]** フォルダーをダブルクリックします。

5. **Certificates**フォルダーは、 **[信頼されたルート証明機関]** フォルダーのサブフォルダーです。 **[証明書]** フォルダーをクリックします。

6. 詳細ウィンドウで、信頼されたルート CA の証明書を参照します。 証明書をダブルクリックします。 **[証明書]** ダイアログボックスが表示されます。

7. **[証明書]** ダイアログボックスで、 **[詳細]** タブをクリックします。

8. フィールドの一覧で、スクロールして **[拇印]** を選択します。

9. 下のウィンドウでは、証明書の SHA-1 ハッシュである16進数の文字列が表示されます。 SHA-1 ハッシュを選択し、コピーコマンドの Windows ショートカットキー \(CTRL @ no__t-1C @ no__t をクリックして、Windows クリップボードにハッシュをコピーします。

10. SHA-1 ハッシュを貼り付ける場所を開き、適切なカーソルを見つけて、[貼り付け] コマンドの Windows ショートカットキー \(CTRL @ no__t-1V @ no__t-2 を押します。 

証明書と NPS の詳細については、「 [PEAP および EAP の要件の証明書テンプレートを構成する](nps-manage-cert-requirements.md)」を参照してください。

NPS の詳細については、「[ネットワークポリシーサーバー (nps)](nps-top.md)」を参照してください。
