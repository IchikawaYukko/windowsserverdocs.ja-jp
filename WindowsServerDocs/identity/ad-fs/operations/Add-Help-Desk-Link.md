---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: ヘルプ デスク リンクの追加
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 1673e6ee6357a9d59e8ac5891625d453bb434088
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358457"
---
# <a name="add-help-desk-link"></a>ヘルプ デスク リンクの追加 


## <a name="to-add-a-help-desk-link"></a>ヘルプデスクのリンクを追加するには  
[Sign @ no__t-0in] \ (サインイン \) ページに表示されるヘルプデスクリンクを追加するには、次の Windows PowerShell コマンドレットと構文を使用します。  

![ヘルプデスクの追加](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> このコマンドレットの `linkText` パラメーターは、既定値である *Help* を使用する場合は必要ありません。 既定値には、すべてのクライアント ロケールでローカライズ済みであるという利点があります。 ページをカスタマイズすると、カスタマイズ設定が優先されるため、サポートするすべての言語でページをカスタマイズする必要があります。  


## <a name="additional-references"></a>その他の参照情報 
[AD FS ユーザーサインインのカスタマイズ](AD-FS-user-sign-in-customization.md)  
