---
title: Sc config
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 26157df1db358dd1a0e0fb48d334dc0e131c5089
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371317"
---
# <a name="sc-config"></a>Sc config



レジストリおよびサービス コントロール マネージャー データベースのサービスのエントリの値を変更します。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---------|-----------|
|\<ServerName >|サービスが配置されているリモート サーバーの名前を指定します。 名前には UNC (汎用名前付け規則) 形式を使用する必要があります (たとえば、\\ @ no__t-1myserver)。 SC.exe をローカルで実行するには、このパラメーターを省略します。|
|\<ServiceName >|によって返されるサービスの名前を指定、 **られて** 操作します。|
|type = {独自の @ no__t-0 share \| kernel \| filesys \| rec \| 適応 \| 相互作用型 = {独自 \| share}} | サービスの種類を指定します。</br>**独自** -独自のプロセスで実行されているサービスを指定します。 実行可能ファイルは他のサービスと共有されません。 これは既定値です。</br>**共有** -共有プロセスとして実行されるサービスを指定します。 その他のサービス実行可能ファイルを共有します。</br>**カーネル** のドライバーを指定します。</br>**filesys** -ファイル システム ドライバーを指定します。</br>**推奨値** -ファイル システムで認められたコンピューターで使用されるファイル システムを識別するドライバーを指定します。</br>**適応** - キーボード、マウスなどのハードウェア デバイスを識別するアダプタのドライバを指定し、ディスク ドライブします。</br>**対話** -ユーザーからの入力を受け取って、デスクトップと対話可能なサービスを指定します。 対話型サービスは、LocalSystem アカウントで実行する必要があります。 この型は、 **type = 独自**または**type = shared**と組み合わせて使用する必要があります (たとえば、 **type = 相互作用** **型 = 独自**)。 使用して **型 = 対話** 自体でエラーが生成されます。|
|開始 = {ブート \| システム \| 自動 \| 要求 \| 無効 \| 遅延-自動}|サービスの開始の種類を指定します。</br>**ブート** のブート ローダーによって読み込まれるデバイス ドライバーを指定します。</br>**システム** のカーネルの初期化中に開始されるデバイス ドライバーを指定します。</br>**自動** - サービスが自動的に各開始ときに、コンピューターの再起動を指定し、コンピューターにログオンできない場合でも実行できます。</br>**必要に応じて** -を手動で起動するサービスを指定します。 これは既定値の場合 **開始 =** が指定されていません。</br>**無効になっている** -サービスを起動できないように指定します。 無効なサービスを開始するには、他のいくつかの値に開始の種類を変更します。</br>**遅延自動** -サービスが自動的に開始短い形式の時刻が、その他の自動サービスの開始後を指定します。|
|error = {normal \| 重大 \| 重大 \| 無視}|サービスは起動時に開始に失敗した場合は、エラーの重大度を指定します。</br>**通常** -サービスの開始に失敗したユーザーに通知エラーをログに記録し、メッセージ ボックスが表示されることを指定します。 起動が続行されます。 これが既定の設定です。</br>**重大な** -エラーが (可能な場合) に記録するように指定します。 コンピューターは、前回正常起動時の構成と再起動を試みます。 コンピューターを再起動することが発生する可能性がサービス可能性がありますもことはできませんを実行します。</br>**重要な** -エラーが (可能な場合) に記録するように指定します。 コンピューターは、前回正常起動時の構成と再起動を試みます。 前回正常起動時の構成が失敗した場合、スタートアップも失敗し、ブート プロセスが Stop エラーで停止します。</br>**無視** -エラーがログに記録し、起動処理は続行するように指定します。 イベント ログにエラーを記録する以外のユーザーに通知は表示されません。|
|binpath = \<Binaryp>|サービスのバイナリ ファイルへのパスを指定します。|
|group = \<LoadOrderGroup >|このサービスがメンバーになっているグループの名前を指定します。 グループの一覧が、レジストリに格納されている、 **HKLM\System\CurrentControlSet\Control\ServiceGroupOrder** サブキー。 既定値は null です。|
|tag = {yes \| no}|CreateService 呼び出し、TagID で取得するかどうかを指定します。 タグは、ブート開始またはシステム開始ドライバーに対してだけ使用されます。|
|依存関係 = @no__t 0dependencies >|サービスまたはこのサービスの前に、まずグループの名前を指定します。 名前は、スラッシュ (/) で区切られます。|
|obj = {\<AccountName > \| \<ObjectName >}|サービスが実行されますが、またはドライバーを実行する Windows ドライバー オブジェクトの名前を指定するアカウントの名を指定します。 既定の設定は **LocalSystem**します。|
|displayname = @no__t 0DisplayName >|ユーザー インターフェイスのプログラムでサービスを識別するためのわかりやすい名前を指定します。 たとえば、1 つの特定のサービスのサブキーの名前は **wuauserv**, 、自動更新のわかりやすい表示名を持ちます。|
|password = \<Password >|パスワードを指定します。 ローカル システム アカウント以外のアカウントを使用する場合に必要です。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>コメント

-   各コマンド ライン オプション (パラメーター) には、等号 (=) は、オプション名の一部です。
-   スペースは、オプションとその値の間で必要な (たとえば、 **型 = 独自**します。 スペースを省略すると、操作は失敗します。

## <a name="BKMK_examples"></a>例

NEWSERVICE サービスのバイナリ パスを指定するには、次のように入力します。
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)