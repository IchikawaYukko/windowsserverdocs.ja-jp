---
title: セットアップおよびブート イベント収集を使用する
description: セットアップおよびブート イベント収集のコレクターとターゲットの設定
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-sbec
ms.localizationpriority: medium
ms.date: 10/16/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247b3f8
author: jaimeo
ms.author: jaimeo
ms.openlocfilehash: d1d24cdf481f19b37093f76cf8741702e1b4de60
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370508"
---
# <a name="get-started-with-setup-and-boot-event-collection"></a>セットアップおよびブート イベント収集を使用する

>適用先:Windows Server

  
## <a name="overview"></a>概要  
セットアップおよびブート イベント収集は、"コレクター" コンピューターを指定して、起動時またはセットアップ プロセスの実行時に、他のコンピューターで発生するさまざまな重要イベントを収集できる Windows Server 2016 の新しい機能です。 その後、収集されたイベントは、イベント ビューアー、メッセージ アナライザー、Wevtutil、または Windows PowerShell コマンドレットを使用して分析できます。  
  
以前は、これらのイベントは監視することができませんでした。これは、コンピューターがセットアップされるまで、イベントを収集するために必要なインフラストラクチャが存在しないためです。 監視できるセットアップおよびブート イベントの種類には次のものがあります。  
  
-   カーネル モジュールとドライバーの読み込み  
  
-   ドライバーのデバイスと初期化の列挙 (CPU の種類などの "デバイス" を含む)  
  
-   ファイル システムの確認とマウント  
  
-   実行可能ファイルの起動  
  
-   システムの更新の開始と完了  
  
-   システムがログオンに利用できるようになった時点、ドメイン コントローラーとの接続を確立した時点、サービスの完了が開始した時点、ネットワーク共有が利用可能になった時点  
  
コレクター コンピューターでは、Windows Server 2016 (デスクトップ エクスペリエンス搭載サーバーまたは Server Core モードのいずれか) を実行している必要があります。 対象のコンピューターでは、Windows 10 または Windows Server 2016 のいずれかを実行している必要があります。 また、Windows Server 2016 を実行して**いない**コンピューターでホストされている仮想マシンでこのサービスを実行することもできます。 次の仮想化されたコレクターと対象のコンピューターの組み合わせが動作することが確認されています。  
  
|仮想化ホスト|コレクター仮想マシン|ターゲット仮想マシン|  
|-----------------------|-----------------------------|--------------------------|  
|Windows 8.1|はい|はい|  
|Windows 10|はい|はい|  
|Windows Server 2016|はい|はい|  
|Windows Server 2012 R2|はい|Ｘ|  
  
## <a name="installing-the-collector-service"></a>コレクター サービスのインストール  
Windows Server 2016 以降、イベント コレクター サービスがオプションの機能として利用可能です。 このリリースでは、管理者特権の Windows PowerShell プロンプトで次のコマンドを指定し、DISM.exe を使用してこの機能をインストールすることができます。  
  
`dism /online /enable-feature /featurename:SetupAndBootEventCollection`  
  
このコマンドは、BootEventCollector と呼ばれるサービスを作成し、空の構成ファイルでそのサービスを起動します。  
  
`get-service -displayname *boot*` を確認することで、インストールが正常に終了したことを確認します。 ブート イベント コレクターが実行されている必要があります。 このコレクターはネットワーク サービス アカウントで実行され、空の構成ファイル (Active.xml) を **%SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config** に作成します。  
  
サーバー マネージャーの役割と機能の追加ウィザードを使用して、セットアップおよびブート イベント収集サービスをインストールすることもできます。  
  
## <a name="configuration"></a>構成  
セットアップおよびブート イベントを収集するために 2 つの項目を構成する必要があります。  
  
-   イベントを送信する対象のコンピューター (つまり、セットアップおよびブートを監視するコンピューター) で、KDNET/EVENT-NET トランスポートおよびイベントの転送を有効にします。  
  
-   コレクター コンピューターで、イベントの受け取り元のコンピューターとイベントの保存場所を指定します。  
  
> [!NOTE]  
> 起動イベントまたはブート イベントをそれ自身に送信するようにコンピューターを構成することはできません。 ただし、2 台のコンピューターを監視する場合は、イベントを互いに送信するようにコンピューターを構成することができます。  
  
### <a name="configuring-a-target-computer"></a>対象のコンピューターの構成  
それぞれの対象のコンピューターで、まず KDNET/EVENT-NET トランスポートを有効にし、次にそのトランスポートを介した ETW イベントの送信を有効にした後、対象のコンピューターを再起動します。 EVENT-NET は、KDNET (カーネル デバッガー プロトコル) に類似したインカーネル トランスポート プロトコルです。 EVENT-NET はイベントのみを送信し、デバッガー アクセスを許可しません。 これら 2 つのプロトコルは相互に排他的です。一度に 1 つのみ有効にできます。  
  
イベント トランスポートを (Windows PowerShell を使用して) リモートで有効にするか、またはローカルで有効にすることができます。  
  
##### <a name="to-enable-event-transport-remotely"></a>イベント トランスポートをリモートで有効にするには  
  
1.  対象のコンピューターに対して Windows PowerShell のリモート処理を既に設定している場合は、手順 3 に進みます。 設定していない場合は、対象のコンピューターで、コマンド プロンプトを開き、次のコマンドを実行します。  
  
    winrm quickconfig  
  
2.  表示された指示に応答し、対象のコンピューターを再起動します。 対象のコンピューターがコレクター コンピューターと同じドメインにない場合は、信頼できるホストとして定義することが必要な場合もあります。 これを行うには :  
  
3.  コレクター コンピューターで、次のコマンドのいずれかを実行します。  
  
    -   Windows PowerShell プロンプトで、`Set-Item -Force WSMan:\localhost\Client\TrustedHosts "<target1>,<target2>,..."` の後に `Set-Item -Force WSMan:\localhost\Client\AllowUnencrypted true` を指定します。 \<target1 > などです。はターゲットコンピューターの名前または IP アドレスです。  
  
    -   コマンドプロンプトで、 **winrm set winrm/config/client @ {TrustedHosts = "\<target1 >, \<target2 >,...";AllowUnencrypted されていない = "true"}**  
  
        > [!IMPORTANT]  
        > これにより暗号化されていない通信が設定されるため、ラボ環境の外部ではこれを行わないでください。  
  
4.  コレクター コンピューターに移動し、次のいずれかの Windows PowerShell コマンドを実行して、リモート接続をテストします。  
  
    ターゲットコンピューターがコレクターコンピューターと同じドメインにある場合は、`New-PSSession -Computer <target> | Remove-PSSession` を実行します。  
  
    対象のコンピューターが同じドメインにない場合は、`New-PSSession -Computer  <target>  -Credential Administrator | Remove-PSSession` を実行します。これにより資格情報を求めるメッセージが表示されます。  
  
    コマンドで何も返されなければ、リモート処理は正常に完了しました。  
  
5.  対象のコンピューターで、管理者特権の Windows PowerShell プロンプトを開き、次のコマンドを実行します。  
  
    `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d>`  
  
    ここで < target_name > はターゲットコンピューターの名前、\<ip > はコレクターコンピューターの IP アドレスです。 \<port > はコレクターが実行されるポート番号です。 Key <a.b.c.d> は通信に必要な暗号化キーで、ドットで区切られた 4 つの英数字文字列で構成されます。 コレクター コンピューターでは、これと同じキーが使用されます。 キーを入力しない場合、システムによってランダムなキーが生成されます。このキーはコレクター コンピューターで必要になるので、メモしておきます。  
  
6.  既にコレクター コンピューターを設定している場合は、新しい対象のコンピューターの情報でコレクター コンピューターの構成ファイルを更新します。 詳細については、「コレクター コンピューターの構成」のセクションを参照してください。  
  
##### <a name="to-enable-event-transport-locally-on-the-target-computer"></a>対象のコンピューターでイベント トランスポートをローカルで有効にするには  
  
1.  管理者特権のコマンド プロンプトを起動し、次のコマンドを実行します。  
  
    **bcdedit/event はい**  
  
    **bcdedit/eventsettings net hostip: 1.2.3.4 port: 50000 key: a. b. d. d**  
  
    ここで "1.2.3.4" は例です。これをコレクター コンピューターの IP アドレスで置き換えます。 また、"50000" をコレクターが実行されるポート番号で置き換え、"a.b.c.d" を通信に必要な暗号化キーで置き換えます。 コレクター コンピューターでは、これと同じキーが使用されます。 キーを入力しない場合、システムによってランダムなキーが生成されます。このキーはコレクター コンピューターで必要になるので、メモしておきます。  
  
2.  既にコレクター コンピューターを設定している場合は、新しい対象のコンピューターの情報でコレクター コンピューターの構成ファイルを更新します。 詳細については、「コレクター コンピューターの構成」のセクションを参照してください。  
  
**イベントトランスポート自体が有効になったので、そのトランスポートを通じて実際に ETW イベントを送信するようにシステムを有効にする必要があります。**  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-remotely"></a>トランスポート経由での ETW イベントの送信をリモートで有効にするには  
  
1.  コレクター コンピューターで、管理者特権の Windows PowerShell ウィンドウを開きます。  
  
2.  `Enable-SbecAutologger -ComputerName <target_name>` を実行します。ここで、<target_name> は対象のコンピューターの名前です。  
  
Windows PowerShell のリモート処理を設定できない場合は、いつでも対象のコンピューターで直接イベントの送信を有効にすることができます。  
  
##### <a name="to-enable-sending-of-etw-events-through-the-transport-locally"></a>トランスポート経由での ETW イベントの送信をローカルで有効にするには  
  
1.  対象のコンピューターで、Regedit.exe を起動し、次のレジストリ キーを見つけます。  
  
    **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\WMI\AutoLogger** このキーの下にさまざまなログ セッションがサブキーとして記載されています。 **Setup Platform**、**NT Kernel Logger**、および **Microsoft-Windows-Setup** はセットアップおよびブート イベント収集で使用するために可能な選択肢ですが、推奨されるオプションは **EventLog-System** です。 これらのキーの詳細については、「[自動ロガー セッションの構成と開始](https://msdn.microsoft.com/library/windows/desktop/aa363687(v=vs.85).aspx)」を参照してください。  
  
2.  EventLog-System キーで、**LogFileMode** の値を **0x10000180** から **0x10080180** に変更します。 これらの設定の詳細については、「[ログ モードの定数](https://msdn.microsoft.com/library/windows/desktop/aa364080(v=vs.85).aspx)」を参照してください。  
  
3.  必要に応じて、コレクター コンピューターへのバグ チェック データの転送も有効にすることができます。 これを行うには、レジストリ キー HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager を見つけ、**0x1** の値を指定してキー **Debug Print Filter** を作成します。  
  
4.  対象のコンピューターを再起動します。  
  
### <a name="choosing-the-network-adapter"></a>ネットワーク アダプターの選択  
対象のコンピューターに複数のネットワーク アダプターがある場合、KDNET ドライバーは一覧で最初に表示されるサポートされたネットワーク アダプターを選択します。 次の手順に従い、セットアップ イベントの転送に使用する特定のネットワーク アダプターを指定することができます。  
  
##### <a name="to-specify-a-network-adapter"></a>ネットワーク アダプターを指定するには  
  
1.  対象のコンピューターで、デバイス マネージャーを開き、 **[ネットワーク アダプター]** を展開し、使用するネットワーク アダプターを見つけて右クリックします。  
  
2.  表示されるメニューで、 **[プロパティ]** をクリックし、 **[詳細]** タブをクリックします。**プロパティ**フィールドのメニューを展開し、 **[場所情報]** の検索 までスクロールします (リストはアルファベット順になっていない場合があります)。その後、それをクリックします。 この値は、 **PCI バス X、デバイス Y、関数 Z**の形式の文字列になります。X.y をメモしておきます。これらは、次のコマンドに必要なバスパラメーターです。  
  
3.  次のコマンドのいずれかを実行します。  
  
    管理者特権の Windows PowerShell プロンプトから: `Enable-SbecBcd -ComputerName <target_name> -CollectorIP <ip> -CollectorPort <port> -Key <a.b.c.d> -BusParams <X.Y.Z>`  
  
    管理者特権のコマンド プロンプトで、**bcdedit /eventsettings  net hostip:aaa port:50000 key:bbb busparams:X.Y.Z** を指定します。  
  
### <a name="validate-target-computer-configuration"></a>対象のコンピューターの設定の検証  
対象のコンピューターの設定を確認するには、管理者特権のコマンド プロンプトを開き、**bcdedit /enum** を実行します。 これが完了したら、**bcdedit /eventsettings** を実行します。 次の値を再確認することができます。  
  
-   キー  
  
-   Debugtype = NET  
  
-   Hostip = @no__t-コレクターの > の IP アドレス  
  
-   ポート = @no__t-コレクターが使用するように指定したポート番号 >  
  
-   DHCP = yes  
  
また、 **/debug** と **/event** は相互に排他的であるため、**bcdedit /event** を有効にしたことを確認してください。 どちらか一方のみ実行できます。 同様に、/eventsettings と /debug、または /dbgsettings と /event を混在させることはできません。  
  
イベント コレクションをシリアル ポートに設定した場合は機能しないことにも注意してください。  
  
### <a name="configuring-the-collector-computer"></a>コレクター コンピューターの構成  
コレクター サービスではイベントを受信し、ETL ファイルに保存します。 これらの ETL ファイルは、イベント ビューアー、メッセージ アナライザー、Wevtutil、Windows PowerShell コマンドレットなどの他のツールで読み取ることができます。  
  
ETW 形式では対象のコンピューター名を指定することができないため、それぞれの対象のコンピューターのイベントは別のファイルに保存する必要があります。 表示ツールにコンピューター名が表示される場合がありますが、それはツールが実行されるコンピューターの名前になります。  
  
正確には、それぞれの対象のコンピューターに ETL ファイルのリングが割り当てられます。 各ファイル名には、000 から構成した最大値 (999 まで) のインデックスが含まれます。 ファイルが構成されている最大サイズに達すると、イベントの書き込みを次のファイルに切り替えます。 最も大きなファイルの後で、ファイル インデックス 000 に戻ります。 このようにして、ファイルは自動的にリサイクルされ、ディスク領域の使用が制限されます。 また、追加の外部保持ポリシーを設定してディスクの使用率をさらに制限することもできます。たとえば、設定した日数を経過しているファイルを削除することができます。  
  
収集された ETL ファイルは通常、**c:\ProgramData\Microsoft\BootEventCollector\Etl** (さらにサブディレクトリがある場合があります) に保持されます。 最終変更時刻で並べ替えることによって、最新のログ ファイルを見つけることができます。 また、コレクターが書き込みを新しいファイルに切り替えた場合に記録する、ステータス ログ (通常、**c:\ProgramData\Microsoft\BootEventCollector\Logs**) もあります。  
  
コレクター自体に関する情報を記録するコレクター ログもあります。 このログを ETW 形式 (イベントが Windows ログ サービスに報告される形式で、これが既定です) で保存するか、またはファイル (通常は **c:\ProgramData\Microsoft\BootEventCollector\Logs**) に保存できます。 ファイルを使用すると、大量のデータを生成する詳細モードを有効にする場合に便利です。 コマンド ラインからコレクターを実行して、標準出力に書き込むログを設定することもできます。  
  
**コレクター構成ファイルを作成しています**  
  
サービスを有効にすると、3つの XML 構成ファイルが作成され、 **c:\ProgramData\Microsoft\BootEventCollector\Config**に格納されます。  
  
-   **Active.xml** このファイルには、コレクター サービスの現在アクティブな構成が含まれます。  インストールの直後には、このファイルには Empty.xml と同じ内容が含まれます。 新しいコレクター構成を設定する場合は、このファイルに保存します。  
  
-   **Empty.xml** このファイルには、設定される既定値で必要とされる最小限の構成要素が含まれます。 このファイルはコレクションを有効にするものではありません。コレクター サービスをアイドル モードで起動できるようにするだけです。  
  
-   **Example.xml** このファイルには、可能な構成要素の例と説明が記録されています。  
  
**ファイルサイズの制限の選択**  
  
行う必要がある決定の 1 つとして、ファイル サイズの制限を設定することがあります。 最適なファイル サイズの制限は、予想されるイベントの量や使用可能なディスク領域によって異なります。 古いデータを消去するという観点からは、小さいファイルの方が便利です。 ただし、各ファイルには、64 KB のヘッダーのオーバーヘッドが含まれています。また、複数のファイルを読み込んで、履歴を結合すると不便な場合があります。 最小ファイルサイズの上限は 256 KB です。 適度に実用的なファイル サイズの制限は 1 MB 以上である必要があり、10 MB がおそらく一般的な適切な値です。 多くのイベントが予想される場合は、制限値を高くすることが妥当です。  
  
構成ファイルに関して留意する必要がある詳細がいくつかあります。  
  
-   対象のコンピューターのアドレス。 IPv4 アドレス、MAC アドレス、または SMBIOS GUID を使用することができます。 使用するアドレスを選択する場合は、これらの要因に注意してください。  
  
    -   IPv4 アドレスは、IP アドレスの静的割り当てに最適です。 ただし、静的 IP アドレスでも DHCP 経由で使用できる必要があります。  
  
    -   MAC アドレスまたは SMBIOS GUID は、あらかじめわかっている場合に便利ですが、IP アドレスは動的に割り当てられます。  
  
    -   IPv6 アドレスは、EVENT-NET プロトコルではサポートされません。  
  
    -   コンピューターを識別するために複数の方法を指定することができます。 たとえば、物理ハードウェアを交換しようとしている場合は、古い MAC アドレスと新しい MAC アドレスの両方を入力すると、いずれかが受け入れられます。  
  
-   コレクター コンピューターとの通信に使用する暗号化キー  
  
-   対象のコンピューターの名前。 コンピューター名として、IP アドレス、ホスト名、またはその他の任意の名前を使用することができます。  
  
-   使用する ETL ファイルの名前とそのリング サイズ構成  
  
##### <a name="to-create-the-configuration-file"></a>構成ファイルを作成するには  
  
1.  管理者特権の Windows PowerShell プロンプトを開き、ディレクトリを %SystemDrive%\ProgramData\Microsoft\BootEventCollector\Config に変更します。  
  
2.  `notepad .\newconfig.xml` を入力し、ENTER を押します。  
  
3.  次の構成例をメモ帳のウィンドウにコピーします。  
  
    ```  
    <collector configVersionMajor="1" statuslog="c:\ProgramData\Microsoft\BootEventCollector\Logs\statuslog.xml">  
      <common>  
        <collectorport value="50000"/>  
        <forwarder type="etl">  
          <set name="file" value="c:\ProgramData\Microsoft\BootEventCollector\Etl\{computer}\{computer}_{#3}.etl"/>  
          <set name="size" value="10mb"/>  
          <set name="nfiles" value="10"/>  
          <set name="toxml" value="none"/>  
        </forwarder>  
        <target>  
          <ipv4 value="192.168.1.1"/>  
          <key value="a.b.c.d"/>  
          <computer value="computer1"/>  
        </target>  
        <target>  
          <ipv4 value="192.168.1.2"/>  
          <key value="d1.e2.f3.g4"/>  
          <computer value="computer2"/>  
        </target>  
      </common>  
    </collector>  
    ```  
  
    > [!NOTE]  
    > ルートノードは @no__t 0collector > です。 その属性は、構成ファイルの構文のバージョンとステータス ログ ファイルの名前を指定します。  
    >   
    > @No__t 0common > 要素は複数のターゲットをグループ化し、それらの共通構成要素を指定しています。ユーザーグループを使用すると、複数のユーザーに共通のアクセス許可を指定できます。  
    >   
    > @No__t 0collectorport > 要素は、コレクターが受信データをリッスンする UDP ポート番号を定義します。 これは、Bcdedit のターゲット構成の手順で指定されたのと同じポートです。 コレクターは 1 つのポートのみをサポートし、すべてのターゲットが同じポートに接続する必要があります。  
    >   
    > @No__t 0forwarder の > 要素は、対象のコンピューターから受信した ETW イベントを転送する方法を指定します。 フォワーダーは 1 種類のみで、イベントを ETL ファイルに書き込みます。 パラメーターは、ファイル名のパターン、リング内の各ファイルのサイズ制限、各コンピューターのリングのサイズを指定します。 "toxml" 設定は、ETW イベントが受信されたときに、XML に変換することなく、バイナリ形式で書き込まれるように指定します。 イベントを XML に変換するかどうかの決定については、「XML イベントの変換」に関するセクションを参照してください。 ファイル名のパターンでは、{computer} にコンピューター名を、{#3} にリング内のファイルのインデックスを代入します。  
    >   
    > この例のファイルでは、2つのターゲットコンピューターが @no__t 0target > 要素で定義されています。 各定義では、@no__t 0ipv4 > の IP アドレスを指定します。 MAC アドレスを使用することもできます (たとえば、mac 値 = "11:22:33:44:55:66" @no__t-< 1 > または < mac 値 = "11-22-33-44-55-66" \/ >) または SMBIOS GUID (たとえば、< guid) を使用することもできます。対象のコンピューターを識別するために、値 = "{269076f947 B774-6E1-b03b03 2 >) @no__t ます。 また、暗号化キー (対象のコンピューターで Bcdedit で指定または生成されたものと同じ) およびコンピューター名を確認してください。  
  
4.  各ターゲットコンピューターの詳細を、構成ファイル内の個別の @no__t 0target > 要素として入力し、Newconfig を保存してメモ帳を閉じます。  
  
5.  `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` を指定して新しい構成を適用します。 Succes フィールドが "true" の出力が返されます。 別の結果が表示される場合は、このトピックの「トラブルシューティング」のセクションを参照してください。  
  
`(Get-SbecActiveConfig).text` で現在アクティブな構成をいつでも確認することができます。  
  
`$result = (Get-Content .\newconfig.xml | Check-SbecConfig); $result` で構成ファイルの有効性チェックを実行することができます。  
  
新しい構成を適用する Windows PowerShell コマンドは、再起動しなくてもサービスを自動的に更新しますが、次のコマンドのいずれかを使用して自分でいつでもサービスを再起動することができます。  
  
-   Windows PowerShell の場合: `Restart-Service BootEventCollector`  
  
-   通常のコマンド プロンプトの場合: **sc stop BootEventCollector; sc start BootEventCollector**  
  
## <a name="configuring-nano-server-as-a-target-computer"></a>対象のコンピューターとしての Nano Server の構成  
Nano Server で提供される最小限のインターフェイスを使用すると、問題を診断することが困難になる場合があります。 セットアップおよびブート イベント収集に自動的に参加し、手動による操作を行うことなくコレクター コンピューターに診断データを送信するように Nano Server イメージを構成することができます。 これを行うには、次の手順を実行します。  
  
#### <a name="to-configure-nano-server-as-a-target-computer"></a>Nano Server を対象のコンピューターとして構成するには  
  
1.  基本的な Nano Server イメージを作成します。 詳細については、[Nano Server の概要](https://technet.microsoft.com/library/mt126167.aspx)に関するページを参照してください。  
  
2.  このトピックの「コレクター コンピューターの構成」のセクションに示すようにコレクター コンピューターをセットアップします。  
  
3.  診断メッセージの送信を有効にするには AutoLogger レジストリ キーを追加します。 これを行うには、手順 1 で作成した Nano Server VHD をマウントし、レジストリ ハイブをロードして、特定のレジストリ キーを追加します。 この例では、Nano Server イメージが C:\NanoServer にあります。パスは異なる場合があるので、適宜手順を調整してください。  
  
    1.  コレクター コンピューターで、 **..\Windows\System32\WindowsPowerShell\v1.0\Modules\BootEventCollector** フォルダーをコピーし、Nano Server VHD を変更するために使用しているコンピューターの **..\Windows\System32\WindowsPowerShell\v1.0\Modules** ディレクトリに貼り付けます。  
  
    2.  管理者特権のアクセス許可を使って Windows PowerShell コンソールを起動し、`Import-Module BootEventCollector` を実行します。  
  
    3.  Nano Server VHD レジストリを更新して自動ロガーを有効にします。 これを行うには、`Enable-SbecAutoLogger -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd` を実行します。 これにより、最も一般的なセットアップおよびブート イベントの基本的な一覧が追加されます。「[イベント トレース セッションの制御](https://msdn.microsoft.com/library/windows/desktop/aa363694(v=vs.85).aspx)」でその他を調べることができます。  
  
4.  Events フラグを有効にし、コレクター コンピューターを設定するように Nano Server イメージの BCD 設定を更新して、診断イベントが適切なサーバーに送信されることを確認します。 コレクター コンピューターの IPv4 アドレス、TCP ポート、およびコレクターの Active.XML ファイルで構成した暗号化キー (このトピックの他の場所で説明されています) を書き留めます。 管理者特権でのアクセス許可を持つ Windows PowerShell コンソールで、次のコマンドを使用します: `Enable-SbecBcd -Path C:\NanoServer\Workloads\IncludingWorkloads.vhd -CollectorIp 192.168.100.1 -CollectorPort 50000 -Key a.b.c.d`  
  
5.  Nano Server の IPv4 アドレスの範囲、特定の IPv4 アドレス、または MAC アドレスのいずれかをコレクター コンピューターの Active.XML ファイルに追加することで、Nano Server コンピューターで送信されたイベントを受信するようにコレクター コンピューターを更新します (このトピックの「コレクター コンピューターの構成」のセクションを参照してください)。  
  
## <a name="starting-the-event-collector-service"></a>イベント コレクター サービスの開始  
コレクター コンピューターに有効な構成ファイルが保存され、対象のコンピューターが構成されたら、対象のコンピューターが再起動されるとすぐに、コレクターへの接続が確立され、イベントが収集されます。  
  
コレクター サービス自体のログ (サービスによって収集されたセットアップおよびブート データとは異なります) は、Microsoft-Windows-BootEvent-Collector/Admin の下にあります。 イベントのグラフィカル インターフェイスには、イベント ビューアーを使用します。 新しいビューを作成し、 **[アプリケーションとサービス ログ]** 、 **[Microsoft]** 、 **[Windows]** の順に展開します **[BootEvent-Collector]** を見つけて展開し、 **[管理者]** を見つけます。  

-   Windows PowerShell の場合: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin`  
  
-   通常のコマンド プロンプトの場合: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
## <a name="troubleshooting"></a>トラブルシューティング  
  
### <a name="troubleshooting-installation-of-the-feature"></a>機能のインストールに関するトラブルシューティング  
  
||Error|エラーの説明|現象|潜在的な問題|  
|-|---------|---------------------|-----------|---------------------|  
|Dism.exe|87|このコンテキストで機能名オプションが認識されない||-   これは、機能名のスペルに誤りがある場合に発生することがあります。 スペルが正しいことを確認して、もう一度試してください。<br />-   使用しているオペレーティング システムのバージョンでこの機能が使用できることを確認します。 Windows PowerShell で、**dism /online /get-features &#124; ?{$_ -match "boot"}** を実行します。 一致するものが返されない場合は、この機能をサポートしていないバージョンを実行している可能性があります。|  
|Dism.exe|0x800f080c|機能 \<name > は不明です。||上記と同じ|  
  
### <a name="troubleshooting-the-collector"></a>コレクターのトラブルシューティング  
  
**ログ**  
コレクターは、自身のイベントを ETW provider Microsoft-Windows-BootEvent-Collector として記録します。 それは、コレクターに関する問題のトラブルシューティングに関して参照する最初の場所です。 [アプリケーションとサービス ログ] > [Microsoft] > [Windows] > [BootEvent-Collector] > [管理者] の [イベント ビューアー] で参照できます。または、次のいずれかのコマンドを使用してコマンド ウィンドウで参照することもできます。  
  
通常のコマンド プロンプトの場合: **wevtutil qe Microsoft-Windows-BootEvent-Collector/Admin**  
  
Windows PowerShell プロンプトの場合: `Get-WinEvent -LogName Microsoft-Windows-BootEvent-Collector/Admin` (`-Oldest` を追加して、古いイベントが先頭になるように時間順に一覧を返すことができます)。  
  
ログの詳細レベルを "error (エラー)" から "warning (警告)"、"info (情報)" (既定値)、"verbose (詳細)"、および "debug (デバッグ)" まで調整することができます。 "info (情報)" より詳細なレベルは、接続していない対象のコンピューターの問題を診断するのに便利ですが、大量のデータが生成される可能性があるため、慎重に使用してください。  
  
最小ログレベルは、構成ファイルの @no__t 0collector > 要素で設定します。 例: < collector configVersionMajor = "1" minlog @ no__t-0 "verbose" >。  
  
詳細レベルでは、受信した各パケットが処理されるときにログに記録されます。 デバッグ レベルでは、処理の詳細がさらに追加され、受信されたすべての ETW パケットのコンテンツもダンプされます。  
  
デバッグ レベルでは、通常のログ システムで表示するのではなく、ファイルにログを書き込むと便利です。 これを行うには、構成ファイルの @no__t 0collector > 要素に要素を追加します。  
  
< collector configVersionMajor = "1" minlog = "debug" log @ no__t-0 "c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt" >  
      
 **コレクターのトラブルシューティングを行うための推奨される方法は次のとおりです。**  
   
1. まず、次のコマンドで、コレクターがターゲットからの接続を受信したことを確認します (ターゲットがメッセージの送信を開始したときにのみファイルが作成されます)。   
   ```  
   Get-SbecForwarding  
   ```  
   このターゲットからの接続があることが返された場合、自動ロガーの設定に問題がある可能性があります。 何も返されなかった場合は、開始する KDNET 接続に問題があります。 KDNET 接続の問題を診断するためには、両方の側 (つまり、コレクターとターゲットの両方) からの接続を確認してみてください  
  
2. コレクターから拡張診断を表示するには、構成ファイルの @no__t 0collector > 要素にこれを追加します。  
   \<collector...minlog = "verbose" >  
   これにより、各受信パケットに関するメッセージが有効になります。  
3. パケットが受信されているかどうかを確認します。 必要に応じて、ETW 経由ではなく、詳細モードでログを直接ファイルに書き込むこともできます。 これを行うには、次のように構成ファイルの @no__t 0collector > 要素に追加します。  
   \<collector...minlog = "verbose" log = "c:\ProgramData\Microsoft\BootEventCollector\Logs\log.txt" >  
      
4. イベント ログで受信パケットに関するメッセージを確認します。 パケットが受信されているかどうかを確認します。 パケットが受信されているが、正しくない場合は、イベントのメッセージで詳細を確認します。  
5. ターゲット側からは、KDNET は、いくつかの診断情報をレジストリに書き込みます。 検索対象   
   **HKLM\SYSTEM\CurrentControlSet\Services\kdnet** でメッセージがないか確認します。  
   KdInitStatus (DWORD) は、成功した場合は 0、エラー時にエラー コードを表示します  
   KdInitErrorString はエラーの説明です (エラーがない場合は、情報メッセージも含まれます)  
  
6. ターゲットで Ipconfig.exe を実行し、報告されるデバイス名を確認します。 KDNET が適切に読み込まれると、デバイス名は元のベンダーのカード名ではなく、"kdnic" のような名前になります。  
7. ターゲットの DHCP が構成されているかどうかを確認してください。 KDNET には DHCP が不可欠です。  
8. コレクターがターゲットと同じネットワークにあることを確認します。 そうでない場合は、ルーティングが正しく構成されているかどうか (特に DHCP の既定のゲートウェイ設定) を確認します。  
  
  
**接続の状態**  
  
`Get-SbecForwarding` を使用して、確立された接続の現在の一覧と、転送されているデータの場所に関する情報を確認できます。  
  
また、`Get-SbecHistory` を使用して接続の状態変更の最新の履歴を取得することもできます。  
  
### <a name="troubleshooting-setting-a-new-configuration"></a>新しい構成の設定に関するトラブルシューティング  
Windows PowerShell コマンド `$result = (Get-Content .\newconfig.xml | Set-SbecActiveConfig); $result` を使用して構成を適用した場合は、変数 `$result` には展開に関する情報が含まれます。 この変数を照会して、そこからさまざまな情報を取得することができます。  
  
`$result.ErrorString` を使用してエラーに関する情報を取得します。 ここでエラーが報告された場合は、新しい構成が適用されず、古い構成は変更されません。  
  
`$result.WarningString` を使用して警告を取得します  
  
`$result.InfoString` を使用して構成の詳細に関する情報を取得します。  
  
`$result | fl *` を使用して完全な結果を取得できます。  
または、結果を変数に保存しない場合は、`Get-Content .\newconfig.xml | Set-SbecActiveConfig | fl *` を使用することができます。  
  
### <a name="troubleshooting-target-computers"></a>対象のコンピューターのトラブルシューティング  
  
||Error|エラーの説明|現象|潜在的な問題|  
|-|---------|---------------------|-----------|---------------------|  
|対象のコンピューター||ターゲットがコレクターに接続されていない||-   対象のコンピューターが構成された後で再起動されませんでした。 対象のコンピューターを再起動します。<br />-   対象のコンピューターに不適切な BCD 設定があります。 「対象のコンピューターの設定の検証」のセクションで設定を確認してください。 必要に応じて修正し、対象のコンピューターを再起動します。<br />-   KDNET/EVENT-NET ドライバーがネットワーク アダプターに接続できなかったか、または正しくないネットワーク アダプターに接続されました。 Windows PowerShell で、`gwmi Win32_NetworkAdapter` を実行し、ServiceName **kdnic** を含むネットワーク アダプターの出力を確認します。 正しくないネットワーク アダプターが選択されている場合は、「ネットワーク アダプターを指定するには」の手順をもう一度実行します。 ネットワーク アダプターがまったく表示されない場合、ドライバーがお使いのネットワーク アダプターをサポートしていない可能性があります。<br>前述の「コレクターのトラブルシューティングに関する推奨されるアプローチ」の特に手順 5 から手順 8 まで**も参照**してください。|  
|コレクター||コレクターがでホストされている VM を移行した後でイベントが表示されなくなった。||コレクター コンピューターの IP アドレスが変更されていないことを確認します。 変更されている場合は、「トランスポート経由での ETW イベントの送信をリモートで有効にするには」を確認してください。|  
|コレクター||ETL ファイルが作成されていない。|`Get-SbecForwarding` の場合は、ターゲットが接続されていてもエラーは発生しませんが、ETL ファイルは作成されません。|対象のコンピューターでおそらくデータがまだ送信されていない可能性があります。ETL ファイルは、データが受信されたときにのみ作成されます。|  
|コレクター||ETL ファイルにイベントが表示されていない。|対象のコンピューターでイベントが送信されましたが、ETL ファイルがメッセージ アナライザーのイベント ビューアーで読み込まれたときに、イベントが存在しませんでした。|-   イベントはまだバッファー内にある可能性があります。 イベントは、完全な 64 KB のバッファーが収集されるか、新しいイベントが発生しないで 約 10 ～ 15 秒のタイムアウトになるまで、ETL ファイルに書き込まれません。 タイムアウトの有効期限が切れるのを待つか、`Save-SbecInstance` を使用してバッファーをフラッシュします。<br />-   イベント マニフェストは、コレクター コンピューター、またはイベント ビューアーまたはメッセージ アナライザーが実行されるコンピューターで利用できません。  この場合、コレクターでイベントを処理することができないか (コレクターのログを確認してください)、またはビューアーでイベントを表示することができない可能性があります。  コレクター コンピューターにすべてのマニフェストをインストールし、対象のコンピューターにインストールする前にコレクター コンピューターに更新プログラムをインストールすることをお勧めします。|
