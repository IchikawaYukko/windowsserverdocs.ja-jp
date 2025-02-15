---
title: nfsadmin
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2658cf610e4328d382b9224f4230d68a022d1cc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373222"
---
# <a name="nfsadmin"></a>nfsadmin

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

使用する **nfsadmin** NFS および NFS クライアントのサーバーを管理します。  
  
## <a name="syntax"></a>構文  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName*`[` @ no__t-7p *Password*`]]` 0l  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` 0r 1*client* 3 all @ no__t-14  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]] {`start 0 stop @ no__t-11  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` 構成*オプション*1  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` creategroup *Name*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` listgroups パラメータ  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` deletegroup *Name*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` addmembers *Name Host*1  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` listmembers  
  
**nfsadmin server** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` deletemembers *Group Host*1  
  
**nfsadmin client** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]] {`start 0 stop @ no__t-11  
  
**nfsadmin client** `[`*computerName*`] [` @ no__t-4u *UserName* `[` @ no__t-7p *Password*`]]` 構成*オプション*1  
  
## <a name="description"></a>説明  
**Nfsadmin** コマンド\-ライン ユーティリティは、Microsoft Services for Network File System を実行しているローカルまたはリモート コンピューターに NFS クライアントや NFS サーバーを管理 \(NFS\)します。 必要な権限がないアカウントでログオンしている場合は、ユーザー名とは、アカウントのパスワードを指定できます。 によって実行されるアクション **nfsadmin** を指定するコマンド引数に依存します。  
  
サービスに加えて\-固有のコマンド引数とオプション、 **nfsadmin** 次を受け入れます。  
  
*コンピューター名*  
管理するリモート コンピューターを指定します。 Windows インターネット ネーム サービスを使用しているコンピューターを指定することができます \(WINS\) 名またはドメイン ネーム システム \(DNS\) 名、またはインターネット プロトコルによって \(IP\) アドレス。  
  
**\-u** *ユーザー名*  
使用する資格情報を持つユーザーのユーザー名を指定します。 フォームでユーザー名にドメイン名を追加する必要があります <em>ドメイン</em> **\\** <em>ユーザー名</em>  
  
**\-p** *パスワード*  
使用して指定されたユーザーのパスワードを指定、 **\-u** オプション。 指定した場合、 **\-u** オプションは、省略、 **\-p** オプション、ユーザーのパスワードを求めるメッセージが表示されます。  
  
#### <a name="administering-server-for-nfs"></a>Nfs サーバーを管理します。  
使用して、 **nfsadmin server** nfs サーバーを管理するコマンドです。 特定のアクションを **nfsadmin server** はコマンドのオプションは、指定した引数に依存します。  
  
**\-l**  
クライアントによって保持されているすべてのロックを一覧表示します。  
  
**\-r** {*client* | **all**}  
保持するロックの解放、 *クライアント* または、 **すべて** すべてのクライアントで指定しました。  
  
**start**  
NFS サーバーサービスを開始します。  
  
**stop**  
サーバーの NFS サービスを停止します。  
  
**config**  
Nfs サーバーの全般設定を指定します。 少なくとも 1 つの使用は、次のオプションを指定する必要があります、 **config** コマンドの引数。  
  
**mapsvr\=** <em>サーバー</em>  
セット *server* nfs サーバーのユーザー名マッピング サーバーとします。 使用する必要がありますが、このオプションは引き続き以前のバージョンと互換性のためにサポートされる、 **sfuadmin** ユーティリティ代わりにします。  
  
**auditlocation\=** {**eventlog** | **ファイル** | **両方** | **none**}  
イベントを監査するかどうかと、イベントの記録場所を指定します。 次の引数のいずれかが必要です。  
  
**ログ**  
だけで、イベント ビューアーのアプリケーション ログに監査イベントが記録されることを指定します。  
  
**file**  
指定されたファイルだけに監査イベントが記録されることを示す **config fname**します。  
  
**both**  
監査対象のイベントで指定されたファイルと同様に、イベント ビューアーのアプリケーション ログで記録されることを示す **config fname**します。  
  
**存在**  
イベントは監査されませんを指定します。  
  
**fname\=** <em>ファイル</em>  
指定されたファイルの設定 *ファイル* 監査ファイルとして。 既定値は %sfudir\\ログ\\nfssvr.log  
  
**fsize\=** \=*サイズ*  
セット *サイズ* メガバイト単位監査ファイルの最大サイズとして。 既定の最大サイズは、7 MB です。  
  
**audit @ no__t**\[ **\+** | **\-** \]**mount** \=0 @no__t-**12**3**5**6**read** 8 @no__t **-20**@no__t **-21 3**4**書き込み**6**8**9**1**2**create** 4**6**7**9**0**delete** @no__t **-42 @no__44**5**7**8**ロック**0**2**3**5**6**all**  
ログに記録するイベントを指定します。 イベントのログ出力を起動するには、プラス記号を入力 \( **\+** \) イベントの名前は、イベントをログ記録を停止する前に、マイナス記号を入力 \( **\-** \) イベント名の前にします。 符号を省略すると、プラス記号が使われます。 使用しないで **すべて** に他の任意のイベントの名前。  
  
**lockperiod\=** <em>(秒)</em>  
NFS サーバーが nfs サーバーへの接続が失われたれて再確立された後、または、NFS サーバー サービスが再起動された後にロックを再要求を待機する秒数を指定します。  
  
Portmapprotocol\={TCP |UDP |TCP\+UDP  
Portmap をサポートするトランスポート プロトコルを指定します。 既定の設定は **TCP\+UDP**します。  
  
mountprotocol\={TCP |UDP |TCP\+UDP}  
トランスポートを指定のプロトコルがサポートをマウントします。 既定の設定は **TCP\+UDP**します。  
  
nfsprotocol\={TCP |UDP |TCP\+UDP}  
指定するトランスポート プロトコルの Network File System \(NFS\) をサポートしています。 既定の設定は **TCP\+UDP**  
  
nlmprotocol\={TCP |UDP |TCP\+UDP}  
ネットワークロックマネージャー \(NLM @ no__t がサポートするトランスポートプロトコルを指定します。 既定の設定は **TCP\+UDP**します。  
  
nsmprotocol\={TCP |UDP |TCP\+UDP}  
指定するトランスポート プロトコルのネットワーク ステータス マネージャー \(NSM\) をサポートしています。 既定の設定は **TCP\+UDP**します。  
  
**enableV3\=** {**はい** | **ない**}  
NFS version 3 プロトコルをサポートするかどうかを指定します。 既定の設定は **はい**します。  
  
**renewauth\=** {**はい** | **ない**}  
クライアント接続がで指定された時間が経過したら再認証される必要があるかどうかを指定 **config renewauthinterval**します。 既定の設定は **ない**します。  
  
**renewauthinterval\=** <em>(秒)</em>  
場合に再認証されるクライアントが強制されるまでの経過秒数を指定 **config renewauth** に設定されている **はい**します。 既定値は、600 秒です。  
  
**dircache\=** <em>サイズ</em>  
ディレクトリ キャッシュのキロバイト単位のサイズを指定します。 として指定した数値 *サイズ* 4 と 128 の 4 の倍数である必要があります。 既定のディレクトリ\-キャッシュ サイズは 128 KB です。  
  
**translationfile**\= @ no__t-2file @ no__t-3  
移動するときに Windows からのファイル名の文字を置換するためのマッピング情報を含むファイルを指定\-を UNIX ベース\-ベースのファイル システムです。 場合 *ファイル* が指定されていない、ファイル名の文字変換が無効にします。 場合の値 **translationfile** が変更されると、変更が有効にするためにサーバーを再起動する必要があります。  
  
**dotfileshidden**\={**はい** | **ない**}  
ピリオドで始まる名前を使用して作成されたファイル \(. \) は、Windows ファイルシステムで非表示とマークされ、その結果、NFS クライアントでは非表示になるかどうかを指定します。 既定の設定は **ない**します。  
  
**casesensitivelookups\=** {**はい** | **ない**}  
ディレクトリ検索が大文字小文字を区別するかどうかを示す \(文字ケースの正確な一致を必要とする\)です。  
  
Windows カーネルのケースを無効にすることも必要\-ケースをサポートするために nfs サーバーの順序で小文字を区別しない\-機密性の高いファイルの名前。 Windows カーネルのケースを無効にすることができます\-を 0 には、次のレジストリ キーをオフにすると小文字を区別しません。  
  
HKLM\\システム\\CurrentControlSet\\コントロール\\セッション マネージャー\\カーネル  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> このセクションでは、Windows Server 2008 R2、Windows Server 2008 および Windows Server 2003 にのみ適用されます。 このセクションでは、Windows Server 2012 R2 または Windows Server 2012 には適用されません。  
  
**ntfscase\=** {**低い** | **上** | **保持**}  
NTFS ファイル システム内のファイル名の文字の大文字と小文字が小文字、大文字、またはディレクトリに格納されている形式で返されるかどうかを指定します。 既定の設定は **保持**します。 場合、この設定は変更できません **casesensitivelookups** に設定されている **はい**します。  
  
**creategroup** *名*  
新しいクライアントグループを作成し、指定した*名前を付け*ます。  
  
**listgroups パラメータ**  
すべてのクライアント グループの名前を表示します。  
  
**deletegroup** *名*  
*名前*で指定したクライアントグループを削除します。  
  
**renamegroup** *OldName NewName*  
*OldName*によって指定されたクライアントグループの名前を*NewName*に変更します。  
  
**addmembers** *Name Host*\[... \]  
*名前*で指定されたクライアントグループに*ホスト*を追加します。  
  
**listmembers** *名*  
*名前*で指定されたクライアントグループ内のホストコンピューターの一覧を表示します。  
  
**deletemembers** *グループのホスト*\[... \]  
*グループ*によって指定されたクライアントグループから、*ホスト*によって指定されたクライアントを削除します。  
  
コマンドオプションまたは引数を指定しない場合、 **nfsadmin server**では、NFS 構成設定の現在のサーバーが表示されます。  
  
#### <a name="administering-client-for-nfs"></a>NFS クライアントを管理します。  
使用して、 **nfsadmin client** NFS クライアントを管理するコマンドです。 特定のアクションを **nfsadmin client** は指定するコマンド引数によって異なります。  
  
**start**  
NFS クライアントサービスを開始します。  
  
**stop**  
NFS サービスのクライアントを停止します。  
  
**config**  
NFS クライアントの全般設定を指定します。 少なくとも 1 つの使用は、次のオプションを指定する必要があります、 **config** コマンドの引数。  
  
**fileaccess\=** <em>モード</em>  
-   ネットワーク ファイル システムに作成されるファイルの既定のアクセス許可モードを指定 \(NFS\) サーバーです。 *モード* 引数は、0 から 7 への 3 桁で構成されます \(包括的な\) ユーザー、グループ、およびその他のユーザーに与える既定のアクセス許可を表す \(それぞれ\)します。 数字は、次のように UNIX @ no__t-0style のアクセス許可に変換されます。0 @ no__t-0none、1 @ no__t-1x、2 @ no__t-2w、3 @ no__t-3wx、4 @ no__t-4r、5 @ no__t-5rx、6 @ no__t-6rw、および 7 @ no__t-7rwx。 たとえば、 **fileaccess\=750** 利用 rwx のアクセス許可の所有者に、rx アクセス許可、グループ、および他のユーザーへのアクセス権限がありません。  
  
**mapsvr\=** <em>サーバー</em>  
セット *server* NFS 用のクライアントのユーザー名マッピング サーバーとします。 使用する必要がありますが、このオプションは引き続き以前のバージョンと互換性のためにサポートされる、 **sfuadmin** ユーティリティ代わりにします。  
  
**mtype\=** {**ハード** | **ソフト**}  
既定のマウントの種類を指定します。 ハード マウントは、NFS クライアントは、成功するまで、失敗した RPC を再試行を続けます。 ソフト マウントの NFS クライアントをエラーを返した呼び出しを再試行した後は、呼び出し元のアプリケーションによって指定された時間数、 **再試行** オプション。  
  
**再試行\=** <em>数</em>  
ソフト マウントの接続を確立しようとする回数を指定します。 この値は、10、包括的に 1 の間でなければなりません。 既定値は 1 です。  
  
**タイムアウト\=** <em>(秒)</em>  
接続を待機する秒数を指定 \(リモート プロシージャ コール\)します。 この値は、0.8、0.9、または 60、包括的には、1 から整数にする必要があります。 既定値は 0.8 です。  
  
**プロトコル @ no__t {TCP |UDP |TCP @ no__t-2UDP}**  
トランスポート プロトコルのサポートを指定します。 既定の設定は **TCP\+UDP**  
  
**rsize\=** <em>サイズ</em>  
読み取りバッファーのサイズを指定します。 この値は、0.5、1、2、4、8、16、または 32 を指定できます。 既定値は 32 です。  
  
**wsize\=** <em>サイズ</em>  
書き込みバッファーのサイズを指定します。 この値は、0.5、1、2、4、8、16、または 32 を指定できます。 既定値は 32 です。  
  
**perf @ no__t-1default**  
既定値に、次のパフォーマンス設定が復元されます。  
  
-   **mtype**  
  
-   **再試行**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>モード</em>  
ネットワーク ファイル システムに作成されるファイルの既定のアクセス許可モードを指定 \(NFS\) サーバーです。 *モード* 引数は、0 から 7 への 3 桁で構成されます \(包括的な\) ユーザー、グループ、およびその他のユーザーに与える既定のアクセス許可を表す \(それぞれ\)します。 数字は、次のように UNIX @ no__t-0style のアクセス許可に変換されます。0 @ no__t-0none、1 @ no__t-1x、2 @ no__t-2w、3 @ no__t-3wx、4 @ no__t-4r、5 @ no__t-5rx、6 @ no__t-6rw、および 7 @ no__t-7rwx。 たとえば、 **fileaccess\=750** 利用 rwx のアクセス許可の所有者に、rx アクセス許可、グループ、および他のユーザーへのアクセス権限がありません。  
  
コマンドオプションまたは引数を指定しない場合、 **nfsadmin client**では、NFS 構成設定の現在のクライアントが表示されます。  
  

