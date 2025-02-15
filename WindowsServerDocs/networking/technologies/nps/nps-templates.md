---
title: NPS テンプレート
description: このトピックでは、Windows Server 2016 のネットワークポリシーサーバーテンプレートの概要について説明します。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bafff7a6a15312ab1bdca2e7b98307bc94731d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405314"
---
# <a name="nps-templates"></a>NPS テンプレート

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

ネットワークポリシーサーバー \(NPS @ no__t テンプレートを使用すると、リモート認証ダイヤルインユーザーサービス \(RADIUS @ no__t クライアントや共有シークレットなどの構成要素を作成できます。これは、ローカルの NPS で再利用したり、他のユーザーが使用するためにエクスポートしたりすることができます。NPSs。

NPS テンプレートは、1つまたは複数のサーバーで NPS を構成するために必要な時間とコストを削減するように設計されています。 テンプレート管理では、次の種類の NPS テンプレートを構成できます。

- 共有シークレット
- RADIUS クライアント
- リモート RADIUS サーバー
- IP フィルター
- 修復サーバーグループ

テンプレートの構成は、NPS を直接構成することとは異なります。 テンプレートを作成しても、NPS の機能には影響しません。 これは、nps コンソールの適切な場所でテンプレートを選択した場合にのみ、テンプレートが NPS の機能に影響を与えることになります。 

たとえば、NPS コンソールの [RADIUS クライアントとサーバー] で RADIUS クライアントを構成した場合、NPS の構成を変更し、ネットワークアクセス @no__t サーバーのいずれかと通信するように NPS を構成する1つの手順を実行します (no__t-1)。 @no__t、次の手順では、NPS と通信するように NAS を構成します。 \)ただし、 **Radius クライアントとサーバー**の下に新しい radius クライアントを作成するのではなく、 **[テンプレートの管理]** の下にある NPS コンソールで新しい radius クライアントテンプレートを構成した場合、テンプレートは作成されていますが、nps を変更していません。機能はまだありません。 NPS の機能を変更するには、NPS コンソールで正しい場所からテンプレートを選択する必要があります。

## <a name="creating-templates"></a>テンプレートの作成

テンプレートを作成するには、NPS コンソールを開き、 **[IP フィルター]** などのテンプレートの種類を右クリックして、 **[新規]** をクリックします。 [新しいテンプレートのプロパティ] ダイアログボックスが開き、テンプレートを構成できます。

## <a name="using-templates-locally"></a>ローカルでのテンプレートの使用

テンプレートを適用できる NPS コンソール内の場所に移動することで、テンプレート**管理**で作成したテンプレートを使用できます。 たとえば、radius クライアントの構成に適用する新しい共有シークレットテンプレートを作成する場合、radius クライアント**とサーバー**および**radius クライアント**では、radius クライアントのプロパティを開きます。 **[既存の共有シークレットを選択し**てください] テンプレートで、使用可能なテンプレートの一覧から以前に作成したテンプレートを選択します。

NPS の詳細については、「[ネットワークポリシーサーバー (nps)](nps-top.md)」を参照してください。
