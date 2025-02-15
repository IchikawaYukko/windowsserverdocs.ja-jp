---
title: QoS ポリシーのアーキテクチャ
description: このトピックでは、サービスの品質 (QoS) ポリシーの概要について説明します。これにより、グループポリシーを使用して、Windows Server 2016 の特定のアプリケーションとサービスのネットワークトラフィック帯域幅に優先順位を付けることができます。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 609d3f28465380b7d15648cfeb73070a39b9362f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395977"
---
# <a name="qos-policy-architecture"></a>QoS ポリシーのアーキテクチャ

>適用対象:Windows Server (半期チャネル)、Windows Server 2016

このトピックでは、QoS ポリシーのアーキテクチャについて説明します。

次の図は、ポリシーベースの QoS のアーキテクチャを示しています。

![QoS ポリシーのアーキテクチャ](../../media/QoS/QoS-Policy-Architecture.jpg)

ポリシーベースの QoS のアーキテクチャは、次のコンポーネントで構成されています。

- **クライアントサービスをグループポリシー**します。 ユーザーとコンピューターの構成グループポリシー設定を管理する Windows サービス。

- **グループポリシーエンジン**。 グループポリシークライアントサービスのコンポーネントの1つ。スタートアップ時に Active Directory からユーザーとコンピューターの構成グループポリシー設定を取得し、既定では90分 @ no__t-1 ごとに @no__t 変更の有無を定期的に確認します。 変更が検出されると、グループポリシーエンジンは新しいグループポリシーの設定を取得します。 グループポリシーエンジンは、QoS ポリシーが更新されると、受信 Gpo を処理し、QoS クライアント側拡張機能に通知します。

- **QoS クライアント側拡張**。 QoS ポリシーが変更されたことをグループポリシーエンジンからの通知を待機し、QoS 検査モジュールに通知するグループポリシークライアントサービスのコンポーネント。

- **Tcp/ip スタック**。 IPv4 および IPv6 の統合サポートが含まれ、Windows フィルタリングプラットフォームをサポートする TCP/IP スタック。 

- **QoS 検査**。 モジュールは、qos クライアント側拡張から QoS ポリシーの変更が検出されるまで待機し、QoS ポリシー設定を取得し、トランスポート層およびペーサーと対話して QoS に一致するトラフィックを内部的にマークする、TCP/IP スタック内のコンポーネントです。ポリシー.

- **NDIS 6.x**。 カーネルモードのネットワークドライバーと、Windows Server およびクライアントオペレーティングシステムのオペレーティングシステムの間の標準インターフェイス。 NDIS 6.x ではライトウェイトフィルターがサポートされています。これは、NDIS 中間ドライバーおよびミニポートドライバーの簡素化されたドライバーモデルであり、パフォーマンスが向上します。

- **QoS ネットワークプロバイダーインターフェイス \(NPI @ no__t**。 ランタイムと対話するカーネルモードドライバー用のインターフェイス。

- **ペーサー. sys**. ポリシーベースの QoS のパケットスケジューリング、および汎用 QoS @no__t を使用するアプリケーションのトラフィック (0GQoS @ no__t と Traffic Control \(TC @ no__t-3 Api) を制御する NDIS 6.x ライトウェイトフィルタードライバー。 Windows Server 2003 および Windows XP では、-sys は Psched を置き換えました。 ペーサーは、ネットワーク接続またはアダプターのプロパティから、QoS パケットスケジューラコンポーネントと共にインストールされます。

このガイドの次のトピックについては、「 [QoS Policy のシナリオ](qos-policy-scenarios.md)」を参照してください。

このガイドの最初のトピックについては、「[サービスの品質 (QoS) ポリシー](qos-policy-top.md)」を参照してください。

