---
title: AD フォレストの回復-RID プールを生成しています
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c37bc129-a5e0-4219-9ba7-b4cf3a9fc9a4
ms.technology: identity-adds
ms.openlocfilehash: aa1f5e8b40aa43fa2601bc6f11efe2fcd4ccd05e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369058"
---
# <a name="ad-forest-recovery---raising-the-value-of-available-rid-pools"></a>AD フォレストの回復-使用可能な RID プールの値を上げる 

>適用先:Windows Server 2016、Windows Server 2012、および 2012 R2、Windows Server 2008 および 2008 R2

次の手順を使用して、RID 操作マスタがその DC を復元した後に割り当てる相対 ID (RID) プールの値を上げることができます。 使用可能な RID プールの値を上げることにより、DC がドメインの復元に使用されたバックアップの後に作成されたセキュリティプリンシパルに RID を割り当てないようにすることができます。 

## <a name="about-active-directory-rid-pools-and-ridavailablepool"></a>Active Directory RID プールと rIDAvailablePool について

各ドメインには、オブジェクト**CN = RID Manager $、cn = System、DC**@no__t-*1 ドメイン*名 > があります。 このオブジェクトには、 **rIDAvailablePool**という名前の属性があります。 この属性値は、ドメイン全体のグローバル RID 空間を保持します。 値は、上限と下限の部分を含む大きな整数です。 この部分では、各ドメインに割り当てることができるセキュリティプリンシパルの数を定義します (0x3FFFFFFF または10億以上)。 下の部分は、ドメインに割り当てられている Rid の数です。 
  
> [!NOTE]
> Windows Server 2016 および2012では、割り当て可能なセキュリティプリンシパルの数が20億を超えて増加しています。 詳細については、「 [RID 発行の管理](https://technet.microsoft.com/library/jj574229.aspx)」を参照してください。 
  
- サンプル値:4611686014132422708  
- 低部分:2100 (割り当てられる次の RID プールの開始)  
- 上部:1073741823 (ドメインに作成できる Rid の合計数)  
  
大きい整数の値を大きくすると、小さい部分の値が増加します。 たとえば、4611686014132522708の合計に対して4611686014132422708のサンプル値に10万を追加すると、新しい低部分は102100になります。 これは、RID マスターによって割り当てられる次の RID プールが2100ではなく102100で始まることを示します。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-adsiedit-and-the-calculator"></a>Adsiedit と電卓を使用して使用可能な RID プールの値を上げるには

1. サーバーマネージャーを開き、 **[ツール]** をクリックし、 **[ADSI エディター]** をクリックします。
2. 右クリックして **[接続]** を選択し、既定の名前付けコンテキストを接続して [ **OK]** をクリックします。
   ![ADSI Edit @ no__t 
3. 次の識別名パスを参照します。**Cn = RID Manager $、cn = System、DC = <domain name>** 。
   ![ADSI Edit @ no__t 
3. を右クリックし、CN = RID Manager $ のプロパティを選択します。 
4. 属性**rIDAvailablePool**を選択し、 **[編集]** をクリックしてから、大きな整数値をクリップボードにコピーします。
   ![ADSI Edit @ no__t  
5. 電卓を起動し、 **[表示]** メニューの **[科学的モード]** をクリックします。 
6. 現在の値に10万を追加します。
   ![ADSI Edit @ no__t 
7. Ctrl + c キーを使用するか、または **[編集]** メニューの **[コピー]** コマンドを使用して、値をクリップボードにコピーします。 
8. Adsiedit の [編集] ダイアログで、この新しい値を貼り付けます。 
   ![ADSI Edit @ no__t 
9. ダイアログで **[OK]** をクリックし、プロパティシートでを**適用**して**rIDAvailablePool**属性を更新します。 
  
### <a name="to-raise-the-value-of-available-rid-pools-using-ldp"></a>LDP を使用して使用可能な RID プールの値を上げるには  
  
1. コマンド プロンプトで、次のコマンドを入力して Enter キーを押します。  
   **ldp**  
2. **[接続]** をクリックし、 **[接続]** をクリックして、RID マネージャーの名前を入力し、 **[OK]** をクリックします。 
   ![LDP @ NO__T
3. **[接続]** をクリックし、 **[バインド]** をクリックして、 **[資格情報でバインド]** する を選択し、管理者の資格情報を入力して、 **[OK]** をクリック 
   ![LDP @ NO__T
4. **[表示]** をクリックし、 **[ツリー]** をクリックして、次の識別名パスを入力します。CN = RID Manager $、CN = System、DC =*domain name*  
   ![LDP @ NO__T
5. **[参照]** をクリックし、 **[変更]** をクリックします。 
6. 現在の**rIDAvailablePool**値に10万を追加し、合計を**値**に入力します。 
7. **[Dn]** に「`cn=RID Manager$,cn=System,dc=` *< ドメイン名 @ no__t-3*」と入力します。 
8. **[エントリ属性の編集]** に、「`rIDAvailablePool`」と入力します。 
9. 操作として **[置換]** を選択し、 **enter キー**を押します。
   ![LDP @ NO__T 
10. **[実行]** をクリックして操作を実行します。 **[閉じる]** をクリックします。
11. 変更を検証するには、 **[表示]** をクリックし、 **[ツリー]** をクリックして、次の識別名のパスを入力します。 CN = RID Manager $、CN = System、DC =*ドメイン名*。   **RIDAvailablePool**属性を確認します。 
   ![LDP @ NO__T

## <a name="next-steps"></a>次の手順

- [AD フォレストの回復ガイド](AD-Forest-Recovery-Guide.md)
- [AD フォレストの回復 - 手順](AD-Forest-Recovery-Procedures.md)
