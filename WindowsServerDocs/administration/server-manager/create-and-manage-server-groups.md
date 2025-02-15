---
title: サーバーグループの作成と管理
description: サーバー マネージャー
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74075f091cd707f6faf73567c1dce6f22a2f6753
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383220"
---
# <a name="create-and-manage-server-groups"></a>サーバーグループの作成と管理

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

このトピックでは、Windows Server のサーバーマネージャーで、ユーザー定義のカスタムサーバーグループを作成する方法について説明します。

## <a name="BKMK_groups"></a>サーバーグループ
サーバープールに追加したサーバーは、サーバーマネージャーの **[すべてのサーバー]** ページに表示されます。 追加したサーバーでカスタム サーバー グループを作成することができます。 サーバーグループを使用すると、サーバープールのより小さなサブセットを論理ユニットとして表示および管理できます。たとえば、組織の会計部門内のすべてのサーバーに対して " **Accounting servers** " というグループを作成したり、シカゴに地理的に配置されているすべてのサーバーに対して**シカゴ**というグループを作成したりすることができます。 サーバーグループを作成すると、サーバーマネージャーのグループのホームページには、イベント、サービス、パフォーマンスカウンター、ベストプラクティスアナライザー結果、およびインストールされている役割と機能に関する情報がグループ全体に表示されます。

サーバーは、複数のグループのメンバーになることができます。

#### <a name="to-create-a-new-server-group"></a>新しいサーバー グループを作成するには

1.  **[管理]** メニューの **[サーバーグループの作成]** をクリックします。

2.  **[サーバー グループ名]** ボックスに、サーバー グループのフレンドリ名 (「 **会計サーバー**」など) を入力します。

3.  サーバーをサーバープールから**選択した**一覧に追加するか、 **[active directory]** 、 **[DNS]** 、または **[インポート]** タブを使用して他のサーバーをグループに追加します。 これらのタブの使用方法の詳細については、このガイドの「[サーバーマネージャーへのサーバーの追加](add-servers-to-server-manager.md)」を参照してください。

4.  グループへのサーバーの追加が完了したら、 **[OK]** をクリックします。 新しいグループは、サーバーマネージャーナビゲーションウィンドウの **[すべてのサーバー]** グループの下に表示されます。

#### <a name="to-edit-an-existing-server-group"></a>既存のサーバー グループを編集するには

1.  次のいずれかを実行します。

    -   サーバーマネージャーナビゲーションウィンドウで、サーバーグループを右クリックし、 **[サーバーグループの編集]** をクリックします。

    -   サーバーグループのホームページで、 **[サーバー]** タイルの **[タスク]** メニューを開き、 **[サーバーグループの編集]** をクリックします。

2.  グループ名を変更するか、グループのサーバーを追加または削除します。

    > [!NOTE]
    > サーバーグループからサーバーを削除しても、サーバーマネージャーからサーバーは削除されません。 グループから削除したサーバーは、サーバー プールの **[すべてのサーバー]** グループに残ります。

3.  グループの変更が完了したら、 **[OK]** をクリックします。

#### <a name="to-delete-an-existing-server-group"></a>既存のサーバー グループを削除するには

1.  次のいずれかを実行します。

    -   サーバーマネージャーナビゲーションウィンドウで、サーバーグループを右クリックし、 **[サーバーグループの削除]** をクリックします。

    -   サーバーグループのホームページで、 **[サーバー]** タイルの **[タスク]** メニューを開き、 **[サーバーグループの削除]** をクリックします。

2.  サーバー グループを削除するかどうかを確認するメッセージが表示されたら、 **[はい]** をクリックします。

    > [!NOTE]
    > サーバーグループを削除しても、サーバーマネージャーからサーバーは削除されません。 削除したグループに含まれていたサーバーは、サーバー プールの **[すべてのサーバー]** グループに残ります。

3.  グループの変更が完了したら、 **[OK]** をクリックします。

## <a name="see-also"></a>関連項目
サーバーマネージャー 
[サーバーマネージャー](server-manager.md) [にサーバーを追加し](add-servers-to-server-manager.md)ます



