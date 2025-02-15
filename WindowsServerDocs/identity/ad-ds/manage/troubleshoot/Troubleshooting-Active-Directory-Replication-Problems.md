---
ms.assetid: b11f7a65-ec7b-4c11-8dc4-d7cabb54cd94
title: Active Directory レプリケーションの問題のトラブルシューティングに関するページ
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cf6b50ab3b4991bd8cab8523494261f1284945a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71409060"
---
# <a name="troubleshooting-active-directory-replication-problems"></a>Active Directory レプリケーションの問題のトラブルシューティングに関するページ

>適用先:Windows Server 2016 では、Windows Server 2012 R2、Windows Server 2012

Active Directory レプリケーションの問題には、いくつかの異なるソースが存在する場合があります。 たとえば、ドメインネームシステム (DNS) の問題、ネットワークの問題、またはセキュリティの問題によっては、Active Directory レプリケーションが失敗する可能性があります。 

このトピックの残りの部分では、Active Directory レプリケーションエラーを修正するためのツールと一般的な方法について説明します。 次のサブトピックでは、現象、原因、および特定のレプリケーションエラーの解決方法について説明します。

## <a name="introduction-and-resources-for-troubleshooting-active-directory-replication"></a>Active Directory レプリケーションのトラブルシューティングの概要とリソース

入力方向または出力方向のレプリケーションが失敗すると、レプリケーショントポロジ、レプリケーションスケジュール、ドメインコントローラー、ユーザー、コンピューター、パスワード、セキュリティグループ、グループメンバーシップ、およびグループポリシーを表す Active Directory オブジェクトが不整合になります。ドメインコントローラー間。 ディレクトリの不整合とレプリケーションエラーによって、操作のために接続されたドメインコントローラに応じて、操作エラーまたは一貫性のない結果が発生する可能性があります。また、グループポリシーとアクセス制御のアクセス許可を適用できなくなる可能性があります。 Active Directory Domain Services (AD DS) は、ネットワーク接続、名前解決、認証と承認、ディレクトリデータベース、レプリケーショントポロジ、およびレプリケーションエンジンに依存します。 レプリケーションの問題の根本原因がすぐにわからない場合は、考えられる多くの原因の原因を特定するために、考えられる原因を体系的に排除する必要があります。

レプリケーションの監視とエラーの診断に役立つ UI ベースのツールについては、「 [Active Directory Replication Status ツール](https://www.microsoft.com/download/details.aspx?id=30005)」を参照してください。

Repadmin ツールを使用して Active Directory レプリケーションが利用可能であることをトラブルシューティングする方法について説明する包括的なドキュメントについては、「」を参照してください。「 [Repadmin を使用した Active Directory レプリケーションの監視とトラブルシューティング](https://go.microsoft.com/fwlink/?LinkId=122830)」を参照してください。

Active Directory レプリケーションのしくみの詳細については、次のテクニカルリファレンスを参照してください。

- [Active Directory レプリケーションモデルのテクニカルリファレンス](https://go.microsoft.com/fwlink/?LinkId=65958)
- [Active Director レプリケーショントポロジのテクニカルリファレンス](https://go.microsoft.com/fwlink/?LinkId=93578)

## <a name="event-and-tool-solution-recommendations"></a>イベントおよびツールソリューションに関する推奨事項

理想的には、ディレクトリサービスイベントログの赤 (エラー) イベントと黄 (警告) イベントが、ソースまたは宛先のドメインコントローラーでレプリケーションが失敗する原因となっている特定の制約を提案します。 イベントメッセージで解決策の手順が提案されている場合は、イベントで説明されている手順を試してください。 Repadmin ツールおよびその他の診断ツールでは、レプリケーションの失敗を解決するのに役立つ情報も提供されます。 

Repadmin を使用したレプリケーションの問題のトラブルシューティングの詳細については、「 [Repadmin を使用した Active Directory レプリケーションの監視とトラブルシューティング](https://go.microsoft.com/fwlink/?LinkId=122830)」を参照してください。

## <a name="ruling-out-intentional-disruptions-or-hardware-failures"></a>意図的な中断またはハードウェア障害の除外

意図的な中断が原因で、レプリケーションエラーが発生することがあります。 たとえば、Active Directory レプリケーションの問題のトラブルシューティングを行う場合は、意図的な切断とハードウェアの障害またはアップグレードをまず除外します。

### <a name="intentional-disconnections"></a>意図的に切断

ステージングサイトに構築され、現在オフラインになっているドメインコントローラーでレプリケーションエラーが報告された場合に、最終的な運用サイト (ブランチオフィスなどのリモートサイト) でその展開を待機している。) を使用して、これらのレプリケーションエラーを考慮することができます。 ドメインコントローラーが、ドメインコントローラーが再接続されるまで連続エラーが発生するように、レプリケーショントポロジからドメインコントローラーを分離しないようにするには、最初にこのようなコンピューターをメンバーサーバーとして追加し、[メディアからのインストール] を使用することを検討してください (IFM) Active Directory Domain Services (AD DS) をインストールする方法。 Ntdsutil コマンドラインツールを使用して、リムーバブルメディア (CD、DVD、またはその他のメディア) に保存し、転送先サイトに発送できるインストールメディアを作成できます。 その後、インストールメディアを使用して、サイトのドメインコントローラーに AD DS をインストールできます。レプリケーションを使用する必要はありません。 

### <a name="hardware-failures-or-upgradestitle"></a>ハードウェアの障害またはアップグレード @ no__t-0

ハードウェア障害 (マザーボード、ディスクサブシステム、ハードドライブの障害など) が原因でレプリケーションの問題が発生した場合は、ハードウェアの問題を解決できるように、サーバーの所有者に通知します。

定期的なハードウェアのアップグレードによって、ドメインコントローラーがサービスを停止する場合もあります。 サーバーの所有者が、このような障害を事前に通信するための十分なシステムを備えていることを確認します。

### <a name="firewall-configuration"></a>ファイアウォールの構成

既定では、Active Directory レプリケーションのリモートプロシージャコール (Rpc) は、ポート135で RPC エンドポイントマッパー (RPCSS) を介して使用可能なポートを介して動的に実行されます。 セキュリティが強化された Windows ファイアウォールとその他のファイアウォールが、レプリケーションを許可するように正しく構成されていることを確認します。 Active Directory レプリケーションとポート設定のポートを指定する方法の詳細について[は、Microsoft サポート技術情報の記事 224196](https://go.microsoft.com/fwlink/?LinkId=22578)を参照してください。 

レプリケーション Active Directory で使用するポートの詳細については、「 [Active Directory レプリケーションツールと設定](https://go.microsoft.com/fwlink/?LinkId=123774)」を参照してください。

ファイアウォール経由で Active Directory レプリケーションを管理する方法の詳細については、「[ファイアウォール経由のレプリケーションの Active Directory](https://go.microsoft.com/fwlink/?LinkId=123775)」を参照してください。

## <a name="responding-to-failure-of-an-outdated-server-running-windows-2000-server"></a>Windows 2000 Server を実行している古いサーバーの障害への対応

Windows 2000 Server を実行しているドメインコントローラが廃棄 (tombstone) の有効期間の日数を超えて失敗した場合、ソリューションは常に同じになります。 

1. 企業ネットワークからプライベートネットワークにサーバーを移動します。
2. 強制的に Active Directory を削除するか、オペレーティングシステムを再インストールしてください。
3. サーバーオブジェクトを復元できないように、Active Directory からサーバーメタデータを削除します。 

スクリプトを使用すると、ほとんどの Windows オペレーティングシステムでサーバーメタデータをクリーンアップできます。 このスクリプトの使用方法の詳細については、「 [Remove Active Directory ドメイン Controller Metadata](https://go.microsoft.com/fwlink/?LinkID=123599)」を参照してください。 

既定では、削除される NTDS 設定オブジェクトは、14日間自動的に復元されます。 したがって、サーバーのメタデータを削除しない (Ntdsutil を使用するか、前に説明したスクリプトを使用してメタデータのクリーンアップを実行する) と、ディレクトリ内でサーバーメタデータが復元されます。これにより、レプリケーションの試行が求められます。 この場合、不足しているドメインコントローラーではレプリケートできないため、エラーは永続的にログに記録されます。

## <a name="root-causes"></a>根本原因

意図的な切断、ハードウェア障害、および古い Windows 2000 ドメインコントローラーを除外する場合、レプリケーションの問題の残りの部分には、ほとんどの場合、次のいずれかの根本原因があります。

- ネットワーク接続:ネットワーク接続が使用できないか、ネットワーク設定が正しく構成されていない可能性があります。
- 名前解決:レプリケーションエラーの一般的な原因としては、DNS の誤った誤りが考えられます。
- 認証と承認:ドメインコントローラーがレプリケーションパートナーに接続しようとすると、認証と承認の問題によって "アクセスが拒否されました" というエラーが発生します。
- ディレクトリデータベース (ストア):ディレクトリデータベースは、レプリケーションのタイムアウトに対応するのに十分な速度でトランザクションを処理できない可能性があります。
- レプリケーションエンジン:サイト間レプリケーションのスケジュールが短すぎると、レプリケーションキューが大きすぎて、送信レプリケーションスケジュールで必要な時間内に処理できなくなる可能性があります。 この場合、一部の変更のレプリケーションは、廃棄 (tombstone) の有効期間を超えても、無期限に停止する可能性があります。
- レプリケーショントポロジ:ドメインコントローラーには、実際のワイドエリアネットワーク (WAN) 接続または仮想プライベートネットワーク (VPN) 接続にマップされた AD DS のサイト間リンクが必要です。 ネットワークの実際のサイトトポロジでサポートされていないレプリケーショントポロジ用に AD DS でオブジェクトを作成した場合、正しく構成されていないトポロジを必要とするレプリケーションは失敗します。

## <a name="general-approach-to-fixing-problems"></a>問題を解決するための一般的な方法

レプリケーションの問題を解決するには、次の一般的な方法を使用します。 

1. レプリケーションの正常性を毎日監視するか、Repadmin.exe を使用してレプリケーションの状態を毎日取得します。
2. 「イベントメッセージとこのガイド」で説明されている方法を使用して、報告されたエラーを適切なタイミングで解決しようとします。 ソフトウェアが問題の原因になっている可能性がある場合は、他のソリューションを続行する前に、ソフトウェアをアンインストールしてください。
3. レプリケーションが失敗する原因となっている問題が、既知の方法で解決できない場合は、サーバーから AD DS を削除し、AD DS を再インストールします。 AD DS の再インストールの詳細については、「[ドメインコントローラーの使用を停止する](https://go.microsoft.com/fwlink/?LinkId=128290)」を参照してください。
4. サーバーがネットワークに接続されている間に AD DS を正常に削除できない場合は、次のいずれかの方法を使用して問題を解決します。

   - ディレクトリサービス復元モード (DSRM) で AD DS 削除を強制し、サーバーメタデータをクリーンアップしてから AD DS を再インストールします。
   - オペレーティングシステムを再インストールし、ドメインコントローラーを再構築します。

AD DS を強制的に削除する方法の詳細については、「[ドメインコントローラーを強制的に削除](https://go.microsoft.com/fwlink/?LinkId=128291)する」を参照してください。

## <a name="using-repadmin-to-retrieve-replication-statustitle"></a>Repadmin を使用してレプリケーションの状態を取得しています @ no__t-0

レプリケーションの状態は、ディレクトリサービスの状態を評価するための重要な方法です。 レプリケーションがエラーなしで動作している場合は、オンラインになっているドメインコントローラがあることを確認します。 また、次のシステムとサービスが動作していることもわかります。


- DNS インフラストラクチャ
- Kerberos 認証プロトコル
- Windows タイムサービス (W32time)
- リモート プロシージャ呼び出し (RPC)
- ネットワーク接続

フォレスト内のすべてのドメインコントローラーのレプリケーションステータスを評価するコマンドを実行して、レプリケーションステータスを毎日監視するには、Repadmin を使用します。 この手順では、Microsoft Excel で開くことができ、レプリケーションエラーをフィルター処理する .csv ファイルが生成されます。

フォレスト内のすべてのドメインコントローラーのレプリケーションの状態を取得するには、次の手順を実行します。 

要件

この手順を完了するには、少なくとも、**Enterprise Admins** グループ、またはそれと同等の権限を持つグループのメンバーである必要があります。 

ツール:

- Repadmin.exe
- Excel (Microsoft Office)

### <a name="to-generate-a-repadmin-showrepl-spreadsheet-for-domain-controllers"></a>ドメインコントローラーの repadmin/showrepl スプレッドシートを生成するには

1. 管理者としてコマンドプロンプトを開きます。[スタート] メニューの [コマンドプロンプト] を右クリックし、[管理者として実行] をクリックします。 [ユーザーアカウント制御] ダイアログボックスが表示されたら、必要に応じて Enterprise Admins の資格情報を入力し、[続行] をクリックします。
2. コマンドプロンプトで、次のコマンドを入力し、enter キーを押します。 `repadmin /showrepl * /csv > showrepl.csv`
3. Excel を開きます。
4. [Office] ボタンをクリックし、[開く] をクリックして、[showrepl .csv] に移動し、[開く] をクリックします。
5. 次のように、列 A とトランスポートの種類の列を非表示または削除します。
6. 非表示または削除する列を選択します。

   - 列を非表示にするには、列を右クリックし、[非表示] をクリックします。
   - 列を削除するには、選択した列を右クリックし、[削除] をクリックします。

7. 列見出し行の下にある行1を選択します。 [表示] タブで、[ウィンドウ枠の固定] をクリックし、[上の行を固定] をクリックします。
8. スプレッドシート全体を選択します。 [データ] タブで、[フィルター] をクリックします。
9. [最後の成功時間] 列で、下矢印をクリックし、[昇順で並べ替え] をクリックします。
10. [ソース DC] 列で、[フィルター] の下矢印をクリックし、[テキストフィルター] をポイントして、[カスタムフィルター] をクリックします。
11. [オートフィルター] ダイアログボックスの [行の表示] で、[以下を含まない] をクリックします。 隣接するテキストボックスに「 <userInput>del</userInput> 」と入力して、削除されたドメインコントローラーの結果を表示しないようにします。
12. [最後の失敗時刻] 列に対して手順 11. を繰り返しますが、[次の値と等しくない] を使用し、値0を入力します。
13. レプリケーションエラーを解決します。

スプレッドシートには、フォレスト内のすべてのドメインコントローラーについて、ソースレプリケーションパートナー、レプリケーションが最後に発生した時刻、および名前付けコンテキスト (ディレクトリパーティション) ごとに最後のレプリケーションエラーが発生した時間が表示されます。 Excel でオートフィルターを使用すると、ドメインコントローラーのみのレプリケーションの正常性、ドメインコントローラーのみの障害、または最新ではないドメインコントローラーのレプリケーションの正常性を表示できます。また、レプリケートしているレプリケーションパートナーを確認することもできます。なく.

## <a name="replication-problems-and-resolutions"></a>レプリケーションの問題と解決策

レプリケーションの問題は、イベントメッセージと、アプリケーションまたはサービスが操作を試行したときに発生するさまざまなエラーメッセージで報告されます。 これらのメッセージは、監視アプリケーションによって収集されるか、またはレプリケーションの状態を取得するときに使用するのが理想的です。

ほとんどのレプリケーションの問題は、ディレクトリサービスのイベントログに記録されるイベントメッセージで識別されます。 レプリケーションの問題は、 <system>repadmin/showrepl</system>コマンドの出力でエラーメッセージの形式で識別されることもあります。

### <a name="repadmin-showrepl-error-messages-that-indicate-replication-problems"></a>レプリケーションの問題を示す repadmin/showrepl エラーメッセージ

Active Directory レプリケーションの問題を特定するには、前のセクションで説明したように、 <system>repadmin/showrepl</system>コマンドを使用します。 次の表は、このコマンドによって生成されるエラーメッセージとエラーの根本原因、およびエラーの解決策を提供するトピックへのリンクを示しています。

|Repadmin エラー|根本原因|ソリューション|
| --- | --- | --- |
|このサーバーとの前回のレプリケーション以降の時間が廃棄 (tombstone) の有効期間を超えました。|ドメインコントローラは、指定されたソースドメインコントローラとの受信レプリケーションに失敗しました。これにより、削除が廃棄、レプリケート、および AD DS からのガベージコレクトが完了します。|イベント ID 2042:このマシンがレプリケートされてから長すぎます|
|受信した近隣ノードがありません。|Repadmin/showrepl によって生成される出力の "受信近隣ノード" セクションに項目が表示されない場合、ドメインコントローラーは別のドメインコントローラーとのレプリケーションリンクを確立できませんでした。|レプリケーション接続の問題を解決する (イベント ID 1925)| 
|アクセスが拒否されました。|2つのドメインコントローラー間にレプリケーションリンクが存在しますが、認証エラーの結果としてレプリケーションを正しく実行することはできません。|レプリケーションのセキュリティの問題を解決する| 
|"ターゲットアカウント名が正しくありません" で < 日時 > の前回の試行が失敗しました。|この問題は、接続、DNS、または認証の問題に関連している可能性があります。 DNS エラーの場合、ローカルドメインコントローラは、そのレプリケーションパートナーのグローバル一意識別子 (GUID) ベースの DNS 名を解決できませんでした。|レプリケーション DNS 参照の問題の修正 (イベント Id 1925、2087、2088) レプリケーションのセキュリティ問題の修正レプリケーション接続の問題の修正 (イベント ID 1925)| 
|LDAP エラー49。|ドメインコントローラコンピュータアカウントがキー配布センター (KDC) と同期されていない可能性があります。|レプリケーションのセキュリティの問題を解決する| 
|ローカルホストへの LDAP 接続を開くことができません|管理ツールが AD DS に接続できませんでした。|レプリケーション DNS 参照の問題を解決する (イベント ID 1925、2087、2088)| 
|Active Directory レプリケーションが割り込まれました。|入力方向のレプリケーションの進行状況が、repadmin/sync コマンドで手動で生成された要求など、優先度の高いレプリケーション要求によって中断されました。|レプリケーションが完了するまで待機します。 この情報メッセージは、通常の操作を示しています。| 
|レプリケーションがポストされ、待機しています。| ドメインコントローラはレプリケーション要求を送信し、応答を待機しています。 このソースからレプリケーションが進行中です。|レプリケーションが完了するまで待機します。 この情報メッセージは、通常の操作を示しています。| 

次の表は、Active Directory レプリケーションの問題を示す可能性がある一般的なイベントと、問題の根本原因、および問題の解決策を提供するトピックへのリンクを示しています。 

|イベント ID とソース|根本原因|ソリューション|
| --- | --- | --- | 
|1311 NTDS KCC|AD DS のレプリケーション構成情報には、ネットワークの物理的なトポロジが正確に反映されていません。|レプリケーション トポロジの問題を解決する (イベント ID 1311)| 
|1388 NTDS レプリケーション|厳密なレプリケーション整合性は無効であり、残留オブジェクトがドメインコントローラーにレプリケートされています。|レプリケーション残留オブジェクトの問題を解決する (イベント ID 1388、1988、2042)|
|1925 NTDS KCC|書き込み可能なディレクトリパーティションのレプリケーションリンクを確立できませんでした。 このイベントの原因は、エラーによって異なる場合があります。| レプリケーション接続の問題の修正 (イベント ID 1925) レプリケーションの DNS 参照の問題の解決 (イベント Id 1925、2087、2088)| 
|1988 NTDS レプリケーション|ローカルドメインコントローラは、ローカルドメインコントローラ上に存在しないソースドメインコントローラからオブジェクトをレプリケートしようとしました。これは、削除され、既にガーベジコレクトされている可能性があるためです。 状況が解決されるまで、このパートナーとの間でこのディレクトリパーティションのレプリケーションは続行されません。|レプリケーション残留オブジェクトの問題を解決する (イベント ID 1388、1988、2042)|
|2042 NTDS レプリケーション|このパートナーが廃棄 (tombstone) の有効期間にわたってレプリケーションを実行していないため、レプリケーションを続行できません。|レプリケーション残留オブジェクトの問題を解決する (イベント ID 1388、1988、2042)| 
|2087 NTDS レプリケーション|AD DS は、ソースドメインコントローラの DNS ホスト名を IP アドレスに解決できませんでした。レプリケーションに失敗しました。|レプリケーション DNS 参照の問題を解決する (イベント ID 1925、2087、2088)| 
|2088 NTDS レプリケーション |AD DS は、ソースドメインコントローラーの DNS ホスト名を IP アドレスに解決できませんでしたが、レプリケーションは成功しました。|レプリケーション DNS 参照の問題を解決する (イベント ID 1925、2087、2088)|
|5805 Net ログオン|コンピューターアカウントの認証に失敗しました。これは通常、同じコンピューター名の複数のインスタンス、またはコンピューター名がすべてのドメインコントローラーにレプリケートされていないことが原因で発生します。|レプリケーションのセキュリティの問題を解決する| 

レプリケーションの概念の詳細については、「 [Active Directory レプリケーションテクノロジ](https://go.microsoft.com/fwlink/?LinkId=41950)」を参照してください。
  
## <a name="next-steps"></a>次の手順

エラーコードに固有のサポート記事など、詳細については、サポート記事を参照してください。[一般的な Active Directory レプリケーションエラーをトラブルシューティングする方法](https://support.microsoft.com/help/3108513)
