---
title: shift
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f74e0f1f9041a4a7b95d83772ea79376c82876de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371252"
---
# <a name="shift"></a>shift



バッチ ファイル内のバッチのパラメーターの位置を変更します。

このコマンドを使用する方法の例については、[例](#BKMK_examples)を参照してください。

## <a name="syntax"></a>構文

```
shift [/n <N>]
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---------|-----------|
|/n \<N >|移行を開始することを指定、 *N*番目の引数、 *N* は 8 ~ 0 の値。 既定で有効になっているコマンド拡張機能が必要です。|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>コメント

- **Shift キーを押し** コマンド バッチのパラメーターの値を変更する **%0** を通じて **%9** 前に各パラメーターをコピーして: の値 **%1** にコピー **%0**, の値 **%2** にコピー **%1**, 、という具合です。 これは、パラメーターの数にかかわらず、同じ操作を実行するバッチ ファイルを作成するため便利です。
- コマンド拡張機能が有効になっている場合、 **shift** コマンドをサポートする、 **/n** コマンド ライン オプションです。 **/N** n 番目の引数で移行を開始するオプションが指定場所 **N** は 8 ~ 0 の値。 など **SHIFT/2** 移動することに **%3** に **%2**, 、 **%4** に **%3**, のままにし、 **%0** と **%1** 影響はありません。 既定では、コマンド拡張機能が有効にします。
- 使用することができます、 **shift** 10 個を超えるバッチ パラメーターを受け取りが可能なバッチ ファイルを作成するコマンドです。 コマンドラインで複数の 10 個のパラメーターを指定する場合、表示される、10 分後に ( **%9**) には、一度に 1 つのシフトをする **%9**します。
- **Shift**コマンドを実行しても、 **% @ no__t*** バッチパラメーターには影響しません。
- いいえ旧バージョンとは **shift** コマンドです。 実装した後、 **shift キーを押し** コマンド、バッチのパラメーターを回復することはできません ( **%0**)、shift キーを押し前に存在します。

## <a name="BKMK_examples"></a>例

Mycopy.bat をというサンプル バッチ ファイルの次の行を使用する方法をデモンストレーションする **shift** バッチ パラメーターの任意の数にします。 この例では、Mycopy.bat は、特定のディレクトリにファイルの一覧をコピーします。 バッチ パラメーターは、ディレクトリとファイル名の引数によって表されます。
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>その他の参照情報

[コマンド ライン構文の記号](command-line-syntax-key.md)