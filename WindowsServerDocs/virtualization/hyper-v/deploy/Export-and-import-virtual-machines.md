---
title: 仮想マシンをエクスポートおよびインポートする
description: Hyper-v マネージャーまたは Windows PowerShell を使用して仮想マシンをエクスポートおよびインポートする方法について説明します。
ms.prod: windows-server
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 6e130ee8a040cd5b56908d77d91bf196a60de6f7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392977"
---
>適用先:Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>仮想マシンのエクスポートとインポート

この記事では、仮想マシンをエクスポートおよびインポートする方法について説明します。仮想マシンは、簡単に移動またはコピーすることができます。 この記事では、エクスポートまたはインポートを行うときの選択方法についても説明します。

## <a name="export-a-virtual-machine"></a>仮想マシンのエクスポート

エクスポートは、すべての必要なファイルを1つのユニット (バーチャルハードディスクファイル、バーチャルマシン構成ファイル、およびチェックポイントファイル) にまとめて収集します。 この操作は、開始または停止のいずれかの状態の仮想マシンで実行できます。

### <a name="using-hyper-v-manager"></a>Hyper-V マネージャーの使用

仮想マシンのエクスポートを作成するには

1. HYPER-V マネージャーでは、仮想マシンを右クリックし、選択 **エクスポート**です。

2. エクスポートされたファイルを保存する場所を選択し、 **[エクスポート]** をクリックします。

エクスポートが完了すると、エクスポート場所にエクスポートされたすべてのファイルが表示されます。

### <a name="using-powershell"></a>PowerShell を使用する

管理者としてセッションを開き、次のようなコマンドを実行します。 \<vm name @ no__t-1 と \<path @ no__t:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

詳細については、「 [Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm)」を参照してください。

## <a name="import-a-virtual-machine"></a>仮想マシンのインポート 

仮想マシンをインポートすると、仮想マシンが Hyper-V ホストに登録されます。 ホストまたは新しいホストにインポートし直すことができます。 同じホストにインポートしている場合は、Hyper-v が使用可能なファイルから仮想マシンを再作成しようとするため、最初に仮想マシンをエクスポートする必要はありません。 仮想マシンをインポートすると、Hyper-v ホストで使用できるように登録されます。

バーチャルマシンのインポートウィザードでは、あるホストから別のホストに移動するときに存在する可能性のある非互換性を修正することもできます。 これは、通常、メモリ、仮想スイッチ、仮想プロセッサなどの物理ハードウェアで異なります。

### <a name="import-using-hyper-v-manager"></a>Hyper-v マネージャーを使用したインポート

仮想マシンをインポートするには:

1. Hyper-v マネージャーの **[アクション]** メニューで、 **[仮想マシンのインポート]** をクリックします。

2. **[次へ]** をクリックします。

3. エクスポートしたファイルが含まれているフォルダーを選択し、 **[次へ]** をクリックします。

4. インポートする仮想マシンを選択します。

5. インポートの種類を選択し、 **[次へ]** をクリックします。 (詳細については、以下の「[型のインポート](#import-types)」を参照してください)。

6. **[完了]** をクリックします。

### <a name="import-using-powershell"></a>PowerShell を使用したインポート

使用するインポートの種類の例に従って、 **VM のインポート**コマンドレットを使用します。 型の詳細については、以下の「[型のインポート](#import-types)」を参照してください。 

#### <a name="register-in-place"></a>インプレース登録

この種類のインポートでは、インポート時に保存されるファイルが使用され、仮想マシンの ID が保持されます。 次のコマンドは、インポートファイルの例を示しています。 独自の値を使用して同様のコマンドを実行します。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>[復元]

仮想マシンファイルの独自のパスを指定して仮想マシンをインポートするには、次のようなコマンドを実行します。例は実際の値に置き換えてください。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>コピーとしてインポート

コピーインポートを完了し、仮想マシンファイルを既定の Hyper-v の場所に移動するには、次のようなコマンドを実行します。例は実際の値に置き換えてください。

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

詳細については、「[インポート-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm)」を参照してください。

### <a name="import-types"></a>インポートの種類

Hyper-v には、次の3種類のインポートが用意されています。

- **インプレース登録**: このタイプは、仮想マシンを格納して実行する場所にエクスポートファイルがあることを前提としています。 インポートされた仮想マシンには、エクスポート時と同じ ID が付けられます。 このため、仮想マシンが既に Hyper-v に登録されている場合は、インポートが機能する前に、仮想マシンを削除する必要があります。 インポートが完了すると、エクスポートファイルは実行中の状態ファイルになり、削除することはできません。

- **仮想マシンを復元**する–選択した場所に仮想マシンを復元するか、既定の [hyper-v] を使用します。 このインポートの種類では、エクスポートされたファイルのコピーを作成し、選択した場所に移動します。 インポートすると、仮想マシンはエクスポート時と同じ ID を持ちます。 このため、仮想マシンが既に Hyper-v で実行されている場合、インポートを完了するには、その仮想マシンを削除する必要があります。 インポートが完了すると、エクスポートされたファイルはそのまま残り、削除または再度インポートできます。

- **仮想マシンをコピー**します。これは、ファイルの場所を選択するという復元の種類に似ています。 違いは、インポートされた仮想マシンには新しい一意の ID があることです。つまり、仮想マシンを同じホストに複数回インポートできます。

