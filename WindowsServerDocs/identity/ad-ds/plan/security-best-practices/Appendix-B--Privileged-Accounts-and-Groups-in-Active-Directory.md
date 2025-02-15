---
ms.assetid: 79b9c912-ea3e-4679-ab41-893e096c4d09
title: 付録 B-Active Directory の特権アカウントとグループ
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7ea17f6cff1e7c212a9cce080fcaa415eb9ca461
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367772"
---
# <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>付録 B:Active Directory の特権付きアカウントとグループ

>適用先:Windows Server 2016 では、Windows Server 2012 R2、Windows Server 2012


## <a name="appendix-b-privileged-accounts-and-groups-in-active-directory"></a>付録 B:Active Directory の特権付きアカウントとグループ  
Active Directory の "Privileged" アカウントとグループは、強力な権限、特権、およびアクセス許可が付与され、Active Directory とドメインに参加しているシステムでほぼすべての操作を実行できるようにするものです。 この付録では、まず、権利、特権、およびアクセス許可について説明し、次に、Active Directory 内の "最高の特権" アカウントとグループ、つまり最も強力なアカウントとグループに関する情報を示します。  

また、Active Directory の組み込みアカウントと既定のアカウントおよびグループについても、権限に加えて情報が提供されます。 最上位の特権アカウントとグループをセキュリティで保護するための特定の構成に関する推奨事項は別の付録として提供されていますが、この付録では、セキュリティ保護に重点を置いているユーザーとグループの識別に役立つ背景情報を提供します. そのためには、攻撃者が悪用して Active Directory のインストールを破棄することもできます。  

### <a name="rights-privileges-and-permissions-in-active-directory"></a>Active Directory の権限、権限、および権限  
権利、アクセス許可、および特権の違いは、Microsoft のドキュメント内でも、混乱を招く可能性があります。 このセクションでは、このドキュメントで使用される各の特性について説明します。 これらの説明は、これらの用語を異なる方法で使用する可能性があるため、他の Microsoft ドキュメントに対して権威を持つものとは見なさないでください。  

#### <a name="rights-and-privileges"></a>権限と特権  
権限と特権は、実質的には、ユーザー、サービス、コンピューター、グループなどのセキュリティプリンシパルに付与されるシステム全体の機能と同じです。 通常、IT プロフェッショナルが使用するインターフェイスでは、これらは "権限" または "ユーザー権利" と呼ばれ、グループポリシーオブジェクトによって割り当てられることがよくあります。 次のスクリーンショットは、セキュリティプリンシパルに割り当てることができる最も一般的なユーザー権利の一部を示しています (Windows Server 2012 ドメインの既定のドメインコントローラー GPO を表します)。 これらの権限の一部は Active Directory に適用されます。たとえば、[**コンピューターアカウントとユーザーアカウントを委任に対して信頼できる**ようにする] ユーザー権利は、Windows オペレーティングシステムに適用されます (**システム時刻の変更**など)。  

![特権アカウントとグループ](media/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory/SAD_8.gif)  

グループポリシーオブジェクトエディターなどのインターフェイスでは、割り当て可能なすべての機能がユーザー権限として広く参照されています。 ただし実際には、一部のユーザー権限はプログラムによって権限として参照されますが、プログラムは特権として参照されます。 表 B-1:ユーザー権利と特権によって、最も一般的に割り当てられたユーザー権限とそのプログラム定数が提供されます。 グループポリシーおよびその他のインターフェイスは、これらのすべてをユーザー権限と呼びますが、プログラムによって権限として識別されるものもあれば、権限として定義されるものもあります。  

次の表に一覧表示されている各ユーザー権利の詳細については、表のリンクを参照するか、@no__t 0Threats 脅威と対策ガイドを参照してください。Microsoft TechNet サイトの「Windows Server 2008 R2 の[脅威と脆弱性の対策](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)」ガイドの「ユーザー権利 @ no__t-0」。 Windows Server 2008 に適用される情報については、Microsoft TechNet サイトの「[脅威と脆弱性の対策](https://technet.microsoft.com/library/cc755181(v=ws.10).aspx)」ドキュメントの「[ユーザー権利](https://technet.microsoft.com/library/dd349804(v=WS.10).aspx)」を参照してください。 このドキュメントの執筆時点では、Windows Server 2012 に対応するドキュメントはまだ公開されていません。  

> [!NOTE]  
> このドキュメントでは、特に指定がない限り、権利と特権を識別するために "権利" と "ユーザー権利" という用語を使用します。  

##### <a name="table-b-1-user-rights-and-privileges"></a>表 B-1:ユーザー権利と特権  

|||  
|-|-|  
|**グループポリシーのユーザー権利**|**定数の名前**|  
|[信頼された呼び出し元として資格情報マネージャーにアクセスする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_2)|SeTrustedCredManAccessPrivilege|  
|[ネットワークからこのコンピューターにアクセスする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_1)|SeNetworkLogonRight|  
|[オペレーティングシステムの一部として機能](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_3)|SeTcbPrivilege|  
|[ドメインにワークステーションを追加する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_4)|SeMachineAccountPrivilege|  
|[プロセスのメモリクォータを調整する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_5)|SeIncreaseQuotaPrivilege|  
|[ローカルログオンを許可する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_6)|SeInteractiveLogonRight|  
|[ターミナルサービスを使用したログオンを許可する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_7)|SeRemoteInteractiveLogonRight|  
|[ファイルとディレクトリのバックアップ](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_8)|SeBackupPrivilege|  
|[走査チェックのバイパス](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_9)|SeChangeNotifyPrivilege|  
|[システム時刻の変更](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_10)|SeSystemtimePrivilege|  
|[タイムゾーンを変更する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_11)|SeTimeZonePrivilege|  
|[ページファイルの作成](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_12)|SeCreatePagefilePrivilege|  
|[トークンオブジェクトを作成する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_13)|SeCreateTokenPrivilege|  
|[グローバルオブジェクトの作成](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_14)|SeCreateGlobalPrivilege|  
|[永続的な共有オブジェクトを作成する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_15)|SeCreatePermanentPrivilege|  
|[シンボリックリンクの作成](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_16)|SeCreateSymbolicLinkPrivilege|  
|[プログラムのデバッグ](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_17)|SeDebugPrivilege|  
|[ネットワークからのこのコンピューターへのアクセスを拒否する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18)|SeDenyNetworkLogonRight|  
|[バッチジョブとしてのログオンを拒否する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_18a)|SeDenyBatchLogonRight|  
|[サービスとしてのログオンを拒否する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_19)|SeDenyServiceLogonRight|  
|[ローカルでのログオンを拒否する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_20)|SeDenyInteractiveLogonRight|  
|[ターミナルサービスを使用したログオンを拒否する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_21)|SeDenyRemoteInteractiveLogonRight|  
|[コンピューターおよびユーザーアカウントが委任に対して信頼されるようにする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_22)|SeEnableDelegationPrivilege|  
|[リモートシステムからの強制シャットダウン](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_23)|SeRemoteShutdownPrivilege|  
|[セキュリティ監査の生成](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_24)|SeAuditPrivilege|  
|[認証後にクライアントを偽装する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_25)|SeImpersonatePrivilege|  
|[プロセスのワーキングセットを増やす](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_26)|SeIncreaseWorkingSetPrivilege|  
|[スケジュールの優先順位を上げる](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_27)|SeIncreaseBasePriorityPrivilege|  
|[デバイスドライバーの読み込みとアンロード](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_28)|SeLoadDriverPrivilege|  
|[メモリ内のページをロックする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_29)|SeLockMemoryPrivilege|  
|[バッチジョブとしてログオンする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_30)|SeBatchLogonRight|  
|[サービスとしてログオン](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_31)|SeServiceLogonRight|  
|[監査とセキュリティログの管理](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_32)|SeSecurityPrivilege|  
|[オブジェクトラベルを変更する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_33)|SeRelabelPrivilege|  
|[ファームウェアの環境値を変更する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_34)|SeSystemEnvironmentPrivilege|  
|[ボリュームの保守タスクを実行する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_35)|SeManageVolumePrivilege|  
|[単一プロセスのプロファイル](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_36)|SeProfileSingleProcessPrivilege|  
|[システムパフォーマンスのプロファイル](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_37)|SeSystemProfilePrivilege|  
|[ドッキングステーションからコンピューターを削除する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_38)|SeUndockPrivilege|  
|[プロセスレベルトークンの置換](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_39)|SeAssignPrimaryTokenPrivilege|  
|[ファイルとディレクトリの復元](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_40)|SeRestorePrivilege|  
|[システムをシャットダウンする](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_41)|SeShutdownPrivilege|  
|[ディレクトリサービスデータの同期](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_42)|SeSyncAgentPrivilege|  
|[ファイルまたはその他のオブジェクトの所有権を取得する](https://technet.microsoft.com/library/db585464-a2be-41b1-b781-e9845182f4b6(v=ws.10)#BKMK_43)|SeTakeOwnershipPrivilege|  

#### <a name="permissions"></a>アクセス許可  
アクセス許可は、ファイルシステム、レジストリ、サービス、Active Directory オブジェクトなどのセキュリティ保護可能なオブジェクトに適用されるアクセス制御です。 セキュリティ保護可能な各オブジェクトには、関連付けられているアクセス制御リスト (ACL) があります。これには、オブジェクトに対してさまざまな操作を実行する権限をセキュリティプリンシパル (ユーザー、サービス、コンピューター、またはグループ) に許可または拒否するアクセス制御エントリ (Ace) が含まれています。 たとえば、Active Directory 内の多くのオブジェクトの Acl には、認証されたユーザーがオブジェクトに関する全般的な情報を読み取ることができる Ace が含まれていますが、機密情報の読み取りやオブジェクトの変更は許可されていません。
各ドメインの組み込みゲストアカウントを除き、ログオンして Active Directory フォレストまたは信頼されたフォレストのドメインコントローラーによって認証されるすべてのセキュリティプリンシパルは、認証されたユーザーのアクセスにセキュリティ識別子 (SID) が追加されています。既定ではトークンです。 このため、ユーザー、サービス、またはコンピューターアカウントがドメイン内のユーザーオブジェクトの全般プロパティを読み取ろうとすると、読み取り操作は成功します。  

セキュリティプリンシパルが、Ace が定義されておらず、プリンシパルのアクセストークンに存在する SID を含んでいるオブジェクトにアクセスしようとすると、プリンシパルはオブジェクトにアクセスできません。 さらに、オブジェクトの ACL 内の ACE に、ユーザーのアクセストークンと一致する SID の拒否エントリが含まれている場合、通常、"拒否" ACE は競合する "許可" ACE をオーバーライドします。 Windows のアクセス制御の詳細については、MSDN web サイトの「 [Access Control](https://msdn.microsoft.com/library/aa374860(v=VS.85).aspx) 」を参照してください。  

このドキュメントでは、アクセス許可とは、セキュリティ保護可能なオブジェクトのセキュリティプリンシパルに対して許可または拒否される機能を指します。 ユーザー権限とアクセス許可の間に競合がある場合は、通常、ユーザー権限が優先されます。 たとえば、Active Directory 内のオブジェクトが、管理者によるオブジェクトへの読み取りおよび書き込みアクセスを拒否する ACL で構成されている場合、ドメインの Administrators グループのメンバーであるユーザーは、そのオブジェクトに関する多くの情報を表示できません。 ただし、管理者グループには、ユーザー権利 "ファイルまたは他のオブジェクトの所有権を取得する" 権限が付与されているため、ユーザーは、問題のオブジェクトの所有権を取得し、オブジェクトの ACL を書き直して、管理者にオブジェクトのフルコントロールを与えることができます。  

このため、このドキュメントでは、アカウントとグループの機能を制限するのではなく、日常の管理に強力なアカウントとグループを使用しないようにすることをお勧めします。 強力な資格情報にアクセスできるユーザーが、その資格情報を使用してセキュリティ保護可能なリソースにアクセスできるようにすることは、実質的に停止することはありません。  

### <a name="built-in-privileged-accounts-and-groups"></a>組み込みの特権アカウントとグループ  
Active Directory は、管理の委任と、権限とアクセス許可を割り当てる際の最小限の特権の原則を示すことを目的としています。 "通常の" ユーザーが Active Directory ドメインにアカウントを持っている場合、既定では、ディレクトリに格納されているものの多くを読み取ることができますが、ディレクトリ内のデータのごく限られたセットのみを変更することができます。 追加の特権を必要とするユーザーは、ディレクトリに組み込まれているさまざまな特権グループのメンバーシップを付与することができます。これにより、ロールに関連する特定のタスクを実行できますが、職務に関係のないタスクを実行することはできません。  

Active Directory では、3つの組み込みグループがあります。これは、ディレクトリ内の最上位の特権グループ (Enterprise Admins (EA) グループ、Domain Admins (DA) グループ、および組み込み管理者 (BA) グループで構成されます。  

4番目のグループである Schema Admins (SA) グループには、悪用されると Active Directory フォレスト全体が破損または破壊される可能性があるという特権がありますが、このグループの機能は EA、DA、および BA グループよりも制限されています。  

これらの4つのグループに加えて、Active Directory にはいくつかの追加の組み込みおよび既定のアカウントとグループがあります。それぞれに、特定の管理タスクを実行できる権限とアクセス許可が付与されています。 この付録では、Active Directory の組み込みまたは既定のグループの詳細については説明しませんが、インストールに表示される可能性が最も高いグループとアカウントの表を提供します。  

たとえば、Active Directory フォレストに Microsoft Exchange Server をインストールすると、ドメイン内の組み込みコンテナーとユーザーコンテナーに、追加のアカウントとグループが作成される場合があります。 この付録では、ネイティブの役割と機能に基づいて Active Directory の組み込みコンテナーとユーザーコンテナーに作成されるグループとアカウントについてのみ説明します。 エンタープライズソフトウェアのインストールによって作成されたアカウントとグループは含まれません。  

#### <a name="enterprise-admins"></a>Enterprise Admins  
Enterprise Admins (EA) グループは、フォレストのルートドメインにあります。既定では、フォレスト内のすべてのドメインの組み込みの Administrators グループのメンバーです。 フォレストルートドメインの組み込みの Administrator アカウントは、EA グループの唯一の既定のメンバーです。 EAs には、フォレスト全体の変更に影響を与える権限とアクセス許可が付与されます。 これらは、ドメインの追加や削除、フォレストの信頼の確立、フォレストの機能レベルの引き上げなど、フォレスト内のすべてのドメインに影響を与える変更です。 適切に設計および実装された委任モデルでは、最初にフォレストを構築するとき、または、送信フォレストの信頼を確立するなどの特定のフォレスト全体の変更を行うときにのみ、EA のメンバーシップが必要になります。  

既定では、EA グループはフォレストルートドメインの [ユーザー] コンテナーにあり、フォレストルートドメインが Windows 2000 Server 混在モードで実行されている場合を除き、ユニバーサルセキュリティグループです。この場合、グループはグローバルセキュリティグループです。 一部の権限は EA グループに直接付与されますが、このグループの権限の多くは、フォレスト内の各ドメインの Administrators グループのメンバであるため、EA グループによって実際に継承されます。 Enterprise Admins には、ワークステーションまたはメンバーサーバーに対する既定の権限はありません。  

#### <a name="domain-admins"></a>Domain Admins  
フォレスト内の各ドメインには独自のドメイン管理者 (DA) グループがあります。これは、ドメインに参加しているすべてのコンピューターのローカル Administrators グループのメンバーに加えて、そのドメインの組み込み管理者 (BA) グループのメンバーになります。 ドメインの DA グループの既定のメンバーは、そのドメインの組み込みの Administrator アカウントのみです。  

DAs は、ドメイン内ではすべて強力ですが、EAs にはフォレスト全体の特権があります。 適切に設計および実装された委任モデルでは、DA メンバーシップが必要になるのは、ドメイン内のすべてのコンピューターに対して高いレベルの特権を持つアカウントが必要な場合、または特定のドメイン全体が必要な場合です。変更を行う必要があります。 ネイティブ Active Directory の委任メカニズムでは、緊急のシナリオでのみ DA アカウントを使用できるという程度の委任が可能ですが、効果的な委任モデルの構築には時間がかかることがあり、多くの組織ではサードパーティを使用します。プロセスを迅速に実行するアプリケーション。  

DA グループは、ドメインの [ユーザー] コンテナーにあるグローバルセキュリティグループです。 フォレスト内のドメインごとに1つの DA グループがあり、DA グループの唯一の既定のメンバーはドメインの組み込みの Administrator アカウントです。 ドメインの DA グループはドメインの BA グループとドメインに参加しているすべてのシステムのローカル管理者グループに入れ子になっているので、DAs は、ドメイン管理者に付与されたアクセス許可だけでなく、すべての権限とアクセス許可も継承します。ドメインに参加しているすべてのシステムのドメインの Administrators グループとローカルの Administrators グループ。  

#### <a name="administrators"></a>管理者  
組み込み管理者 (BA) グループは、DAs と EAs が入れ子になっているドメインの組み込みコンテナー内のドメインローカルグループであり、このグループには、ディレクトリおよびドメインコントローラーの直接の権限とアクセス許可の多くが付与されています。 ただし、ドメインの Administrators グループには、メンバーサーバーまたはワークステーションに対する権限はありません。 ドメインに参加しているコンピューターのローカル管理者グループのメンバーシップは、ローカルの特権が付与されている場所です。ここで説明するグループのうち、既定では、ドメインに参加しているすべてのコンピューターのローカル管理者グループのメンバーであるのは、DAs だけです。  

Administrators グループは、ドメインの組み込みコンテナー内のドメインローカルグループです。 既定では、すべてのドメインの BA グループには、ローカルドメインの組み込みの Administrator アカウント、ローカルドメインの DA グループ、およびフォレストのルートドメインの EA グループが含まれます。 Active Directory およびドメインコントローラーにおける多くのユーザー権限は、EAs や DAs ではなく、管理者グループのみに付与されます。 ドメインの BA グループには、ほとんどのディレクトリオブジェクトに対するフルコントロールアクセス許可が付与され、ディレクトリオブジェクトの所有権を取得できます。 EA および DA グループにはフォレストとドメインで特定のオブジェクト固有のアクセス許可が付与されますが、グループの多くの機能は、実際には BA グループのメンバーシップから "継承" されます。  

> [!NOTE]  
> これらはこれらの特権グループの既定の構成ですが、3つのグループのいずれかのメンバーは、ディレクトリを操作して他のすべてのグループのメンバーシップを取得できます。 場合によっては、他のユーザーにとっては難しいこともありますが、潜在的な特権の観点からは、3つのグループすべてが実質的に同等であると見なされる必要があります。  

#### <a name="schema-admins"></a>Schema Admins  
スキーマ管理者 (SA) グループは、フォレストのルートドメイン内のユニバーサルグループであり、EA グループと同様に、そのドメインの組み込みの Administrator アカウントだけが既定のメンバーになります。 SA グループのメンバーシップを使用すると、攻撃者は、Active Directory フォレスト全体のフレームワークである Active Directory スキーマを危険にさらすことができますが、SAs にはスキーマ以外の既定の権限とアクセス許可がほとんどありません。  

SA グループのメンバーシップを慎重に管理して監視する必要がありますが、一部の点では、このグループは、前に説明した3つの最上位の特権グループに限定されます。これは、特権の範囲が非常に狭いためです。つまり、SAs にはスキーマ以外の管理者権限はありません。  

#### <a name="additional-built-in-and-default-groups-in-active-directory"></a>Active Directory の追加の組み込みおよび既定のグループ  
ディレクトリへの管理の委任を容易にするために、Active Directory には、特定の権限とアクセス許可が付与されているさまざまな組み込みおよび既定のグループが付属しています。 これらのグループについては、次の表で簡単に説明します。  

次の表に、Active Directory の組み込みグループと既定のグループの一覧を示します。 既定では、両方のグループのセットが存在します。ただし、組み込みのグループは Active Directory の組み込みコンテナーに配置され、既定では Active Directory の [ユーザー] コンテナーに既定のグループが配置されます (既定では)。 組み込みコンテナー内のグループはすべてドメインローカルグループですが、Users コンテナー内のグループは、3つの個別のユーザーアカウント (Administrator、Guest、および Krbtgt) に加えて、ドメインローカルグループ、グローバルグループ、ユニバーサルグループの組み合わせです。  

この付録の前半で説明した最上位の特権グループに加えて、一部の組み込みアカウントと既定のアカウントおよびグループには昇格された特権が付与され、保護されていて、セキュリティで保護された管理ホストでのみ使用される必要があります。 これらのグループとアカウントは、表 B-1 の網掛けされた行にあります。Active Directory の組み込みおよび既定のグループとアカウント。 これらのグループとアカウントには、Active Directory またはドメインコントローラーを侵害するために誤用される可能性のある権限とアクセス許可が付与されているため、@no__t の「付録 C:Active Directory @ no__t の保護されたアカウントとグループ。  

##### <a name="table-b-1-built-in-and-default-accounts-and-groups-in-active-directory"></a>表 B-1:Active Directory の組み込みアカウントと既定のアカウントおよびグループ  

||||  
|-|-|-|  
|**アカウントまたはグループ**|**既定のコンテナー、グループのスコープ、および種類**|**説明と既定のユーザー権利**|  
|Access Control アシスタンスオペレーター (Windows Server 2012 の Active Directory)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、このコンピューター上のリソースに対する承認属性とアクセス許可をリモートで照会できます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Account Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|メンバーは、ドメインユーザーとグループアカウントを管理できます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Administrator アカウント|ユーザーコンテナー<br /><br />グループではありません|ドメインを管理するためのビルトインアカウント。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />プロセスのメモリ クォータの調整<br /><br />ローカル ログオンを許可<br /><br />リモート デスクトップ サービスを使ったログオンを許可する<br /><br />ファイルとディレクトリのバックアップ<br /><br />走査チェックのバイパス<br /><br />システム時刻の変更<br /><br />タイム ゾーンの変更<br /><br />ページ ファイルの作成<br /><br />グローバル オブジェクトの作成<br /><br />シンボリック リンクの作成<br /><br />プログラムのデバッグ<br /><br />コンピューターとユーザー アカウントに委任時の信頼を付与<br /><br />リモート コンピューターからの強制シャットダウン<br /><br />認証後にクライアントを借用する<br /><br />プロセス ワーキング セットの増加<br /><br />スケジューリング優先順位の繰り上げ<br /><br />デバイス ドライバーのロードとアンロード<br /><br />バッチ ジョブとしてログオン<br /><br />監査とセキュリティ ログの管理<br /><br />ファームウェア環境値の修正<br /><br />ボリュームの保守タスクを実行<br /><br />単一プロセスのプロファイル<br /><br />システム パフォーマンスのプロファイル<br /><br />ドッキング ステーションからコンピューターを削除<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />ファイルとその他のオブジェクトの所有権の取得|  
|管理者グループ|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|管理者は、ドメインに対する完全で無制限のアクセス権を持ちます。<br /><br />**直接ユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />プロセスのメモリ クォータの調整<br /><br />ローカル ログオンを許可<br /><br />リモート デスクトップ サービスを使ったログオンを許可する<br /><br />ファイルとディレクトリのバックアップ<br /><br />走査チェックのバイパス<br /><br />システム時刻の変更<br /><br />タイム ゾーンの変更<br /><br />ページ ファイルの作成<br /><br />グローバル オブジェクトの作成<br /><br />シンボリック リンクの作成<br /><br />プログラムのデバッグ<br /><br />コンピューターとユーザー アカウントに委任時の信頼を付与<br /><br />リモート コンピューターからの強制シャットダウン<br /><br />認証後にクライアントを借用する<br /><br />スケジューリング優先順位の繰り上げ<br /><br />デバイス ドライバーのロードとアンロード<br /><br />バッチ ジョブとしてログオン<br /><br />監査とセキュリティ ログの管理<br /><br />ファームウェア環境値の修正<br /><br />ボリュームの保守タスクを実行<br /><br />単一プロセスのプロファイル<br /><br />システム パフォーマンスのプロファイル<br /><br />ドッキング ステーションからコンピューターを削除<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />ファイルとその他のオブジェクトの所有権の取得<br /><br />継承されたユーザー権限:<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|許可された RODC パスワードレプリケーショングループ|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ドメイン内のすべての読み取り専用ドメインコントローラーにパスワードをレプリケートできます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Backup Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|バックアップオペレーターは、ファイルのバックアップまたは復元のみを目的として、セキュリティ制限を上書きできます。<br /><br />**直接ユーザー権限:**<br /><br />ローカル ログオンを許可<br /><br />ファイルとディレクトリのバックアップ<br /><br />バッチ ジョブとしてログオン<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Cert Publishers|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ディレクトリに証明書を発行することが許可されています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|証明書サービス DCOM アクセス|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|証明書サービスがドメインコントローラーにインストールされている場合 (推奨されません)、このグループは、ドメインユーザーとドメインコンピューターへの DCOM 登録アクセスを許可します。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|複製可能ドメインコントローラー (Windows Server 2012AD DS の AD DS)|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメインコントローラであるこのグループのメンバは複製される可能性があります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Cryptographic Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|メンバーは、暗号化操作を実行する権限を持っています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|デバッガーユーザー|これは既定のグループでも組み込みグループでもありませんが、AD DS に存在する場合は、さらに調査が発生します。|Debugger Users グループが存在することは、デバッグツールが、Visual Studio、SQL、Office、またはデバッグ環境を必要とサポートするその他のアプリケーションによって、システムにインストールされていることを示します。 このグループは、コンピューターへのリモートデバッグアクセスを許可します。 このグループがドメインレベルに存在する場合、デバッガーまたはデバッガーを含むアプリケーションがドメインコントローラーにインストールされていることを示します。|  
|拒否された RODC パスワードレプリケーショングループ|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、パスワードをドメイン内の読み取り専用ドメインコントローラーにレプリケートすることはできません。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|DHCP 管理者|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、DHCP サーバーサービスへの管理アクセス権を持っています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|DHCP ユーザー|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、DHCP サーバーサービスに対する表示のみのアクセス権を持ちます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Distributed COM Users|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、このコンピューターで分散 COM オブジェクトの起動、アクティブ化、および使用を許可されています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|DnsAdmins|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、DNS サーバーサービスへの管理アクセス権を持っています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|DnsUpdateProxy|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|このグループのメンバーは、動的更新を実行できないクライアントに代わって動的更新を実行することが許可されている DNS クライアントです。 このグループのメンバーは、通常、DHCP サーバーです。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Domain Admins|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメインの指定された管理者。ドメイン管理者は、ドメインに参加しているすべてのコンピューターのローカル管理者グループのメンバーであり、ドメインの Administrators グループに加えて、ローカルの Administrators グループに付与された権限とアクセス許可を受け取ります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />プロセスのメモリ クォータの調整<br /><br />ローカル ログオンを許可<br /><br />リモート デスクトップ サービスを使ったログオンを許可する<br /><br />ファイルとディレクトリのバックアップ<br /><br />走査チェックのバイパス<br /><br />システム時刻の変更<br /><br />タイム ゾーンの変更<br /><br />ページ ファイルの作成<br /><br />グローバル オブジェクトの作成<br /><br />シンボリック リンクの作成<br /><br />プログラムのデバッグ<br /><br />コンピューターとユーザー アカウントに委任時の信頼を付与<br /><br />リモート コンピューターからの強制シャットダウン<br /><br />認証後にクライアントを借用する<br /><br />プロセス ワーキング セットの増加<br /><br />スケジューリング優先順位の繰り上げ<br /><br />デバイス ドライバーのロードとアンロード<br /><br />バッチ ジョブとしてログオン<br /><br />監査とセキュリティ ログの管理<br /><br />ファームウェア環境値の修正<br /><br />ボリュームの保守タスクを実行<br /><br />単一プロセスのプロファイル<br /><br />システム パフォーマンスのプロファイル<br /><br />ドッキング ステーションからコンピューターを削除<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />ファイルとその他のオブジェクトの所有権の取得|  
|ドメインコンピューター|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメインに参加しているすべてのワークステーションとサーバーは、このグループの既定のメンバーになります。<br /><br />**既定の直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|ドメイン コントローラー|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメイン内のすべてのドメインコントローラー。 メモ:ドメインコントローラは、Domain Computers グループのメンバではありません。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|ドメインのゲスト|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメイン内のすべてのゲスト<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|ドメインユーザー|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|ドメイン内のすべてのユーザー<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Enterprise Admins (フォレストのルートドメインにのみ存在)|ユーザーコンテナー<br /><br />ユニバーサルセキュリティグループ|エンタープライズ管理者は、フォレスト全体の構成設定を変更するアクセス許可を持っています。Enterprise Admins は、すべてのドメインの管理者グループのメンバーであり、そのグループに付与された権限とアクセス許可を受け取ります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />プロセスのメモリ クォータの調整<br /><br />ローカル ログオンを許可<br /><br />リモート デスクトップ サービスを使ったログオンを許可する<br /><br />ファイルとディレクトリのバックアップ<br /><br />走査チェックのバイパス<br /><br />システム時刻の変更<br /><br />タイム ゾーンの変更<br /><br />ページ ファイルの作成<br /><br />グローバル オブジェクトの作成<br /><br />シンボリック リンクの作成<br /><br />プログラムのデバッグ<br /><br />コンピューターとユーザー アカウントに委任時の信頼を付与<br /><br />リモート コンピューターからの強制シャットダウン<br /><br />認証後にクライアントを借用する<br /><br />プロセス ワーキング セットの増加<br /><br />スケジューリング優先順位の繰り上げ<br /><br />デバイス ドライバーのロードとアンロード<br /><br />バッチ ジョブとしてログオン<br /><br />監査とセキュリティ ログの管理<br /><br />ファームウェア環境値の修正<br /><br />ボリュームの保守タスクを実行<br /><br />単一プロセスのプロファイル<br /><br />システム パフォーマンスのプロファイル<br /><br />ドッキング ステーションからコンピューターを削除<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />ファイルとその他のオブジェクトの所有権の取得|  
|エンタープライズ読み取り専用ドメインコントローラー|ユーザーコンテナー<br /><br />ユニバーサルセキュリティグループ|このグループには、フォレスト内のすべての読み取り専用ドメインコントローラーのアカウントが含まれます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Event Log Readers|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|のこのグループのメンバーは、ドメインコントローラーのイベントログを読み取ることができます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Group Policy Creator Owners|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|このグループのメンバーは、ドメイン内のグループポリシーオブジェクトを作成および変更できます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Guest|ユーザーコンテナー<br /><br />グループではありません|これは、認証されたユーザーの SID がアクセストークンに追加されていない AD DS ドメイン内の唯一のアカウントです。 そのため、Authenticated Users グループへのアクセスを許可するように構成されているすべてのリソースには、このアカウントからアクセスすることはできません。 この動作は、Domain Guests グループおよび Guests グループのメンバーには当てはまりません。ただし、これらのグループのメンバーには、認証済みユーザー SID がアクセストークンに追加されています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Guests|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|ゲストは既定で Users グループのメンバと同じアクセス権を持ちますが、ゲストアカウントは除きます。これは前述のように制限されています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Hyper-v 管理者 (Windows Server 2012)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーには、Hyper-v のすべての機能に対する完全で無制限のアクセス権が付与されています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|IIS_IUSRS|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|インターネットインフォメーションサービスによって使用される組み込みグループ。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|入力方向のフォレストの信頼ビルダー (フォレストのルートドメインにのみ存在)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、このフォレストに対して、一方向の信頼関係を作成できます。 (出力方向のフォレストの信頼の作成は、Enterprise Admins 用に予約されています)。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Krbtgt|ユーザーコンテナー<br /><br />グループではありません|Krbtgt アカウントは、ドメイン内の Kerberos キー配布センターのサービスアカウントです。 このアカウントには、Active Directory に格納されているすべてのアカウントの資格情報へのアクセス権があります。 このアカウントは既定で無効になっているため、有効にしないでください。<br /><br />**ユーザーの権利:** なし|  
|Network Configuration Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーには、ネットワーク機能の構成を管理できる特権が付与されます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|パフォーマンス ログ ユーザー|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、パフォーマンスカウンターのログ記録のスケジュール設定、トレースプロバイダーの有効化、およびローカルでのコンピューターへのリモートアクセス経由でのイベントトレースの収集を行うことができます。<br /><br />**直接ユーザー権限:**<br /><br />バッチ ジョブとしてログオン<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|パフォーマンス監視ユーザー|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ローカルおよびリモートでパフォーマンスカウンターデータにアクセスできます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Pre-Windows 2000 Compatible Access|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループは、Windows 2000 Server より前のオペレーティングシステムとの下位互換性を維持するために存在し、メンバーがドメイン内のユーザーとグループの情報を読み取ることができるようにします。<br /><br />**直接ユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />走査チェックのバイパス<br /><br />**継承されたユーザー権限:**<br /><br />ドメインにワークステーションを追加<br /><br />プロセス ワーキング セットの増加|  
|Print Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ドメインプリンターを管理できます。<br /><br />**直接ユーザー権限:**<br /><br />ローカル ログオンを許可<br /><br />デバイス ドライバーのロードとアンロード<br /><br />システムのシャットダウン<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|RAS および IAS サーバー|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのサーバーは、ドメイン内のユーザーアカウントのリモートアクセスプロパティを読み取ることができます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|RDS エンドポイントサーバー (Windows Server 2012)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのサーバーは、ユーザーの RemoteApp プログラムとパーソナル仮想デスクトップが実行される仮想マシンとホストセッションを実行します。 このグループは、RD 接続ブローカーを実行しているサーバーに設定する必要があります。 展開で使用されている RD セッションホストサーバーと RD 仮想化ホストサーバーは、このグループに存在する必要があります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|RDS 管理サーバー (Windows Server 2012)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのサーバーは、リモートデスクトップサービスを実行しているサーバーで日常的な管理操作を実行できます。 このグループは、リモートデスクトップサービス展開内のすべてのサーバーに設定する必要があります。 RDS Central Management サービスを実行しているサーバーをこのグループに含める必要があります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|RDS リモートアクセスサーバー (Windows Server 2012)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのサーバーを使用すると、RemoteApp プログラムおよび個人用仮想デスクトップからこれらのリソースにアクセスできます。 インターネットに接続する展開では、通常、これらのサーバーはエッジネットワークに展開されます。 このグループは、RD 接続ブローカーを実行しているサーバーに設定する必要があります。 展開で使用されている RD ゲートウェイサーバーと RD Web アクセスサーバーは、このグループに存在する必要があります。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Read-Only Domain Controllers|ユーザーコンテナー<br /><br />グローバルセキュリティグループ|このグループには、ドメイン内のすべての読み取り専用ドメインコントローラーが含まれます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Remote Desktop Users|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーには、RDP を使用してリモートでログオンする権限が付与されます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|リモート管理ユーザー (Windows Server 2012)|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、管理プロトコル (Windows リモート管理サービスを介した WS-MANAGEMENT など) を介して WMI リソースにアクセスできます。 これは、ユーザーへのアクセスを許可する WMI 名前空間にのみ適用されます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Replicator|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|では、ドメイン内のレガシファイルレプリケーションがサポートされています。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|スキーマ管理者 (フォレストのルートドメインにのみ存在)|ユーザーコンテナー<br /><br />ユニバーサルセキュリティグループ|スキーマ管理者は、スキーマの書き込みが許可されている場合にのみ、Active Directory スキーマを変更できる唯一のユーザーです。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Server Operators|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ドメインサーバーを管理できます。<br /><br />**直接ユーザー権限:**<br /><br />ローカル ログオンを許可<br /><br />ファイルとディレクトリのバックアップ<br /><br />システム時刻の変更<br /><br />タイム ゾーンの変更<br /><br />リモート コンピューターからの強制シャットダウン<br /><br />ファイルとディレクトリの復元<br /><br />システムのシャットダウン<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|ターミナルサーバーライセンスサーバー|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、TS CAL (接続ユーザー数) の使用状況を追跡および報告するために、Active Directory のユーザーアカウントをライセンスの発行に関する情報で更新できます。<br /><br />**既定の直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|Users|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|ユーザーには Active Directory で多くのオブジェクトと属性の読み取りを許可するアクセス許可がありますが、ほとんどは変更できません。 ユーザーは、偶発的または意図的なシステム全体の変更を行うことができず、ほとんどのアプリケーションを実行できます。<br /><br />**直接ユーザー権限:**<br /><br />プロセス ワーキング セットの増加<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス|  
|Windows 認証アクセスグループ|組み込みコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、ユーザーオブジェクトの計算された Tokenグループ Globalanduniversal 属性にアクセスします<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
|WinRMRemoteWMIUsers_ (Windows Server 2012)|ユーザーコンテナー<br /><br />ドメインローカルセキュリティグループ|このグループのメンバーは、管理プロトコル (Windows リモート管理サービスを介した WS-MANAGEMENT など) を介して WMI リソースにアクセスできます。 これは、ユーザーへのアクセスを許可する WMI 名前空間にのみ適用されます。<br /><br />**直接ユーザー権限:** なし<br /><br />**継承されたユーザー権限:**<br /><br />ネットワークからこのコンピューターにアクセスする<br /><br />ドメインにワークステーションを追加<br /><br />走査チェックのバイパス<br /><br />プロセス ワーキング セットの増加|  
