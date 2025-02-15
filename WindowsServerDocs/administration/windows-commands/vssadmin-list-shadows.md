---
title: Vssadmin リストの影
description: Vssadmin list shadows コマンドの説明です。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 49bee3deac463b68fda94097bb183bcbf1c89810
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362594"
---
# <a name="vssadmin-list-shadows"></a>Vssadmin リストの影

>適用対象:Windows 10、Windows 8.1、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

指定したボリュームの既存のシャドウコピーをすべて一覧表示します。 パラメーターを指定せずにこのコマンドを使用すると、コンピューター上のすべてのボリュームシャドウコピーが、**シャドウコピーセット**によって指定された順序で表示されます。

## <a name="syntax"></a>構文

```PowerShell
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

## <a name="parameters"></a>パラメーター

|パラメーター|説明|
|---|---|
|/for = @no__t 0ForVolumeSpec >|シャドウコピーが表示されるボリュームを指定します。|
|/shadow = \<ShadowID >|ShadowID によって指定されたシャドウコピーの一覧を表示します。 シャドウコピー ID を取得するには、 **vssadmin list shadows**コマンドを使用します。 シャドウコピー ID を入力するときは、次の形式を使用します。各*X*は16進文字を表します。<br><br>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX-XXXX-XXXX-XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX|

## <a name="additional-references"></a>その他の参照情報

* [コマンドライン構文のキー](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771080(v%3dws.11))
* [Vssadmin](vssadmin.md)