---
title: 信頼された TPM ルート証明書をインストールする
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 06/27/2019
ms.openlocfilehash: 15614ce1065170bc557fad10a168b3dda6a5b05a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386550"
---
# <a name="install-trusted-tpm-root-certificates"></a>信頼された TPM ルート証明書をインストールする

>適用対象:Windows Server 2019、Windows Server (半期チャネル)、Windows Server 2016

TPM 構成証明を使用するように HGS を構成する場合は、サーバーの Tpm のベンダーを信頼するように HGS を構成する必要もあります。
この追加の検証プロセスにより、認証された信頼できる Tpm だけが HGS で証明できるようになります。
信頼されていない TPM を @no__t 0 に登録しようとすると、TPM ベンダが信頼されていないことを示すエラーが表示されます。

Tpm を信頼するには、サーバーの Tpm で保証キーの署名に使用されるルート証明書と中間署名証明書を HGS にインストールする必要があります。
データセンターで複数の TPM モデルを使用する場合は、モデルごとに異なる証明書をインストールすることが必要になる場合があります。
HGS は、ベンダー証明書の "TrustedTPM_RootCA" および "TrustedTPM_IntermediateCA" 証明書ストアを検索します。

> [!NOTE]
> TPM ベンダー証明書は、Windows で既定でインストールされる証明書とは異なり、TPM ベンダーによって使用される特定のルート証明書と中間証明書を表します。

信頼できる TPM ルート証明書と中間証明書のコレクションは、お客様の便宜を目的として Microsoft によって公開されています。
これらの証明書をインストールするには、次の手順を実行します。
TPM 証明書が以下のパッケージに含まれていない場合は、TPM ベンダーまたはサーバー OEM に問い合わせて、特定の TPM モデルのルート証明書と中間証明書を取得します。

**すべての HGS サーバー**で、次の手順を繰り返します。

1.  [@No__t-1](https://go.microsoft.com/fwlink/?linkid=2097925)から最新のパッケージをダウンロードします。

2.  Cab ファイルの信頼性を確認するために、その署名を確認します。 署名が有効でない場合は、続行しないでください。

    ```powershell
    Get-AuthenticodeSignature .\TrustedTpm.cab
    ```
    
    次に出力の例を示します。
    
    ```
    Directory: C:\Users\Administrator\Downloads
        
    SignerCertificate                         Status                                 Path
    -----------------                         ------                                 ----
    0DD6D4D4F46C0C7C2671962C4D361D607E370940  Valid                                  TrustedTpm.cab
    ```

2.  Cab ファイルを展開します。

    ```
    mkdir .\TrustedTPM
    expand.exe -F:* <Path-To-TrustedTpm.cab> .\TrustedTPM
    ```

3.  既定では、構成スクリプトによって、TPM ベンダーごとに証明書がインストールされます。 特定の TPM ベンダーの証明書のみをインポートする場合は、組織によって信頼されていない TPM ベンダーのフォルダーを削除します。

4.  展開されたフォルダーでセットアップスクリプトを実行して、信頼された証明書パッケージをインストールします。

    ```
    cd .\TrustedTPM
    .\setup.cmd
    ```

以前のインストール時に、新しい証明書または意図的にスキップされた証明書を追加するには、HGS クラスター内のすべてのノードで上記の手順を繰り返します。
既存の証明書は信頼されたままですが、拡張された cab ファイルで見つかった新しい証明書は、信頼された TPM ストアに追加されます。

## <a name="next-step"></a>次の手順

> [!div class="nextstepaction"]
> [ファブリック DNS の構成](guarded-fabric-configuring-fabric-dns-tpm.md)



