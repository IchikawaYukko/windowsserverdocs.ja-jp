---
title: ユーザー アカウント制御のしくみ
description: Windows Server のセキュリティ
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-tpm
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da83ddb2-6182-417c-aa8e-0b47b2e17d13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: f12a690c06a37a6e1c01674bcdb96d0a9010a245
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403337"
---
# <a name="how-user-account-control-works"></a>ユーザー アカウント制御のしくみ

>適用先:Windows Server (半期チャネル)、Windows Server 2016

ユーザーアカウント制御 (UAC) を使用すると、悪意のあるプログラム (マルウェアとも呼ばれます) によってコンピューターが破損するのを防ぐことができ、組織はより適切に管理されたデスクトップを展開できます。 UAC では、管理者がシステムへの管理者レベルのアクセスを明示的に承認しない限り、アプリケーションとタスクは常に管理者以外のアカウントのセキュリティコンテキストで実行されます。 UAC は、未承認のアプリケーションの自動インストールをブロックし、システム設定が不注意に変更されるのを防ぎます。

## <a name="uac-process-and-interactions"></a>UAC のプロセスと対話
管理者アクセストークンを必要とする各アプリケーションは、管理者に同意を求める必要があります。 1 つの例外は、親プロセスと子プロセスの間に存在する関係です。 子プロセスは、親プロセスからユーザーアクセストークンを継承します。 ただし、親と子の両方のプロセスは、同じ整合性レベルを持つ必要があります。 Windows Server 2012 では、整合性レベルをマークすることでプロセスを保護します。 整合性レベルは、信頼性の測定値です。 "高" 整合性アプリケーションはディスク パーティション アプリケーションなどのシステム データを変更するタスクを実行するアプリケーションで、"低" 整合性アプリケーションは Web ブラウザーなどのオペレーティング システムを危険にさらす可能性があるタスクを実行するアプリケーションです。 整合性レベルが低いアプリケーションでは、より高い整合性レベルを持つアプリケーションのデータを変更できません。 標準ユーザーが管理者アクセストークンを必要とするアプリケーションを実行しようとすると、UAC は、ユーザーが有効な管理者資格情報を入力することを要求します。

このプロセスがどのように行われるかを理解するために、Windows Server 2012 のログオンプロセスの詳細を確認することが重要です。

### <a name="windows-server-2012-logon-process"></a>Windows Server 2012 ログオンプロセス
次の図は、管理者のログオンプロセスが標準ユーザーのログオンプロセスとどのように異なるかを示しています。

![管理者のログオンプロセスが標準ユーザーのログオンプロセスとどのように異なるかを示す図](../media/How-User-Account-Control-Works/UACWindowsLogonProcess.gif)

既定では、標準ユーザーと管理者はリソースにアクセスし、標準ユーザーのセキュリティコンテキストでアプリケーションを実行します。 ユーザーがコンピューターにログオンすると、そのユーザーのアクセス トークンがシステムにより作成されます。 アクセス トークンには、固有のセキュリティ識別子 (SID) や Windows 特権などの、ユーザーが許可されるアクセスのレベルについての情報が含まれています。

管理者がログオンするときは、2 つの異なるアクセス トークンがユーザー用に作成されます。標準ユーザー アクセス トークンと、管理者アクセス トークンです。 標準ユーザー アクセス トークンには管理者アクセス トークンと同じユーザー固有の情報が含まれていますが、管理用の Windows 特権と SID は削除されます。 標準ユーザーアクセストークンは、管理タスク (標準ユーザーアプリケーション) を実行しないアプリケーションを開始するために使用されます。 次に、標準ユーザーアクセストークンを使用してデスクトップ (explorer.exe) が表示されます。 explorer.exe は、他のすべてのユーザー側から開始されるプロセスのアクセス トークンの継承元となる親プロセスです。 その結果、アプリケーションが完全な管理者アクセストークンを使用することを承認するための同意または資格情報をユーザーが入力しない限り、すべてのアプリケーションが標準ユーザーとして実行されます。

Administrators グループのメンバーであるユーザーは、標準ユーザー アクセス トークンを使って、ログオン、Web の閲覧、電子メールの読み取りを行うことができます。 管理者が管理者アクセストークンを必要とするタスクを実行する必要がある場合、Windows Server 2012 は自動的にユーザーに承認を求めるメッセージを表示します。 これは昇格時のプロンプトと呼ばれ、ローカル セキュリティ ポリシー スナップイン (Secpol.msc) またはグループ ポリシーを使って動作を構成することができます。

> [!NOTE]
> "昇格" という用語は、完全な管理者アクセストークンを使用することをユーザーに確認するために、Windows Server 2012 でプロセスを参照するために使用されます。

### <a name="the-uac-user-experience"></a>UAC ユーザー エクスペリエンス
UAC を有効にしている場合、標準ユーザーのユーザー エクスペリエンスと管理者承認モードでの管理者のユーザー エクスペリエンスは異なります。 Windows Server 2012 を実行する場合、推奨されるセキュリティ保護方法として、プライマリユーザーアカウントを標準ユーザーアカウントに設定することをお勧めします。 標準ユーザーとして実行することは、管理環境のセキュリティを最大化するために役立ちます。 組み込みの UAC 昇格コンポーネントにより、標準ユーザーはローカル管理者アカウントの有効な資格情報を入力することで、簡単に管理タスクを実行できます。 既定では、標準ユーザーの組み込みの UAC 昇格コンポーネントは、資格情報プロンプトです。

標準ユーザーとして実行するための別の方法は、管理者承認モードで管理者として実行することです。 組み込みの UAC 昇格コンポーネントにより、ローカルの Administrators グループのメンバーは承認を提供することで管理タスクを簡単に実行できます。 既定では、管理者承認モードでの管理者アカウント用の組み込みの UAC 昇格コンポーネントは、同意のプロンプトと呼ばれます。 UAC の昇格時のプロンプト動作を構成するには、ローカルセキュリティポリシースナップイン (Secpol.msc) またはグループポリシーを使用します。

**同意および資格情報のプロンプト**

UAC が有効になっていると、Windows Server 2012 は、完全な管理者アクセストークンを必要とするプログラムまたはタスクを開始する前に、有効なローカル管理者アカウントの資格情報の入力を求めます。またはプロンプトが表示されます。 このプロンプトにより、悪意のあるソフトウェアが警告なしにインストールされないようになります。

**同意プロンプト**

ユーザーがユーザーの管理アクセス トークンを必要とするタスクを実行しようとした場合、同意のプロンプトが表示されます。 UAC の同意プロンプトのスクリーンショットを次に示します。

![UAC の同意プロンプトのスクリーンショット](../media/How-User-Account-Control-Works/UACConsentPrompt.gif)

**資格情報のプロンプト**

標準ユーザーがユーザーの管理アクセストークンを必要とするタスクを実行しようとすると、資格情報プロンプトが表示されます。 この標準ユーザーの既定のプロンプト動作は、ローカルセキュリティポリシースナップイン (Secpol.msc) またはグループポリシーを使用して構成できます。 管理者は、ユーザーアカウント制御を設定することによって、資格情報を指定することも必要になります。[管理者承認モード] ポリシー設定の管理者が資格情報の入力を要求する場合の昇格時のプロンプトの動作。

次のスクリーンショットは、UAC 資格情報プロンプトの例です。

![UAC 資格情報のプロンプトの例を示すスクリーンショット](../media/How-User-Account-Control-Works/UACCredentialPrompt.gif)

**UAC の昇格時のプロンプト**

UAC の昇格時のプロンプトは、アプリケーション固有のものになるように色分けされています。これにより、アプリケーションの潜在的なセキュリティリスクをすぐに識別できるようになります。 管理者のフルアクセストークンを使用してアプリケーションを実行しようとすると、Windows Server 2012 はまず、実行可能ファイルを分析して発行元を特定します。 アプリケーションは、まず、実行可能ファイルの発行元に基づいて次の3つのカテゴリに分けられます。Windows Server 2012、発行元の検証済み (署名済み)、および発行元が確認されていません (署名なし)。 次の図は、Windows Server 2012 がユーザーに提示する色の昇格を確認する方法を示しています。

昇格のプロンプトの色分けは、次のように行われます。

-   赤の盾アイコン付きの赤の背景:アプリケーションがグループポリシーによってブロックされているか、またはブロックされているパブリッシャーからのものです。

-   青の背景と金色の盾アイコン:このアプリケーションは、Windows Server 2012 管理アプリケーション (コントロールパネル項目など) です。

-   青い盾アイコンが付いた青の背景:アプリケーションは Authenticode を使用して署名され、ローカルコンピューターによって信頼されています。

-   黄色い盾のアイコンが付いた黄色の背景:アプリケーションは署名されていないか署名されていますが、まだローカルコンピューターによって信頼されていません。

**シールドアイコン**

**[日付と時刻のプロパティ]** などの一部のコントロール パネル項目には、管理者と標準ユーザーの操作の組み合わせが含まれています。 標準ユーザーは時計を表示しタイム ゾーンを変更することができますが、ローカル システム時刻を変更するには完全な管理者アクセス トークンが必要です。 **[日付と時刻のプロパティ]** コントロール パネル項目のスクリーン ショットを次に示します。

![\* * [日付と時刻のプロパティ] * * [コントロールパネル] 項目を示すスクリーンショット](../media/How-User-Account-Control-Works/UACShieldIcon.gif)

**[日付と時刻の変更]** ボタンの盾アイコンは、プロセスに完全な管理者アクセストークンが必要であることを示し、UAC の昇格時のプロンプトが表示されます。

**昇格時のプロンプトをセキュリティで保護する**

昇格プロセスは、プロンプトをセキュリティで保護されたデスクトップに誘導することでより保護されます。 同意および資格情報のプロンプトは、Windows Server 2012 のセキュリティで保護されたデスクトップに既定で表示されます。 Windows プロセスのみがセキュリティで保護されたデスクトップにアクセスできます。 高いレベルのセキュリティを確保するには、@no__t 0User アカウント制御を維持することをお勧めします。昇格 @ no__t-0 ポリシー設定が有効になっている場合は、セキュリティで保護されたデスクトップに切り替えます。

実行可能ファイルが昇格を要求するときは、ユーザー デスクトップとも呼ばれる対話機能を持つデスクトップがセキュリティで保護されたデスクトップに切り替わります。 セキュリティで保護されたデスクトップではユーザー デスクトップが暗くなり、続行する前に応答する必要がある昇格時のプロンプトが表示されます。 ユーザーが [はい] または [いいえ] をクリックすると、デスクトップがユーザーのデスクトップに切り替わります。

マルウェアは、セキュリティで保護されたデスクトップの偽を提供できますが、ユーザーアカウント制御は次のような場合に発生します。[管理者承認モードの管理者に対する昇格時のプロンプト] ポリシー設定が [同意を求める] に設定されている場合、ユーザーが偽で [はい] をクリックしても、マルウェアの昇格は行われません。 ポリシー設定が資格情報の入力を求められるように設定されている場合、マルウェア偽は、資格情報のプロンプトでユーザーから資格情報を収集できます。 ただし、マルウェアが管理者特権を持つことはなく、収集されたパスワードが使われた場合でも、システムにはマルウェアによるユーザー インターフェイスの制御を軽減する別の保護があります。

マルウェアによってセキュリティで保護されたデスクトップの偽が発生しても、ユーザーがコンピューターにマルウェアを以前にインストールしていないと、この問題は発生しません。 UAC が有効になっている場合、管理者アクセス トークンを必要とするプロセスを警告なしにインストールすることはできないため、ユーザーは **[はい]** をクリックするか、管理者の資格情報を提供して明示的に同意する必要があります。 UAC の昇格時のプロンプトの特定の動作は、グループ ポリシーに依存します。

## <a name="uac-architecture"></a>UAC のアーキテクチャ
次の図は、UAC のアーキテクチャの詳細を示しています。

![UAC アーキテクチャの詳細を示す図](../media/How-User-Account-Control-Works/UACArchitecture.gif)

各コンポーネントの理解を深めるには、次の表を確認してください。

|コンポーネント|説明|
|-------|--------|
|**User**||
|ユーザーが特権を必要とする操作を行う|操作によりファイル システムやレジストリが変更される場合は、仮想化が呼び出されます。 その他のすべての操作では ShellExecute が呼び出されます。|
|ShellExecute|ShellExecute は CreateProcess を呼び出します。 ShellExecute は、CreateProcess から ERROR_ELEVATION_REQUIRED エラーを検索します。 エラーを受け取った場合、ShellExecute はアプリケーション情報サービスを呼び出し、管理者特権のプロンプトで要求されたタスクの実行を試みます。|
|CreateProcess|アプリケーションが昇格を必要とする場合、CreateProcess は ERROR_ELEVATION_REQUIRED を使用して呼び出しを拒否します。|
|**システム**||
|アプリケーション情報サービス|ローカル管理タスクや高い整合性レベルを必要とするアプリケーションなど、1つ以上の昇格された特権を必要とするアプリケーションや、実行するユーザー権限を必要とするアプリケーションを起動するのに役立つシステムサービス。 アプリケーション情報サービスは、昇格が必要なときに管理ユーザーの完全なアクセストークンを使用してアプリケーションの新しいプロセスを作成することによって、このようなアプリケーションを開始するのに役立ちます。また、ユーザーによって同意が付与されます (グループポリシーによって異なります)。|
|ActiveX インストールの昇格|ActiveX がインストールされていない場合、システムは UAC スライダー レベルをチェックします。 ActiveX がインストールされている場合、@no__t 0User アカウント制御:[昇格時に @ no__t-0 グループポリシー設定] がオンになっている場合は、セキュリティで保護されたデスクトップに切り替えます。|
|UAC スライダー レベルのチェック|UAC では、次の4つのレベルの通知から選択できます。また、通知レベルを選択するために使用するスライダーもあります。<br /><br /><ul><li>高<br /><br />    スライダーが **[常に通知する]** に設定されている場合、システムはセキュリティで保護されたデスクトップが有効になっているかどうかをチェックします。</li><li>Medium<br /><br />    スライダーが既定値に設定されている場合は、 **[プログラムが自分のコンピューターに変更を加えようとしたときにのみ通知**する] を指定します。 ** ユーザーアカウント制御:署名され、検証済みの @ no__t-0 ポリシー設定が有効になっている実行可能ファイルのみを昇格します:<br /><br /><ul><li>ポリシー設定が有効になっている場合は、実行を許可する前に、特定の実行可能ファイルに対して公開キー基盤 (PKI) 証明書パス検証が適用されます。</li><li>ポリシー設定が有効になっていない場合 (既定)、特定の実行可能ファイルの実行が許可される前に、PKI 証明書パスの検証は強制されません。 @No__t 0User アカウント制御:[昇格時に @ no__t-0 グループポリシー設定] がオンになっている場合は、セキュリティで保護されたデスクトップに切り替えます。</li></ul></li><li>Low<br /><br />    スライダーが **[自分のコンピューターに変更を加えようとしたときのみ通知**する] に設定されている場合 (デスクトップでは暗転しない)、CreateProcess が呼び出されます。</li><li>通知しない<br /><br />    スライダーが **[いつ通知しない]** に設定されている場合は、プログラムがコンピューターのインストールまたは変更を試みたときに、UAC プロンプトが通知されません。 **重要:**   この設定はお勧めできません。 この設定は、@no__t 0User アカウント制御を設定するのと同じです。管理者が確認**せずに昇格**するように、管理者承認モード @ no__t-0 ポリシー設定の管理者に対する昇格時のプロンプトの動作。</li></ul>|
|セキュリティで保護されたデスクトップが有効|@No__t 0User アカウント制御:[昇格時にセキュリティで保護されたデスクトップに切り替える] @ no__t-0 ポリシー設定がオンになっていることを確認します。<br /><br />-セキュリティで保護されたデスクトップが有効になっている場合、すべての昇格要求は、管理者と標準ユーザーのプロンプト動作のポリシー設定に関係なく、セキュリティで保護されたデスクトップに送られます。<br />-セキュリティで保護されたデスクトップが有効になっていない場合は、すべての昇格要求が対話型ユーザーのデスクトップに送られ、管理者と標準ユーザーのユーザーごとの設定が使用されます。|
|CreateProcess|CreateProcess は、アプリケーションが昇格を必要とするかどうかを評価するために、AppCompat、Fusion、およびインストーラーの検出を呼び出します。 実行可能ファイルは、実行可能ファイルのアプリケーションマニフェストに格納されている要求された実行レベルを決定するために検査されます。 マニフェストで指定されている要求された実行レベルがアクセストークンと一致せず、エラー (ERROR_ELEVATION_REQUIRED) が ShellExecute に返されると、CreateProcess は失敗します。 |
|AppCompat|AppCompat データベースには、アプリケーションのアプリケーション互換性修正エントリの情報が保存されます。|
|Fusion|Fusion データベースには、アプリケーションを記述するアプリケーション マニフェストからの情報が保存されます。 マニフェスト スキーマが更新されて、新しい要求実行レベル フィールドが追加されます。|
|インストーラーの検出|インストーラーの検出では、セットアップの実行可能ファイルが検出されます。これにより、ユーザーの知識や同意なしにインストールが実行されるのを防ぐことができます。|
|**カーネル**||
|仮想化|仮想化テクノロジを使用すると、準拠していないアプリケーションに対して、原因を特定できないような方法で実行または失敗しないようにすることができます。 UAC もファイルとレジストリの仮想化と、保護された領域への書き込みを行うアプリケーションのログ記録を提供します。|
|ファイル システムとレジストリ|ユーザーごとのファイルとレジストリの仮想化は、コンピューターごとのレジストリとファイルの書き込み要求を、ユーザーごとの同等の場所にリダイレクトします。 読み取り要求はまず仮想化されたユーザーごとの場所にリダイレクトされ、次にコンピューターごとの場所にリダイレクトされます。|

以前のバージョンの Windows では、Windows Server 2012 UAC が変更されています。 新しいスライダーでは、UAC が完全にオフになることはありません。 新しい設定は次のようになります。

-   UAC サービスが実行され続けます。

-   管理者によって開始されたすべての昇格要求が、UAC プロンプトを表示せずに自動的に承認されるようにします。

-   標準ユーザーのすべての昇格要求を自動的に拒否します。

> [!IMPORTANT]
> UAC を完全に無効にするには、ポリシー @no__t 0 ユーザーアカウント制御を無効にする必要があります。すべての管理者を管理者承認モード @ no__t-0 で実行します。

> [!WARNING]
> UAC を無効にすると、調整されたアプリケーションが Windows Server 2012 で動作しなくなります。

### <a name="virtualization"></a>仮想化
エンタープライズ環境のシステム管理者はシステムをセキュリティで保護しようとするとため、多くの基幹業務 (LOB) アプリケーションは標準ユーザー アクセス トークンのみを使うように設計されています。 このため、UAC が有効になっている Windows Server 2012 を実行している場合、IT 管理者はほとんどのアプリケーションを置き換える必要はありません。

 Windows Server 2012 には、UAC に準拠しておらず、管理者のアクセストークンを正常に実行する必要があるアプリケーションのファイルおよびレジストリの仮想化テクノロジが含まれています。 仮想化により、UAC に準拠していないアプリケーションであっても、Windows Server 2012 と互換性があることが保証されます。 UAC に準拠していない管理アプリケーションが、プログラムファイルなどの保護されたディレクトリへの書き込みを試みた場合、UAC は、変更しようとしているリソースの独自の仮想化されたビューをアプリケーションに提供します。 この仮想化コピーは、ユーザーのプロファイル内に保持されます。 この方法では、非対応アプリケーションを実行するユーザーごとに、仮想化されたファイルのコピーが個別に作成されます。

ほとんどのアプリケーションタスクは、仮想化機能を使用して正常に動作します。 仮想化ではほとんどのアプリケーションを実行できますが、これは短期的な修正であり、長期的なソリューションではありません。 アプリケーション開発者は、ファイル、フォルダー、およびレジストリの仮想化に依存するのではなく、できるだけ早く Windows Server 2012 ロゴプログラムに準拠するようにアプリケーションを変更する必要があります。

次のシナリオでは、仮想化はオプションではありません。

1.  仮想化は、完全な管理アクセストークンを使用して昇格および実行されるアプリケーションには適用されません。

2.  仮想化では、32ビットアプリケーションのみがサポートされます。 昇格されていない64ビットアプリケーションは、Windows オブジェクトへのハンドル (一意の識別子) を取得しようとすると、アクセス拒否メッセージを受信するだけです。 ネイティブ Windows 64 ビットアプリケーションは、UAC と互換性があり、正しい場所にデータを書き込むために必要です。

3.  アプリケーションに、要求された実行レベルの属性を持つアプリケーションマニフェストが含まれている場合、アプリケーションの仮想化は無効になります。

### <a name="request-execution-levels"></a>要求の実行レベル
アプリケーションマニフェストは、アプリケーションが実行時にバインドする必要がある共有 side-by-side アセンブリとプライベート side-by-side アセンブリを記述および識別する XML ファイルです。 Windows Server 2012 では、アプリケーションマニフェストに、UAC アプリケーションの互換性のためのエントリが含まれています。 アプリケーションマニフェストにエントリを含む管理アプリケーションは、ユーザーのアクセストークンへのアクセス許可をユーザーに要求します。 アプリケーションマニフェストにはエントリがありませんが、ほとんどの管理アプリケーションは、アプリケーション互換性修正プログラムを使用して変更せずに実行できます。 アプリケーション互換性修正プログラムは、UAC に準拠していないアプリケーションが Windows Server 2012 で適切に動作できるようにするデータベースエントリです。

すべての UAC 準拠アプリケーションは、アプリケーションマニフェストに追加された要求実行レベルを持っている必要があります。 アプリケーションがシステムへの管理アクセスを必要とする場合は、要求された実行レベルの "管理者の要求" でアプリケーションをマークすると、システムがこのプログラムを管理アプリケーションとして識別し、必要な昇格手順。 要求された実行レベルは、アプリケーションに必要な特権を指定します。

### <a name="installer-detection-technology"></a>インストーラー検出テクノロジ
インストールプログラムは、ソフトウェアを展開するように設計されたアプリケーションです。 ほとんどのインストール プログラムは、システム ディレクトリとレジストリ キーに書き込みます。 これらの保護されたシステムの場所は、通常、インストーラー検出テクノロジで管理者だけが書き込むことができます。これは、標準ユーザーにはプログラムをインストールするための十分なアクセス権がないことを意味します。 Windows Server 2012 ヒューリスティックは、インストールプログラムを検出し、管理者の資格情報または承認を管理者ユーザーに要求して、アクセス権で実行します。 また、Windows Server 2012 では、アプリケーションをアンインストールする更新プログラムとプログラムがヒューリスティックによって検出されます。 UAC の設計上の目標の 1 つは、インストール プログラムがファイル システムとレジストリの保護されている領域に書き込むことで、ユーザーの知らない間に、またはユーザーの同意なくインストールが実行されないようにすることです。

インストーラー検出は次の対象にのみ適用されます。

-   32 ビット実行可能ファイル。

-   要求実行レベル属性を持たないアプリケーション。

-   UAC が有効になった標準ユーザーとして実行されている対話型プロセス。

32 ビット プロセスが作成される前に、インストーラーであるかどうかを判断するため、次の属性が確認されます。

-   ファイル名に "install"、"setup"、"update" などのキーワードが含まれているかどうか。

-   バージョン管理リソースフィールドには、次のキーワードが含まれています。[ベンダー]、[会社名]、[製品名]、[ファイルの説明]、[元のファイル名]、[内部名]、[エクスポート名] を指定します。

-   サイド バイ サイド マニフェスト内のキーワードが実行可能ファイルに埋め込まれているかどうか。

-   特定の StringTable エントリのキーワードが実行可能ファイルでリンクされているかどうか。

-   リソース スクリプト データのキー属性が実行可能ファイルでリンクされているかどうか。

-   実行可能ファイル内に対象となるバイトのシーケンスがあるかどうか。

> [!NOTE]
> キーワードとバイトのシーケンスは、さまざまなインストーラー テクノロジから確認された共通の特徴から取得されました。

> [!NOTE]
> ユーザーアカウント制御:インストールプログラムを検出するには、[アプリケーションのインストールを検出し、昇格を要求する] ポリシー設定をインストーラーで検出できるようにする必要があります。 この設定は既定で有効になっており、ローカルセキュリティポリシースナップイン (Secpol.msc) を使用してローカルで構成することも、ドメイン、OU、またはグループポリシー (Gpedit.msc) で特定のグループ用に構成することもできます。


