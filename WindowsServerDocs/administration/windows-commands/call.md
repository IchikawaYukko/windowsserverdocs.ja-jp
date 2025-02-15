---
title: 呼び出し
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 0e5f9f2b0102c12ee0925bb434fdeddde85e34cd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379718"
---
# <a name="call"></a>呼び出し



親バッチプログラムを停止せずに、別のバッチプログラムから1つのバッチプログラムを呼び出します。 **Call**コマンドは、呼び出しの対象としてラベルを受け取ります。

> [!NOTE]
> スクリプトまたはバッチファイルの外部で使用される場合、**呼び出し**はコマンドプロンプトには影響しません。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>パラメーター

|           パラメーター           |                                                                         説明                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive >:][\< パス >] <FileName> | 呼び出すバッチプログラムの場所と名前を指定します。 *FileName*パラメーターは必須であり、.bat または .cmd 拡張子を持つ必要があります。 |
|      \<BatchParameters >       |                                            バッチプログラムに必要なコマンドライン情報を指定します。                                             |
|           : \<Label >           |                                            バッチプログラムコントロールのジャンプ先のラベルを指定します。                                             |
|         \<Arguments >          |                     バッチプログラムの新しいインスタンスに渡されるコマンドライン情報を指定します。このとき、: Label から開始し*ます。*                     |
|              /?               |                                                             コマンド プロンプトにヘルプを表示します。                                                             |

## <a name="batch-parameters"></a>バッチパラメーター

次の表に、バッチスクリプト引数の参照 ( **% 0**、 **% 1**、...) を示します。

バッチスクリプト内の **% @ no__t**は、すべての引数を参照します (例: **% 1**、 **% 2**、 **% 3**...)

バッチパラメーター ( **% n**) の代替として、次のオプションの構文を使用できます。

|バッチパラメーター|説明|
|---------------|-----------|
|% ~ 1|**% 1**を展開し、囲まれた引用符 ("") を削除します。|
|% ~ f1|**% 1**を完全修飾パスに展開します。|
|% ~ d1|**% 1**をドライブ文字のみに拡張します。|
|% ~ p1|**% 1**をパスのみに展開します。|
|% ~ n1|**% 1**をファイル名のみに展開します。|
|% ~ x1|**% 1**をファイル名拡張子のみに拡張します。|
|% ~ s1|**% 1**を短い名前のみを含む完全修飾パスに展開します。|
|% ~ a1|**% 1**をファイル属性に展開します。|
|% ~ t1|**% 1**をファイルの日付と時刻に拡張します。|
|% ~ z1|**% 1**をファイルのサイズに拡張します。|
|% ~ $PATH: 1|PATH 環境変数に示されているディレクトリを検索し、 **% 1**を、見つかった最初のディレクトリの完全修飾名に展開します。 環境変数名が定義されていない場合、または検索によってファイルが見つからない場合、この修飾子は空の文字列に展開されます。|

次の表は、修飾子と、複合結果のバッチパラメーターを組み合わせる方法を示しています。

|修飾子を使用したバッチパラメーター|説明|
|-----------------------------|-----------|
|% ~ sjc-dp1|**% 1**をドライブ文字とパスのみに拡張します。|
|% ~ nx1|**% 1**をファイル名と拡張子のみに拡張します。|
|% ~ dp $ PATH: 1|**% 1**の path 環境変数に示されているディレクトリを検索し、見つかった最初のディレクトリのドライブ文字とパスに展開します。|
|% ~ ftza1|**% 1**を拡張して、 **dir**コマンドと同様の出力を表示します。|

上記の例では、 **% 1**と PATH を他の有効な値に置き換えることができます。 <strong>@No__t-1</strong>構文は、有効な引数番号で終了します。 <strong>@No__t-1</strong>修飾子は、 **% @ no__t-4 @ no__t**と共に使用することはできません。

## <a name="remarks"></a>コメント

-   バッチパラメーターの使用

    バッチパラメーターには、バッチプログラムに渡すことができるすべての情報を含めることができます。これには、コマンドラインオプション、ファイル名、バッチパラメーター **% 0**から **% 9**、変数 ( **% baud%** など) が含まれます。
-   *Label*パラメーターの使用

    *Label*パラメーターを指定して**call**を使用すると、新しいバッチファイルコンテキストを作成し、指定したラベルの後のステートメントに制御を渡すことができます。 バッチファイルの末尾が初めて検出されたとき (つまり、ラベルにジャンプした後)、control は**call**ステートメントの後のステートメントに戻ります。 バッチファイルの終わりが2回目になると、バッチスクリプトが終了します。
-   パイプとリダイレクトシンボルの使用

    **呼び出し**でパイプ ( **|** ) とリダイレクトシンボル ( **<** または **>** ) を使用しないでください。
-   再帰呼び出しを行う

    自身を呼び出すバッチプログラムを作成できます。 ただし、終了条件を指定する必要があります。 それ以外の場合、親と子のバッチプログラムは無限にループする可能性があります。
-   コマンド拡張機能の使用

    コマンド拡張機能が有効になっている場合は、呼び出しの対象として *ラベル* **が受け入れら** れます。 正しい構文は次のとおりです。

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>例

別の batch プログラムから Checknew .bat プログラムを実行するには、親 batch プログラムに次のコマンドを入力します。
```
call checknew
```
親バッチプログラムが2つのバッチパラメーターを受け取り、それらのパラメーターを Checknew に渡す場合は、親 batch プログラムに次のコマンドを入力します。
```
call checknew %1 %2
```

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)
