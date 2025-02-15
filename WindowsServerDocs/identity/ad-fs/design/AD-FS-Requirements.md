---
ms.assetid: 8ce6e7c4-cf8e-4b55-980c-048fea28d50f
title: SQL Server を使用するフェデレーション サーバー ファーム
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 720c20437f7e6da875b809b2816f0d4df5d210d6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359192"
---
# <a name="ad-fs-requirements"></a>AD FS の要件

AD FS を展開するときに従う必要があるさまざまな要件を次に示します。  
  
-   5\. コマンド プロンプトで、「[ipconfig /all](AD-FS-Requirements.md#BKMK_1)」と入力して Enter キーを押します。  
  
-   [ハードウェア要件](AD-FS-Requirements.md#BKMK_2)  
  
-   [ソフトウェア要件](AD-FS-Requirements.md#BKMK_3)  
  
-   [AD DS の要件](AD-FS-Requirements.md#BKMK_4)  
  
-   [構成データベースの要件](AD-FS-Requirements.md#BKMK_5)  
  
-   [ブラウザーの要件](AD-FS-Requirements.md#BKMK_6)  
  
-   [エクストラネットの要件](AD-FS-Requirements.md#BKMK_extranet)  
  
-   [ネットワーク要件](AD-FS-Requirements.md#BKMK_7)  
  
-   [属性ストアの要件](AD-FS-Requirements.md#BKMK_8)  
  
-   [アプリケーションの要件](AD-FS-Requirements.md#BKMK_9)  
  
-   [認証の要件](AD-FS-Requirements.md#BKMK_10)  
  
-   [Workplace join の要件](AD-FS-Requirements.md#BKMK_11)  
  
-   [暗号化の要件](AD-FS-Requirements.md#BKMK_12)  
  
-   [アクセス許可の要件](AD-FS-Requirements.md#BKMK_13)  
  
## <a name="BKMK_1"></a>証明書の要件  
証明書は、フェデレーションサーバー、web アプリケーションプロキシ、要求\-対応アプリケーション、および web クライアント間の通信をセキュリティで保護するうえで最も重要な役割を果たします。 証明書の要件は、このセクションで説明するように、フェデレーションサーバーまたはプロキシコンピューターを設定するかどうかによって異なります。  
  
**フェデレーションサーバーの証明書**  
  
|||  
|-|-|  
|**証明書の種類**|**要件、サポート & を理解する**|  
|**Secure Sockets Layer \(SSL\)証明書:** これは、フェデレーションサーバーとクライアント間の通信をセキュリティで保護するために使用される標準の SSL 証明書です。|-この証明書は、公的に\*信頼されている X509 v3 証明書である必要があります。<br />-AD FS エンドポイントにアクセスするすべてのクライアントは、この証明書を信頼する必要があります。 公共\( \(のサード\-パーティ\)の証明機関CA\)によって発行された証明書を使用することを強くお勧めします。 テストラボ環境のフェデレーション\-サーバーでは、自己署名 SSL 証明書を正常に使用できます。 ただし、運用環境では、パブリック CA から証明書を取得することをお勧めします。<br />-Windows Server 2012 R2 でサポートされている SSL 証明書の任意のキーサイズをサポートします。<br />-CNG キーを使用する証明書はサポートされません。<br />-Workplace Join\/デバイス登録サービスと共に使用する場合、AD FS サービスの SSL 証明書のサブジェクト代替名には、その後にユーザープリンシパル名\(が続くenterpriseregistration値が含まれている必要があります。組織\)の UPN サフィックス (たとえば、enterpriseregistration.contoso.com)。<br />-ワイルドカード証明書がサポートされています。 AD FS ファームを作成するときに、AD FS サービス\(のサービス名 ( **adfs.contoso.com**など) を入力するように求められます。<br />-Web アプリケーションプロキシに同じ SSL 証明書を使用することを強くお勧めします。 ただし、Web アプリケーションプロキシを介して Windows 統合認証エンドポイントをサポートする場合と、拡張保護認証を既定の設定\)で\(有効にする場合は、これが同じである**必要**があります。<br />-この証明書のサブジェクト名は、展開する AD FS の各インスタンスのフェデレーションサービス名を表すために使用されます。 このため、パートナーに対して会社または組織の名前を最もよく表す\-証明書を発行した新しい CA でサブジェクト名を選択することを検討してください。<br />    証明書の id は、フェデレーションサービス名\((fs.contoso.com\)など) と一致している必要があります。Id は、dNSName 型のサブジェクト代替名拡張であるか、サブジェクト代替名のエントリがない場合は、共通名として指定されたサブジェクト名のいずれかになります。 証明書には複数のサブジェクト代替名エントリを指定できます。そのうちの1つがフェデレーションサービス名と一致しています。<br />-   **重要:** AD FS ファーム内のすべての Web アプリケーションプロキシだけでなく、AD FS ファームのすべてのノードで同じ SSL 証明書を使用することを強くお勧めします。|  
|**サービス通信証明書:** この証明書は、フェデレーション サーバー間の通信のセキュリティを確保するための WCF メッセージ セキュリティに必要です。|-既定では、SSL 証明書がサービス通信証明書として使用されます。  ただし、サービス通信証明書として別の証明書を構成することもできます。<br />-   **重要:** サービス通信証明書として ssl 証明書を使用している場合、ssl 証明書の有効期限が切れると、更新された ssl 証明書がサービス通信証明書として構成されていることを確認してください。 これは自動的には行われません。<br />-この証明書は、WCF メッセージセキュリティを使用する AD FS のクライアントによって信頼されている必要があります。<br />\(-公共\)のサード\-パーティ\)の証明機関\(CA によって発行されたサーバー認証証明書を使用することをお勧めします。<br />-サービス通信証明書を、CNG キーを使用する証明書にすることはできません。<br />-この証明書は、AD FS 管理コンソールを使用して管理できます。|  
|**トークン\-署名証明書:** これは標準の X509 証明書であり、フェデレーション サーバーが発行するすべてのトークンについて署名のセキュリティを確保するために使用されます。|-既定では、AD FS によっ\-て、2048ビットキーを持つ自己署名証明書が作成されます。<br />-CA が発行した証明書もサポートされており、AD FS\-管理スナップインを使用して変更できます。<br />-CA が発行した証明書は、CSP 暗号化プロバイダーを介してアクセス & 格納されている必要があります。<br />-トークン署名証明書を、CNG キーを使用する証明書にすることはできません。<br />-AD FS では、トークン署名に外部で登録された証明書は必要ありません。<br />    有効期限が切れる前\-に自己署名入り証明書が自動的に更新されます。まず、新しい証明書をセカンダリ証明書として構成し、パートナーがそれらを使用できるようにします。次に、[自動] というプロセスでプライマリに切り替えます。 AD FS証明書のロールオーバー。トークン署名に対して自動的に生成された既定の証明書を使用することをお勧めします。<br />    トークン署名用に別の証明書を構成する必要があるポリシーが組織にある場合は、インストール時に、 \(Powershell を使用して証明書を指定することができます。この場合、インストールの– signingcertificatethumbprint パラメーターを使用します。AdfsFarm コマンド\)レット。 \-  インストール後、AD FS 管理コンソール、または get-adfscertificate と Get\-\-get-adfscertificate を設定した Powershell コマンドレットを使用して、トークン署名証明書を表示および管理できます。<br />    外部で登録された証明書をトークン署名に使用した場合、AD FS は証明書の自動更新またはロールオーバーを実行しません。  このプロセスは、管理者が実行する必要があります。<br />    1つの証明書の有効期限が近づいたときに証明書のロールオーバーを許可するには、AD FS でセカンダリトークン署名証明書を構成できます。 既定では、すべてのトークン署名証明書はフェデレーションメタデータで公開され\-ますが、実際にトークンに署名するために AD FS によって使用されるのはプライマリトークン署名証明書のみです。|  
|**トークン\-暗号化\/解除暗号化証明書:** これは、すべての受信トークンの暗号化を解除\/するために使用される標準の X509 証明書です。 これは、フェデレーション メタデータでも公開されています。|-既定では、AD FS によっ\-て、2048ビットキーを持つ自己署名証明書が作成されます。<br />-CA が発行した証明書もサポートされており、AD FS\-管理スナップインを使用して変更できます。<br />-CA が発行した証明書は、CSP 暗号化プロバイダーを介してアクセス & 格納されている必要があります。<br />-トークン\-暗号化解除\/の暗号化証明書を、CNG キーを使用する証明書にすることはできません。<br />-既定では、AD FS は、トークンの暗号化解除のために\-、内部的に生成された独自の自己署名証明書を生成して使用します。  AD FS には、この目的で外部に登録された証明書は必要ありません。<br />    さらに、AD FS は、有効期限\-が切れる前に自己署名証明書を自動的に更新します。<br />    **トークンの暗号化解除には、自動的に生成された既定の証明書を使用することをお勧めします。**<br />    トークンの暗号化解除用に異なる証明書を構成する必要があるポリシーが組織にある場合は、Powershell \(を使用してインストール時に証明書を指定することができます。AdfsFarm\-コマンドレット\)をインストールします。  インストール後、AD FS 管理コンソール、または get-adfscertificate と Get\-\-get-adfscertificate を設定した Powershell コマンドレットを使用して、トークン暗号化解除証明書を表示および管理できます。<br />    **外部で登録された証明書をトークンの暗号化解除に使用する場合、AD FS は証明書の自動更新を実行しません。このプロセスは、管理者**が実行する必要があります。<br />-AD FS サービスアカウントは、ローカルコンピューターの個人用ストア\-にあるトークン署名証明書の秘密キーにアクセスできる必要があります。 この処理は、セットアップによって行われます。 また、AD FS 管理スナップイン\-を使用して、後でトークン\-署名証明書を変更した場合にこのアクセスを確認することもできます。|  
  
> [!CAUTION]  
> トークン\-署名とトークン\-暗号化解除\/の暗号化に使用される証明書は、フェデレーションサービスの安定性を確保するために重要です。 独自のトークン\-署名 & トークン\-を管理し\/ているお客様は、証明書の暗号化を解除するため、これらの証明書がバックアップされ、復旧イベントの発生時に個別に使用できる必要があります。  
  
> [!NOTE]  
> AD FS では、デジタル署名に使用さ\(れる\)セキュリティで保護されたハッシュアルゴリズムの sha\-レベルを、\-sha 1 また\)は sha 256 \(の安全性が高くなるように変更できます。 AD FS は、Makecert コマンド\(\-ラインツール\)で使用される既定のハッシュアルゴリズムである MD5 など、他のハッシュ方式での証明書の使用をサポートしていません。 セキュリティのベストプラクティスとして、すべての署名に\-既定\(\)で設定されている SHA 256 を使用することをお勧めします。 Sha\-1 は、Microsoft 以外\-の製品や AD FS のレガシバージョンなど、sha\-256 を使用した通信をサポートしていない製品と相互運用する必要がある場合にのみ使用することをお勧めします。  
  
> [!NOTE]  
> CA から証明書を受け取ったら、すべての証明書がローカル コンピューターの個人証明書ストアにインポートされていることを確認してください。 証明書 MMC スナップイン\-を使用して、個人用ストアに証明書をインポートできます。  
  
## <a name="BKMK_2"></a>ハードウェア要件  
Windows Server 2012 R2 の AD FS フェデレーションサーバーには、次の最小および推奨ハードウェア要件が適用されます。  
  
||||  
|-|-|-|  
|**ハードウェア要件**|**最小要件**|**推奨要件**|  
|CPU 速度|1.4 GHz 64\-ビットプロセッサ|クワッド\-コア、2 GHz|  
|RAM|512 MB|4 GB|  
|ディスク領域|32 GB|100 GB|  
  
## <a name="BKMK_3"></a>ソフトウェア要件  
次の AD FS 要件は、Windows Server® 2012 R2 オペレーティングシステムに組み込まれているサーバーの機能を対象としています。  
  
-   エクストラネットアクセスの場合は、Windows server® 2012 R2 リモートアクセス\-サーバーの役割の Web アプリケーションプロキシ役割サービスの一部を展開する必要があります。 以前のバージョンのフェデレーションサーバープロキシは、Windows Server® 2012 R2 の AD FS ではサポートされていません。  
  
-   フェデレーションサーバーと Web アプリケーションプロキシの役割サービスを同じコンピューターにインストールすることはできません。  
  
## <a name="BKMK_4"></a>AD DS の要件  
**ドメインコントローラーの要件**  
  
すべてのユーザードメインおよび AD FS サーバーが参加しているドメインのドメインコントローラーは、Windows Server 2008 以降を実行している必要があります。  
  
> [!NOTE]  
> Windows Server 2003 ドメインコントローラーを使用する環境のすべてのサポートは、Windows Server 2003 の延長サポート終了日後に終了します。 お客様は、できるだけ早くドメインコントローラーをアップグレードすることを強くお勧めします。 Microsoft サポートのライフサイクルの詳細については、[このページ](https://support.microsoft.com/lifecycle/search/default.aspx?sort=PN&alpha=Windows+Server+2003&Filter=FilterNO)を参照してください。 Windows Server 2003 ドメインコントローラー環境に固有の問題が検出された場合、修正プログラムはセキュリティの問題に対してのみ発行され、Windows Server 2003 の延長サポートの有効期限が切れる前に修正プログラムを発行できる場合にのみ発行されます。  
  
**ドメインの\-機能レベルの要件**  
  
AD FS サーバーが参加しているすべてのユーザーアカウントドメインとドメインは、Windows Server 2003 以降のドメインの機能レベルで動作している必要があります。  
  
ほとんどの AD FS 機能では、機能\-レベルの変更を正常に動作させる AD DS 必要はありません。 ただし、クライアント証明書認証の証明書が明示的に AD DS 内のユーザー アカウントにマッピングされている場合に認証を正しく動作させるには、ドメイン機能レベルが Windows Server 2008 以上であることが必要です。  
  
**スキーマの要件**  
  
-   AD FS では、AD DS に対するスキーマ\-の変更や機能レベルの変更は必要ありません。  
  
-   Workplace Join 機能を使用するには AD FS サーバーが参加しているフォレストのスキーマを Windows Server 2012 R2 に設定する必要があります。  
  
**サービスアカウントの要件**  
  
-   標準サービスアカウントは、AD FS のサービスアカウントとして使用できます。 グループの管理されたサービスアカウントもサポートされています。 このためには、少なくと\(も1つのドメインコントローラーが必要です\) 。 Windows Server 2012 以降を実行している2つ以上のドメインコントローラーを展開することをお勧めします。  
  
-   ドメイン\-に参加しているクライアントと AD FS 間で Kerberos 認証を機能させる\_には、サービスアカウントに ' HOST\/< adfs サービス\_名 > ' を SPN として登録する必要があります。 既定では、この操作を実行するための十分なアクセス許可がある場合、新しい AD FS ファームを作成するときに、AD FS によって構成されます。  
  
-   AD FS サービスアカウントは、AD FS サービスに対して認証を行うユーザーを含むすべてのユーザードメインで信頼されている必要があります。  
  
**ドメインの要件**  
  
-   すべての AD FS サーバーは、AD DS ドメインに参加している必要があります。  
  
-   ファーム内のすべての AD FS サーバーを1つのドメインに展開する必要があります。  
  
-   AD FS サーバーが参加しているドメインは、AD FS サービスに対して認証を行うユーザーを含むすべてのユーザーアカウントドメインを信頼する必要があります。  
  
**複数フォレストの要件**  
  
-   AD FS サーバーが参加しているドメインは、AD FS サービスに対して認証を行うユーザーを含むすべてのユーザーアカウントドメインまたはフォレストを信頼する必要があります。  
  
-   AD FS サービスアカウントは、AD FS サービスに対して認証を行うユーザーを含むすべてのユーザードメインで信頼されている必要があります。  
  
## <a name="BKMK_5"></a>構成データベースの要件  
次に、構成ストアの種類に基づいて適用される要件と制限を示します。  
  
**WID**  
  
-   100を超える証明書利用者信頼がある場合、WID ファームには、最大30個のフェデレーションサーバーがあります。  
  
-   SAML 2.0 のアーティファクト解決プロファイルは、WID 構成データベースではサポートされていません。  トークンリプレイ検出は、WID 構成データベースではサポートされていません。 この機能は、AD FS がフェデレーションプロバイダとして機能し、外部要求プロバイダからのセキュリティトークンを使用する場合にのみ使用されます。  
  
-   フェールオーバーまたは地理的負荷分散のために個別のデータセンターに AD FS サーバーを展開することは、サーバーの数が30を超えない限りサポートされます。  
  
WID ファームを使用する場合の概要を次の表に示します。  実装を計画するときに使用します。  
  
||||  
|-|-|-|  
||1 \- 100 RP 信頼|100を超える RP 信頼|  
|1 \-個の AD FS ノード|WID がサポートされる|WID \- SQL が必要な場合はサポートされません|  
|30を超える AD FS ノード|WID \- SQL が必要な場合はサポートされません|WID \- SQL が必要な場合はサポートされません|  
  
**SQL Server**  
  
Windows Server 2012 R2 の AD FS については SQL Server 2008 以降を使用できます。  
  
## <a name="BKMK_6"></a>ブラウザーの要件  
ブラウザーまたはブラウザーコントロールを使用して AD FS 認証を実行する場合は、ブラウザーが次の要件を満たしている必要があります。  
  
-   JavaScript を有効にする必要があります  
  
-   Cookie を有効にする必要があります  
  
-   Server Name Indication \(SNI\)がサポートされている必要があります  
  
-   ユーザー証明書 & デバイス証明書\(の認証 workplace\)join 機能については、ブラウザーで SSL クライアント証明書の認証をサポートする必要があります。  
  
いくつかの主要なブラウザーとプラットフォームでは、レンダリングと機能の検証が行われています。詳細は次のとおりです。 この表に記載されていないブラウザーおよびデバイスは、上記の要件を満たしている場合は引き続きサポートされます。  
  
|||  
|-|-|  
|**マイクロ**|**Platforms**|  
|IE 10.0|Windows 7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|IE 11.0|Windows7、Windows 8.1、Windows Server 2008 R2、Windows Server 2012、Windows Server 2012 R2|  
|Windows Web 認証ブローカー|Windows 8.1|  
|Firefox \[v21\]|Windows 7、Windows 8.1|  
|Safari \[v7\]|iOS 6、Mac OS\-X 10.7|  
|Chrome \[v27\]|Windows 7、Windows 8.1、windows server 2012、windows server 2012 R2、Mac OS\-X 10.7|  
  
> [!IMPORTANT]  
> 既知の\-問題 (Firefox):デバイス証明書を使用してデバイスを識別する Workplace Join 機能は、Windows プラットフォームでは機能しません。 Firefox は、現在、Windows クライアントのユーザー証明書ストアにプロビジョニングされた証明書を使用した SSL クライアント証明書の認証の実行をサポートしていません。  
  
**Cookie**  
  
AD FS は、\-サインイン\-、サインアウト\-、シングルサイン\-オン\(SSO\)、およびその他の機能を提供するために、クライアントコンピューターに保存する必要があるセッションベースの永続的な cookie を作成します。 したがって、クライアント ブラウザーは Cookie を受け入れるように設定されている必要があります。 認証に使用される cookie は、常に、セキュリティ\(で\)保護されたハイパーテキスト転送プロトコルの HTTPS セッション cookie であり、元のサーバー用に書き込まれます。 クライアント ブラウザーがこの Cookie を受け入れるように設定されていない場合は、AD FS が正しく動作することができません。 永続的 Cookie は、ユーザーが選択したクレーム プロバイダーを記憶するために使用されます。 構成ファイルの構成設定を使用して、AD FS サインイン\-ページで無効にすることができます。 セキュリティ上の\/理由により、TLS SSL のサポートが必要です。  
  
## <a name="BKMK_extranet"></a>エクストラネットの要件  
AD FS サービスへのエクストラネットアクセスを提供するには、Web アプリケーションプロキシ役割サービスを、セキュリティで保護された方法で AD FS サービスに対してプロキシするエクストラネット接続ロールとしてデプロイする必要があります。 これにより、AD FS サービスエンドポイントを分離できるだけでなく、インターネット\(から送信される要求\)からのトークン署名証明書など、すべてのセキュリティキーを分離することができます。 さらに、ソフトエクストラネットのアカウントロックアウトなどの機能では、Web アプリケーションプロキシを使用する必要があります。 Web アプリケーションプロキシの詳細については、「 [Web アプリケーションプロキシ](https://technet.microsoft.com/library/dn584107.aspx)」を参照してください。  
  
エクストラネットアクセスに3番目の @ no__t-0party プロキシを使用する場合は、この3番目の @ no__t パーティプロキシは、http: \/\/download.microsoft.com @ no__t に定義されているプロトコルをサポートする必要があり[ます。 @ no__t-69 @ no__t-75 @ no__t-8E @ no__t-99 5EF66AF @ no__t-109026 @ no__t-114BB0 @ no__t-12A41D @ no__t-13A4F81802D92C @ no__t-14%5bMS\-5ADFSPIP%5d.pdf](https://download.microsoft.com/download/9/5/E/95EF66AF-9026-4BB0-A41D-A4F81802D92C/%5bMS-ADFSPIP%5d.pdf)  
  
## <a name="BKMK_7"></a>ネットワーク要件  
組織に AD FS を正常に展開するには、次のネットワークサービスを適切に構成することが重要です。  
  
**企業ファイアウォールの構成**  
  
Web アプリケーションプロキシとフェデレーションサーバーファームの間にあるファイアウォールと、クライアントと Web アプリケーションプロキシ間のファイアウォールの両方で、TCP ポート443が受信可能である必要があります。  
  
さらに、X509 ユーザー証明書\(\)を使用するクライアントユーザー証明書認証 clienttls 認証が必要な場合、Windows Server 2012 R2 の AD FS では、ファイアウォールで TCP ポート49443を受信できるようにする必要があります。クライアントおよび Web アプリケーションプロキシ。 これは、Web アプリケーションプロキシとフェデレーションサーバー\)の間のファイアウォールでは必要ありません。  
  
**DNS の構成**  
  
-   イントラネットにアクセスするには、社内\(ネットワークイントラネット\)内の AD FS サービスにアクセスするすべてのクライアントが、SSL 証明\(書\)によって提供される AD FS サービス名名を負荷に解決できる必要があります。AD FS サーバーまたは AD FS サーバーのバランサー。  
  
-   エクストラネットアクセスの場合\(、企業ネットワークのエクストラネット\/インターネット\)の外部から AD FS サービスにアクセスするすべてのクライアントは、SSL 証明\(書によって提供されるADFSサービス名を解決できる必要があります。\) Web アプリケーションプロキシサーバーまたは Web アプリケーションプロキシサーバーのロードバランサー。  
  
-   エクストラネットアクセスが正常に機能するには、DMZ 内の各 Web アプリケーションプロキシサーバーが、SSL 証明書\(\)によって提供される AD FS サービス名名を、AD FS サーバーまたは AD FS サーバーのロードバランサーに解決できる必要があります。 これは、DMZ ネットワークの代替 DNS サーバーを使用するか、HOSTS ファイルを使用してローカルサーバーの解決策を変更することによって実現できます。  
  
-   Web アプリケーションプロキシを介して公開されるエンドポイントのサブセットについて、Windows 統合認証がネットワーク内およびネットワーク外部で動作する場合\(は、\) CNAME ではない a レコードを使用してロードバランサーをポイントする必要があります。  
  
フェデレーションサービスとデバイス登録サービス用に企業 DNS を構成する方法については、「[フェデレーションサービスと DRS 用に企業](https://technet.microsoft.com/library/dn486786.aspx)Dns を構成する」を参照してください。  
  
Web アプリケーションプロキシ用の企業 dns の構成の詳細について[は、「手順 1:Web アプリケーションプロキシインフラストラクチャ](https://technet.microsoft.com/library/dn383644.aspx)を構成します。  
  
NLB を使用してクラスターの IP アドレスまたはクラスターの FQDN を構成する方法については、「 [http\/:\/\/\/go.microsoft.com fwlink でのクラスターパラメーターの指定」を参照してください。LinkId\=75282](https://go.microsoft.com/fwlink/?LinkId=75282)。  
  
## <a name="BKMK_8"></a>属性ストアの要件  
AD FS では、ユーザーを認証し、それらのユーザーのセキュリティ要求を抽出するために、少なくとも1つの属性ストアを使用する必要があります。 AD FS がサポートする属性ストアの一覧については、「[属性ストアの役割](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md)」を参照してください。  
  
> [!NOTE]  
> 既定では、"Active Directory" 属性ストアが AD FS によって自動的に作成されます。 属性ストアの要件\(は、フェデレーションユーザー\)をホストするアカウントパートナーとして、またはフェデレーションアプリケーション\)を\(ホストしているリソースパートナーとして、組織が動作しているかどうかによって異なります。  
  
**LDAP 属性ストア**  
  
他のライトウェイトディレクトリアクセスプロトコル\(LDAP\)\-ベースの属性ストアを使用する場合は、Windows 統合認証をサポートする ldap サーバーに接続する必要があります。 LDAP 接続文字列は、RFC 2255 での説明に従い、LDAP URL の形式で記述されている必要があります。  
  
また、AD FS サービスのサービスアカウントに、LDAP 属性ストアのユーザー情報を取得する権限があることも必要です。  
  
**属性ストアの SQL Server**  
  
Windows Server 2012 R2 の AD FS を正常に動作させるには、SQL Server の属性ストアをホストするコンピューターで Microsoft SQL Server 2008 以上を実行している必要があります。 SQL\-ベースの属性ストアを使用する場合は、接続文字列も構成する必要があります。  
  
**カスタム属性ストア**  
  
複雑なシナリオを実現するために、独自の属性ストアを開発することができます。  
  
-   AD FS 内蔵のポリシー言語からカスタム属性ストアを参照できるので、次に示すようなシナリオの拡張が可能になります。  
  
    -   ローカルで認証されるユーザーのクレームを作成する  
  
    -   外部で認証されるユーザーのクレームを補足する  
  
    -   トークンを取得することをユーザーに許可する  
  
    -   ユーザーの行動に基づいてトークンを取得することをサービスに許可する  
  
    -   AD FS によって発行されたセキュリティトークンの追加データを証明書利用者に発行する。  
  
-   すべてのカスタム属性ストアは、.NET 4.0 以降の上に構築する必要があります。  
  
カスタム属性ストアを使用する場合は、接続文字列も構成する必要があります。 その場合は、カスタム属性ストアへの接続を有効にする任意のカスタムコードを入力できます。 この場合の接続文字列は、名前\/と値のペアのセットであり、カスタム属性ストアの開発者によって実装されたものと解釈されます。カスタム属性ストアの開発と使用の詳細については、「[属性ストアの概要](https://go.microsoft.com/fwlink/?LinkId=190782)」を参照してください。  
  
## <a name="BKMK_9"></a>アプリケーションの要件  
AD FS は、\-次のプロトコルを使用する要求に対応するアプリケーションをサポートしています。  
  
-   WS\-フェデレーション  
  
-   Ws-trust\-  
  
-   IDPLite &、eGov 1.5 プロファイルを使用した SAML 2.0 プロトコル。  
  
-   OAuth 2.0 Authorization Grant Profile  
  
AD FS は、Web アプリケーションプロキシでサポートさ\-れ\-ている要求に対応していないアプリケーションの認証と承認もサポートします。  
  
## <a name="BKMK_10"></a>認証の要件  
**AD DS 認証\(プライマリ認証\)**  
  
イントラネットアクセスの場合、AD DS の次の標準的な認証メカニズムがサポートされています。  
  
-   Kerberos & NTLM に Negotiate を使用した Windows 統合認証  
  
-   ユーザー名\/パスワードを使用したフォーム認証  
  
-   AD DS のユーザーアカウントにマップされた証明書を使用した証明書認証  
  
エクストラネットアクセスの場合、次の認証メカニズムがサポートされています。  
  
-   ユーザー名\/パスワードを使用したフォーム認証  
  
-   AD DS のユーザーアカウントにマップされている証明書を使用した証明書認証  
  
-   Windows 統合認証を受け入れる\(WS\-信頼\)エンドポイントに対してのみ、Negotiate NTLM を使用した windows 統合認証。  
  
証明書認証の場合:  
  
-   は、pin で保護できるスマートカードにまで拡張されます。  
  
-   ユーザーが pin を入力するための GUI は、AD FS によって提供されるものではなく、クライアント TLS を使用するときに表示されるクライアントオペレーティングシステムの一部である必要があります。  
  
-   スマートカードのリーダーと暗号化\(サービス\)プロバイダーの CSP は、ブラウザーが配置されているコンピューターで動作する必要があります。  
  
-   スマートカード証明書は、すべての AD FS サーバーと Web アプリケーションプロキシサーバーの信頼されたルートにチェーンされている必要があります。  
  
-   証明書は、AD DS 内のユーザー アカウントに、次のいずれかの方法でマッピングされている必要があります。  
  
    -   証明書サブジェクト名が、AD DS 内のユーザー アカウントの LDAP 識別名に対応している。  
  
    -   証明書のサブジェクト別名拡張機能には、 \(AD DS\)のユーザーアカウントのユーザープリンシパル名 UPN が含まれています。  
  
イントラネットで Kerberos を使用したシームレスな Windows 統合認証の場合  
  
-   このサービス名は、信頼済みサイトまたはローカルイントラネットサイトの一部として使用する必要があります。  
  
-   また、AD FS ファームが\/実行さ\_れているサービスアカウントには、ホスト < adfs サービス\_名 > SPN を設定する必要があります。  
  
**Multi-factor\-authentication**  
  
AD FS は、ベンダー \(\) \/の顧客が独自の multi-factor\-authentication アダプターを構築できるプロバイダーモデルを使用して、AD DS でサポートされるプライマリ認証以外の追加の認証をサポートします。ログイン時に管理者が登録して使用することができます。  
  
すべての MFA アダプターは、.NET 4.5 上に構築する必要があります。  
  
MFA の詳細については、「[機密性の高いアプリケーションの追加 Multi-Factor Authentication によるリスク管理](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)」を参照してください。  
  
**デバイス認証**  
  
AD FS は、エンドユーザーがデバイスに参加するときに、デバイス登録サービスによってプロビジョニングされた証明書を使用したデバイス認証をサポートします。  
  
## <a name="BKMK_11"></a>Workplace join の要件  
エンドユーザーは、AD FS を使用して自分のデバイスを組織に参加させることができます。 これは AD FS のデバイス登録サービスでサポートされています。 その結果、エンドユーザーは AD FS でサポートされているアプリケーション全体で SSO の利点を享受できます。 また、管理者は、組織に社内参加しているデバイスに対してのみアプリケーションへのアクセスを制限することで、リスクを管理できます。 次に、このシナリオを有効にするための要件を示します。  
  
-   AD FS は Windows 8.1 および iOS 5\+デバイスの workplace join をサポートしています  
  
-   Workplace Join 機能を使用するには、AD FS サーバーが参加しているフォレストのスキーマが Windows Server 2012 R2 である必要があります。  
  
-   AD FS サービスの SSL 証明書のサブジェクト代替名には、次のように、組織のユーザープリンシパル名\(UPN\)サフィックスが続く値 enterpriseregistration が含まれている必要があります。enterpriseregistration.corp.contoso.com。  
  
## <a name="BKMK_12"></a>暗号化の要件  
次の表は、AD FS トークン署名、トークン暗号化\/復号化機能に関する追加の暗号化サポート情報を示しています。  
  
||||  
|-|-|-|  
|**アルゴリズム**|**キーの長さ**|**プロトコル\/アプリケーション\/のコメント**|  
|TripleDES – Default 192 \(supported 192 – 256\) \- [http:\/\/www.w3.org\/200104\/xmlenc\#TripleDES\/cbc\-](http://www.w3.org/2001/04/xmlenc#tripledes-cbc)|>\=192|セキュリティトークンを復号化するためにサポートされているアルゴリズム。 このアルゴリズムを使用したセキュリティトークンの暗号化はサポートされていません。|  
|AES128 \- [http:\/www.w3.org\/200104xmlenc\#AES128cbc\-\/\/\/](http://www.w3.org/2001/04/xmlenc#aes128-cbc)|128|セキュリティトークンを復号化するためにサポートされているアルゴリズム。 このアルゴリズムを使用したセキュリティトークンの暗号化はサポートされていません。|  
|AES192 \- [http:\/www.w3.org\/200104xmlenc\#AES192cbc\-\/\/\/](http://www.w3.org/2001/04/xmlenc#aes192-cbc)|192|セキュリティトークンを復号化するためにサポートされているアルゴリズム。 このアルゴリズムを使用したセキュリティトークンの暗号化はサポートされていません。|  
|AES256 \- [http:\/www.w3.org\/200104xmlenc\#AES256cbc\-\/\/\/](http://www.w3.org/2001/04/xmlenc#aes256-cbc)|256|**[既定]** 。 セキュリティトークンを暗号化するためにサポートされているアルゴリズム。|  
|Tripledeskeywrap) \- [http:\/www.w3.org\/200104xmlenc\#kwtripledes\-\/\/\/](http://www.w3.org/2001/04/xmlenc#kw-tripledes)|.NET 4.0 でサポートされているすべてのキーサイズ\+|セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|Aes128keywrap) \- [http:\/www.w3.org\/200104xmlenc\#kwaes128\-\/\/\/](http://www.w3.org/2001/04/xmlenc#kw-aes128)|128|セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|Aes192keywrap) \- [http:\/www.w3.org\/200104xmlenc\#kwaes192\-\/\/\/](http://www.w3.org/2001/04/xmlenc#kw-aes192)|192|セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|Aes256keywrap) \- [http:\/www.w3.org\/200104xmlenc\#kwaes256\-\/\/\/](http://www.w3.org/2001/04/xmlenc#kw-aes256)|256|セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|RsaV15KeyWrap \- [http:\/www.w3.org\/200104xmlenc\#rsa1\-5\/\/\/\_](http://www.w3.org/2001/04/xmlenc#rsa-1_5)|1024|セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|Rsaoaepkeywrap) \- [http:\/www.w3.org\/200104xmlenc\#rsaoaep\-rsa-oaep-mgf1p\/\/\/\-](http://www.w3.org/2001/04/xmlenc#rsa-oaep-mgf1p)|1024|既定値です。 セキュリティトークンを暗号化する対称キーを暗号化するためにサポートされているアルゴリズム。|  
|Sha1\-[http:\/www.w3.org\/PICSDSig\/SHA110\_.html\/\/\_](http://www.w3.org/PICS/DSig/SHA1_1_0.html)|N\/A|アーティファクトの SourceId 生成で AD FS サーバーによって使用されます。このシナリオでは、STS は SAML \(2.0 標準\)の推奨事項に従って SHA1 を使用して、アーティファクト sourceiD の短い160ビット値を作成します。<br /><br />また、ADFS web agent \(レガシコンポーネントによって WS2003 期間\)から、"最終更新" 時刻値の変更を識別して、STS から情報を更新するタイミングを認識できるようにするために使用されます。|  
|SHA1withRSA\-<br /><br />[http:\/\/www.w3.orgPICSDSig\/RSA SHA1 10\_.html\/\-\/\_](http://www.w3.org/PICS/DSig/RSA-SHA1_1_0.html)|N\/A|AD FS サーバーが SAML authenticationrequest の署名を検証する場合、アーティファクト解決要求または応答に署名する\-場合、トークン署名証明書を作成する場合に使用します。<br /><br />このような場合、sha256 は既定値であり、パートナー \(の\)証明書利用者が sha256 をサポートできず、sha1 を使用する必要がある場合にのみ、sha1 が使用されます。|  
  
## <a name="BKMK_13"></a>アクセス許可の要件  
AD FS のインストールと初期構成を実行する管理者には、ローカルドメイン\(のドメイン管理者のアクセス許可 (フェデレーションサーバーが参加しているドメイン) が必要です。\)  
  
## <a name="see-also"></a>関連項目  
[Windows Server 2012 R2 での AD FS 設計ガイド](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

