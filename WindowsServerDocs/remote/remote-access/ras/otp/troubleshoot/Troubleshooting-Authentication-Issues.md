---
title: 認証の問題のトラブルシューティング
description: このトピックは、「Windows Server 2016 で OTP 認証を使用してリモートアクセスを展開する」の一部です。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73fe8458910cbe7dfaf000a6546bcba9263a9683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404311"
---
# <a name="troubleshooting-authentication-issues"></a>認証の問題のトラブルシューティング

>適用先:Windows Server (半期チャネル)、Windows Server 2016

このトピックでは、ユーザーが OTP 認証を使用して DirectAccess に接続しようとしたときに発生する可能性のある問題に関するトラブルシューティング情報について説明します。 DirectAccerss OTP 関連のイベントは、イベントビューアーの [**アプリケーションとサービスログ]/[Microsoft]/[Windows**]/[OtpCredentialProvider] の下にあるクライアントコンピューターに記録されます。 DirectAccess OTP での問題のトラブルシューティングを行うときに、このログが有効になっていることを確認してください。  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>OTP 証明書を発行する CA にアクセスできませんでした  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 ユーザー <username> の OTP 証明書の登録は、CA サーバー < CA_name > で失敗しました。要求は失敗しました。エラーの原因として考えられる原因:Ca サーバー名を解決できません。最初の DirectAccess トンネルを介して CA サーバーにアクセスできないか、CA サーバーへの接続を確立できません。  
  
**原因**  
  
ユーザーが有効な1回限りのパスワードを指定し、DirectAccess サーバーが証明書の要求に署名しました。ただし、クライアントコンピューターは、OTP 証明書を発行する CA に接続して、登録プロセスを完了することはできません。  
  
**ソリューション**  
  
DirectAccess サーバーで、次の Windows PowerShell コマンドを実行します。  
  
1.  構成されている OTP 発行 Ca の一覧を取得し、' CAServer 値を確認してください: `Get-DAOtpAuthentication`  
  
2.  Ca が管理サーバーとして構成されていることを確認してください。 `Get-DAMgmtServer -Type All`  
  
3.  クライアントコンピューターによってインフラストラクチャトンネルが確立されていることを確認します。セキュリティが強化された Windows ファイアウォール コンソールで、監視、**セキュリティアソシエーション** の順に展開し、**メインモード** をクリックして、DirectAccess の正しいリモートアドレスと共に IPsec のセキュリティアソシエーションが表示されることを確認します。configuration.  
  
## <a name="directaccess-server-connectivity-issues"></a>DirectAccess サーバーの接続に関する問題  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーの受信**(クライアントイベントログ)  
  
次のエラーのいずれかです。  
  
-   ベースパス < OTP_authentication_path > およびポート < OTP_authentication_port > を使用して、リモートアクセスサーバー < DirectAccess_server_hostname > に対する接続を確立できません。 エラーコード: < internal_error_code >。  
  
-   基本パス < OTP_authentication_path > およびポート < OTP_authentication_port > を使用して、リモートアクセスサーバー < DirectAccess_server_hostname > にユーザーの資格情報を送信することはできません。 エラーコード: < internal_error_code >。  
  
-   ベースパス < OTP_authentication_path > およびポート < OTP_authentication_port > を使用して、リモートアクセスサーバー < DirectAccess_server_hostname > から応答を受信しませんでした。 エラーコード: < internal_error_code >。  
  
**原因**  
  
クライアントコンピューターがインターネット経由で DirectAccess サーバーにアクセスできません。ネットワークの問題が発生しているか、DirectAccess サーバーで IIS サーバーが正しく構成されていないことが原因です。  
  
**ソリューション**  
  
クライアントコンピューターのインターネット接続が機能していることを確認し、DirectAccess サービスが実行されていて、インターネット経由でアクセス可能であることを確認してください。  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>DirectAccess OTP ログオン証明書の登録に失敗しました  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 CA < CA_name > からの証明書の登録に失敗しました。 OTP 署名証明書によって要求が正常に署名されなかったか、ユーザーに登録するためのアクセス許可がありません。  
  
**原因**  
  
ユーザーが指定したワンタイムパスワードは正しいものの、発行元の証明機関 (CA) が OTP ログオン証明書の発行を拒否しました。 証明書の要求が正しい EKU (OTP 登録機関のアプリケーションポリシー) で適切に署名されていないか、ユーザーが DA OTP テンプレートに対する "登録" アクセス許可を持っていない可能性があります。  
  
**ソリューション**  
  
Directaccess otp ユーザーが DirectAccess OTP ログオン証明書に登録するアクセス許可を持っていること、および適切な "アプリケーションポリシー" が DA OTP 登録機関の署名テンプレートに含まれていることを確認します。 また、リモートアクセスサーバーの DirectAccess 登録機関の証明書が有効であることを確認します。 「3.2 OTP 証明書テンプレートを計画する」および「3.3 登録機関証明書を計画する」を参照してください。  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>コンピューターアカウント証明書が見つからないか無効です  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。  Otp に必要なコンピューター証明書がローカルコンピューターの証明書ストアに見つからないため、OTP 認証を完了できません。  
  
**原因**  
  
DirectAccess OTP 認証には、DirectAccess サーバーとの SSL 接続を確立するためにクライアントコンピューター証明書が必要です。ただし、証明書の有効期限が切れた場合など、クライアントコンピューターの証明書が見つからないか、有効ではありません。  
  
**ソリューション**  
  
コンピューター証明書が存在し、有効であることを確認します。  
  
1.  クライアントコンピューターの MMC 証明書コンソールで、ローカルコンピューターアカウントの **[個人/証明書]** を開きます。  
  
2.  コンピューター名と一致する証明書が発行されていることを確認し、証明書をダブルクリックします。  
  
3.  [証明**書] ダイアログボックス**の [証明**書のパス**] タブで、[証明書の**状態**] の下に "この証明書は OK です。" と表示されていることを確認します。  
  
有効な証明書が見つからない場合は、管理者特権でのコマンドプロンプトから `gpupdate /Force` を実行するか、クライアントコンピューターを再起動して、無効な証明書 (存在する場合) を削除し、コンピューター証明書を再登録します。  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>OTP 証明書を発行する CA が見つかりません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 DA サーバーが発行元の CA のアドレスを返さなかったため、OTP 認証を完了できません。  
  
**原因**  
  
OTP 証明書を発行する Ca が構成されていないか、OTP 証明書を発行する構成済みのすべての Ca が応答していません。  
  
**ソリューション**  
  
1.  次のコマンドを使用して、OTP 証明書を発行する Ca の一覧を取得します (CA 名は CAServer 表示されています): `Get-DAOtpAuthentication`。  
  
2.  Ca が構成されていない場合:  
  
    1.  コマンド `Set-DAOtpAuthentication` またはリモートアクセス管理コンソールのいずれかを使用して、DirectAccess OTP ログオン証明書を発行する Ca を構成します。  
  
    2.  新しい構成を適用し、管理者特権でのコマンドプロンプトから `gpupdate /Force` を実行するか、クライアントコンピューターを再起動して、クライアントが DirectAccess の GPO 設定を強制的に更新するようにします。  
  
3.  Ca が構成されている場合は、それらがオンラインであり、登録要求に応答していることを確認します。  
  
## <a name="misconfigured-directaccess-server-address"></a>DirectAccess サーバーアドレスの構成が正しくありません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 OTP 認証を正常に完了できません。 リモートアクセスサーバーの名前またはアドレスを特定できません。  エラーコード: < error_code >。 DirectAccess の設定は、サーバー管理者が検証する必要があります。  
  
**原因**  
  
DirectAccess サーバーのアドレスが正しく構成されていません。  
  
**ソリューション**  
  
@No__t-0 を使用して構成された DirectAccess サーバーのアドレスを確認し、正しく構成されていない場合はアドレスを修正します。  
  
管理者特権でのコマンドプロンプトから `gpupdate /force` を実行し、クライアントコンピューターを再起動して、クライアントコンピューターに最新の設定が展開されていることを確認します。  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>OTP ログオン証明書要求を生成できませんでした  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 OTP 認証の証明書要求を初期化できません。 秘密キーを生成できないか、またはユーザー <username> がドメインコントローラーの証明書テンプレート < OTP_template_name > にアクセスできません。  
  
**原因**  
  
このエラーには、次の2つの原因が考えられます。  
  
-   ユーザーには OTP ログオンテンプレートを読み取るためのアクセス許可がありません。  
  
-   ネットワークの問題のため、ユーザーのコンピューターがドメインコントローラーにアクセスできません。  
  
**ソリューション**  
  
-   OTP ログオンテンプレートでアクセス許可の設定を確認し、DirectAccess OTP に対してプロビジョニングされたすべてのユーザーが ' 読み取り ' アクセス許可を持っていることを確認してください。  
  
-   ドメインコントローラが管理サーバーとして構成されていること、およびクライアントコンピュータがインフラストラクチャトンネルを介してドメインコントローラに接続できることを確認します。 「3.2 OTP 証明書テンプレートを計画する」を参照してください。  
  
## <a name="no-connection-to-the-domain-controller"></a>ドメインコントローラーへの接続がありません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 OTP 認証用のドメインコントローラーとの接続を確立できません。 エラーコード: < error_code >。  
  
**原因**  
  
このエラーには、次の2つの原因が考えられます。  
  
-   ユーザーのコンピューターにネットワーク接続がありません。  
  
-   ドメインコントローラは、インフラストラクチャトンネルを介してアクセスできません。  
  
**ソリューション**  
  
-   PowerShell プロンプトから次のコマンドを実行して、ドメインコントローラーが管理サーバーとして構成されていることを確認してください。 `Get-DAMgmtServer -Type All`。  
  
-   クライアントコンピューターが、インフラストラクチャトンネルを介してドメインコントローラーに接続できることを確認します。  
  
## <a name="otp-provider-requires-challengeresponse"></a>OTP プロバイダーにはチャレンジ/レスポンスが必要です  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 ユーザー (<username>) のリモートアクセスサーバー (< DirectAccess_server_name >) による OTP 認証には、ユーザーからのチャレンジが必要でした。  
  
**原因**  
  
使用される OTP プロバイダーでは、ユーザーは Windows Server 2012 DirectAccess OTP でサポートされていない RADIUS チャレンジ/応答交換の形式で追加の資格情報を提供する必要があります。  
  
**ソリューション**  
  
どのシナリオでも、チャレンジ/レスポンスを必要としないように OTP プロバイダーを構成します。  
  
## <a name="incorrect-otp-logon-template-used"></a>無効な OTP ログオンテンプレートが使用されています  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 ユーザー <username> が証明書を要求した CA テンプレートは、OTP 証明書を発行するように構成されていません。  
  
**原因**  
  
DirectAccess OTP ログオンテンプレートが置き換えられ、クライアントコンピューターが古いテンプレートを使用して認証しようとしています。  
  
**ソリューション**  
  
次のいずれかを実行して、クライアントコンピューターが最新の OTP 構成を使用していることを確認します。  
  
-   管理者特権でのコマンドプロンプトから次のコマンドを実行してグループポリシー更新を強制します: `gpupdate /Force`。  
  
-   クライアント コンピューターを再起動します。  
  
## <a name="missing-otp-signing-certificate"></a>OTP 署名証明書がありません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントイベントログ)。 OTP 署名証明書が見つかりません。 OTP 証明書の登録要求に署名できません。  
  
**原因**  
  
DirectAccess OTP 署名証明書がリモートアクセスサーバーに見つかりません。そのため、リモートアクセスサーバーがユーザー証明書の要求に署名することはできません。 署名証明書がないか、署名証明書の有効期限が切れており、更新されていません。  
  
**ソリューション**  
  
リモートアクセスサーバーで次の手順を実行します。  
  
1.  PowerShell コマンドレットを実行して、構成されている OTP 署名証明書テンプレート名を確認し、`SigningCertificateTemplateName` の値を調べ @no__t ます。  
  
2.  このテンプレートに登録されている有効な証明書がコンピューターに存在することを確認するには、証明書 MMC スナップインを使用します。  
  
3.  そのような証明書が存在しない場合は、期限切れの証明書を削除し (存在する場合)、このテンプレートに基づいて新しい証明書を登録します。  
  
OTP 署名証明書テンプレートを作成するには、「3.3 登録機関の証明書を計画する」を参照してください。  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>ユーザーの UPN/DN が見つからないか、正しくありません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーの受信**(クライアントイベントログ)  
  
次のエラーのいずれかです。  
  
-   ユーザー <username> は OTP で認証できません。 Active Directory のユーザー名に UPN が定義されていることを確認します。 エラーコード: < error_code >。  
  
-   ユーザー <username> は OTP で認証できません。 Active Directory で、ユーザー名に対して DN が定義されていることを確認します。 エラーコード: < error_code >。  
  
**受信したエラー** (サーバーイベントログ)  
  
OTP 認証に指定されたユーザー名 @no__t 0 は存在しません。  
  
**原因**  
  
ユーザーアカウントでユーザープリンシパル名 (UPN) または識別名 (DN) の属性が適切に設定されていません。これらのプロパティは、DirectAccess OTP を適切に機能させるために必要です。  
  
**ソリューション**  
  
ドメインコントローラーの Active Directory ユーザーとコンピューター コンソールを使用して、認証ユーザーにこれらの属性が両方とも正しく設定されていることを確認します。  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>OTP 証明書がログインに対して信頼されていません  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**原因**  
  
OTP 証明書を発行する CA は、エンタープライズ NTAuth ストアに含まれていません。そのため、登録済みの証明書をログオンに使用することはできません。 これは、クロスドメイン CA 信頼が確立されていないマルチドメイン環境やマルチフォレスト環境で発生する可能性があります。  
  
**ソリューション**  
  
OTP 証明書を発行する CA 階層のルートの証明書が、ユーザーが認証しようとしているドメインのエンタープライズ NTAuth 証明書ストアにインストールされていることを確認します。  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows はユーザーの資格情報を確認できませんでした  
**シナリオ**。 次のエラーで OTP を使用してユーザーを認証できませんでした:"内部エラーのため認証に失敗しました"  
  
**エラーを受信しました**(クライアントコンピューター)。 Windows が資格情報を確認しているときに問題が発生しました。 もう一度やり直すか、管理者にお問い合わせください。  
  
**原因**  
  
DirectAccess OTP ログオン証明書に CRL が含まれていない場合、Kerberos 認証プロトコルは機能しません。 DirectAccess OTP ログオン証明書には、次のいずれかの理由で CRL が含まれていません。  
  
-   DirectAccess OTP ログオンテンプレートは、[発行された**証明書に失効情報を含めない**] オプションを使用して構成されています。  
  
-   CA は Crl を公開しないように構成されています。  
  
**ソリューション**  
  
1.  このエラーの原因を確認するには、リモートアクセス管理コンソールの **[手順2リモートアクセスサーバー]** で、 **[編集]** をクリックし、**リモートアクセスサーバーのセットアップ**ウィザードで **[OTP 証明書テンプレート]** をクリックします。 OTP 認証用に発行された証明書の登録に使用される証明書テンプレートをメモしておきます。 証明機関コンソールを開き、左側のウィンドウで **[証明書テンプレート]** をクリックし、OTP ログオン証明書をダブルクリックして、証明書テンプレートのプロパティを表示します。  
  
    この問題を解決するには、テンプレートのプロパティ ダイアログボックスの **サーバー** タブで、OTP ログオン証明書の証明書を構成し、**発行された証明書に失効情報を含め**ない チェックボックスをオンにしないでください。  
  
2.  CA サーバーで、証明機関 MMC を開き、発行元の CA を右クリックして、**プロパティ** をクリックします。 **[拡張]** タブで、CRL の発行が正しく構成されていることを確認します。  
  


