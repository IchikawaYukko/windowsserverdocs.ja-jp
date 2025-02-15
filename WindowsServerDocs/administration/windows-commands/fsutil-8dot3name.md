---
ms.assetid: a0c6dbfe-d898-496d-9356-825f7fbd90ec
title: Fsutil 8dot3name
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: b0261daf230cbf93951a9c111fdbf5aafefb061b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377055"
---
# <a name="fsutil-8dot3name"></a>Fsutil 8dot3name

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

短い名前 (8dot3 名) の動作の設定を照会または変更します。次に例を示します。

-   短い名前の動作について現在の設定を照会します

-   指定されたディレクトリパスから短い名前が削除された場合に影響を受ける可能性のあるレジストリキーを指定して、指定したディレクトリパスをスキャンします

-   短い名前の動作を制御する設定を変更します。 この設定は、指定したボリュームまたは既定のボリューム設定に適用できます。

-   ディレクトリ内のすべてのファイルの短い名前を削除します。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
fsutil 8dot3name [query] [<VolumePath>]
fsutil 8dot3name [scan] [/s] [/l [<log file>] ] [/v] <DirectoryPath>
fsutil 8dot3name [set] { <DefaultValue> | <VolumePath> {1|0}}
fsutil 8dot3name [strip] [/t] [/s] [/f] [/l [<log file.] ] [/v] <DirectoryPath>
```

## <a name="parameters"></a>パラメーター

|                 パラメーター                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    説明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           クエリ [<VolumePath>]            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                           ファイルシステムに対して8dot3 短い名前の作成動作の状態を照会します。<br /><br />*Volumepath*がパラメーターとして指定されていない場合は、すべてのボリュームに対して既定の8dot3name 作成動作設定が表示されます。                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|           @no__t をスキャンする-0            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        8dot3 短い名前がファイル名から削除された場合に影響を受ける可能性があるレジストリキーについて、指定した*DirectoryPath*にあるファイルをスキャンします。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| 設定 {<DefaultValue> &#124; <VolumePath>} | 次のインスタンスで8dot3 名を作成するときのファイルシステムの動作を変更します。<br /><br /><ul><li>*Defaultvalue*が指定されている場合、レジストリキー **HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreationNtfsDisable8dot3NameCreation**は defaultvalue に設定されます。.<br /><br />    *DefaultValue*には、次の値を指定できます。<br /><br /><ul><li>**0**:システム上のすべてのボリュームに対して8dot3 名の作成を有効にします。</li><li>**1**:システム上のすべてのボリュームに対して8dot3 名の作成を無効にします。</li><li>**2**:8dot3 名の作成をボリュームごとに設定します。</li><li>**3**:システムボリューム以外のすべてのボリュームに対して8dot3 名の作成を無効にします。</li></ul></li><li>*Volumepath*を指定すると、ディスクフラグ8dot3name プロパティに指定されたボリュームが、指定されたボリューム (**0**) に対して8dot3 名の作成を有効にするように設定されるか、指定されたボリューム (**1**) で8dot3 名の作成を無効にするように設定されます。<br /><br />    指定されたボリュームに対して8dot3 名の作成を有効または無効にするには、8dot3 名の作成の既定のファイルシステムの動作を値**2**に設定する必要があります。</li></ul> |
|           ストリップ <DirectoryPath>           |                                                                                                                                                                                                                                                                                                                  指定した*DirectoryPath*にあるすべてのファイルの8dot3 ファイル名を削除します。 *DirectoryPath*のファイル名が260文字を超える場合、8dot3 ファイル名は削除されません。<br /><br />このコマンドは、8dot3 ファイル名が完全に削除されたファイルを指すレジストリキーを一覧表示しますが、変更しません。<br /><br />ファイルから8dot3 ファイル名を完全に削除した場合の影響の詳細については、「[解説](Fsutil-8dot3name.md#BKMK_remarks)」を参照してください。                                                                                                                                                                                                                                                                                                                  |
|               <VolumePath>                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       ドライブ名の後にコロン、または**ボリューム {** <em>guid</em> **}** の形式の GUID を指定します。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                    /f                     |                                                                                                                                                                                                                                                                                                   8dot3 ファイル名を使用してファイルを指すレジストリキーがある場合でも、指定した*DirectoryPath*にあるすべてのファイルに8dot3 ファイル名が削除されるように指定します。 この場合、この操作によって8dot3 ファイル名が削除されますが、8dot3 ファイル名を使用しているファイルを指すレジストリキーは変更されません。 **警告:**  **/F**パラメーターを使用する前に、ディレクトリまたはボリュームをバックアップすることをお勧めします。これは、プログラムをアンインストールできないなど、予期しないアプリケーションエラーが発生する可能性があるためです。                                                                                                                                                                                                                                                                                                    |
|              /l [<log file>]              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                  情報を書き込むログファイルを指定します。<br /><br />**/L**パラメーターが指定されていない場合は、すべての情報が既定のログファイルに書き込まれます。<br /><br />**%temp%\8dot3_removal_log @ (** <em>GMT YYYY-MM-DD HH-mm-SS</em> **) .log**                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                    /s                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      指定した*DirectoryPath*のサブディレクトリに操作を適用する必要があることを指定します。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|                    /t                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                          8dot3 ファイル名の削除をテストモードで実行するように指定します。 8dot3 ファイル名の実際の削除を除くすべての操作が実行されます。 テストモードを使用すると、8dot3 ファイル名を使用するファイルを指すレジストリキーを見つけることができます。                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                    /v                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       ログファイルに書き込まれたすべての情報がコマンドラインにも表示されることを指定します。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |

## <a name="BKMK_remarks"></a>」

-   8dot3 ファイル名を完全に削除し、8dot3 ファイル名をポイントするレジストリキーを変更しないと、アプリケーションをアンインストールできないなど、予期しないアプリケーションエラーが発生する可能性があります。 8dot3 ファイル名を削除する前に、まずディレクトリまたはボリュームをバックアップすることをお勧めします。

## <a name="BKMK_examples"></a>例
GUID で指定されたディスクボリュームの8dot3 名の無効化動作を照会するには、次のように入力します。 {92884 2df-5a01-11e6f6e6963},、次のように入力します。

```
fsutil 8dot3name query Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

また、 **behavior**サブコマンドを使用して、8dot3 名の動作を照会することもできます。

*D:\MyData*ディレクトリとすべてのサブディレクトリにある8dot3 ファイル名を削除するには、次のように入力します。ログファイルには、 *mylogfile .log*として指定します。

```
fsutil 8dot3name strip /l mylogfile.log /s d:\MyData
```

### <a name="additional-references"></a>その他の参照情報
[コマンド ライン構文の記号](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)

[Fsutil の動作](Fsutil-behavior.md)


