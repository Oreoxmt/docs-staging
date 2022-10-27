---
title: Monitor a TiDB Cluster
summary: Learn how to monitor your TiDB cluster.
---

# TiDBクラスタを監視する {#monitor-a-tidb-cluster}

このドキュメントでは、 TiDB Cloudで TiDB クラスターを監視する方法について説明します。

## クラスタのステータスとノードのステータス {#cluster-status-and-node-status}

クラスター ページで、実行中の各クラスターの現在の状態を確認できます。

### クラスタステータス {#cluster-status}

| クラスタの状態      | 説明                               |
| :----------- | :------------------------------- |
| **普通**       | 通常稼働（データ移行含む）                    |
| **作成**       | クラスターの作成                         |
| **輸入中**      | クラスターはデータをインポートしています             |
| **スケーリング**   | TiDB、TiKV、または TiFlash ノードのスケーリング |
| **アップグレード中** | TiDB のバージョンアップ                   |
| **利用不可**     | TiDB Cloudサービスは利用できません           |
| **不健康**      | ノードの一部が利用できない、十分なレプリカがないなど       |
| **回復**       | バックアップの回復                        |
| **一時停止**     | クラスタは一時停止しています                   |
| **再開中**      | クラスターの再開                         |

### TiDB ノードのステータス {#tidb-node-status}

| TiDB ノードのステータス | 説明               |
| :------------- | :--------------- |
| **普通**         | 通常走行             |
| **作成**         | ノードの作成           |
| **利用不可**       | TiDB ノードが利用できない  |
| **終了中**        | TiDB ノードが終了しています |

### TiKV ノードのステータス {#tikv-node-status}

| TiKV ノードのステータス | 説明                |
| :------------- | :---------------- |
| **普通**         | 通常走行              |
| **作成**         | ノードの作成            |
| **利用不可**       | TiKV ノードは利用できません  |
| **終了中**        | TiKV ノードが終了しています  |
| **出発**         | 終了前の現在のノード データの移行 |

## 指標のモニタリング {#monitoring-metrics}

TiDB Cloudでは、次のページからクラスターの一般的に使用されるメトリックを表示できます。

-   クラスタの概要ページ
-   クラスタ監視ページ

### クラスタの概要ページの指標 {#metrics-on-the-cluster-overview-page}

クラスターの概要ページには、合計 QPS、レイテンシー、接続、TiFlash 要求 QPS、TiFlash 要求期間、TiFlash ストレージ サイズ、TiKV ストレージ サイズ、TiDB CPU、TiKV CPU、TiKV IO 読み取り、TiKV IO 書き込みなど、クラスターの一般的なメトリックが表示されます。

クラスターの概要ページでメトリックを表示するには、次の手順を実行します。

1.  [**クラスター]**ページに移動します。

2.  クラスターの名前をクリックして、そのクラスターの概要ページに移動します。

### クラスター監視ページの指標 {#metrics-on-the-cluster-monitoring-page}

クラスター監視ページには、クラスターの標準メトリックの完全なセットが表示されます。これらのメトリックを表示することで、パフォーマンスの問題を簡単に特定し、現在のデータベースの展開が要件を満たしているかどうかを判断できます。

> **ノート：**
>
> 現在、クラスター監視ページは[開発者層のクラスター](/tidb-cloud/select-cluster-tier.md#developer-tier)で利用できません。

クラスター監視ページでメトリックを表示するには、次の手順を実行します。

1.  クラスターの [**診断**] タブに移動します。

2.  [**モニタリング**] タブをクリックします。

詳細については、 [ビルトインモニタリング](/tidb-cloud/built-in-monitoring.md)を参照してください。

## 組み込みのアラート {#built-in-alerting}

TiDB Cloudには、組み込みのアラート条件がいくつかあります。プロジェクト内のTiDB CloudクラスターがTiDB Cloud組み込みアラート条件をトリガーするたびに、電子メール通知を受信するようにTiDB Cloudを構成できます。

詳細については、 [組み込みアラート](/tidb-cloud/monitor-built-in-alerting.md)を参照してください。

## サードパーティの統合 {#third-party-integrations}

### 必要なアクセス {#required-access}

サードパーティの統合設定を編集するには、組織への`Organization Owner`つのアクセス権またはターゲット プロジェクトへの`Project Member`のアクセス権が必要です。

### サードパーティの統合をビューまたは変更する {#view-or-modify-third-party-integrations}

1.  TiDB Cloudコンソールで、表示または変更するターゲット プロジェクトを選択し、[**プロジェクト設定**] タブをクリックします。
2.  左ペインで [**統合**] をクリックします。利用可能なサードパーティ統合が表示されます。

### 利用可能な統合 {#available-integrations}

| サードパーティ サービス                 | Configuration / コンフィグレーションの詳細                                                                                                                                                                                                                                 |
| :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Datadog の統合**              | TiDB クラスターに関するメトリック データを[データドッグ](https://www.datadoghq.com/)に送信するようにTiDB Cloudを構成します。これらのメトリクスは、Datadog ダッシュボードで表示できます。 Datadog が追跡するすべてのメトリクスの詳細なリストを取得するには、 [Datadog 統合](/tidb-cloud/monitor-datadog-integration.md)を参照してください。                              |
| **Prometheus と Grafana の統合** | TiDB Cloudから Prometheus の Scrape_config ファイルを取得し、そのファイルの内容を使用して Prometheus を構成します。これらのメトリックは、Grafana ダッシュボードで表示できます。 Prometheus が追跡するすべてのメトリックの詳細なリストを取得するには、 [Prometheus と Grafana の統合](/tidb-cloud/monitor-prometheus-and-grafana-integration.md)を参照してください。 |