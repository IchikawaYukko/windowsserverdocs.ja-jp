---
ms.assetid: e6b72a80-e8b7-4305-be0c-0a290f468d36
title: Windows Server 2003 組織への AD DS の展開
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 7426ba8fa3ea5077510655ea4877d0e0b69226ff
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408901"
---
# <a name="deploying-ad-ds-in-a-windows-server-2003-organization"></a>Windows Server 2003 組織への AD DS の展開

>適用先:Windows Server 2016 では、Windows Server 2012 R2、Windows Server 2012

組織で現在 Windows Server 2003 Active Directory が実行されている場合は、ドメインコントローラーのオペレーティングシステムの一部またはすべてのインプレースアップグレードを実行することで、Windows Server 2008 Active Directory Domain Services (AD DS) を展開できます。 Windows Server 2008 または Windows Server 2008 を実行するドメインコントローラーを環境に導入します。  
  
Windows Server 2008 を実行しているドメインコントローラーを既存の Windows Server 2003 Active Directory ドメインに追加するには、その前に、コマンドラインツール**adprep**を実行する必要があります。 Adprep は AD DS スキーマを拡張し、選択したオブジェクトの既定のセキュリティ記述子を更新し、一部のアプリケーションに必要な新しいディレクトリオブジェクトを追加します。 Adprep は、Windows Server 2008 インストールディスク (\sources\adprep\adprep.exe) で使用できます。 詳細については、「Adprep ([https://go.microsoft.com/fwlink/?LinkId=99215](https://go.microsoft.com/fwlink/?LinkId=99215))」を参照してください。  
  
次の図は、windows Server 2003 Active Directory を現在実行しているネットワーク環境に Windows Server 2008 AD DS を展開する手順を示しています。  
  
![windows 2003 組織での展開](media/Deploying-AD-DS-in-a-Windows-Server-2003-Organization/900c4eee-1119-4a9a-9310-755597428b71.gif)  
  
> [!NOTE]  
> ドメインまたはフォレストの機能レベルを Windows Server 2008 に設定する場合は、環境内のすべてのドメインコントローラーで Windows Server 2008 オペレーティングシステムを実行する必要があります。  
  
Windows server 2008 AD DS の展開の一部として Windows Server 2003 環境からアップグレードされたリソースドメインとアカウントドメインを統合するには、フォレスト間またはフォレスト内のドメイン再構築が必要になることがあります。 フォレスト間で AD DS ドメインを再構築すると、AD DS における組織の表現の複雑さを軽減し、関連する管理コストを削減するのに役立ちます。 フォレスト内の AD DS ドメインを再構築すると、レプリケーショントラフィックを削減し、必要なユーザーとグループ管理の量を減らし、グループの管理を簡素化することで、組織の管理オーバーヘッドを削減できます。ポリシー. 詳細については、「ADMT v1.0 移行ガイド ([https://go.microsoft.com/fwlink/?LinkId=93678](https://go.microsoft.com/fwlink/?LinkId=93678))」を参照してください。  
  
Windows Server 2003 Active Directory を実行している組織で AD DS を計画および展開するために使用できる詳細なタスクの一覧については、「[Checklist リスト:Windows Server 2003 組織に AD DS を展開する @ no__t-0  
  


