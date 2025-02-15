---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: ワーク フォルダーの展開
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: サーバー役割のインストール、同期共有の作成、DNS レコードの作成などの、ワーク フォルダーを展開する方法。
ms.openlocfilehash: 7fe39ded6d262d9310bce30239345a9f42e43c04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365860"
---
# <a name="deploying-work-folders"></a>ワーク フォルダーの展開

>適用対象:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows 10、Windows 8.1、Windows 7

このトピックでは、ワーク フォルダーを展開するために必要な手順について説明します。 既に「[ワーク フォルダーの展開の計画](plan-work-folders.md)」を読んでいることを前提としています。  
  
 複数のサーバーとテクノロジが関連する可能性のあるプロセスである、ワーク フォルダーを展開するには、以下の手順を使用します。  
  
> [!TIP]
>  最もシンプルなワーク フォルダーの展開は、インターネットを介した同期をサポートしない単一のファイル サーバー (通常は同期サーバーと呼ばれています) です。これは、テスト ラボや、ドメインに参加しているクライアント コンピューターの同期ソリューションとして有益な展開となることがあります。 単純な展開を作成するための最小限の手順は次のとおりです。 
>  -   手順 1:SSL 証明書を取得する  
>  -   手順 2:DNS レコードを作成する 
>  -   手順 3:ファイル サーバーでワーク フォルダーをインストールする  
>  -   手順 4:同期サーバーで SSL 証明書をバインドする
>  -   手順 5:ワーク フォルダーのセキュリティ グループを作成する  
>  -   手順 7:ユーザー データの同期共有を作成する  
  
## <a name="step-1-obtain-ssl-certificates"></a>手順 1:SSL 証明書を取得する  
 ワーク フォルダーでは、ワーク フォルダー クライアントとワーク フォルダー サーバーの間を HTTPS によって安全に同期する必要があります。 ワーク フォルダーで使用される SSL 証明書の要件は次のとおりです。  
  
- 証明書は信頼された証明機関によって発行されている必要があります。 証明書は、ドメインに参加していないインターネット ベースのデバイスによって使用されるため、ほとんどのワーク フォルダーの実装では、公的に信頼された CA を使用することをお勧めします。  
  
- 証明書は有効である必要があります。  
  
- 証明書の秘密キーは、(複数のサーバーに証明書をインストールする必要があるため) エクスポートできる必要があります。  
  
- 証明書のサブジェクト名には、インターネット経由でワーク フォルダー サービスを検出するために使用されるパブリック ワーク フォルダー URL が含まれている必要があります。これは `workfolders.` *<domain_name>* の形式である必要があります。  
  
- 使用されている各同期サーバーのサーバー名を一覧表示する証明書に、サブジェクト代替名 (SAN) が存在している必要があります。

  ワーク フォルダーの証明書管理の[ブログ](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/)は、ワーク フォルダーでの証明書の使用に関する追加情報を提供します。
  
## <a name="step-2-create-dns-records"></a>手順 2:DNS レコードを作成する  
 ユーザーがインターネット経由で同期できるようにするには、パブリック DNS にホスト (A) レコードを作成して、インターネット クライアントがワーク フォルダー URL を解決できるようにする必要があります。 この DNS レコードは、リバース プロキシ サーバーの外部インターフェイスに解決される必要があります。  
  
 内部ネットワーク上で、workfolders という名前で DNS の CNAME レコードを作成します。これはサーバー ワーク フォルダーの FDQN に解決されます。 ワークフォルダークライアントが自動検出を使用する場合、ワークフォルダーサーバーを検出するために使用\/される URL は、https:/workfolders.domain.com です。 自動検出を使用する場合は、workfolders CNAME レコードが DNS 内に存在する必要があります。  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>手順 3:ファイル サーバーでワーク フォルダーをインストールする  
 ワーク フォルダーは、ドメインに参加しているサーバーに、サーバー マネージャーまたは Windows PowerShell を使用して、ローカルまたはネットワーク経由でリモートからインストールできます。 これは、ネットワーク経由で複数の同期サーバーを構成している場合に役立ちます。  
  
サーバー マネージャーで役割を展開するには、次の操作を行います。  
  
1.  **役割と機能の追加ウィザード**を開始します。  
  
2.  **[インストールの種類の選択]** ページで **[役割ベースまたは機能ベースのインストール]** を選択します。  
  
3.  **[対象サーバーの選択]** ページで、ワーク フォルダーのインストール先のサーバーを選択します。  
  
4.  **[サーバーの役割の選択]** ページで、 **[ファイル サービスおよび記憶域サービス]** 、 **[ファイル サービスおよび iSCSI サービス]** の順に展開し、 **[ワーク フォルダー]** を選択します。  
  
5.  **[IIS ホスト可能な Web コア]** をインストールするかどうかを確認するメッセージが表示されたら、 **[OK]** をクリックしてインターネット インフォメーション サービス (IIS) の最小バージョンをインストールします。  
  
6.  ウィザードが完了するまで **[次へ]** をクリックします。

Windows PowerShell を使用してこの役割を展開するには、次のコマンドレットを使用します。
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>手順 4:同期サーバーで SSL 証明書をバインドする
 ワーク フォルダーでは、IIS ホスト可能な Web コアがインストールされます。これは、IIS をフル インストールすることなく、Web サービスを有効にするように設計された IIS コンポーネントです。 IIS ホスト可能な Web コアをインストールした後、ファイル サーバーの既定の Web サイトにサーバーの SSL 証明書をバインドする必要があります。 ただし、IIS ホスト可能な Web コアでは、IIS 管理コンソールはインストールされません。

 証明書を既定の Web インターフェイスにバインドするには、2 つのオプションがあります。 いずれのオプションを使用する場合も、コンピューターの個人用ストアに証明書の秘密キーをインストールしておく必要があります。

- IIS 管理コンソールがインストールされているサーバーで IIS 管理コンソールを使用します。 コンソール内から、管理するファイル サーバーに接続し、そのサーバーの既定の Web サイトを選択します。 既定の Web サイトは無効として表示されますが、サイトのバインドを編集し、証明書を選択してその Web サイトにバインドすることはできます。

- netsh コマンドを使用して証明書を既定の Web サイトの https インターフェイスにバインドします。 コマンドは次のとおりです。

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>手順 5:ワーク フォルダーのセキュリティ グループを作成する
 同期共有を作成する前に、Domain Admins グループまたは Enterprise Admins グループのメンバーが Active Directory Domain Services (AD DS) にワーク フォルダー用のセキュリティ グループをいくつか作成する必要があります (手順 6 で説明されているように、特定のコントロールの委任も必要になる場合があります)。 必要なグループは次のとおりです。

- 同期共有との同期が許可されているユーザーを指定するために、同期共有ごとに 1 つのグループ。

- ワーク フォルダー管理者が、(複数の同期サーバーを使用する場合に) ユーザーを適切な同期サーバーにリンクする各ユーザー オブジェクトの属性を編集できるように、すべてのワーク フォルダー管理者用に 1 つのグループ。

  他のセキュリティ要件との潜在的な競合を回避できるように、グループは標準的な命名規則に従い、ワーク フォルダーについてのみ使用してください。

  適切なセキュリティ グループを作成するには、次の手順を複数回実行します。各同期共有について 1 回、必要に応じてファイル サーバー管理者のグループを作成するために 1 回実行します。

#### <a name="to-create-security-groups-for-work-folders"></a>ワーク フォルダーのセキュリティ グループを作成するには

1. Windows Server 2012 R2 または Windows Server 2016 が実行され、Active Directory 管理センターがインストールされているコンピューターで、サーバー マネージャーを開きます。

2.  **[ツール]** メニューの **[Active Directory 管理センター]** をクリックします。 Active Directory 管理センターが表示されます。

3.  新しいグループを作成するコンテナー (たとえば、適切なドメインまたは OU のユーザー コンテナー) を右クリックし、 **[新規]** 、 **[グループ]** の順にクリックします。

4.  **[グループの作成]** ウィンドウの **[グループ]** で、次の設定を指定します。

    -   **[グループ名]** に、セキュリティ グループの名前を入力します。たとえば、**HR 同期共有ユーザー**、**ワーク フォルダー管理者**などです。  
  
    -   **[グループのスコープ]** で、 **[セキュリティ]** 、 **[グローバル]** の順にクリックします。  
  
5.  **[メンバー]** セクションの **[追加]** をクリックします。 [ユーザー、連絡先、コンピューター、サービス アカウントまたはグループの選択] ダイアログ ボックスが表示されます。  
  
6.  特定の同期共有へのアクセスを許可するユーザーまたはグループの名前を入力する (同期共有へのアクセスを制御するグループを作成している場合) か、ワーク フォルダー管理者の名前を入力して (適切な同期サーバーを自動的に検出するようにユーザー アカウントを構成している場合)、 **[OK]** をクリックし、もう一度 **[OK]** をクリックします。

Windows PowerShell を使用してセキュリティ グループを作成するには、次のコマンドレットを使用します。

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>手順 6:必要に応じてユーザー属性の制御をワーク フォルダー管理者に委任する  
 複数の同期サーバーを展開し、適切な同期サーバーに自動的にユーザーを誘導する場合、AD DS で各ユーザー アカウントの属性を更新する必要があります。 ただし、通常、この属性を更新するには、Domain Admins グループまたは Enterprise Admins グループのメンバーになる必要がありますが、頻繁にユーザーを追加したり、同期サーバー間でユーザーを移動したりする必要がある場合、これは面倒な作業になる可能性があります。  
  
 そのため、Domain Admins グループまたは Enterprise Admins グループのメンバーは、次の手順で説明するように、ユーザー オブジェクトの msDS-SyncServerURL プロパティを変更する権限を、手順 5. で作成したワーク フォルダー管理者グループに委任する場合があります。  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>AD DS でユーザー オブジェクトの msDS-SyncServerURL プロパティを編集する権限を委任する  
  
1.  Windows Server 2012 R2 または Windows Server 2016 が実行され、Active Directory ユーザーとコンピューターがインストールされているコンピューターで、サーバー マネージャーを開きます。  
  
2.  **[ツール]** メニューの **[Active Directory ユーザーとコンピューター]** をクリックします。 Active Directory ユーザーとコンピューターが表示されます。  
  
3.  ワーク フォルダーのすべてのユーザー オブジェクトが含まれている OU を右クリック (ユーザーが複数の OU またはドメインに格納されている場合は、すべてのユーザーに共通のコンテナーを右クリック) し、 **[制御の委任]** をクリックします。 オブジェクト制御の委任ウィザードが表示されます。  
  
4.  **[ユーザーまたはグループ]** ページで **[追加]** をクリックし、 ワーク フォルダー管理者用に作成したグループ (たとえば、**ワーク フォルダー管理者**) を指定します。  
  
5.  **[委任するタスク]** ページで、 **[委任するカスタム タスクを作成する]** をクリックします。  
  
6.  **[Active Directory オブジェクトの種類]** ページで、 **[フォルダー内の次のオブジェクトのみ]** をクリックし、 **[ユーザー オブジェクト]** チェック ボックスをオンにします。  
  
7.  **[アクセス許可]** ページで、 **[全般]** チェック ボックスをオフにし、 **[プロパティ固有]** チェック ボックスをオンにします。次に、 **[msDS-SyncServerUrl の読み取り]** チェック ボックスと **[msDS-SyncServerUrl の書き込み]** チェック ボックスをオンにします。

Windows PowerShell を使用してユーザー オブジェクトの msDS-SyncServerURL のプロパティを編集する権限を委任するには、DsAcls コマンドを利用する次のサンプル スクリプトを使用します。
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  ユーザー数の多いドメインでは、委任操作に時間がかかる場合があります。  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>手順 7:ユーザー データの同期共有を作成する  
 この時点で、ユーザーのファイルを格納する同期サーバー上のフォルダーを指定する準備ができました。 このフォルダーは同期共有と呼ばれ、1 つの同期共有を作成するには、次の手順を実行します。  
  
1. 同期共有と同期共有に格納されるユーザー ファイル用の空き領域がある NTFS ボリュームがない場合は、新しいボリュームを作成し、NTFS ファイル システムでフォーマットします。  
  
2. サーバー マネージャーで、 **[ファイル サービスおよび記憶域サービス]** をクリックし、 **[ワーク フォルダー]** をクリックします。  
  
3. 既存の同期共有の一覧が詳細ウィンドウの上部に表示されます。 新しい同期共有を作成するには、 **[タスク]** メニューの **[新しい同期共有]** をクリックします。 新しい同期共有ウィザードが表示されます。  
  
4. **[サーバーとパスの選択]** ページで、同期共有を格納する場所を指定します。 このユーザー データに対して作成されたファイル共有がある場合は、その共有を選択できます。 新しいフォルダーを作成することもできます。  
  
   > [!NOTE]
   >  (既存のファイル共有を選択しない場合) 既定では、ファイル共有によって同期共有に直接アクセスすることはできません。 同期共有をファイル共有からアクセスできるようにする場合は、サーバー マネージャーの **[共有]** タイルを使用するか、[New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) コマンドレットを使用して (可能であればアクセス ベースの列挙が有効な) ファイル共有を作成します。  
  
5. **[ユーザー フォルダーの構造の指定]** ページで、同期共有内のユーザー フォルダーの命名規則を選択します。 2 つのオプションを使用できます。  
  
   - **[ユーザーのエイリアス]** では、ドメイン名が含まれないユーザー フォルダーが作成されます。 既にフォルダー リダイレクトや別のユーザー データ ソリューションで使用中のファイル共有を使用している場合は、この命名規則を選択します。 必要に応じて、 **[次のサブフォルダーのみ同期]** チェック ボックスをオンにして、ドキュメント フォルダーなど、特定のサブフォルダーのみを同期できます。  
  
   - <strong>alias@domain [ユーザー エイリアス@ドメイン]</strong> では、ドメイン名が含まれるユーザー フォルダーが作成されます。 フォルダー リダイレクトや別のユーザー データ ソリューションで使用中のファイル共有を使用しない場合、この命名規則を選択すると、共有の複数のユーザーのエイリアスが同じである場合にフォルダー名の競合を回避できます (複数のユーザーが異なるドメインに属する場合に、このような競合が発生することがあります)。  
  
6. **[同期共有名を入力します]** ページで、同期共有の名前と説明を指定します。 これはネットワークでアドバタイズされませんが、サーバー マネージャーや Windows PowerShell で表示されるため、同期共有を互いに区別するのに役立ちます。  
  
7. **[グループへの同期アクセスの許可]** ページで、この同期共有の使用を許可されたユーザーの一覧を表示する、作成済みのグループを指定します。  
  
   > [!IMPORTANT]
   >  パフォーマンスとセキュリティを向上させるには、個々のユーザーではなくグループにアクセスを許可し、Authenticated Users や Domain Users などの一般的なグループを避けてできるだけ具体的に指定します。 ユーザー数の多いグループにアクセスを許可すると、ワーク フォルダーが AD DS に照会するのにかかる時間が長くなります。 ユーザー数が多い場合は、複数の同期共有を作成すると負荷を分散させるのに役立ちます。  
  
8. **[デバイス ポリシーの指定]** ページで、クライアント PC やデバイスのセキュリティの制限を要求するかどうかを指定します。 個別に選択できる 2 つのデバイス ポリシーがあります:  
  
   -   **[ワーク フォルダーを暗号化する]** : クライアント PC やデバイスでワーク フォルダーを暗号化することを要求します。  
  
   -   **[自動的に画面をロックする (パスワードが必要]** : クライアント PC やデバイスが 15 分後に自動的に画面をロックし、画面のロックを解除するときに 6 文字以上の長いパスワードを要求します。また、再試行が 10 回失敗すると、デバイス ロックアウト モードをアクティブ化します。  
  
       > [!IMPORTANT]
       >  Windows 7 PC と、ドメインに参加している PC 上の非管理者に対してパスワード ポリシーを適用するには、コンピューター ドメインに対してグループ ポリシーのパスワード ポリシーを使用し、ワーク フォルダー パスワード ポリシーから対象のドメインを除外します。 同期共有を作成した後、[Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) コマンドレットを使用することによってドメインを除外できます。 グループ ポリシーのパスワード ポリシーの設定については、「[パスワード ポリシー](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx)」を参照してください。  
  
9. 選択内容を確認し、ウィザードを完了して同期共有を作成します。

Windows PowerShell を使用して同期共有を作成するには、[New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx) コマンドレットを使用します。 この方法の例を次に示します。  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
上の例では、パス *K:\Share-1* を指定して新しい同期共有 *Share01* を作成し、*HR Sync Share Users* という名前のグループにアクセスを許可しています。  
  
> [!TIP]
>  同期共有を作成すると、ファイル サーバー リソース マネージャーの機能を使用して、共有内のデータを管理できます。 たとえば、サーバー マネージャーのワーク フォルダー ページ内の **[クォータ]** のタイルを使用して、ユーザー フォルダーのクォータを設定できます。 また、[ファイル スクリーンの管理](https://technet.microsoft.com/library/cc732074.aspx)を使用して、ワーク フォルダーで同期するファイルの種類を制御できます。さらに高度なファイルの分類タスクについては「[ダイナミック アクセス制御](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview)」で説明されているシナリオを使用できます。  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>手順 8:必要に応じてテクニカルサポートの電子メールアドレスを指定する   
 ファイル サーバーにワーク フォルダーをインストールした後、必要に応じて、サーバーの管理部門の連絡先電子メール アドレスを指定できます。 電子メール アドレスを追加するには、次の手順に従います。  
  
#### <a name="specifying-an-administrative-contact-email"></a>管理部門の連絡先メール アドレスを指定する 
  
1.  サーバー マネージャーで、 **[ファイル サービスおよび記憶域サービス]** をクリックし、 **[サーバー]** をクリックします。  
  
2.  同期サーバーを右クリックし、 **[ワーク フォルダーの設定]** をクリックします。 [ワーク フォルダーの設定] ウィンドウが表示されます。  
  
3.  ナビゲーション ウィンドウで、 **[サポート用電子メール]** をクリックし、ユーザーがワーク フォルダーに関する問い合わせのメールを送信する際に使用する電子メール アドレスを入力します。 完了したら **[OK]** をクリックします。  
  
     ワーク フォルダーのユーザーは、コントロール パネルの [ワーク フォルダー] 項目にあるリンクをクリックすることによって、ここで指定したアドレスに、クライアント PC の診断情報を含む電子メールを送信できます。  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>手順 9:必要に応じてサーバー自動検出をセットアップする  
 環境で複数の同期サーバーをホストしている場合、AD DS のユーザー アカウントで **msDS-SyncServerURL** プロパティを設定して、サーバー自動検出を構成してください。  
  
>[!NOTE]
>Active Directory の msDS-SyncServerURL プロパティは、Web アプリケーション プロキシや Azure AD アプリケーション プロキシなどのプロキシ ソリューションを介してワーク フォルダーにアクセスするリモート ユーザーに対して定義しないでください。 "-SyncServerURL" プロパティが定義されている場合、ワークフォルダークライアントは、リバースプロキシソリューションを介してアクセスできない内部 URL にアクセスしようとします。 Web アプリケーション プロキシまたは Azure AD アプリケーション プロキシを使用する場合は、ワーク フォルダー サーバーごとに固有のプロキシ アプリケーションを作成する必要があります。 詳細については[、「AD FS と Web アプリケーションプロキシを使用したワークフォルダーの展開」を参照してください。[Azure AD アプリケーションプロキシを使用してワークフォルダーを展開](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)する方法について説明します。](deploy-work-folders-adfs-overview.md)


 この作業を行う前に、Windows Server 2012 R2 のドメイン コントローラーをインストールするか、`Adprep /forestprep` コマンドと `Adprep /domainprep` コマンドを使用して、フォレストとドメインのスキーマを更新する必要があります。 これらのコマンドを安全に実行する方法については、「[Adprep.exe の実行](https://technet.microsoft.com/library/dd464018.aspx)」を参照してください。  
  
 手順 5. と手順 6. で説明したように、ファイル サーバー管理者のセキュリティ グループを作成し、この特定のユーザーの属性を変更するためのアクセス許可を委任することもできます。 これらの手順を実行していない場合、Domain Admins グループまたは Enterprise Admins グループのメンバーになり、各ユーザーの自動検出を構成する必要があります。  
  
#### <a name="to-specify-the-sync-server-for-users"></a>ユーザーの同期サーバーを指定するには  
  
1.  Active Directory 管理センターがインストールされているコンピューターで、サーバー マネージャーを開きます。  
  
2.  **[ツール]** メニューの **[Active Directory 管理センター]** をクリックします。 Active Directory 管理センターが表示されます。  
  
3.  適切なドメインの **[ユーザー]** コンテナーに移動し、同期共有に割り当てるユーザーを右クリックして **[プロパティ]** をクリックします。  
  
4.  ナビゲーション ウィンドウで、 **[拡張機能]** をクリックします。  
  
5.  **[属性エディター]** タブをクリックし、 **[msDS-SyncServerUrl]** を選択して **[編集]** をクリックします。 [複数値の文字列エディター] ダイアログ ボックスが表示されます。  
  
6.  **[追加する値]** ボックスで、このユーザーに同期を許可する同期サーバーの URL を入力し、 **[追加]** をクリックします。次に、 **[OK]** をクリックし、もう一度 **[OK]** をクリックします。  
  
    > [!NOTE]
    >  同期サーバーの URL は、単純に `https://` または `http://` (セキュリティで保護された接続が必要であるかどうかに依存する) の後に、同期サーバーの完全修飾ドメイン名を続けたものです。 たとえば、 **https:/\/sync1.contoso.com のよう**になります。

複数のユーザーの属性にデータを挿入するには、Active Directory PowerShell を使用します。 手順 5. で説明した *HR Sync Share Users* グループのすべてのメンバーに対して属性を設定する例を次に示します。
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>手順 10:必要に応じて、Web アプリケーションプロキシ、Azure AD アプリケーションプロキシ、または別のリバースプロキシを構成します。  

リモート ユーザーが自分のファイルにアクセスできる環境を整えるためには、リバース プロキシによってワーク フォルダーを公開して、外部のインターネットからワーク フォルダーを利用できるように構成する必要があります。 これには Web アプリケーション プロキシ、Azure Active Directory アプリケーション プロキシ、または別のリバース プロキシ ソリューションを使用できます。  
  
AD FS と Web アプリケーション プロキシを使ってワーク フォルダーのアクセスを構成する手順については、「[AD FS と Web アプリケーション プロキシ (WAP) を使ったワーク フォルダーの展開](deploy-work-folders-adfs-overview.md)」をご覧ください。 Web アプリケーション プロキシの背景情報については、「[Windows Server 2016 の Web アプリケーション プロキシ](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)」を参照してください。 ワーク フォルダーなどのアプリケーションの、Web アプリケーション プロキシを使用したインターネット上への公開について詳しくは、「[AD FS の事前認証を使用してアプリケーションを公開する](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication)」をご覧ください。
 
Azure Active Directory アプリケーション プロキシを使って、ワーク フォルダーのアクセスを構成する手順については、「[Azure Active Directory アプリケーション プロキシを使って、ワーク フォルダーへのリモート アクセスを有効にする](https://blogs.technet.microsoft.com/filecab/?p=7945)」をご覧ください。 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>手順 11:必要に応じてグループ ポリシーを使用してドメインに参加している PC を構成する  

ワーク フォルダーを展開する対象の、ドメインに参加している PC の数が多い場合は、グループ ポリシーを使用して、次のクライアント PC の構成タスクを実行できます。  
  
- ユーザーが同期する同期サーバーを指定する  
  
- 既定の設定を使用して、自動的にワーク フォルダーをセットアップする (この作業を行う前に「[ワーク フォルダーの実装の設計](plan-work-folders.md)」のグループ ポリシーに関する説明を参照してください)。  
  
  これらの設定を制御するには、ワーク フォルダー用の新しいグループ ポリシー オブジェクト (GPO) を作成し、必要に応じて次のグループ ポリシー設定を構成します。  
  
- "ユーザーの構成\ポリシー\管理用テンプレート\Windows コンポーネント\WorkFolders" の "Work Folders の設定を指定する" ポリシー設定  
  
- "コンピューターの構成\ポリシー\管理用テンプレート\Windows コンポーネント\WorkFolders" の "すべてのユーザーに自動セットアップを強制する" ポリシー設定  
  
> [!NOTE]
>  これらのポリシー設定は、Windows 8.1 または Windows Server 2012 R2 以降でグループ ポリシーの管理を実行しているコンピューターからグループ ポリシーを編集する場合にのみ利用できます。 以前のオペレーティング システムに含まれるバージョンのグループ ポリシーの管理では、この設定は利用できません。 これらのポリシー設定は、[Windows 7 のワーク フォルダー](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) アプリがインストールされている Windows 7 PC に適用されます。  
  
##  <a name="BKMK_LINKS"></a>関連項目  
 その他の関連情報については、次の情報を参照してください。  
  
|コンテンツの種類|リファレンス|  
|------------------|----------------|  
|**正しく**|-   [ワークフォルダー](work-folders-overview.md)|  
|**計画**|-   [ワークフォルダーの実装の設計](plan-work-folders.md)|
|**展開**|-   [AD FS と Web アプリケーションプロキシ (WAP) を使用したワークフォルダーの展開](deploy-work-folders-adfs-overview.md)<br />-   [ワークフォルダーのテストラボの展開](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx)(ブログの投稿)<br />-   [ワークフォルダーサーバーの Url の新しいユーザー属性](http://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx)(ブログの投稿)|  
|**テクニカル リファレンス**|-   [対話型ログオン:コンピューターアカウントのロックアウトのしきい値](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [同期共有のコマンドレット](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|
