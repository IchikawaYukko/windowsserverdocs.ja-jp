---
title: 手順 3 OTP 証明書の展開を計画する
description: このトピックは、「Windows Server 2016 で OTP 認証を使用してリモートアクセスを展開する」の一部です。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8406286599e5b03173ce1b5d6c34c35245a9094
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366950"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>手順 3 OTP 証明書の展開を計画する

>適用先:Windows Server (半期チャネル)、Windows Server 2016

RADIUS サーバーを計画した後、ワンタイムパスワード (OTP) 証明書を発行する CA、OTP 証明書テンプレート、およびリモートで使用される登録機関証明書を含む証明機関 (CA) の要件を計画する必要があります。すべての DirectAccess クライアント OTP 証明書要求に署名するためのアクセスサーバー。 これらの証明書は次のように使用されます。  
  
1.  DirectAccess クライアントは OTP 証明書を要求し、リモートアクセスサーバーは要求を受信します。  
  
2.  リモートアクセスサーバーは OTP 資格情報を確認し、有効である場合、サーバーは登録機関として機能し、有効期間の短い署名証明書を使用して OTP 証明書の登録要求に署名します。  
  
3.  リモートアクセスサーバーは、署名された証明書の登録要求を DirectAccess クライアントに送り返します。  
  
4.  次に、クライアントは、サーバーによって署名された証明書の登録要求を使用して、CA から OTP 証明書を登録します。  
  
5.  CA は、資格情報と要求を検証します。  
  
|タスク|説明|  
|----|--------|  
|[3.1 OTP CA を計画する](#bkmk_3_1_CA)|OTP 認証用の DirectAccess クライアントに証明書を発行するために使用する証明機関 (CA) を計画します。|  
|[3.2 OTP 証明書テンプレートを計画する](#bkmk_3_2_OTP_Cert)|OTP 証明書テンプレートを計画します。|
|[3.3 登録機関の証明書を計画する](#bkmk_33RACert)|すべての OTP 認証証明書要求に署名するための登録機関の証明書を計画します。|

## <a name="bkmk_3_1_CA"></a>3.1 OTP CA を計画する  
ワンタイムパスワード認証 (OTP) を使用して DirectAccess を展開するには、DirectAccess クライアントコンピューターに OTP 認証証明書を発行するための内部 CA が必要です。 このため、通常の IPsec コンピューター認証に使用される証明書を発行するために使用するのと同じ内部 CA を使用できます。  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 OTP 証明書テンプレートを計画する  
各 DirectAccess クライアントは、内部ネットワークへのアクセスを取得するために OTP 認証証明書を必要とします。 OTP 証明書の内部 CA でテンプレートを構成する必要があります。 OTP 証明書テンプレートを構成するときは、次の点に注意してください。  
  
-   OTP 認証を実行する必要があるすべてのユーザーは、このテンプレートの読み取りと登録のアクセス許可を持っている必要があります。  
  
-   サブジェクト名は、証明書の要求を実行するリモートアクセスサーバーの名前ではなく、OTP のユーザー名と一致するように、Active Directory 情報から構築する必要があります。 サブジェクト名は完全に識別される名前の形式にする必要があり、サブジェクトの別名は UPN 形式である必要があります。 これにより、登録されている OTP 証明書がスマートカード Kerberos 認証に対して有効になります。  
  
-   証明書の目的は、スマートカードログオンである必要があります。  
  
-   発行には、1つの承認された署名が必要です。 署名は、登録機関の署名証明書テンプレートで設定された定義済みの DirectAccess OTP アプリケーションポリシーを使用して構成する必要があります。  
  
-   有効期間は1時間に設定する必要があります。  
  
    > [!NOTE]  
    > CA サーバーが Windows Server 2003 コンピューターの場合は、テンプレートを別のコンピューターで構成する必要があります。 これは、2008/Vista より前のバージョンの Windows を実行している場合は、**有効期間**を時間単位で設定することができないためです。 テンプレートの構成に使用するコンピューターに、証明書サービスの役割がインストールされていない場合、またはクライアントコンピューターである場合は、証明書テンプレートスナップインのインストールが必要になることがあります。 このトピックの詳細について[は、ここ](https://technet.microsoft.com/library/cc732445.aspx)をクリックしてください。  
  
-   更新期間は0に設定する必要があります。  
  
-   Optional証明書と要求を CA データベースに格納することはできません。  
  
-   証明書の拡張キー使用法のパラメーターは、次のように正しく設定する必要があります。  
  
    -   DirectAccess 登録署名証明書テンプレートには、キー1.3.6.1.4.1.311.81.1.1 を使用します。  
  
    -   OTP 認証証明書テンプレートには、キー1.3.6.1.4.1.311.20.2.2 キーを使用します。  
  
## <a name="bkmk_33RACert"></a>3.3 登録機関の証明書を計画する  
DirectAccess クライアントが OTP 証明書を要求すると、リモートアクセスサーバーはクライアントから要求を受信します。 リモートアクセスサーバーは、登録機関の証明書を使用して、クライアントからのすべての OTP 証明書要求に署名します。 CA は、要求がリモートアクセスサーバーの登録機関の証明書によって署名されている場合にのみ、証明書を発行します。 証明書は、内部 CA によって発行されている必要があります。証明書を自己署名することはできません。 OTP 証明書を発行した CA によって発行される必要はありませんが、OTP 証明書を発行する CA は、登録機関署名証明書を発行する CA を信頼する必要があります。  
  
## <a name="BKMK_Links"></a>関連項目  
  
-   [手順 4:リモートアクセスサーバーの OTP を計画する @ no__t-0  
  


