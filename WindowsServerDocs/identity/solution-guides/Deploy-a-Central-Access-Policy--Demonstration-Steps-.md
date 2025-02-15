---
ms.assetid: 8738c03d-6ae8-49a7-8b0c-bef7eab81057
title: 集約型アクセス ポリシーの展開 (デモンストレーション手順)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 09b7edcd843dfe65d7e2391612f029cf18b633ec
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357500"
---
# <a name="deploy-a-central-access-policy-demonstration-steps"></a>集約型アクセス ポリシーの展開 (デモンストレーション手順)

>適用先:Windows Server 2016 では、Windows Server 2012 R2、Windows Server 2012

このシナリオでは、金融部門のセキュリティ運用では、集約型情報セキュリティを操作して、ファイル サーバーに保管されたアーカイブ金融情報を保護できるように集約型アクセス ポリシーのニーズを指定します。 各国のアーカイブ金融情報には、同じ国の金融従業員が読み取り専用としてアクセスできます。 集約型金融管理グループは、すべての国の金融情報にアクセスできます。  

集約型アクセス ポリシーの展開には、以下のフェーズがあります。  

|フェーズ|説明  
|---------|---------------  
|[Plan:ポリシーの必要性と、展開に必要な構成を特定する @ no__t-0|ポリシーのニーズおよび展開に必要な構成を特定します。 
|[Implement:コンポーネントとポリシーを構成する @ no__t-0|コンポーネントおよびポリシーを構成します。  
|[集約型アクセスポリシーを展開する](Deploy-a-Central-Access-Policy--Demonstration-Steps-.md#BKMK_1.4)|ポリシーを展開します。  
|@no__t の管理:ポリシーを変更してステージングする @ no__t-0|ポリシーの変更とステージング。 

## <a name="BKMK_1.1"></a>テスト環境のセットアップ  
開始する前に、このシナリオをテストするラボを設定する必要があります。 ラボを設定する手順の詳細については [Appendix B をご覧ください。テスト環境 @ no__t を設定しています。  

## <a name="BKMK_1.2"></a>行うポリシーのニーズおよび展開に必要な構成を特定する  
このセクションでは、展開の計画フェーズで役立つ一連の大まかな手順を示します。  

||手順|例|  
|-|--------|-----------|  
|1.1|ビジネスで集約型アクセス ポリシーが必要であると判断する|ファイル サーバーに保管されている金融情報を保護するために、金融部門のセキュリティ運用では、集約型情報セキュリティを操作して、集約型アクセス ポリシーのニーズを指定します。|  
|1.2|アクセス ポリシーを表現する|金融ドキュメントは、金融部門のメンバーのみが読み取るようにする必要があります。 金融部門のメンバーは、属している国のドキュメントのみにアクセスする必要があります。 書き込みアクセス権限を備えているのは、金融管理者のみにします。 FinanceException グループのメンバーに対して例外が許可されるようにします。 このグループは読み取りアクセス権限を備えています。|  
|1.3|Windows Server 2012 コンストラクトでアクセスポリシーを表現する|ターゲット:<br /><br />-Resource. Department は Finance を含みます。<br /><br />アクセス規則:<br /><br />-Allow read User. Country = Resource. Country AND User department = Resource. Department<br />-フルコントロールユーザーを許可します。 MemberOf (FinanceAdmin)<br /><br />例外:<br /><br />Allow read memberOf(FinanceException)|  
|1.4|ポリシーに必要なファイル プロパティを決定する|次の項目でファイルにタグを付けます。<br /><br />-Department<br />-国|  
|1.5|ポリシーに必要な要求の種類とグループを決定する|要求の種類:<br /><br />-国<br />-Department<br /><br />ユーザー グループ:<br /><br />-FinanceAdmin<br />-FinanceException|  
|1.6|このポリシーを適用するサーバーを決定する|すべての金融ファイル サーバーでポリシーを適用します。|  

## <a name="BKMK_1.3"></a>導入コンポーネントおよびポリシーを構成する  
このセクションでは、金融ドキュメント用の集約型アクセス ポリシーを展開する例を示します。  

|いいえ|手順|例|  
|------|--------|-----------|  
|2.1|要求の種類を作成する|次の要求の種類を作成します。<br /><br />-Department<br />-国|  
|2.2|リソース プロパティを作成する|次のリソース プロパティを作成して有効にします。<br /><br />-Department<br />-国|  
|2.3|集約型アクセス規則を構成する|前のセクションで決定したポリシーが含まれた金融ドキュメント規則を作成します。|  
|2.4|集約型アクセス ポリシー (CAP) を構成する|金融ポリシーという CAP を作成し、その CAP に金融ドキュメント規則を追加します。|  
|2.5|集約型アクセス ポリシーのターゲットをファイル サーバーに設定する|金融ポリシー CAP をファイル サーバーにパブリッシュします。|  
|2.6|KDC での信頼性情報、複合認証、および Kerberos 防御のサポートを有効にする|contoso.com について KDC での信頼性情報、複合認証、および Kerberos 防御のサポートを有効にします。|  

次の手順では、次の 2 つの信頼性情報の種類を作成します。国と部門。  

#### <a name="to-create-claim-types"></a>要求の種類を作成するには  

1. Hyper-v マネージャーでサーバー DC1 を開き、パスワード<strong>pass@word1</strong>で contoso\administrator としてログオンします。  

2. Active Directory 管理センターを開きます。  

3. **ツリー ビュー アイコン**をクリックし、 **[ダイナミック アクセス制御]** を展開し、 **[要求の種類]** を選択します。  

   **[要求の種類]** を右クリックし、 **[新規作成]** をクリックしてから、 **[要求の種類]** をクリックします。  

   > [!TIP]  
   > **[タスク]** ウィンドウから **[要求の種類の作成:]** ウィンドウを開きます。 **[タスク]** ウィンドウで **[新規作成]** をクリックしてから、 **[要求の種類]** をクリックします。  

4. **[ソース属性]** リストで属性のリストを下にスクロールし、 **[department]** をクリックします。 これにより、 **[表示名]** フィールドに **[department]** のデータが設定されます。 **[OK]** をクリックします。  

5. **[タスク]** ウィンドウで **[新規作成]** をクリックしてから、 **[要求の種類]** をクリックします。  

6. **[ソース属性]** リストで属性のリストを下にスクロールし、 **[c]** 属性 (国名) をクリックします。 **[表示名]** フィールドに **country** と入力します。  

7. **[提案された値]** セクションで **[次の値を提案します:]** を選択してから、 **[追加]** をクリックします。  

8. **[値]** フィールドおよび **[表示名]** フィールドに **US** と入力してから、 **[OK]** をクリックします。  

9. 上記ステップを繰り返します。 **[提案された値の追加]** ダイアログ ボックスの **[値]** フィールドおよび **[表示名]** フィールドに **JP** と入力してから、 **[OK]** をクリックします。  

@no__t 0solution ガイド](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell の同等のコマンド</em>***  

以下の Windows PowerShell コマンドレットは、前述の手順と同じ機能を実行します。 ここでは書式上の制約のために、折り返されて複数の行にわたって表示される場合もありますが、各コマンドレットは 1 行に入力します。  


    New-ADClaimType country -SourceAttribute c -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("US","US","")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("JP","JP","")))  
    New-ADClaimType department -SourceAttribute department  



> [!TIP]  
> Active Directory 管理センターの Windows PowerShell 履歴ビューアーを使用して、Active Directory 管理センターで実行する各手順の Windows PowerShell コマンドレットを検索できます。 詳細については、「 [Windows PowerShell 履歴ビューアー](https://technet.microsoft.com/library/hh831702) 」を参照してください。  

次の手順では、リソース プロパティを作成します。 次の手順では、ドメイン コントローラーのグローバル リソース プロパティ リストに自動的に追加されるリソース プロパティを作成して、ファイル サーバーで使用可能にします。  

#### <a name="to-create-and-enable-pre-created-resource-properties"></a>事前作成のリソース プロパティを作成して有効にするには  

1.  Active Directory 管理センターの左側のウィンドウで、 **[ツリー ビュー]** をクリックします。 **[ダイナミック アクセス制御]** を展開してから、 **[リソース プロパティ]** を選択します。  

2.  **[リソース プロパティ]** を右クリックし、 **[新規作成]** をクリックしてから、 **[参照用リソース プロパティ]** をクリックします。  

    > [!TIP]  
    > **[タスク]** ウィンドウからリソース プロパティを選択することもできます。 **[新規作成]** をクリックしてから、 **[参照用リソース プロパティ]** をクリックします。  

3.  **[提案された値リストを共有する要求の種類の選択]** で、 **[country]** をクリックします。  

4.  **[表示名]** フィールドに **country** と入力してから、 **[OK]** をクリックします。  

5.  **[リソース プロパティ]** リストをダブルクリックし、 **[Department]** リソース プロパティまで下にスクロールします。 右クリックし、 **[有効にする]** をクリックします。 これにより、ビルトイン **[Department]** リソース プロパティが有効になります。  

6.  これで、Active Directory 管理センターのナビゲーション ウィンドウの **[リソース プロパティ]** リストに、次の 2 つの有効になっているリソース プロパティが含まれています。  

    -   Country  

    -   部署  

@no__t 0solution ガイド](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell の同等のコマンド</em>***  

以下の Windows PowerShell コマンドレットは、前述の手順と同じ機能を実行します。 ここでは書式上の制約のために、折り返されて複数の行にわたって表示される場合もありますが、各コマンドレットは 1 行に入力します。  

```  
New-ADResourceProperty Country -IsSecured $true -ResourcePropertyValueType MS-DS-MultivaluedChoice -SharesValuesWith country  
Set-ADResourceProperty Department_MS -Enabled $true  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Country  
Add-ADResourcePropertyListMember "Global Resource Property List" -Members Department_MS  

```  

次の手順では、リソースにアクセスできるユーザーを定義する集約型アクセス規則を作成します。 このシナリオでは、ビジネス規則は次のようにします。  

-   金融ドキュメントは、金融部門のメンバーのみが読み取ることができます。  

-   金融部門のメンバーは、属している国のドキュメントのみにアクセスできます。  

-   書き込みアクセス権限を備えることができるのは、金融管理者のみにします。  

-   FinanceException グループのメンバーに対して例外を許可します。 このグループは読み取りアクセス権限を備えています。  

-   管理者およびドキュメント所有者は引き続き、フル アクセスできます。  

または、Windows Server 2012 の構成要素を使用して規則を表現します。  

ターゲット:Resource.Department Contains Finance  

アクセス規則:  

-   Allow Read User.Country=Resource.Country AND User.department = Resource.Department  

-   Allow Full control User.MemberOf(FinanceAdmin)  

-   Allow Read User.MemberOf(FinanceException)  

#### <a name="to-create-a-central-access-rule"></a>集約型アクセス規則を作成するには  

1. Active Directory 管理センターの左側のウィンドウで、 **[ツリー ビュー]** をクリックし、 **[ダイナミック アクセス制御]** を選択してから、 **[集約型アクセス規則]** をクリックします。  

2. **[集約型アクセス規則]** を右クリックし、 **[新規作成]** をクリックしてから、 **[集約型アクセス規則]** をクリックします。  

3. **[名前]** フィールドに「**金融ドキュメント規則**」と入力します。  

4. **[ターゲット リソース]** セクションで **[編集]** をクリックし、 **[集約型アクセス規則]** ダイアログ ボックスで **[条件の追加]** をクリックします。 次の条件を追加します。   
   **[リソース]** **[Department]** **[等しい]** **[値]** **[Finance]** 。次に、 **[OK]** をクリックします。  

5. **[アクセス許可]** セクションで **[次のアクセス許可を現在のアクセス許可として使用する]** を選択し、 **[編集]** をクリックし、 **[アクセス許可のセキュリティの詳細設定]** ダイアログ ボックスで **[追加]** をクリックします。  

   > [!NOTE]  
   > **[次のアクセス許可を、提案されたアクセス許可として使用する]** オプションを使用することで、ステージングのポリシーを作成できます。 これを行う方法について詳しくは、「保守:ポリシーを変更およびステージングする」セクション (このトピック内) を参照してください。  

6. **[アクセス許可のアクセス許可エントリ]** ダイアログ ボックスで **[プリンシパルの選択]** をクリックし、「**Authenticated Users**」と入力してから、 **[OK]** をクリックします。  

7. **[アクセス許可のアクセス許可エントリ]** ダイアログ ボックスで **[条件の追加]** をクリックし、次の条件を追加します。   
   **[ユーザー]** **[国]** **[その他]** **[リソース]** **[国]**   
    **[条件の追加]** をクリックします。   
    **[および]**   
   **[ユーザー]** **[Department]** **[任意]** **[リソース]** **[department]** をクリックします。 **[アクセス許可]** を **[読み取り]** に設定します。  

8. **[OK]** をクリックしてから、 **[追加]** をクリックします。 **[プリンシパルの選択]** をクリックし、「 **FinanceAdmin**」と入力して、 **[OK]** をクリックします。  

9. **[変更]、[読み取りと実行]、[読み取り]、[書き込み]** の各アクセス許可を選択してから、 **[OK]** をクリックします。  

10. **[追加]** をクリックし、 **[プリンシパルの選択]** をクリックし、**FinanceException** と入力してから、 **[OK]** をクリックします。 アクセス許可が **[読み取り]** および **[読み取りと実行]** になるように選択します。  

11. **[OK]** を 3 回クリックして終了し、Active Directory 管理センターに戻ります。  

    @no__t 0solution ガイド](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell の同等のコマンド</em>***  

    以下の Windows PowerShell コマンドレットは、前述の手順と同じ機能を実行します。 ここでは書式上の制約のために、折り返されて複数の行にわたって表示される場合もありますが、各コマンドレットは 1 行に入力します。  


~~~
$countryClaimType = Get-ADClaimType country  
$departmentClaimType = Get-ADClaimType department  
$countryResourceProperty = Get-ADResourceProperty Country  
$departmentResourceProperty = Get-ADResourceProperty Department  
$currentAcl = "O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;0x1200a9;;;S-1-5-21-1787166779-1215870801-2157059049-1113)(A;;0x1301bf;;;S-1-5-21-1787166779-1215870801-2157059049-1112)(A;;FA;;;SY)(XA;;0x1200a9;;;AU;((@USER." + $countryClaimType.Name + " Any_of @RESOURCE." + $countryResourceProperty.Name + ") && (@USER." + $departmentClaimType.Name + " Any_of @RESOURCE." + $departmentResourceProperty.Name + ")))"  
$resourceCondition = "(@RESOURCE." + $departmentResourceProperty.Name + " Contains {`"Finance`"})"  
New-ADCentralAccessRule "Finance Documents Rule" -CurrentAcl $currentAcl -ResourceCondition $resourceCondition  
~~~


> [!IMPORTANT]  
> 上記のコマンドレットの例では、グループ FinanceAdmin およびユーザーのセキュリティ ID (SID) は作成時に決定されるため、お客様の例では異なるものになります。 例えば、FinanceAdmin に指定されている SID 値 (S-1-5-21-1787166779-1215870801-2157059049-1113) は、ご使用の展開で作成する必要がある FinanceAdmin グループの実際の SID に置き換える必要があります。 Windows PowerShell を使用して、このグループの SID 値を検索し、その値を変数に割り当ててから、ここでその変数を使用できます。 詳細については、「[Windows PowerShell ヒント:Sid @ no__t を使用して作業する。  

これで、ユーザーが同じ国および同じ部門のドキュメントにアクセスできるようにする集約型アクセス規則が作成されました。 この規則により、FinanceAdmin グループはドキュメントを編集でき、FinanceException グループはドキュメントを読み取ることができます。 この規則のターゲットは、金融として分類されているドキュメントのみです。  

#### <a name="to-add-a-central-access-rule-to-a-central-access-policy"></a>集約型アクセス規則を集約型アクセス ポリシーに追加するには  

1. Active Directory 管理センターの左側のウィンドウで、 **[ダイナミック アクセス制御]** をクリックしてから、 **[集約型アクセス ポリシー]** をクリックします。  

2. **[タスク]** ウィンドウの **[新規作成]** をクリックし、 **[集約型アクセス ポリシー]** をクリックします。  

3. **[集約型アクセス ポリシーの作成:]** の **[名前]** ボックスに「**金融ポリシー**」と入力します。  

4. **[メンバー集約型アクセス規則]** で **[追加]** をクリックします。  

5. **[金融ドキュメント規則]** をダブルクリックして **[次の集約型アクセス規則を追加します]** リストに追加してから、 **[OK]** をクリックします。  

6. **[OK]** をクリックして完了します。 これで、金融ポリシーという集約型アクセス ポリシーが作成されました。  

   @no__t 0solution ガイド](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell の同等のコマンド</em>***  

   以下の Windows PowerShell コマンドレットは、前述の手順と同じ機能を実行します。 ここでは書式上の制約のために、折り返されて複数の行にわたって表示される場合もありますが、各コマンドレットは 1 行に入力します。  

   ```  
   New-ADCentralAccessPolicy "Finance Policy" Add-ADCentralAccessPolicyMember   
   -Identity "Finance Policy"   
   -Member "Finance Documents Rule"  

   ```  

#### <a name="to-apply-the-central-access-policy-across-file-servers-by-using-group-policy"></a>グループ ポリシーを使用してファイル サーバー全体で集約型アクセス ポリシーを適用するには  

1.  **[スタート]** 画面の **[検索]** ボックスに「**グループ ポリシーの管理**」と入力します。 **[グループ ポリシーの管理]** をダブルクリックします。  

    > [!TIP]  
    > **[管理ツールを表示]** の設定が無効になっていると、 **[管理ツール]** フォルダーとその内容は **[設定]** の結果に表示されません。  

    > [!TIP]  
    > 運用環境では、ファイル サーバー組織単位 (OU) を作成し、このポリシーを適用するすべてのファイル サーバーをその OU に追加する必要があります。 次に、グループ ポリシーを作成し、この OU をそのポリシーに追加できます。  

2.  この手順では、テスト環境のセクション「[ドメイン コントローラーを作成する](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_Build)」で作成したグループ ポリシー オブジェクトを編集して、作成した集約型アクセス ポリシーを組み込みます。 グループ ポリシー管理エディターで、ドメイン (この例では contoso.com) の組織単位に移動して選択します。 **[グループ ポリシーの管理]** 、 **[フォレスト: contoso.com]** 、 **[ドメイン]** 、 **[contoso.com]** 、 **[Contoso]** 、 **[FileServerOU]** 。  

3.  **[FlexibleAccessGPO]** を右クリックしてから、 **[編集]** をクリックします。  

4.  グループ ポリシー管理エディター ウィンドウで、**コンピューターの構成** に移動し、**ポリシー** を展開し、**Windows の設定** を展開し、**セキュリティの設定** をクリックします。  

5.  **[ファイル システム]** を展開し、 **[集約型アクセス ポリシー]** を右クリックしてから、 **[集約型アクセス ポリシーの管理]** をクリックします。  

6.  **[集約型アクセス ポリシー構成]** ダイアログ ボックスで **[金融ポリシー]** を追加してから、 **[OK]** をクリックします。  

7.  **[監査ポリシーの詳細な構成]** まで下にスクロールしてこれを展開します。  

8.  **[監査ポリシー]** を展開し、 **[オブジェクト アクセス]** を選択します。  

9. **[集約型アクセス ポリシー ステージングの監査]** をダブルクリックします。 3 つすべてのチェック ボックスを選択してから、 **[OK]** をクリックします。 この手順により、システムは、集約型アクセス ステージング ポリシーに関連した監査イベントを受信できるようになります。  

10. **[ファイル システム プロパティの監査]** をダブルクリックします。 3 つすべてのチェック ボックスを選択してから、 **[OK]** をクリックします。  

11. グループ ポリシー管理エディターを閉じます。 これで、集約型アクセス ポリシーがグループ ポリシーに組み込まれました。  

ドメインのドメインコントローラーで信頼性情報またはデバイスの承認データを提供するには、ダイナミックアクセス制御をサポートするようにドメインコントローラーを構成する必要があります。  

#### <a name="to-enable-support-for-claims-and-compound-authentication-for-contosocom"></a>contoso.com の信頼性情報および複合認証のサポートを有効にするには  

1.  グループ ポリシーの管理を開き、**contoso.com** をクリックしてから、 **[ドメイン コントローラー]** をクリックします。  

2.  **[既定のドメイン コントローラー ポリシー]** を右クリックし、 **[編集]** をクリックします。  

3.  グループ ポリシー管理エディター ウィンドウで **コンピューターの構成** をダブルクリックし、**ポリシー** をダブルクリックし、**管理用テンプレート** をダブルクリックし、**システム** をダブルクリックしてから、**KDC** をダブルクリックします。  

4.  **[KDC で信頼性情報、複合認証、および Kerberos 防御をサポートする]** をダブルクリックします。 **[KDC で信頼性情報、複合認証、および Kerberos 防御をサポートする]** ダイアログ ボックスで **[有効]** をクリックし、 **[オプション]** ドロップ ダウン リストから **[サポートされています]** を選択します。 (集約型アクセス ポリシーでユーザーの信頼性情報を使用するには、この設定を有効にする必要があります)。  

5.  **[グループ ポリシーの管理]** を閉じます。  

6.  コマンド プロンプトを開き、「`gpupdate /force`」と入力します。  

## <a name="BKMK_1.4"></a>集約型アクセスポリシーを展開する  

||手順|例|  
|-|--------|-----------|  
|3.1|ファイル サーバー上の該当する共有フォルダーに CAP を割り当てる。|ファイル サーバー上の該当する共有フォルダーに集約型アクセス ポリシーを割り当てます。|  
|3.2|アクセスが適切に構成されていることを確認する。|さまざまな国や部門のユーザーのアクセスを確認します。|  

この手順では、ファイル サーバーに集約型アクセス ポリシーを割り当てます。 以前の手順で作成した集約型アクセス ポリシーを受信するファイル サーバーにログオンし、ポリシーを共有フォルダーに割り当てます。  

#### <a name="to-assign-a-central-access-policy-to-a-file-server"></a>ファイル サーバーに集約型アクセス ポリシーを割り当てるには  

1. Hyper-V マネージャーでサーバー FILE1 に接続します。 Contoso\administrator を使用して、サーバーにログオンします。パスワードは<strong>pass@word1</strong>です。  

2. 管理者特権でのコマンド プロンプトを開き、コマンド **gpupdate /force** を入力します。 これにより、グループ ポリシーの変更がサーバーで有効になります。  

3. Active Directory からグローバル リソース プロパティーを更新する必要もあります。 管理者特権で Windows PowerShell ウィンドウを開き、「`Update-FSRMClassificationpropertyDefinition`」と入力します。 Enter キーをクリックしてから、Windows PowerShell を閉じます。  

   > [!TIP]
   > ファイル サーバーにログオンしてグローバル リソース プロパティを更新することもできます。 ファイル サーバーからグローバル リソース プロパティを更新するには、次のようにします。  
   > 
   > 1. パスワード<strong>pass@word1</strong>を使用して、contoso\administrator としてファイルサーバー FILE1 にログオンします。  
   > 2. ファイル サーバー リソース マネージャーを開きます。 ファイル サーバー リソース マネージャーを開くには、 **[スタート]** をクリックし、「 **ファイル サーバー リソース マネージャー**」と入力してから、 **[ファイル サーバー リソース マネージャー]** をクリックします。  
   > 3. ファイル サーバー リソース マネージャーで **[ファイル分類管理]** をクリックし、 **[分類プロパティ]** を右クリックしてから、 **[更新]** をクリックします。  

4. エクスプローラーを開き、左側のウィンドウでドライブ D をクリックします。 **[Finance Documents]** フォルダーを右クリックし、 **[プロパティ]** をクリックします。  

5. **[分類]** タブをクリックし、 **[Country]** をクリックしてから、 **[値]** フィールドで **[US]** を選択します。  

6. **[Department]** をクリックしてから、 **[値]** フィールドで **[Finance]** を選択し、 **[適用]** をクリックします。  

   > [!NOTE]  
   > 集約型アクセス ポリシーは、金融部門のファイルをターゲットにするように構成されていることに注意してください。 以前の手順では、Country 属性および Department 属性を使用して、フォルダー内のすべてのドキュメントをマークしています。  

7. **[セキュリティ]** タブをクリックし、 **[詳細設定]** をクリックします。 **[集約型ポリシー]** タブをクリックします。  

8. **[変更]** をクリックし、ドロップダウン メニューから **[金融ポリシー]** を選択してから、 **[適用]** をクリックします。 **[金融ドキュメント規則]** がポリシーにリストされているのを確認できます。 この項目を展開して、Active Directory の規則を作成したときに設定したすべてのアクセス許可を表示します。  

9. **[OK]** をクリックしてエクスプローラーに戻ります。  

次の手順では、アクセスが適切に構成されているかを確認します。  ユーザー アカウントには、適切な部門属性が設定されている必要があります (これは、Active Directory 管理センターを使用して設定します)。 新規ポリシーの結果を表示する最もシンプルな方法としては、エクスプローラーの **[有効なアクセス]** タブを使用します。 **[有効なアクセス]** タブには、特定のユーザー アカウントのアクセス権限が表示されます。  

#### <a name="to-examine-the-access-for-various-users"></a>各種ユーザーのアクセスを調べるには  

1.  Hyper-V マネージャーでサーバー FILE1 に接続します。 contoso\administrator を使用してサーバーにログオンします。 エクスプローラーで D:\ に移動します。 **[Finance Documents]** フォルダーを右クリックしてから、 **[プロパティ]** をクリックします。  

2.  **[セキュリティ]** タブをクリックし、 **[詳細設定]** をクリックしてから、 **[有効なアクセス]** タブをクリックします。  

3.  ユーザーのアクセス許可を確認するには、 **[ユーザーの選択]** をクリックし、ユーザーの名前を入力して、有効な **[アクセスの表示]** をクリックします。有効なアクセス権が表示されます。 以下に例を示します。  

    -   Myriam Delesalle (MDelesalle) は金融部門に属しており、フォルダーに対する読み取りアクセス権を必要としています。  

    -   Miles Reid (MReid) は FinanceAdmin グループのメンバーであり、フォルダーに対する変更アクセス権を必要としています。  

    -   Esther Valle (EValle) は金融部門に属していませんが、FinanceException グループのメンバーであり、読み取りアクセス権が必要です。  

    -   Maira Wenzel (MWenzel) は金融部門に属しておらず、また FinanceAdmin グループと FinanceException グループのいずれのメンバーでもありません。 フォルダーに対するどのアクセスも備えていてはなりません。  

    有効なアクセス ウィンドウの **アクセスの制限者** とう最後の列に注目してください。 この列には、ユーザーのアクセス許可に影響を与えるゲートが示されます。 この場合、共有および NTFS のアクセス許可により、すべてのユーザーにフル コントロールが許可されています。 ただし、集約型アクセス ポリシーは、以前に構成した規則に基づいてアクセスを制限します。  

## <a name="BKMK_1.5"></a>保存ポリシーを変更およびステージングする  

||||  
|-|-|-|  
|数値|手順|例|  
|4.1|クライアント用のデバイスの信頼性情報を構成する|グループ ポリシー設定を設定してデバイスの信頼性情報を有効にします。|  
|4.2|デバイスの信頼性情報を有効にする|デバイスの国の信頼性情報の種類を有効にします。|  
|4.3|変更する既存の集約型アクセス規則にステージング ポリシーを追加する|金融ドキュメント規則を変更して、ステージング ポリシーを追加します。|  
|4.4|ステージング ポリシーの結果を表示する|Velle のアクセス許可があるかどうかを確認します。|  

#### <a name="to-set-up-group-policy-setting-to-enable-claims-for-devices"></a>デバイスの信頼性情報を有効にするためにグループ ポリシー設定をセットアップするには  

1.  DC1 にログオンし、グループ ポリシーの管理を開き、**contoso.com** をクリックし、 **[既定のドメイン ポリシー]** をクリックし、右クリックして **[編集]** を選択します。  

2.  グループ ポリシー管理エディター ウィンドウで **コンピューターの構成**、**ポリシー**、**管理用テンプレート**、**システム**、**Kerberos** に移動します。  

3.  **[要求、複合認証、および Kerberos 防御の Kerberos クライアント サポート]** を選択し、 **[有効にする]** をクリックします。  

#### <a name="to-enable-a-claim-for-devices"></a>デバイスの信頼性情報を有効にするには  

1. Hyper-v マネージャーでサーバー DC1 を開き、パスワード<strong>pass@word1</strong>で contoso\Administrator としてログオンします。  

2. **[ツール]** メニューから Active Directory 管理センターを開きます。  

3. **[ツリー ビュー]** をクリックし、 **[ダイナミック アクセス制御]** を展開し、 **[要求の種類]** をダブルクリックし、**country** 要求をダブルクリックします。  

4. **[この種類の要求は次のクラスに対して発行できます]** で **[コンピューター]** チェック ボックスを選択します。 **[OK]** をクリックします。   
   これで、 **[ユーザー]** と **[コンピューター]** の両チェック ボックスが選択されています。 デバイスでユーザーに加えて country 要求を使用できるようになりました。  

次の手順では、ステージング ポリシー規則を作成します。 ステージング ポリシーを使用して、新規ポリシー エントリを有効にする前にその効果をモニターできます。 次の手順では、ステージング ポリシー エントリを作成し、共有フォルダーでその効果をモニターします。  

#### <a name="to-create-a-staging-policy-rule-and-add-it-to-the-central-access-policy"></a>ステージング ポリシー規則を作成して集約型アクセス ポリシーに追加するには  

1. Hyper-v マネージャーでサーバー DC1 を開き、パスワード<strong>pass@word1</strong>で contoso\Administrator としてログオンします。  

2. Active Directory 管理センターを開きます。  

3. **[ツリー ビュー]** をクリックし、 **[ダイナミック アクセス制御]** を展開し、 **[集約型アクセス規則]** を選択します。  

4. **[金融ドキュメント規則]** を右クリックしてから、 **[プロパティ]** をクリックします。  

5. **[提案されたアクセス許可]** セクションで **[アクセス許可のステージング構成を有効にする]** チェック ボックスを選択し、 **[編集]** をクリックしてから、 **[追加]** をクリックします。 **[提案されたアクセス許可のアクセス許可エントリ]** ウィンドウで **[プリンシパルの選択]** リンクをクリックし、「**Authenticated Users**」と入力してから、 **[OK]** をクリックします。  

6. **[条件の追加]** リンクをクリックし、次の条件を追加します。   
    **[ユーザー]** **[country]** **[いずれかを満たす]** **[リソース]** **[Country]** .  

7. **[条件の追加]** を再度クリックし、次の条件を追加します。  
   **[および]**   
    **[デバイス]** **[country]** **[いずれかを満たす]** **[リソース]** **[Country]**  

8. **[条件の追加]** を再度クリックし、次の条件を追加します。  
   と   
    **[ユーザー]** **[グループ]** **[すべてのメンバーのメンバー]** **[値]** \(**FinanceException**)  

9. FinanceException グループを設定するには、 **[項目の追加]** をクリックし、 **[ユーザー、コンピューター、サービス アカウントまたはグループの選択]** ウィンドウで **FinanceException** と入力します。  

10. **[アクセス許可]** をクリックし、 **[フル コントロール]** を選択し、 **[OK]** をクリックします。  

11. 提案されたアクセス許可のセキュリティの詳細設定 ウィンドウで **FinanceException** を選択し、**削除** をクリックします。  

12. **[OK]** を 2 回クリックして完了します。  

@no__t 0solution ガイド](media/Deploy-a-Central-Access-Policy--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell の同等のコマンド</em>***  

以下の Windows PowerShell コマンドレットは、前述の手順と同じ機能を実行します。 ここでは書式上の制約のために、折り返されて複数の行にわたって表示される場合もありますが、各コマンドレットは 1 行に入力します。  

```  
Set-ADCentralAccessRule  
-Identity: "CN=FinanceDocumentsRule,CN=CentralAccessRules,CN=ClaimsConfiguration,CN=Configuration,DC=Contoso.com"  
-ProposedAcl: "O:SYG:SYD:AR(A;;FA;;;BA)(A;;FA;;;SY)(A;;0x1301bf;;;S-1-21=1426421603-1057776020-1604)"  
-Server: "WIN-2R92NN8VKFP.Contoso.com"  

```  

> [!NOTE]  
> 上記のコマンドレットの例では、サーバー値はテスト ラボ環境のサーバーを反映しています。 Windows PowerShell 履歴ビューアーを使用して、Active Directory 管理センターで実行する各手順の Windows PowerShell コマンドレットを検索できます。 詳細については、「 [Windows PowerShell 履歴ビューアー](https://technet.microsoft.com/library/hh831702) 」を参照してください。  

この提案されたアクセス許可セットでは、FinanceException グループのメンバーは、属している国のファイルにドキュメントとして同じ国のデバイスを使用してアクセスした場合、そのファイルに対するフル アクセスを備えています。 金融部門のユーザーがファイルにアクセスしようとした場合、ファイル サーバーのセキュリティ ログに監査エントリが書き込まれます。 ただし、ポリシーがステージングのレベルから上げられるまで、セキュリティ設定は適用されません。  

次の手順では、ステージング ポリシーの結果を確認します。 現在の規則に基づいてアクセス許可を備えているユーザー名を使用して、共有フォルダーにアクセスします。 Esther Valle (EValle) は FinanceException のメンバーであり、現在、読み取り権限を備えています。 このステージング ポリシーでは、EValle はどの権限も備えていてはなりません。  

#### <a name="to-verify-the-results-of-the-staging-policy"></a>ステージング ポリシーの結果を確認するには  

1. Hyper-v マネージャーでファイルサーバー FILE1 に接続し、パスワード<strong>pass@word1</strong>を使用して contoso\administrator としてログオンします。  

2. コマンド プロンプト ウィンドウを開き、**gpupdate /force** と入力します。 これにより、グループ ポリシーの変更がサーバーで有効になります。  

3. Hyper-V マネージャーでサーバー CLIENT1 に接続します。 現在ログオンしているユーザーをログオフします。 仮想マシン CLIENT1 を再起動します。 次に、contoso\EValle pass@word1 を使用してコンピューターにログオンします。  

4. デスクトップのショートカットをダブルクリックして \\ \ File1\finance documents ドキュメントにします。 EValle は引き続きファイルにアクセスできます。 FILE1 に切り替えます。  

5. デスクトップのショートカットから **[イベント ビューアー]** を開きます。 **[Windows ログ]** を展開してから、 **[セキュリティ]** を選択します。 **[集約型アクセスポリシーステージング]** タスクカテゴリで、**イベント ID 4818**のエントリを開きます。 EValle にアクセスが許可されていたことが分かります。ただし、ステージング ポリシーに従えば、このユーザーはアクセスが拒否されていました。  

## <a name="next-steps"></a>次の手順  
System Center Operations Manager などの集約型サーバー管理システムがある場合、イベントをモニターするように構成することもできます。 これにより、管理者は、集約型アクセス ポリシーを適用する前にその効果をモニターできます。  


