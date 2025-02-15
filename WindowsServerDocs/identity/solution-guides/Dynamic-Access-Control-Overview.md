---
ms.assetid: 9ee8a6cb-7550-46e2-9c11-78d0545c3a97
title: ダイナミック アクセス制御の概要
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 343e51f113f54c3965ef45d49f5d8fd64c260991
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357510"
---
# <a name="dynamic-access-control-overview"></a>ダイナミック アクセス制御の概要

>適用先:Windows Server 2012 R2、Windows Server 2012

この IT プロフェッショナル向けのトピックでは、Windows Server 2012 と Windows 8 で導入されたダイナミック アクセス制御とそれに関連する要素の概要について説明します。  
  
ドメインベースのダイナミック アクセス制御によって管理者は、明確に定義された規則 (リソースの秘密度、ユーザーの業務または役割、これらのリソースへのアクセスに使用されるデバイスの構成などを含めることができる) に基づいてアクセス制御の許可と制限を適用できます。  
  
たとえば、ユーザーがオフィスのコンピューターからリソースにアクセスするときは、仮想プライベート ネットワークを介してポータブル コンピューターを使用するときとは異なるアクセス許可を持つ場合があります。 また、ネットワーク管理者が定めたセキュリティ要件をデバイスが満たしている場合にのみアクセスが許可される場合もあります。 動的 Access Control を使用すると、ユーザーのジョブまたはロールが変更された場合 (AD DS でユーザーのアカウント属性が変更された場合)、追加の管理者の介入なしに、ユーザーのアクセス許可が動的に変更されます。  
  
ダイナミック アクセス制御は、Windows Server 2012 と Windows 8 より前の Windows オペレーティング システムではサポートされていません。 ダイナミック アクセス制御がサポートされているバージョンの Windows とサポートされていないバージョンの Windows 環境で構成すると、サポートされているバージョンのみにこの変更が実装されます。  
  
ダイナミック アクセス制御に関連する機能と概念の代表的なものを次に示します。  
  
-   [集約型アクセス規則](#BKMK_Rules)  
  
-   [集約型アクセスポリシー](#BKMK_Policies)  
  
-   [保険](#BKMK_Claims)  
  
-   [式](#BKMK_Expressions2)  
  
-   [提案されたアクセス許可](#BKMK_Permissions2)  
  
### <a name="BKMK_Rules"></a>集約型アクセス規則  
集約型のアクセス規則とは、ユーザー グループ、ユーザーの信頼性情報、デバイスの信頼性情報、リソース プロパティなどが関係する 1 つ以上の条件を含む承認規則のことです。 複数の集約型アクセス規則を、単一の集約型アクセス ポリシーとして結合できます。  
  
1 つ以上の集約型アクセス規則が 1 つのドメインに対して定義された場合、ファイル共有の管理者は特定の規則を特定のリソースおよびビジネス要件と適合させることができます。  
  
### <a name="BKMK_Policies"></a>集約型アクセスポリシー  
集約型アクセス ポリシーは、条件式を含む承認ポリシーです。 たとえば、ファイル内の個人を特定できる情報 (PII) へのアクセスをファイル所有者と人事 (HR) 部門のメンバーのみに制限するビジネス要件が組織にあるとします。 これは、組織内のファイル サーバーにある PII ファイルに適用される組織全体のポリシーを表します。 組織がこのポリシーを実装するためには、次の処理が可能でなければなりません。  
  
-   PII を含むファイルを特定し、マークする。  
  
-   PII 情報を表示できる HR のメンバーを特定およびグループ化する。  
  
-   集約型アクセス規則に集約型アクセス ポリシーを追加し、組織内のすべてのファイル サーバーに保存された、PII を含むすべてのファイルにこの集約型アクセス規則を適用する。  
  
集約型のアクセス ポリシーは、組織のサーバー全体に適用され、上位のセキュリティとして機能します。 これらのポリシーは、ファイルやフォルダーに適用されているローカルのアクセス ポリシーまたは随意アクセス制御リスト (DACL) に追加されるものです (置き換わるわけではありません)。  
  
### <a name="BKMK_Claims"></a>保険  
信頼性情報とは、ドメイン コントローラーによって公開されたユーザー、デバイス、リソースなどに関する一意の情報一式のことです。 要求の有効な例として、ユーザーの役職、ファイルの部署分類、またはコンピューターの正常性状態があります。 各エントリには複数の信頼性情報を含めることができ、信頼性情報を任意に組み合わせてリソース アクセスを承認できます。 サポートされているバージョンの Windows では、次の信頼情報の種類を使用できます。  
  
-   **ユーザーの信頼性情報**。特定のユーザーに関連付けられる Active Directory 属性です。  
  
-   **デバイスの信頼性情報**。特定のコンピューター オブジェクトに関連付けられる Active Directory 属性です。  
  
-   **リソース属性**。承認判断で使用するようにマークされ、Active Directory で公開される、グローバル リソース プロパティです。  
  
信頼性情報を使用して管理者は、式、規則、ポリシーに含めることができる、ユーザー、デバイス、リソースに関する組織またはエンタープライズ全体の明確な声明を作成できます。  
  
### <a name="BKMK_Expressions2"></a>式  
条件式は、アクセス制御管理の拡張機能の 1 つです。条件式を使用することで、特定の条件 (デバイスのグループ メンバーシップ、場所、セキュリティ状態など) が満たされる場合だけリソースへのアクセスを許可または拒否することができます。 式は、ACL エディターの [セキュリティの詳細設定] ダイアログ ボックス、または Active Directory 管理センター (ADAC) の集約型アクセス規則エディターを使用して管理できます。  
  
式は、管理者が複雑化するビジネス環境内で条件を柔軟に使用して重要リソースへのアクセスを管理するのに役立ちます。  
  
### <a name="BKMK_Permissions2"></a>提案されたアクセス許可  
提案されたアクセス許可を利用すると、アクセス制御の設定値を実際に変更することなく、設定値を変更した場合の影響を正確にモデル化できます。  
  
リソースに対する有効なアクセスを予測することで、変更を加える前にリソースのアクセス許可の計画と設定をスムーズに進めることができます。  
  
## <a name="additional-changes"></a>その他の変更  
ダイナミック アクセス制御をサポートするバージョンの Windows では、次の点も改善されています。  
  
### <a name="support-in-the-kerberos-authentication-protocol-to-reliably-provide-user-claims-device-claims-and-device-groups"></a>Kerberos 認証プロトコルにおける、ユーザーの信頼性情報、デバイスの信頼性情報、デバイス グループなどの確実な提供のサポート。  
既定で、サポートされるバージョンの Windows を実行するデバイスは、ダイナミック アクセス制御関連の Kerberos チケット (複合認証に必要なデータが含まれる) を処理できます。 ドメイン コントローラーは、複合認証関連の情報が含まれる Kerberos チケットの発行と対応ができます。 ダイナミック アクセス制御を認識するようにドメインが設定されていると、デバイスは初期認証時にドメイン コントローラーから信頼性情報を受け取り、サービス チケット要求を提出するときに複合認証チケットを受け取ります。 複合認証を行うと、ダイナミック アクセス制御を認識する、リソース上のユーザーとデバイスの識別情報を含むアクセス トークンが生成されます。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>ドメインのダイナミック アクセス制御を有効にするキー配布センター (KDC) グループ ポリシー設定の使用のサポート。  
ドメイン コントローラーはすべて、 **[コンピューターの構成]、[ポリシー]、[管理用テンプレート]、[システム][KDC]、[ダイナミック アクセス制御と Kerberos 防御をサポートする]** の順にアクセスした場所に置かれている管理用テンプレートのポリシー設定を同じにする必要があります。  
  
### <a name="support-for-using-the-key-distribution-center-kdc-group-policy-setting-to-enable-dynamic-access-control-for-a-domain"></a>ドメインのダイナミック アクセス制御を有効にするキー配布センター (KDC) グループ ポリシー設定の使用のサポート。  
ドメイン コントローラーはすべて、 **[コンピューターの構成]、[ポリシー]、[管理用テンプレート]、[システム][KDC]、[ダイナミック アクセス制御と Kerberos 防御をサポートする]** の順にアクセスした場所に置かれている管理用テンプレートのポリシー設定を同じにする必要があります。  
  
### <a name="support-in-active-directory-to-store-user-and-device-claims-resource-properties-and-central-access-policy-objects"></a>Active Directory における、ユーザーの信頼性情報、デバイスの信頼性情報、リソース プロパティ、集約型アクセス ポリシー オブジェクトなどの保存のサポート。  
  
### <a name="support-for-using-group-policy-to-deploy-central-access-policy-objects"></a>グループ ポリシーによる集約型アクセス ポリシー オブジェクトの展開のサポート。  
次のグループポリシー設定を使用すると、集約型アクセスポリシーオブジェクトを組織内のファイルサーバーに展開できます。**Computer Configuration\Policies\ Settings\File System\Central アクセスポリシー**。  
  
### <a name="support-for-claims-based-file-authorization-and-auditing-for-file-systems-by-using-group-policy-and-global-object-access-auditing"></a>グループ ポリシーとグローバル オブジェクト アクセスの監査による、ファイル システムを対象にした信頼性情報ベースのファイル承認/監査のサポート。  
提案されたアクセス許可を使用して集約型アクセス ポリシーの有効なアクセスを監査するには、ステージングされた集約型アクセス ポリシー監査を有効にする必要があります。 コンピューターのこの設定値は、グループ ポリシー オブジェクト (GPO) の **[セキュリティの設定]** の **[監査ポリシーの詳細な構成]** で指定します。 GPO でセキュリティ設定を構成すると、GPO をネットワーク内のコンピューターに展開できます。  
  
### <a name="support-for-transforming-or-filtering-claim-policy-objects-that-traverse-active-directory-forest-trusts"></a>Active Directory フォレストの信頼を走査する信頼性情報ポリシー オブジェクトの変換またはフィルター処理のサポート。  
フォレストの信頼を走査する入力方向または出力方向の信頼性情報のフィルター処理または変換を行うことができます。 信頼性情報のフィルター処理と変換には、次に示す 3 つの基本的なシナリオが存在します。  
  
-   **値ベースのフィルター処理**。フィルターを、信頼性情報の値に基づかせることができます。 これにより、信頼される側のフォレストは、特定の値を持つ信頼性情報が信頼する側のフォレストに送信されることを防ぐことができます。 信頼する側のフォレストのドメイン コントローラーは、値ベースのフィルター処理を使用して、信頼される側のフォレストから入る特定の値を持つ信頼性情報のフィルター処理を行うことによって特権の昇格攻撃を避けることができます。  
  
-   **信頼性情報の種類ベースのフィルター処理**。信頼性情報の値ではなく信頼性情報の種類に基づいたフィルターです。 信頼性情報の名前によって信頼性情報の種類を特定します。 信頼されているフォレスト内で信頼性情報の種類ベースのフィルター処理を使用し、信頼する側のフォレストに対して情報を開示する信頼性情報を Windows が送信できないようにします。  
  
-   **信頼性情報の種類ベースの変換**。意図したターゲットに信頼性情報を送信する前にその情報を操作します。 信頼される側のフォレストで信頼性情報の種類ベースの変換を使用し、特定の情報が含まれる既知の信頼性情報を一般化します。 変換を使用し、信頼性情報の種類、信頼性情報の値、またはこの両方を一般化できます。  
  
## <a name="software-requirements"></a>ソフトウェア要件  
ダイナミック アクセス制御の信頼性情報と複合認証は、Kerberos 認証の拡張機能を必要とします。このため、ダイナミック アクセス制御をサポートするドメインはすべて、十分な数のドメイン コントローラーでサポートされるバージョンの Windows を実行し、ダイナミック アクセス制御対応の Kerberos クライアントからの認証をサポートする必要があります。 既定では、デバイスは、他のサイトのドメイン コントローラーを使用する必要があります。 このようなドメイン コントローラーが使用できない場合、認証は失敗します。 したがって、次に示す状況の 1 つをサポートする必要があります。  
  
-   ダイナミック アクセス制御をサポートするすべてのドメインが、サポートされるバージョンの Windows または Windows Server を実行しているすべてのデバイスからの認証をサポートするには、サポートされるバージョンの Windows Server を実行している適切なドメイン コントローラーを用意する必要があります。  
  
-   サポートされるバージョンの Windows 実行するデバイス、または信頼性情報または複合 ID を使用してリソースを保護することがないデバイスは、ダイナミック アクセス制御の Kerberos プロトコル サポートを無効にする必要があります。  
  
ユーザー信頼性情報をサポートするドメインの場合、信頼性情報と複合認証をサポートし、Kerberos 防御を提供するために、サポートされるバージョンの Windows Server を実行する各ドメイン コントローラーに適切な設定値を指定する必要があります。 KDC 管理用テンプレート ポリシーの設定値を指定するには、次のように操作します。  
  
-   **[常に信頼性情報を提供する]** 。すべてのドメイン コントローラーがサポートされるバージョンの Windows Server を実行している場合は、この設定を使用します。 また、ドメインの機能レベルを Windows Server 2012 以上に設定します。  
  
-   **[サポート]** 。この設定を使用する場合は、ドメイン コントローラーを監視し、ダイナミック アクセス制御によって保護されたリソースにアクセスする必要があるクライアント コンピューターの数に対応できるだけの十分な数のドメイン コントローラーがサポートされるバージョンの Windows Server を実行していることを確認してください。  
  
ユーザードメインとファイルサーバードメインが異なるフォレストにある場合は、ファイルサーバーのフォレストルート内のすべてのドメインコントローラーを、Windows Server 2012 以上の機能レベルで設定する必要があります。  
  
クライアントがダイナミック アクセス制御を認識しない場合は、それらの 2 つのフォレスト間に双方向の信頼関係が存在していなければなりません。  
  
要求がフォレストを離れるときに変換される場合は、ユーザーのフォレストルート内のすべてのドメインコントローラーが、Windows Server 2012 以上の機能レベルで設定されている必要があります。  
  
Windows Server 2012 または Windows Server 2012 R2 を実行するファイル サーバーには、信頼性情報を搬送しないユーザー トークンのためにユーザーの信頼性情報を取得する必要があるかどうかを指定するグループ ポリシー設定が必要です。 この設定値は、既定で **[自動]** に指定されます。この結果、そのファイル サーバー用のユーザー信頼性情報またはデバイス信頼性情報を含む集約型ポリシーが存在する場合には、このグループ ポリシー設定が **[オン]** になります。 ファイル サーバーにユーザーの信頼性情報が入った随意 ACL が含まれる場合は、このグループ ポリシーを **[オン]** にする必要があります。これにより、サーバーは、サーバーにアクセスする際に信頼性情報を提供しないユーザーに代わって信頼性情報を要求する必要があることを認識できます。  
  
## <a name="additional-resource"></a>その他のリソース  
このテクノロジに基づくソリューションの実装の詳細については、@no__t 0Dynamic Access Control を参照してください。シナリオの概要 @ no__t-0。  
  


