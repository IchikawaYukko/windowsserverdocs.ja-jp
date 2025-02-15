---
title: WSUS データベースを (Windows Internal Database) WID から SQL に移行する
description: Windows Server Update Service (WSUS) トピック-Windows Internal Database インスタンスから SQL Server のローカルまたはリモートインスタンスに WSUS データベース (SUSDB) を移行する方法について説明します。
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09g7dr
author: coreyp-at-msft
ms.author: coreyp
manager: dougkim
ms.date: 07/25/2018
ms.openlocfilehash: 0977aa1fd9a6848bd7b85bb592b6a82556277e72
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361582"
---
>適用対象:Windows Server 2012、Windows Server 2012 R2、Windows Server 2016

# <a name="migrating-the-wsus-database-from-wid-to-sql"></a>WID から SQL への WSUS データベースの移行

WSUS データベース (SUSDB) を Windows Internal Database インスタンスから SQL Server のローカルまたはリモートインスタンスに移行するには、次の手順を実行します。

## <a name="prerequisites"></a>前提条件

- SQL インスタンス。 既定の**MSSQLServer**またはカスタムインスタンスを指定できます。
- SQL Server Management Studio
- WID 役割がインストールされた WSUS
- IIS (これは通常、サーバーマネージャーによって WSUS をインストールする場合に含まれます)。 まだインストールされていないため、である必要があります。

## <a name="migrating-the-wsus-database"></a>WSUS データベースの移行

### <a name="stop-the-iis-and-wsus-services-on-the-wsus-server"></a>WSUS サーバー上の IIS および WSUS サービスを停止します。

PowerShell (管理者特権) から、次のように実行します。

```powershell
    Stop-Service IISADMIN
    Stop-Service WsusService
```

### <a name="detach-susdb-from-the-windows-internal-database"></a>Windows Internal Database から SUSDB をデタッチする

#### <a name="using-sql-management-studio"></a>SQL Management Studio の使用

1. [ **SUSDB** - @ no__t **Tasks** ] を右クリックし - @ no__t] をクリックします。 [**デタッチ**: ![image1 @ no__t-8] をクリックします。
2. [**既存の接続を削除**する] をオンにし、[ **OK]** をクリックします (アクティブな接続が存在する場合は省略可能)。
    ![image2 @ no__t-1

#### <a name="using-command-prompt"></a>コマンド プロンプトを使用

> [!IMPORTANT]
> 次の手順は、 **sqlcmd**ユーティリティを使用して、Windows Internal database インスタンスから WSUS データベース (SUSDB) をデタッチする方法を示しています。 **Sqlcmd**ユーティリティの詳細については、「 [sqlcmd ユーティリティ](https://go.microsoft.com/fwlink/?LinkId=81183)」を参照してください。
> 1. 管理者特権でのコマンドプロンプトを開く
> 2. 次の SQL コマンドを実行して、 **sqlcmd**ユーティリティを使用して Windows Internal database インスタンスから WSUS データベース (SUSDB) をデタッチします。

```batchfile
        sqlcmd -S \\.\pipe\Microsoft##WID\tsql\query
        use master
        GO
        alter database SUSDB set single_user with rollback immediate
        GO
        sp_detach_db SUSDB
        GO
```

### <a name="copy-the-susdb-files-to-the-sql-server"></a>SUSDB ファイルを SQL Server にコピーします。

1. **SUSDB**と**SUSDB\_log.ldf**を WID データフォルダー ( **% SystemDrive%** \* * Windows\WID\Data * *) から SQL インスタンスのデータフォルダーにコピーします。

> [!TIP]
> たとえば、SQL インスタンスフォルダーが**C:\Program SERVER\MSSQL12. sql の場合、MSSQLSERVER\MSSQL**、WID Data フォルダーは**C:\Windows\WID\Data で**、SUSDB ファイルを**C:\WINDOWS\WID\DATA**から**C:\Program Server\MSSQL12. SQL にコピーします。MSSQLSERVER\MSSQL\Data**

### <a name="attach-susdb-to-the-sql-instance"></a>SUSDB を SQL インスタンスにアタッチする

1. **SQL Server Management Studio**の **[インスタンス]** ノードで、 **[データベース]** を右クリックし、 **[アタッチ]** をクリックします。
    ![image3 @ no__t-1
2. **[データベースの接続]** ボックスの **[アタッチするデータベース]** で、 **[追加]** ボタンをクリックし、(WID フォルダーからコピーした) **SUSDB**ファイルを見つけて、 **[OK]** をクリックします。
    ![image4 @ no__t ![image5 @ no__t

> [!TIP]
> これは、Transact-sql を使用して行うこともできます。  [データベースをアタッチする](https://docs.microsoft.com/sql/relational-databases/databases/attach-a-database)手順については、SQL のドキュメントを参照してください。
>
> 例 (前の例のパスを使用):
> ```sql
>    USE master;
>    GO
>    CREATE DATABASE SUSDB
>    ON
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Data\SUSDB.mdf'),
>        (FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\Log\SUSDB_Log.ldf')
>        FOR ATTACH;
>    GO
>```

### <a name="verify-sql-server-and-database-logins-and-permissions"></a>SQL Server とデータベースのログインとアクセス許可を確認する

#### <a name="sql-server-login-permissions"></a>SQL Server のログイン権限

SUSDB をアタッチした後、次の手順を実行して、 **NT AUTHORITY\NETWORK SERVICE**に SQL Server のインスタンスへのログイン権限があることを確認します。

1. SQL Server Management Studio にアクセス
2. インスタンスを開く
3. **[セキュリティ]** をクリックします。
4. **[ログイン]** をクリックします。

**NT AUTHORITY\NETWORK SERVICE**アカウントが一覧表示されます。 そうでない場合は、新しいログイン名を追加して追加する必要があります。

> [!IMPORTANT]
> SQL インスタンスが WSUS とは別のコンピューターにある場合は、WSUS サーバーのコンピューターアカウントが **[FQDN] \\ [WSUSComputerName] $** の形式で一覧表示されている必要があります。  そうでない場合は、次の手順を使用して追加し、 **nt AUTHORITY\NETWORK SERVICE**を WSUS サーバーのコンピューターアカウント ( **[FQDN] \\ [WSUSComputerName] $** ) に置き換えます。これは、nt AUTHORITY \ に権限***を付与することに加え***て、 **ネットワークサービス**

##### <a name="adding-nt-authoritynetwork-service-and-granting-it-rights"></a>NT AUTHORITY\NETWORK サービスを追加してアクセス権を付与する

1. **[ログイン]** を右クリックし、 **[新しいログイン...]** をクリックします。
    ![image6 @ no__t-1
2. **[全般]** ページで、**ログイン名**(**NT AUTHORITY\NETWORK SERVICE**) に入力し、既定の**データベース**を SUSDB に設定します。
    ![image7 @ no__t-1
3. **[サーバーの役割]** ページで、 **[パブリック]** と **[sysadmin]** が選択されていることを確認します。
    ![image8 @ no__t-1
4. **[ユーザーマッピング]** ページで、次のようにします。
    - **[このログインにマップ]** されたユーザー で、 **[SUSDB]** を選択します。
    - @No__t-0Database ロールのメンバーシップ:SUSDB @ no__t-0 の場合は、次のチェックボックスがオンになっていることを確認します。
        - **共用**
        - **webService** ![image9 @ no__t-2
5. **[OK]** をクリックします。

[ログイン] の下に**NT AUTHORITY\NETWORK SERVICE**が表示されます。
![image10 @ no__t-1

#### <a name="database-permissions"></a>データベース権限

1. SUSDB を右クリックします。
2. **プロパティ**の選択
3. **[アクセス許可]**

**NT AUTHORITY\NETWORK SERVICE**アカウントが一覧表示されます。

1. そうでない場合は、アカウントを追加します。
2. [ログイン名] ボックスに、次の形式で WSUS コンピューターを入力します。
    > [**FQDN] \\ [WSUSComputerName] $**
3. **既定のデータベース**が**SUSDB**に設定されていることを確認します。

    > [!TIP]
    > 次の例では、FQDN は**Contosto.com** 、WSUS コンピューター名は**WsusMachine**です。
    >
    > ![image11](images/image11.png)

4. **[ユーザーマッピング]** ページで、 **[このログインにマップ]** されたユーザー の下にある**SUSDB**データベースを選択します。
5. @No__t-1 の [データベースロールのメンバーシップ] で**webservice**を確認します。SUSDB "** : ![image12 @ no__t
6. **[OK]** をクリックして設定を保存します。
    > [!NOTE]
    > 変更を有効にするには、SQL サービスの再起動が必要になる場合があります。

### <a name="edit-the-registry-to-point-wsus-to-the-sql-server-instance"></a>レジストリを編集して、WSUS が SQL Server インスタンスを指すようにします。

> [!IMPORTANT]
> 慎重にこのセクションの手順に従います。 誤ってレジストリを変更すると、重大な問題が発生する可能性があります。 変更する前に、問題が発生した場合に[復元するためにレジストリをバックアップ](https://support.microsoft.com/en-us/help/322756)します。

1. **[スタート]** ボタンをクリックし、 **[ファイル名を指定して実行]** をクリックして、「**regedit**」と入力し、 **[OK]** をクリックします。
2. 次のキーを見つけます。**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\UpdateServices\Server\Setup\SqlServerName**
3. **[値]** テキストボックスに「 **[ServerName] \\ [InstanceName]** 」と入力し、 **[OK]** をクリックします。 インスタンス名が既定のインスタンスの場合は、「 **[ServerName]** 」と入力します。
4. 次のキーを見つけます。**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-WidDatabase** ![image13 @ no__t-2
5. キーの名前を**Updateservices-Database** ![image41 @ no__t-2 に変更します。

    > [!NOTE]
    > このキーを更新しない場合、 **WsusUtil**は、移行した SQL インスタンスではなく、WID のサービスを試行します。

### <a name="start-the-iis-and-wsus-services-on-the-wsus-server"></a>WSUS サーバーで IIS および WSUS サービスを開始する

PowerShell (管理者特権) から、次のように実行します。

```powershell
    Start-Service IISADMIN
    Start-Service WsusService
```

> [!NOTE]
> WSUS コンソールを使用している場合は、を閉じて再起動します。

## <a name="uninstalling-the-wid-role-not-recommended"></a>WID ロールのアンインストール (推奨されません)

> [!WARNING]
> WID ロールを削除すると、インストール後のタスクに WSUSUtil が必要とするスクリプトを含むデータベースフォルダー ( **%SystemDrive%\Program Files\Update Services\Database**) も削除されます。 WID ロールをアンインストールする場合は、事前に **%SystemDrive%\Program Files\Update Services\Database**フォルダーをバックアップしていることを確認してください。

PowerShell の使用:

```powershell
Uninstall-WindowsFeature -Name 'Windows-Internal-Database'
```

WID ロールが削除されたら、次のレジストリキーが存在することを確認します。**HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Update Services\Server\Setup\Installed Role Services\UpdateServices-Database**