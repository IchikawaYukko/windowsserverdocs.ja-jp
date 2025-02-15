---
title: sfc
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c58c25da-e028-42a6-9e10-973484a4b953
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca470d519e9f3425c0c58fd0070a76c7038ec9b5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384016"
---
# <a name="sfc"></a>sfc

>適用先:Windows Server (半期チャネル)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

スキャンし、保護されているすべてのシステムの整合性がファイルし、正しいバージョンと正しくないバージョンが置き換えられますことを確認します。
このコマンドの使用方法の例については、「[例](#BKMK_examples)」を参照してください。

## <a name="syntax"></a>構文
```
sfc [/scannow] [/verifyonly] [/scanfile=<file>] [/verifyfile=<file>] [/offwindir=<offline windows directory> /offbootdir=<offline boot directory>]
```

### <a name="parameters"></a>パラメーター
|パラメーター|説明|
|-------|--------|
|/scannow|すべての保護されたシステム ファイルの整合性をスキャンし、可能であれば、問題のあるファイルを修復します。|
|/verifyonly|すべての保護されたシステム ファイルの整合性をスキャンします。 修復操作は実行されません。|
|/scanfile|指定されたファイルの整合性をスキャンし、可能であれば、問題が検出された場合、ファイルを修復します。|
|@no__t 0file >|指定した完全なパスとファイル名|
|/verifyfile|指定されたファイルの整合性を検証します。 修復操作は実行されません。|
|/offwindir|オフライン修復用のオフラインの windows ディレクトリの場所を指定します。|
|/offbootdir|オフラインのオフライン ブート ディレクトリの場所を指定します|
|/?|コマンド プロンプトにヘルプを表示します。|

## <a name="remarks"></a>コメント
-   実行する管理者グループのメンバーとしてログオンする必要があります **sfc.exe**します。
-   **sfc**によって保護されたファイルが上書きされたことが検出されると、 **systemroot\system32\dllcache**フォルダーから正しいバージョンのファイルが取得され、正しくないファイルが置き換えられます。
-   Windows Server 2003、windows server 2008、および Windows Server 2008 R2 の**sfc**には、機能上の違いがあります。
-   Windows Server 2003 の**sfc**の詳細については、Microsoft サポート技術情報の[記事 310747](https://go.microsoft.com/fwlink/?LinkId=227069)を参照してください。
-   Windows Server 2008 および Windows Server 2008 R2 の**sfc**の詳細については、「[システムファイルチェッカー](https://go.microsoft.com/fwlink/?LinkId=227071)」を参照してください。

## <a name="BKMK_examples"></a>例
確認する、 **kernel32.dll ファイル**, 、種類。
```
sfc /verifyfile=c:\windows\system32\kernel32.dll
```
セットアップのオフライン修復するには、 **kernel32.dll** 設定オフライン ブート ディレクトリとファイル **d:** とオフラインの windows ディレクトリに設定 **d:\windows**, 、種類。
```
sfc /scanfile=d:\windows\system32\kernel32.dll /offbootdir=d:\ /offwindir=d:\windows
```

## <a name="additional-references"></a>その他の参照情報
-   [コマンド ライン構文の記号](command-line-syntax-key.md)

