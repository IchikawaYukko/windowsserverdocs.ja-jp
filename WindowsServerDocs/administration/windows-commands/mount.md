---
title: mount
description: 'Windows コマンドに関するトピック * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a225c847055198a9a48962a3b40969556f10ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373587"
---
# <a name="mount"></a>mount



**Mount**を使用して、Network File SYSTEM (NFS) ネットワーク共有をマウントできます。

## <a name="syntax"></a>構文

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>説明

**Mount**コマンドラインユーティリティは、 *ComputerName*によって識別される NFS サーバーによってエクスポートされた*共有名*で識別されるファイルシステムをマウントし、 *DeviceName*によって指定されたドライブ文字またはアスタリスク ( **&#42;** ) は、使用可能な最初のドライバー文字によって使用されます。 ユーザーは、ローカルコンピューター上のドライブであるかのように、エクスポートされたファイルシステムにアクセスできます。 オプションや引数を指定せずに**マウント**すると、マウントされているすべての NFS ファイルシステムに関する情報が表示されます。

**Mount**ユーティリティは、NFS クライアントがインストールされている場合にのみ使用できます。

**Mount**ユーティリティでは、次のオプションと引数を使用できます。


|          項目          |                                                                                                                                                                                                                                                定義                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o の場合は、\<buffersize > |                                                                                                                                                                                            読み取りバッファーのサイズを kb 単位で設定します。 使用できる値は1、2、4、8、16、および32です。既定値は 32 KB です。                                                                                                                                                                                            |
| -o wsize = \<buffersize > |                                                                                                                                                                                           書き込みバッファーのサイズを kb 単位で設定します。 使用できる値は1、2、4、8、16、および32です。既定値は 32 KB です。                                                                                                                                                                                            |
| -o timeout = \<seconds >  |                                                                                                                                                                       リモートプロシージャコール (RPC) のタイムアウト値を秒単位で設定します。 使用可能な値は、0.8、0.9、および範囲1-60 の任意の整数です。既定値は0.8 です。                                                                                                                                                                       |
|   -o retry = \<number >   |                                                                                                                                                                                             ソフトマウントの再試行回数を設定します。 許容される値は、1-10 の範囲の整数です。既定値は1です。                                                                                                                                                                                             |
|     -o mtype = {soft     |                                                                                                                                                                                                                                                  ハード                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       匿名ユーザーとしてマウントします。                                                                                                                                                                                                                                       |
|       -o nolock        |                                                                                                                                                                                                                                ロックを無効にします (既定値は**有効**です)。                                                                                                                                                                                                                                |
|    -o casesensitive    |                                                                                                                                                                                                                         サーバー上のファイル参照を強制的に大文字と小文字を区別します。                                                                                                                                                                                                                          |
| -o fileaccess = \<mode >  | NFS 共有で作成された新しいファイルの既定のアクセス許可モードを指定します。 *Ogw*の形式で、3桁の数字として*mode*を指定します。ここで、 *o*、 *g*、および*w*はそれぞれ、ファイルの所有者、グループ、および世界に与えられたアクセス権を表す数字です。 数字は0-7 の範囲で指定する必要があります。次の意味があります。</br>0アクセスなし</br>-1: x (実行アクセス)</br>-2: w (書き込みアクセス)</br>-3: wx</br>-4: r (読み取りアクセス)</br>-5: rx</br>-6: rw</br>-7: rwx |
|    -o lang = {euc-jp     |                                                                                                                                                                                                                                                  euc-tw                                                                                                                                                                                                                                                  |
|     -u: \<UserName >     |                                                                                                                                                                             共有のマウントに使用するユーザー名を指定します。 *Username*の前に円記号 ( **@no__t 2**) が付いていない場合は、UNIX ユーザー名として扱われます。                                                                                                                                                                             |
|     -p: \<Password >     |                                                                                                                                                                                          共有をマウントするために使用するパスワード。 アスタリスク ( **&#42;** ) を使用すると、パスワードの入力を求めるメッセージが表示されます。                                                                                                                                                                                          |

> [!NOTE]
