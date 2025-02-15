---
ms.assetid: ''
title: 追跡可能性のための Windows タイム
description: 多くの部門では、システムが UTC に対して追跡可能である必要があります。  これは、システムのオフセットを UTC に対して証明できることを意味します。
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 307739042426088fa92c50e6ea4dc5d2a744f15a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405208"
---
# <a name="windows-time-for-traceability"></a>追跡可能性のための Windows タイム
>適用対象:Windows Server 2016 バージョン1709以降、および Windows 10 バージョン1703以降


多くの部門では、システムが UTC に対して追跡可能である必要があります。  これは、システムのオフセットを UTC に対して証明できることを意味します。  規制遵守シナリオを有効にするために、Windows 10 (バージョン1703以降) および Windows Server 2016 (バージョン1709以降) では、オペレーティングシステムの観点から画像を提供して、実行された操作を理解するための新しいイベントログを提供します。システムクロック。  これらのイベントログは、Windows タイムサービスに対して継続的に生成され、後で分析するために調査またはアーカイブできます。

これらの新しいイベントによって、次の質問に答えることができます。

* システムクロックが変更された
* クロックの頻度が変更された
* Windows タイムサービスの構成が変更された

## <a name="availability"></a>可用性

これらの改善点は、Windows 10 バージョン1703以降、および Windows Server 2016 バージョン1709以降に含まれています。

## <a name="configuration"></a>構成

この機能を実現するための構成は必要ありません。  これらのイベントログは既定で有効になっており、イベントビューアーの [**アプリケーションとサービス] Log\Microsoft\Windows\Time-Service\Operational**チャネルにあります。


## <a name="list-of-event-logs"></a>イベントログの一覧

次のセクションでは、追跡可能性のシナリオで使用するためにログに記録されるイベントの概要を示します。

# <a name="257tab257"></a>[257](#tab/257)
このイベントは、Windows タイムサービス (W32Time) が開始されたときにログに記録され、現在の時刻、現在のティック数、ランタイム構成、タイムプロバイダー、および現在のクロックレートに関する情報をログに記録します。

|||
|---|---|
|イベントの説明 |サービスの開始 |
|詳細 |W32time の起動時に発生します |
|ログに記録されたデータ |<ul><li>現在の時刻 (UTC)</li><li>現在のティック数</li><li>W32Time の構成</li><li>時間プロバイダーの構成</li><li>クロックレート</li></ul> |
|調整メカニズム  |[なし] : このイベントは、サービスが開始されるたびに発生します。 |

**例:**
```
W32time service has started at 2018-02-27T04:25:17.156Z (UTC), System Tick Count 3132937.
```

**[コマンド:]**

この情報は、次のコマンドを使用して照会することもできます。

*W32Time と Time プロバイダーの構成*
```
w32tm.exe /query /configuration
```

*クロックレート*
```
w32tm.exe /query /status /verbose
```


# <a name="258tab258"></a>[258](#tab/258)
このイベントは、Windows タイムサービス (W32Time) が停止しているときにログに記録され、現在の時刻とティック数に関する情報をログに記録します。

|||
|---|---|
|イベントの説明 |サービスの停止 |
|詳細 |W32time のシャットダウン時に発生します |
|ログに記録されたデータ |<ul><li>現在の時刻 (UTC)</li><li>現在のティック数</li></ul> |
|調整メカニズム  |[なし] : このイベントは、サービスが停止するたびに発生します。 |

**テキストの例:** 
`W32time service is stopping at 2018-03-01T05:42:13.944Z (UTC), System Tick Count 6370250.`

# <a name="259tab259"></a>[259](#tab/259)
このイベントは、現在のタイムソースと選択されたタイムソースの一覧を定期的にログに記録します。  また、現在のティック数をログに記録します。  このイベントは、タイムソースが変更されるたびに発生しません。  このドキュメントの後半に記載されている他のイベントは、この機能を提供します。

|||
|---|---|
|イベントの説明 |NTP クライアントプロバイダーの定期的な状態 |
|詳細 |NTP クライアントによって使用されるタイムソースの一覧 |
|ログに記録されたデータ |<ul><li>使用可能なタイムソース</li><li>ログ記録時に選択された参照タイムサーバー</li><li>現在のティック数</li></ul>  |
|調整メカニズム  |8時間ごとに1回記録されます。 |

**テキストの例:** NTP クライアントプロバイダーの定期的な状態:

Ntp クライアントは、次の NTP サーバーから時刻データを受信しています:

server1、0x8 (ntp. m | 0x8 | [::]: 123-> [IPAddress]: 123) server2. fabrikam. .com, 0x8 (ntp. m | 0x8 | [::]: 123-> [IPAddress]: 123); また、選択した参照タイムサーバーは、Server1. fabrikam .com, 0x8 (ntp. m | 0x8 | [::]: 123-> [IPAddress]: 123) (RefID: 0x08d6648e63) です。 システムティックカウント13187937

**コマンド**この情報は、次のコマンドを使用して照会することもできます。

*ピアの識別*
`w32tm.exe /query /peers`

# <a name="260tab260"></a>[260](#tab/260)

|||
|---|---|
|イベントの説明 |タイムサービスの構成と状態 |
|詳細 |W32time は、その構成と状態を定期的にログに記録します。 これは、の呼び出しに相当します。<br><br>`w32tm /query /configuration /verbose`<br>スイッチまたは<br>`w32tm /query /status /verbose` |
|調整メカニズム  |8時間ごとに1回記録されます。 |

# <a name="261tab261"></a>[261](#tab/261)
これにより、SetSystemTime API を使用してシステム時刻が変更されると、各インスタンスがログに記録されます。

|||
|---|---|
|イベントの説明 |システム時刻が設定されています |
|調整メカニズム  |[なし] :<br><br>これは、時間の同期が妥当なシステムではめったに発生しませんが、発生するたびにログに記録する必要があります。 このイベントのログ記録中に、TimeJumpAuditOffset 設定は無視されます。この設定は、Windows システムイベントログのイベントを調整することを意図したものであるためです。 |

# <a name="262tab262"></a>[262](#tab/262)

|||
|---|---|
|イベントの説明 |システムクロックの周波数調整 |
|詳細 |システムクロックの頻度は、クロックの同期が終了したときに W32time によって絶えず変更されます。 イベントログをオーバーランさせずに、クロック周波数に対する "適度に有意な" 調整を行う必要があります。 |
|調整メカニズム  |TimeAdjustmentAuditThreshold の下にあるすべてのクロック調整 (最小値 = 128、100万単位、既定値は800、100万単位) はログに記録されません。<br><br>2 (クロック周波数の現在の粒度の変更) では、クロック精度の120マイクロ秒/秒の変化が発生します。<br><br>同期されたシステムでは、調整の大半がこのレベルを下回っています。 より詳細な追跡を行う場合は、この設定を調整するか、PerfCounters を使用するか、両方を実行することができます。 |

# <a name="263tab263"></a>[263](#tab/263)

|||
|---|---|
|イベントの説明 |タイムサービスの設定または読み込まれたタイムプロバイダーの一覧を変更します。 |
|詳細 |W32time 設定を再読み込みすると、特定の重要な設定がメモリ内で変更される可能性があります。これは、時間の同期の全体的な精度に影響を与える可能性があります。<br><br>W32time は、時刻の同期に影響を与える可能性がある設定を再度読み取るときに、それぞれの発生をログに記録します。 |
|調整メカニズム  |[なし] :<br><br>このイベントは、管理者または GP の更新プログラムによってタイムプロバイダが変更され、W32time がトリガーされた場合にのみ発生します。 設定の変更の各インスタンスを記録します。 |


# <a name="264tab264"></a>[264](#tab/264)

|||
|---|---|
|イベントの説明 |NTP クライアントによって使用されるタイムソースの変更 |
|詳細 |タイムサーバー/ピアが状態 (**保留中の > の同期**、**同期 > 到達でき**ない、またはその他の移行) を変更したときに、NTP クライアントがタイムサーバー/ピアの現在の状態を使用してイベントを記録します。 |
|調整メカニズム  |最大頻度–一時的な問題と不適切なプロバイダーの実装からログを保護するために、5分ごとに1回だけです。 |

# <a name="265tab265"></a>[265](#tab/265)

|||
|---|---|
|イベントの説明 |タイムサービスソースまたは階層番号の変更 |
|詳細 |W32time タイムソースと階層番号は、時間追跡の重要な要素であり、これらに対する変更はすべてログに記録する必要があります。 W32time に時間がなく、信頼性の高いタイムソースとして構成されていない場合は、タイムサーバーとしてアドバタイズが停止され、設計によって無効なパラメーターを含む要求に応答します。 このイベントは、NTP トポロジにおける状態の変化を追跡するために重要です。 |
|調整メカニズム  |[なし] : |


# <a name="266tab266"></a>[266](#tab/266)

|||
|---|---|
|イベントの説明 |再同期が要求された時刻 |
|詳細 |この操作は次のようにトリガーされます。<ul><li>ネットワークの変更が発生したとき</li><li>接続されたスタンバイ/休止状態からシステムが戻る</li><li>長時間同期していない場合</li><li>管理者が再同期コマンドを発行する</li></ul>この操作を実行すると、NTP クライアントによってフィルターがクリアされるため、細かい時間同期の精度がすぐに失われます。 |
|調整メカニズム  |最大頻度-5 分ごとに1回。<br><br>不適切なネットワークカード (または不適切なスクリプト) がこの操作を繰り返しトリガーする可能性があり、ログがいっぱいになる可能性があります。 そのため、このイベントを調整する必要があります。<br><br>正確な時刻の同期にかかる時間は5分を超えるので、調整によって時間の精度が失われる原因となった元のイベントに関する情報は失われないことに注意してください。  |

---
