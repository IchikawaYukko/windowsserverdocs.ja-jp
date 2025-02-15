---
title: Server Core の管理
description: Windows Server の Server Core インストールの管理方法について説明します。
ms.prod: windows-server
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.date: 12/18/2018
ms.openlocfilehash: bcc4bf7b3fbdbff1aed2c8dd07b90346fe9eebab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383433"
---
# <a name="administer-a-server-core-server"></a>Server Core サーバーの管理

>適用対象:Windows Server 2019、Windows Server 2016、および Windows Server (半期チャネル)

Server Core には UI がないため、基本的な管理タスクを実行するには、Windows PowerShell コマンドレット、コマンドラインツール、またはリモートツールを使用する必要があります。 以下のセクションでは、基本的なタスクに使用する PowerShell コマンドレットとコマンドの概要について説明します。 また、現在パブリックプレビュー中の統合された管理ポータルである[Windows 管理センター](../../manage/windows-admin-center/overview.md)を使用して、のインストールを管理することもできます。 

## <a name="administrative-tasks-using-powershell-cmdlets"></a>PowerShell コマンドレットを使用した管理タスク
Windows PowerShell コマンドレットを使用して基本的な管理タスクを実行するには、次の情報を使用します。

### <a name="set-a-static-ip-address"></a>静的 IP アドレスを設定する
Server Core サーバーをインストールすると、既定では DHCP アドレスが使用されます。 静的 IP アドレスが必要な場合は、次の手順を使用して設定できます。

現在のネットワーク構成を表示するには、 **Get Ne? configuration**を使用します。

既に使用している IP アドレスを表示するには、 **new-netipaddress**を使用します。

静的 IP アドレスを設定するには、次の手順を実行します。 

1. **Get Ne? インターフェイス**を実行します。 
2. IP インターフェイスまたは**Interfacedescription**文字列の**IfIndex**列の番号に注意してください。 複数のネットワークアダプターがある場合は、静的 IP アドレスを設定するインターフェイスに対応する数値または文字列をメモします。
3. 次のコマンドレットを実行して、静的 IP アドレスを設定します。

   ```powershell
   New-NetIPaddress -InterfaceIndex 12 -IPAddress 192.0.2.2 -PrefixLength 24 -DefaultGateway 192.0.2.1
   ```

   それぞれの文字の説明は次のとおりです。
   - **InterfaceIndex**は、手順 2. の**IfIndex**の値です。 (この例では 12)
   - **IPAddress**は、設定する静的 IP アドレスです。 (この例では、191.0.2.2)
   - [**プレフィックス] は、** 設定する IP アドレスのプレフィックス長 (別の形式のサブネットマスク) です。 (この例では、24)
   - **DefaultGateway**は、デフォルトゲートウェイの IP アドレスです。 (この例では、192.0.2.1)
4. 次のコマンドレットを実行して、DNS クライアントサーバーのアドレスを設定します。 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4
   ```
   
   それぞれの文字の説明は次のとおりです。
   - **InterfaceIndex**は、手順 2. の IfIndex の値です。
   - **Serveraddresses**は、DNS サーバーの IP アドレスです。
5. 複数の DNS サーバーを追加するには、次のコマンドレットを実行します。 

   ```powershell
   Set-DNSClientServerAddress –InterfaceIndex 12 -ServerAddresses 192.0.2.4,192.0.2.5
   ```

   この例では、 **192.0.2.4 と 192.0.2.5**と**です。** は両方とも DNS サーバーの IP アドレスです。

DHCP の使用に切り替える必要がある場合は、 **Set-DnsClientServerAddress – InterfaceIndex 12 – ResetServerAddresses**を実行します。

### <a name="join-a-domain"></a>ドメインに参加
コンピューターをドメインに参加させるには、次のコマンドレットを使用します。

1. **コンピューターの追加**を実行します。 ドメインに参加するための資格情報とドメイン名の両方を入力するように求められます。
2. ローカルの Administrators グループにドメインユーザーアカウントを追加する必要がある場合は、コマンドプロンプトで次のコマンドを実行します (PowerShell ウィンドウではありません)。

   ```
   net localgroup administrators /add <DomainName>\<UserName>
   ```
3. コンピューターを再起動します。 これを行うには、**コンピューターの再起動**を実行します。

### <a name="rename-the-server"></a>サーバー名を変更する
サーバーの名前を変更するには、次の手順に従います。

1. **hostname** または **ipconfig** コマンドで現在のサーバー名を確認します。
2. **1new_name @ no__t の名前変更 @no__t を**実行します。
3. コンピューターを再起動します。

### <a name="activate-the-server"></a>サーバーのライセンス認証をする

**Slmgr.vbs – ipk @ no__t-1productkey @ no__t を**実行します。 次に、 **slmgr.vbs – ato**を実行します。 アクティブ化が成功した場合、メッセージは表示されません。

> [!NOTE]
> また、[キー管理サービス (KMS) サーバー](../../get-started/server-2016-activation.md)を使用して、またはリモートで、サーバーを電話でアクティブ化することもできます。 リモートからライセンス認証するには、リモートコンピューターから次のコマンドレットを実行します。 
> 
> ```
> cscript windows\system32\slmgr.vbs <ServerName> <UserName> <password>:-ato
> ```
 
### <a name="configure-windows-firewall"></a>Windows ファイアウォールを構成する

Windows PowerShell コマンドレットとスクリプトを使用して、Windows ファイアウォールを Server Core コンピューターにローカルに構成できます。 Windows ファイアウォールの構成に使用できるコマンドレットについては、「 [Netsecurity](/powershell/module/netsecurity/?view=win10-ps) 」を参照してください。

### <a name="enable-windows-powershell-remoting"></a>Windows PowerShell のリモート処理を有効にする

Windows PowerShell のリモート処理を有効にすることができます。Windows PowerShell のリモート処理では、あるコンピューターで入力したコマンドが別のコンピューターで実行されます。 **Enable-psremoting**を使用して Windows PowerShell リモート処理を有効にします。

詳細については、「[リモートの FAQ につい](/powershell/module/microsoft.powershell.core/about/about_remote_faq?view=powershell-5.1)て」を参照してください。

## <a name="administrative-tasks-from-the-command-line"></a>コマンドラインからの管理タスク
コマンドラインから管理タスクを実行するには、次の参照情報を使用します。

### <a name="configuration-and-installation"></a>構成とインストール

|                             タスク                              |                                                                                                                                                                                                                 コマンド                                                                                                                                                                                                                 |
|---------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|             ローカルの管理パスワードを設定する             |                                                                                                                                                                                                      **net ユーザー管理者**\*                                                                                                                                                                                                      |
|                  コンピューターのドメインへの参加                  |                                                                                                                                                       **netdom join% computername%** **/domain: \<domain @ no__t-3/userd: \<domain @ no__t-5username @ no__t-6/passwordd:** \* <br> コンピューターを再起動します。                                                                                                                                                        |
|              ドメインが変更されたことを確認する              |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|                コンピューターをドメインから削除する                |                                                                                                                                                                                                   **netdom \< computername @ no__t-2 を削除します。**                                                                                                                                                                                                    |
|         ローカルの Administrators グループにユーザーを追加する          |                                                                                                                                                                                       **net localgroup Administrators/add \<domain @ no__t-2username @ no__t-3**                                                                                                                                                                                       |
|       ローカルの Administrators グループからユーザーを削除する       |                                                                                                                                                                                     **net localgroup Administrators/delete \<domain @ no__t-2username @ no__t-3**                                                                                                                                                                                      |
|               ローカル コンピューターにユーザーを追加する                |                                                                                                                                                                                                **net ユーザー \<domain \ username @ no__t-2 \*/add**                                                                                                                                                                                                 |
|               ローカル コンピューターにグループを追加する               |                                                                                                                                                                                                 **net localgroup \<group name @ no__t-2/add**                                                                                                                                                                                                  |
|          ドメインに参加しているコンピューターの名前を変更する          |                                                                                                                                                           **netdom renamecomputer% computername%/newname: \< 新しいコンピューター名 @ no__t-2/userd: \<domain @ no__t-4username @ no__t-5/passwordd:** \*                                                                                                                                                            |
|                 新しいコンピューター名を確認する                 |                                                                                                                                                                                                                 **set**                                                                                                                                                                                                                 |
|         ワーク グループ内のコンピューターの名前を変更する         |                                                                                                                                                                **netdom renamecomputer \< currentcomputername @ no__t-2/NewName: \<newcomputername @ no__t** <br>コンピューターを再起動します。                                                                                                                                                                 |
|                ページング ファイルの管理を無効にする                 |                                                                                                                                                                        **wmic computersystem where name = "\<computername @ no__t-2" set [自動ページファイルの設定] = False**                                                                                                                                                                         |
|                   ページング ファイルを構成する                   |                                                            **wmic pagefileset where name = "\<path/filename @-2" set InitialSize = \<initialsize @ no__t-4, MaximumSize = \<maxsize @ no__t-6** <br>*Path/filename*はページングファイルのパスと名前、 *initialsize*はページングファイルの開始サイズ (バイト単位)、 *maxsize*はページファイルの最大サイズ (バイト単位) です。                                                             |
|                 静的 IP アドレスに変更する                 | **ipconfig/all** <br>関連する情報を記録するか、テキストファイル (**ipconfig/all > ipconfig**) にリダイレクトします。<br>**netsh interface ipv4 show インターフェイス**<br>インターフェイスリストがあることを確認します。<br>**netsh interface ipv4 set address name \<ID、interface list @ no__t-2 source = static address = \<preferred IP address @ no__t-4 gateway = \<gateway address @ no__t-6**<br>**Ipconfig/all**を実行して、DHCP Enabled が**No**に設定されていることを確認します。 |
|                   静的 DNS アドレスを設定します。                   |   <strong>netsh interface ipv4 add dnsserver name = \< ネットワークインターフェイスカードの名前または ID @ no__t アドレス = \< プライマリ DNS サーバーの IP アドレス @ no__t-4 index = 1 <br></strong>netsh interface ipv4 add dnsserver name = \< セカンダリ DNS サーバーの名前 @ no__t-2 address = \<IP サーバーの IP アドレス @ no__t-4 index = 2 @ no__t-5 @ no__t-6 <br> 必要に応じて、サーバーを追加します。<br>**Ipconfig/all**を実行して、アドレスが正しいことを確認します。   |
| 静的 IP アドレスから DHCP によって提供された IP アドレスに変更する |                                                                                                                                      **netsh interface ipv4 set address name = \< ローカルシステムの IP アドレス @ no__t-2 source = DHCP** <br>**Ipconfig/all**を実行して、DCHP Enabled が**Yes**に設定されていることを確認します。                                                                                                                                      |
|                      プロダクト キーを入力する                      |                                                                                                                                                                                                   **slmgr.vbs – ipk \< プロダクトキー @ no__t**                                                                                                                                                                                                    |
|                  サーバーをローカルにライセンス認証する                  |                                                                                                                                                                                                           **slmgr.vbs-ato**                                                                                                                                                                                                            |
|                 サーバーをリモートからライセンス認証する                  |                                            **cscript slmgr.vbs – ipk \<product key @ no__t-2 @ no__t-3server name @ no__t @ no__t-5username @ no__t-6 @ no__t-7password @ no__t-8** <br>**cscript slmgr.vbs-ato \< servername @ no__t-2 \<username @ no__t \<password @ no__t-6** <br>**Cscript slmgr.vbs**を実行してコンピューターの GUID を取得する <br> **Cscript slmgr.vbs の実行-dli \< guid @ no__t-2** <br>ライセンスステータスが "ライセンス済み **(アクティブ化済み)** " に設定されていることを確認します。                                             |

### <a name="networking-and-firewall"></a>ネットワークとファイアウォール

|タスク|コマンド| 
|----|-------|
|プロキシサーバーを使用するようにサーバーを構成する|**netsh Winhttp set proxy \<servername @ no__t: \<port number @ no__t-4** <br>**注:** Server Core インストールでは、接続を許可するためにパスワードを必要とするプロキシ経由でインターネットにアクセスすることはできません。|
|インターネットアドレスのプロキシをバイパスするようにサーバーを構成する|**netsh winhttp set proxy \<servername @ no__t: \<port number @ no__t-4 バイパス-list = "\<local @ no__t"**| 
|IPSEC 構成を表示または変更する|**netsh ipsec**| 
|NAP 構成を表示または変更する|**netsh nap**| 
|IP から物理アドレスへの変換を表示または変更する|**arp**| 
|ローカルルーティングテーブルを表示または構成する|**回送**| 
|DNS サーバーの設定を表示または構成する|**nslookup**| 
|プロトコル統計と現在の TCP/IP ネットワーク接続を表示する|**netstat**| 
|NetBIOS over TCP/IP (NBT) を使用してプロトコル統計と現在の TCP/IP 接続を表示する|**nbtstat**| 
|ネットワーク接続のホップを表示する|**pathping**| 
|ネットワーク接続のホップをトレースする|**tracert**| 
|マルチキャスト ルーターの構成を表示する|**mrinfo**| 
|ファイアウォールのリモート管理を有効にする|**netsh advfirewall firewall set rule group = "Windows Defender ファイアウォールリモート管理" 新しい有効化 = はい**| 
 

### <a name="updates-error-reporting-and-feedback"></a>更新プログラム、エラー報告、およびフィードバック

|                               タスク                                |                                                                                                                               コマンド                                                                                                                                |
|-------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                         更新プログラムをインストールする                         |                                                                                                                    **wusa \<update\>.msu/quiet**                                                                                                                    |
|                      インストールされている更新プログラムの一覧を表示する                       |                                                                                                                            **systeminfo**                                                                                                                            |
|                         更新プログラムを削除する                          |                                 **展開/f: \* \<update\>.msu c:\test** <br>C:\test\ に移動し、テキストエディターで \<update\>.xml を開きます。<br>**[インストール]** を **[削除]** に置き換えて、ファイルを保存します。<br>**pkgmgr/n: \<update\>.xml**                                 |
|                    自動更新を構成する                    |          現在の設定を確認するには、 **cscript%systemroot%\system32\scregedit.wsf/AU/v \* @ no__t @ no__t、自動更新を有効にするには、次のようにします。 \* @ no__t-5cscript scregedit.exe/au 4** <br>自動更新を無効にするには: **cscript%systemroot%\system32\scregedit.wsf/AU 1**          |
|                      エラー報告を有効にする                       | 現在の設定を確認するには: **serverWerOptin/query** <br>詳細レポートを自動的に送信するには: **serverWerOptin/detailed** <br>概要レポートを自動的に送信するには: **serverWerOptin/summary** <br>エラー報告を無効にするには: **serverWerOptin/disable** |
| カスタマー エクスペリエンス向上プログラム (CEIP) に参加する |                                                     現在の設定を確認するには: **serverCEIPOptin/query** <br>CEIP を有効にするには: **serverCEIPOptin/enable** <br>CEIP を無効にするには: **serverCEIPOptin/disable**                                                      |

### <a name="services-processes-and-performance"></a>サービス、プロセス、およびパフォーマンス

|                               タスク                               |                                                                                                                                                                                                             コマンド                                                                                                                                                                                                              |
|------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                    実行中のサービスを一覧表示する                     |                                                                                                                                                                                                  **sc クエリ**または**net start**                                                                                                                                                                                                   |
|                         サービスを開始する                          |                                                                                                                                                                                 **sc start \<service name @ no__t-2**または**net start \<service name @ no__t**                                                                                                                                                                                  |
|                          サービスを停止する                          |                                                                                                                                                                                  **sc stop \< サービス名 @ no__t-2**または**net stop \<service name @ no__t**                                                                                                                                                                                   |
| 実行中のアプリケーションと関連するプロセスの一覧を取得する |                                                                                                                                                                                                           **tasklist**                                                                                                                                                                                                           |
|                        タスク マネージャーを開始する                        |                                                                                                                                                                                                           **taskmgr-networking**                                                                                                                                                                                                            |
|    イベントトレースセッションとパフォーマンスログの作成と管理    | カウンター、トレース、構成データの収集、API を作成するには **、次のように**します。 <br>データコレクターのプロパティを照会するには: **logman クエリ** <br>データ収集を開始または停止するには: **logman start @ no__t-1stop** <br>コレクターを削除するには: **logman delete** <br> コレクターのプロパティを更新するには: **logman update** <br>XML ファイルからデータコレクターセットをインポートする、または XML ファイルにエクスポートするには、次のようにします。 **logman import @ no__t-1export** |

### <a name="event-logs"></a>イベント ログ

|タスク|コマンド| 
|----|-------|
|イベントログの一覧表示|**wevtutil el**| 
|指定されたログのイベントを照会する|**wevtutil qe/f: テキスト \< ログ名 @ no__t-2**| 
|イベントログをエクスポートする|**wevtutil epl \< ログ名 @ no__t-2**| 
|イベントログの消去|**wevtutil cl \< ログ名 @ no__t-2**| 


### <a name="disk-and-file-system"></a>ディスクとファイル システム

|                   タスク                   |                        コマンド                        |
|------------------------------------------|-------------------------------------------------------|
|          ディスク パーティションを管理する          | コマンドの完全な一覧を表示するには、 **diskpart/?** を実行します。  |
|           ソフトウェアの RAID を管理する           | コマンドの完全な一覧を表示するには、 **diskraid/?** を実行します。  |
|        ボリューム マウント ポイントを管理する        | コマンドの完全な一覧を表示するには、 **mountvol/?** を実行します。  |
|           ボリュームを最適化する            |  コマンドの完全な一覧を表示するには、 **defrag/?** を実行します。   |
| ボリュームを NTFS ファイル システムに変換する |        **convert \< ボリューム文字 @ no__t-2/FS: NTFS**         |
|              ファイルを圧縮する              |  コマンドの完全な一覧を表示するには、 **compact/?** を実行します。  |
|          開いているファイルを管理する           | コマンドの完全な一覧については、「 **openfiles/?** 」を実行してください。 |
|          VSS フォルダーを管理する          | コマンドの完全な一覧については、「 **vssadmin/?** 」を実行してください。  |
|        ファイル システムを管理する        |  コマンドの完全な一覧を表示するには、 **fsutil/?** を実行します。   |
|    ファイルまたはフォルダーの所有権を取得する    |  コマンドの完全な一覧を表示するには、 **icacls/?** を実行します。   |
 
### <a name="hardware"></a>ハードウェア

|タスク|コマンド| 
|----|-------|
|新しいハードウェア デバイスのドライバーを追加する|% Homedrive% \\ @ no__t-1driver フォルダー @ no__t にあるフォルダーにドライバーをコピーします。 **Pnputil の実行-i-a% homedrive% \\ @ no__t-2driver フォルダー @ no__t-3 @ no__t**|
|ハードウェア デバイスのドライバーを削除する|読み込まれたドライバーの一覧を表示するには、 **sc query type = driver**を実行します。 次**に、sc delete \<service_name @ no__t-2**を実行します。|
