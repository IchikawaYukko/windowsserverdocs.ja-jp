---
title: PowerShell スクリプトのパフォーマンスに関する考慮事項
description: PowerShell でのパフォーマンスのスクリプト作成
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: JasonSh
author: lzybkr
ms.date: 10/16/2017
ms.openlocfilehash: 2898cf5ee965da77c9f6a3473e55c1cee6b53f2b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71354978"
---
# <a name="powershell-scripting-performance-considerations"></a>PowerShell スクリプトのパフォーマンスに関する考慮事項

.NET を直接利用してパイプラインを回避する PowerShell スクリプトは、慣用的な PowerShell よりも高速になる傾向があります。 慣用的な PowerShell は、通常、コマンドレットと PowerShell 関数を頻繁に使用します。多くの場合、パイプラインを利用し、必要な場合にのみ .NET にドロップします。

>[!Note] 
> ここで説明する手法の多くは、PowerShell の慣用的なではないため、PowerShell スクリプトの読みやすさが低下する可能性があります。 スクリプト作成者は、それ以外の場合は、慣用的な PowerShell を使用することをお勧めします。

## <a name="suppressing-output"></a>出力の抑制

パイプラインへのオブジェクトの書き込みを回避するには、さまざまな方法があります。

```PowerShell
$null = $arrayList.Add($item)
[void]$arrayList.Add($item)
```

@No__t-0 への割り当てまたは `[void]` へのキャストはほぼ同じであり、通常はパフォーマンスが重要な場合に推奨されます。

```PowerShell
$arrayList.Add($item) > $null
```

@No__t-0 へのファイルのリダイレクトは、前の方法とほとんど同じですが、ほとんどのスクリプトではその違いに気付くことはありません。
このシナリオによっては、ファイルのリダイレクトによって若干のオーバーヘッドが生じます。

```PowerShell
$arrayList.Add($item) | Out-Null
```

@No__t-0 へのパイプは、代替手段と比較すると大きなオーバーヘッドが発生します。
パフォーマンスに影響するコードでは、これを回避する必要があります。

```PowerShell
$null = . {
    $arrayList.Add($item)
    $arrayList.Add(42)
}
```

スクリプトブロックを導入して (ドットソーシングまたはそれ以外の方法を使用して) 呼び出し、その結果を `$null` に割り当てることは、スクリプトの大きなブロックの出力を抑制するための便利な手法です。
この手法は @no__t 0 にパイプを使用して実行されるため、パフォーマンスに影響するスクリプトでは回避する必要があります。
この例の追加のオーバーヘッドは、以前インラインスクリプトであったスクリプトブロックを作成して呼び出すことから取得されます。


## <a name="array-addition"></a>配列の加算

多くの場合、項目のリストを生成するには、加算演算子を使用した配列を使用します。

```PowerShell
$results = @()
$results += Do-Something
$results += Do-SomethingElse
$results
```

配列は不変であるため、これは非常に inefficent になる可能性があります。
配列に追加するたびに、左側と右の両方のオペランドのすべての要素を保持するために十分な大きさの新しい配列が作成され、その後、両方のオペランドの要素が新しい配列にコピーされます。
小さいコレクションの場合、このオーバーヘッドは問題にならない可能性があります。
大規模なコレクションの場合、これは明らかに問題になる可能性があります。

代替手段がいくつかあります。
実際に配列を必要としない場合は、代わりに ArrayList を使用することを検討してください。

```PowerShell
$results = [System.Collections.ArrayList]::new()
$results.AddRange((Do-Something))
$results.AddRange((Do-SomethingElse))
$results
```

配列が必要な場合は、独自の `ArrayList` を使用して、配列が必要なときに単に `ArrayList.ToArray` を呼び出すことができます。
または、PowerShell を使用して `ArrayList` と `Array` を作成することもできます。

```PowerShell
$results = @(
    Do-Something
    Do-SomethingElse
)
```

この例では、PowerShell によって @no__t 0 が作成され、配列式内のパイプラインに書き込まれた結果を保持します。
@No__t-0 に割り当てる直前に、PowerShell は `ArrayList` を `object[]` に変換します。

## <a name="processing-large-files"></a>大きなファイルの処理

PowerShell でファイルを処理する慣用的なの方法は、次のようになります。

```PowerShell
Get-Content $path | Where-Object { $_.Length -gt 10 }
```

これは、.NET Api を直接使用するよりもはるかに低い順序で実行できます。

```PowerShell
try
{
    $stream = [System.IO.StreamReader]::new($path)
    while ($line = $stream.ReadLine())
    {
        if ($line.Length -gt 10)
        {
            $line
        }
    }
}
finally
{
    $stream.Dispose()
}
```

## <a name="avoid-write-host"></a>書き込みホストを避ける

通常、出力をコンソールに直接書き込むのは好ましくありませんが、意味がある場合は、多くのスクリプトで `Write-Host` が使用されます。

多くのメッセージをコンソールに書き込む必要がある場合、`Write-Host` は `[Console]::WriteLine()` よりもはるかに低い順序になります。 ただし、`[Console]::WriteLine()` は、powershell .exe や powershell_ise などの特定のホストに対してのみ適切な代替であることに注意してください。すべてのホストで動作することは保証されていません。

@No__t-0 を使用する代わりに、[書き込み出力](/powershell/module/Microsoft.PowerShell.Utility/Write-Output?view=powershell-5.1)の使用を検討してください。

