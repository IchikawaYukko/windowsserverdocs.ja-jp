---
title: ファイルの有効期限タスクを作成する
description: この記事では、まもなく期限切れになるファイルを処理するためのファイル管理タスクの作成プロセスを説明します。
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 0901c17203252414a37ccc5205a0946b8bef0d41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394231"
---
# <a name="create-a-file-expiration-task"></a>ファイルの有効期限タスクを作成する

> 適用対象:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

次の手順では、期限切れが近づいたファイルを処理するためのファイル管理タスクの作成プロセスについて説明します。 ファイルの有効期限タスクを使用すると、一定の条件に合致するすべてのファイルが、指定した有効期限切れ用ディレクトリに自動的に移動されます。管理者はこのディレクトリに移動されたファイルをバックアップして削除できます。

ファイルの有効期限タスクが実行されると、有効期限ディレクトリの中に、タスクの実行サーバー名ごとに新しいディレクトリが作成されます。

新しいディレクトリの名前は、ファイル管理タスクの名前とタスクの実行時間に基づいて付けられます。 有効期限切れのファイルが検出されると、検出されたファイルは、元のディレクトリ構造を保ったまま、新しいディレクトリに移動されます。

## <a name="to-create-a-file-expiration-task"></a>ファイルの有効期限タスクを作成するには

1. **[ファイル管理タスク]** ノードをクリックします。

2. **[ファイル管理タスク]** を右クリックし、 **[ファイル管理タスクの作成]** をクリックします (または、 **[操作]** ウィンドウの **[ファイル管理タスクの作成]** をクリックします)。 **[ファイル管理タスクの作成]** ダイアログ ボックスが表示されます。

3. **[全般]** タブで、次の情報を入力します。

   -   **名前**です。 新しいタスクの名前を入力します。  

   -   **説明**です。 必要に応じて、このタスクの説明ラベルを入力します。  
    
   -   **[スコープ]** 。 **[追加]** ボタンを使用して、このタスクの操作対象となるディレクトリを追加します。 **[削除]** ボタンを使用すると、必要に応じて、一覧からディレクトリを削除できます。 ファイル管理タスクは、一覧に含まれるすべてのフォルダーとサブフォルダーに適用されます。

4. **[操作]** タブで、次の情報を入力します。

   - **[種類]** 。 ドロップダウン ボックスから **[ファイルの有効期限]** を選択します。

   - **[有効期限切れのディレクトリ]** 。 有効期限が切れるファイルの移動先となるディレクトリを選択します。

     > [!Warning]
     > 前の手順で定義した、タスクの適用範囲に含まれるディレクトリは選択しないでください。 これを行うと反復ループが発生し、システムが不安定になり、データが失われる可能性があります。

5. 必要に応じて、 **[通知]** タブで **[追加]** をクリックし、ファイルに対する操作の実行に先立って、指定した日数前までに電子メール通知の送信、イベントの記録、コマンドやスクリプトの実行を完了するように設定できます。

   - **[通知を送信するタスクが実行されるまでの日数]** ボックスで、ファイルに対して操作を実行する少なくとも何日前までに通知を送信するかについて、値を入力するか選択します。

     > [!Note]
     > 通知は、タスクが実行される場合のみ送信されます。 通知を送信するように指定された最小日数に対してスケジュールされたタスクが指定されていない場合、それより前にスケジュールされたタスクの当日に通知が送信されます。

   - 電子メール通知を構成するには、 **[電子メール メッセージ]** タブをクリックし、次の情報を入力します。

     - しきい値に達したときに管理者に通知するには、 **[次の管理者に電子メールを送信する]** チェック ボックスをオンにし、通知を受け取る管理者のアカウント名を入力します。 <em>account@domain</em>  の形式を使用し、複数のアカウントはセミコロンで区切ります。  

     - 有効期限切れが近いファイルの所有者に電子メールを送信するには、 **[ファイルの有効期限が切れるユーザーに電子メールを送信する]** チェック ボックスをオンにします。

     - メッセージを構成するには、用意されている既定の件名行とメッセージ本文を編集します。 角かっこで囲まれたテキストには、通知の原因となったクォータ イベントに関する変数情報が挿入されます。 たとえば、 **\[Source File Owner @ no__t-2**変数は、ファイルの有効期限が間もなく切れるユーザーの名前を挿入します。 テキストに追加の変数を挿入するには、 **[変数の挿入]** をクリックします。

     - 有効期限が切れるファイルの一覧を添付するには、 **[アクションが実行されるファイルの電子メールの一覧に追加する]** をクリックし、 **[一覧上のファイルの最大数]** に値を入力するか選択します。

     - 追加のヘッダー ([差出人]、[Cc]、[Bcc]、[返信先] など) を構成するには、 **[追加電子メール ヘッダー]** をクリックします。  

   - イベントを記録するには、 **[イベント ログ]** タブをクリックして **[イベント ログへ警告を送信]** チェック ボックスをオンにし、既定のログ エントリを編集します。  

   - コマンドまたはスクリプトを実行するには、 **[コマンド]** タブをクリックし、 **[コマンドまたはスクリプトの実行]** チェック ボックスをオンにします。 次に、コマンドを入力するか、 **[参照]** をクリックして、スクリプトが格納されている場所を指定します。 必要に応じて、コマンドの引数の入力、コマンドやスクリプト用の作業ディレクトリの選択、コマンド セキュリティ設定の変更を行うこともできます。

6. 必要に応じて、 **[レポート]** タブを使用して、1 つ以上のログまたは記憶域レポートを生成します。

   - ログを生成するには、 **[ログを生成する]** チェック ボックスをオンにし、1 つ以上の使用可能なログを選択します。  

   - レポートを生成するには、 **[レポートを生成する]** チェック ボックスをオンにし、1 つ以上の使用可能なレポートの形式を選択します。  

   - 生成されたログまたは記憶域レポートを電子メールで送信するには、 **[次の管理者にレポートを送信する]** チェック ボックスをオンにし、1 人以上の管理者向け電子メールの受信者を <em>account@domain</em> の形式で入力します。 複数のアドレスは、セミコロンで区切ります。

     > [!Note]
     > レポートは、インシデント レポート用の既定の場所に保存されます。これは、 **[ファイル サーバー リソース マネージャーのオプション]** ダイアログ ボックスで変更できます。
        
7. 必要に応じて、 **[条件]** タブを使用して、定義された条件セットと一致するファイルだけにこのタスクを実行します。 次の設定を使用できます。

    -   **[プロパティの条件]** 。 ファイルの分類に基づいて新しい条件を作成するには、 **[追加]** をクリックします。 **[プロパティの条件]** ダイアログ ボックスが開き、プロパティ、プロパティに対して実行する演算子、およびプロパティを比較する値を選択できます。 **[OK]** をクリックした後、追加条件を作成するか、既存の条件を編集または削除できます。

    -   **[ファイルの最終変更からの日数]** 。 チェック ボックスをオンにしてから、日数をスピン ボックスに入力します。 ファイル管理タスクは、指定した日数を超えた期間、変更されていないファイルに対してのみ実行されます。

    -   **[ファイルの最終アクセスからの日数]** 。 チェック ボックスをオンにしてから、日数をスピン ボックスに入力します。 サーバーがファイルの最終アクセス日を示すタイムスタンプを追跡するように設定されている場合、ファイル管理タスクは、指定した日数よりも長い期間、アクセスされていないファイルに対してのみ実行されます。 サーバーがアクセス時間を追跡するように設定されていない場合、この条件は無効です。

    -   **[ファイルの作成からの日数]** 。 チェック ボックスをオンにしてから、日数をスピン ボックスに入力します。 タスクは、指定された日数以前に作成されたファイルに対してのみ適用されます。  

    -   **[次の日から有効]** 。 このファイル管理タスクがファイルの処理を開始する日付を設定します。 このオプションは、ユーザーに通知するなどの事前の準備を行うまで、タスクの開始を遅らせるために使用できます。

8. **[スケジュール]** タブで、 **[スケジュールの作成]** をクリックし、 **[スケジュール]** ダイアログ ボックスで  **[新規作成]** をクリックします。 既定のスケジュールとして毎日午前 9:00 という設定が表示されますが、 この既定のスケジュールは変更できます。 スケジュールの設定を終了したら、 **[OK]** をクリックし、再度 **[OK]** をクリックします。

## <a name="see-also"></a>関連項目

-   [分類の管理](classification-management.md)
-   [ファイル管理タスク](file-management-tasks.md)