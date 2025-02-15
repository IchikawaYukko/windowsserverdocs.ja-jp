---
title: doskey
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4874fd43-d5ea-45f3-ae24-388ae925ed76
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d45a2ddfeba7ec136add07eac11c3a8522ef872b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377689"
---
# <a name="doskey"></a>doskey



は、Doskey (以前に入力したコマンドラインコマンドを再呼び出し)、コマンドラインの編集、およびマクロの作成を呼び出します。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
doskey [/reinstall] [/listsize=<Size>] [/macros:[all | <ExeName>] [/history] [/insert | /overstrike] [/exename=<ExeName>] [/macrofile=<FileName>] [<MacroName>=[<Text>]]
```

## <a name="parameters"></a>パラメーター

|       パラメーター        |                                                                                                                          説明                                                                                                                           |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /reinstall       |                                                                                            Doskey の新しいコピーをインストールし、コマンド履歴バッファーをクリアします。                                                                                            |
|   /listsize = \<Size >    |                                                                                                履歴バッファー内のコマンドの最大数を指定します。                                                                                                 |
|        /マクロ         |                                        すべての**doskey**マクロの一覧を表示します。 **/マクロ**でリダイレクトシンボル ( **>** ) を使用して、リストをファイルにリダイレクトすることができます。 **/マクロ**は、 **/m**に省略できます。                                         |
|      /マクロ: すべて       |                                                                                                        すべての実行可能ファイルの**doskey**マクロを表示します。                                                                                                         |
|   /マクロ: \<ExeName >   |                                                                                             *Exename*によって指定された実行可能ファイルの**doskey**マクロが表示されます。                                                                                              |
|        /history        |                                    メモリに格納されているすべてのコマンドを表示します。 **/History**でリダイレクトシンボル ( **>** ) を使用して、リストをファイルにリダイレクトすることができます。 **/History**は、 **/h**として省略できます。                                    |
| /挿入 | 入力した新しいテキストを古いテキストで挿入することを指定します。 |
| /overstrike | 新しいテキストが古いテキストを上書きすることを指定します。 |
|  /exename = \<ExeName >   |                                                                                        **Doskey**マクロを実行するプログラム (つまり、実行可能ファイル) を指定します。                                                                                         |
| /マクロファイル = \<FileName > |                                                                                              インストールするマクロを含むファイルを指定します。                                                                                               |
| \<MacroName > = [\< Text >]  | *テキスト*で指定されたコマンドを実行するマクロを作成します。 *MacroName*マクロに割り当てる名前を指定します。 *Text*記録するコマンドを指定します。 *テキスト*を空白のままにした場合、割り当てられたコマンドの*MacroName*はクリアされます。 |
|           /?           |                                                                                                              コマンド プロンプトにヘルプを表示します。                                                                                                              |

## <a name="remarks"></a>コメント

- Doskey の使用

  Doskey は、すべての文字ベースの対話型プログラム (プログラムデバッガーやファイル転送プログラムなど) で常に使用できます。また、起動する各プログラムに対して、コマンド履歴バッファーとマクロを保持します。 プログラムから**doskey**コマンドラインオプションを使用することはできません。 プログラムを開始する前に、 **doskey**コマンドラインオプションを実行する必要があります。 プログラムキーの割り当ては、 **doskey**キーの割り当てをオーバーライドします。
- コマンドの呼び出し

  コマンドを再現するには、Doskey を起動した後で、次のいずれかのキーを使用します。 プログラム内で Doskey を使用すると、そのプログラムのキー割り当てが優先されます。  

  |    キー     |                              説明                              |
  |------------|-----------------------------------------------------------------------|
  |  上矢印  |  表示される前に使用したコマンドを取り消します。  |
  | 下方向キー |  表示されたコマンドの後に使用したコマンドを取り消します。   |
  |  PageUp   |    現在のセッションで使用した最初のコマンドを呼び出します。    |
  | PageDown  | 現在のセッションで使用していた最新のコマンドを呼び出します。 |


- コマンドラインの編集

  Doskey を使用すると、現在のコマンドラインを編集できます。 プログラム内で Doskey を使用すると、そのプログラムのキー割り当てが優先され、一部の Doskey 編集キーが機能しない可能性があります。

  次の表に、 **doskey**の編集キーとその機能を示します。  

  | キーまたはキーの組み合わせ |                                                                                                                                                       説明                                                                                                                                                       |
  |------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |       左方向キー       |                                                                                                                                      挿入ポイントを1文字前に移動します。                                                                                                                                      |
  |      右矢印       |                                                                                                                                    挿入ポイントを1文字前に移動します。                                                                                                                                     |
  |    CTRL + ←     |                                                                                                                                        挿入ポイントを1ワード左に移動します。                                                                                                                                         |
  |    CTRL + →    |                                                                                                                                       挿入ポイントを1ワード前に移動します。                                                                                                                                       |
  |          Home          |                                                                                                                                 カーソル位置を行の先頭に移動します。                                                                                                                                 |
  |          END           |                                                                                                                                    カーソル位置を行の末尾に移動します。                                                                                                                                    |
  |          Esc           |                                                                                                                                          表示からコマンドをクリアします。                                                                                                                                           |
  |           F1           |                                                                      テンプレート内の列の1文字を、コマンドプロンプトウィンドウの同じ列にコピーします。 (テンプレートは、入力した最後のコマンドが格納されているメモリバッファーです)。                                                                       |
  |           F2           |                                                                 F2 キーを押した後に入力する次のキーを、テンプレート内で前方に検索します。 Doskey は、指定した文字までのテンプレートからテキストを挿入します。                                                                  |
  |           F3           |                                                 テンプレートの残りの部分をコマンドラインにコピーします。 Doskey は、コマンドラインの挿入ポイントによって示される位置に対応する、テンプレート内の位置から文字のコピーを開始します。                                                 |
  |           F4           |                                                                            F4 キーを押した後に入力した文字が次に出現する位置までの、現在の挿入位置からのすべての文字を削除します。                                                                            |
  |           F5           |                                                                                                                                   テンプレートを現在のコマンドラインにコピーします。                                                                                                                                    |
  |           F6           |                                                                                                                    現在の挿入ポイントの位置に、ファイルの終端文字 (CTRL + Z) を配置します。                                                                                                                    |
  |           F7           | メモリに格納されているこのプログラムのすべてのコマンドをダイアログボックスに表示します。 上方向キーと下方向キーを使用して目的のコマンドを選択し、ENTER キーを押してコマンドを実行します。 コマンドの前に並んでいる番号をメモし、この数値を F9 キーと組み合わせて使用することもできます。 |
  |         ALT + F7         |                                                                                                                          現在の履歴バッファーのメモリに格納されているすべてのコマンドを削除します。                                                                                                                          |
  |           F8           |                                                                                                           現在のコマンドの文字で始まる履歴バッファー内のすべてのコマンドを表示します。                                                                                                            |
  |           F9           |                                             履歴バッファーのコマンド番号の入力を求め、指定した番号に関連付けられたコマンドを表示します。 ENTER キーを押してコマンドを実行します。 すべての数値とそれに関連付けられたコマンドを表示するには、F7 キーを押します。                                             |
  |        ALT + F10         |                                                                                                                                             すべてのマクロ定義を削除します。                                                                                                                                              |


- プログラム内での**doskey**の使用

  プログラムデバッガーやファイル転送プログラム (FTP) など、特定の文字ベースの対話型プログラムは、自動的に Doskey を使用します。 Doskey を使用するには、プログラムがコンソールプロセスであり、バッファーされた入力を使用する必要があります。 プログラムキーの割り当ては、 **doskey**キーの割り当てをオーバーライドします。 たとえば、プログラムが関数に対して F7 キーを使用している場合、ポップアップウィンドウで**doskey**コマンド履歴を取得することはできません。

  Doskey を使用すると、開始または繰り返しを行う各プログラムのコマンド履歴を保持できます。 プログラムのプロンプトで前のコマンドを編集し、プログラム用に作成された**doskey**マクロを開始することができます。 同じコマンドプロンプトウィンドウからプログラムを終了して再起動すると、前のプログラムセッションのコマンド履歴が使用可能になります。

  プログラムを開始する前に、Doskey を実行する必要があります。 プログラムにシェルコマンドがある場合でも、プログラムのコマンドプロンプトから**doskey**コマンドラインオプションを使用することはできません。

  プログラムでの Doskey の動作をカスタマイズし、そのプログラムの**doskey**マクロを作成する場合は、doskey を変更してプログラムを起動するバッチプログラムを作成できます。
- 既定の挿入モードの指定

  挿入キーを押すと、テキストを置き換えずに、既存のテキストの途中で**doskey**コマンドラインにテキストを入力できます。 ただし、ENTER キーを押すと、キーボードが置換モードに戻ります。 挿入モードに戻るには、もう一度 INSERT キーを押す必要があります。

  ENTER キーを押すたびにキーボードを挿入モードに切り替えるには、 **/挿入**を使用します。 **/Overstrike**を使用するまで、キーボードは挿入モードのままになります。 挿入キーを押すと、置換モードに戻ることができますが、ENTER キーを押すと、Doskey によってキーボードが挿入モードに戻ります。

  挿入キーを使用して、あるモードから別のモードに変更すると、挿入ポイントの形状が変わります。
- マクロの作成

  Doskey を使用して、1つ以上のコマンドを実行するマクロを作成できます。 次の表に、マクロを定義するときにコマンド操作を制御するために使用できる特殊文字を示します。  

  |   文字   |                                                                                                                                                                               説明                                                                                                                                                                               |
  |---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
  |   $G または $g    |                                                                                   出力をリダイレクトします。 これらの特殊文字のいずれかを使用して、画面ではなく、デバイスまたはファイルに出力を送信します。 この文字は、出力のリダイレクトシンボル ( **>** ) に相当します。                                                                                    |
  | $G $ G または $g $ g  |                                                         ファイルの末尾に出力を追加します。 これらの2つの文字のいずれかを使用して、ファイル内のデータを置き換えるのではなく、既存のファイルに出力を追加します。 これらの2つの文字は、出力の追加リダイレクトシンボルに相当します ( **>>** )。                                                         |
  |   $L または $l    |                                                                                  入力をリダイレクトします。 これらの特殊文字のいずれかを使用して、キーボードからではなく、デバイスまたはファイルからの入力を読み取ります。 この文字は、入力のリダイレクトシンボルに相当します ( **<** )。                                                                                  |
  |   $B または $b    |                                                                                                                                    マクロ出力をコマンドに送信します。 これらの特殊文字は、パイプを使用した場合と同じです (\* @ no__t-1                                                                                                                                     |
  |   $T または $t    |                                                            コマンドを区切ります。 **Doskey**コマンドラインでマクロまたは型コマンドを作成するときに、これらの特殊文字のいずれかを使用してコマンドを区切ります。 これらの特殊文字は、コマンドラインでアンパサンド ( **&** ) を使用することと同じです。                                                            |
  |      $$       |                                                                                                                                                              ドル記号 ( **$** ) を指定します。                                                                                                                                                               |
  | $1 ~ $9 |             マクロを実行するときに指定する任意のコマンドライン情報を表します。 特殊文字 **$1** ~ **$9**は、マクロを実行するたびにコマンドラインで異なるデータを使用できるバッチパラメーターです。 **Doskey**コマンドの **$1**文字は、バッチプログラムの **% 1**文字に似ています。             |
  |      $\*      | マクロ名を入力するときに指定するすべてのコマンドライン情報を表します。 特殊文字 **$ @ no__t-2**\* は、置換可能なパラメーターであり、バッチパラメーター **$1** ~ **$9**に似ていますが、1つの重要な違いがあります。マクロ名の後にコマンドラインで入力するものはすべて、マクロの **$ @ no__t-8**@no__t に置き換えられます。 |


- **Doskey**マクロの実行

  マクロを実行するには、最初の位置から開始して、コマンドプロンプトでマクロ名を入力します。 マクロが **$ @ no__t*** またはバッチパラメーター **$1** ~ **$9**で定義されている場合は、スペースを使用してパラメーターを区切ります。 バッチプログラムから**doskey**マクロを実行することはできません。
- Windows Server 2003 ファミリコマンドと同じ名前のマクロを作成する

  特定のコマンドを特定のコマンドラインオプションと共に使用する場合は、コマンドと同じ名前のマクロを作成できます。 マクロまたはコマンドを実行するかどうかを指定するには、次のガイドラインに従います。  
  -   マクロを実行するには、コマンドプロンプトでマクロ名を入力します。 マクロ名の前にスペースを追加しないでください。
  -   コマンドを実行するには、コマンドプロンプトで1つまたは複数のスペースを挿入し、コマンド名を入力します。
- マクロの削除

  マクロを削除するには、次のように入力します。  
  ```
  doskey <MacroName> =
  ```

## <a name="BKMK_examples"></a>例

マクロおよび **/history**コマンドラインオプション**は、マクロ**とコマンドを保存するバッチプログラムを作成する場合に便利です。 たとえば、現在のすべての**doskey**マクロを格納するには、次のように入力します。
```
doskey /macros > macinit 
```
Macinit に格納されているマクロを使用するには、次のように入力します。
```
doskey /macrofile=macinit 
```
最近使用したコマンドを含む .Tmp という名前のバッチプログラムを作成するには、次のように入力します。
```
doskey /history> tmp.bat 
```
複数のコマンドを含むマクロを定義するには、次のように **$t**を使用してコマンドを区切ります。
```
doskey tx=cd temp$tdir/w $*
```
前の例では、TX マクロは現在のディレクトリを Temp に変更し、ディレクトリの一覧をワイド表示形式で表示します。 マクロの末尾にある **$ @ no__t*** を使用して、TX の実行時に他のコマンドラインオプションを**dir**に追加できます。

次のマクロでは、新しいディレクトリ名にバッチパラメーターを使用しています。
```
doskey mc=md $1$tcd $1
```
マクロは、新しいディレクトリを作成し、現在のディレクトリから新しいディレクトリに変更します。

上記のマクロを使用して、Books という名前のディレクトリを作成し、変更するには、次のように入力します。
```
mc books
```
Mstest.exe というプログラムの**doskey**マクロを作成するには、次のように、 **/exename**を含めます。
```
doskey /exename=ftp.exe go=open 172.27.1.100$tmget *.TXT c:\reports$tbye 
```
上記のマクロを使用するには、FTP を開始します。 FTP プロンプトで、次のように入力します。
```
go
```
FTP では、 **open**、 **mget**、および**bye**コマンドを実行します。

すばやく無条件にディスクをフォーマットするマクロを作成するには、次のように入力します。
```
doskey qf=format $1 /q /u
```
ドライブ A でディスクをすばやく無条件にフォーマットするには、次のように入力します。
```
qf a:
```
Vlist というマクロを削除するには、次のように入力します。
```
doskey vlist =
```

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)
