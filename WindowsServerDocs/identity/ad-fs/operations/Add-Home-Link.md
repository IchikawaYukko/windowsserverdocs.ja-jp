---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: ホーム リンクの追加
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5f1fad340629304fdf960139be05b8dbc2690e0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407773"
---
# <a name="add-home-link"></a>ホーム リンクの追加 

[Sign @ no__t] ページに表示されるホームリンクを追加するには、次の Windows PowerShell コマンドレットと構文を使用します。 


![ホームリンクの追加](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> このコマンドレットの `linkText` パラメーターは、既定値である *Home*を使用する場合は必要ありません。 既定値には、すべてのクライアント ロケールでローカライズ済みであるという利点があります。 [Sign @ no__t-0in] ページをカスタマイズすると、カスタマイズが優先されます。そのため、サポートするすべての言語でをカスタマイズする必要があります。

## <a name="additional-references"></a>その他の参照情報 
[AD FS ユーザーサインインのカスタマイズ](AD-FS-user-sign-in-customization.md)  
