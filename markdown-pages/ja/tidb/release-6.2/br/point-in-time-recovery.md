---
title: Point-in-Time Recovery
summary: Learn the design, capabilities, and architecture of Point-in-Time Recovery (PITR).
---

# ポイントインタイム リカバリ {#point-in-time-recovery}

ポイントインタイム リカバリ (PITR) を使用すると、TiDB クラスターのスナップショットを過去の任意の時点から新しいクラスターに復元できます。 v6.2.0 で、TiDB は PITR in [復元する](/br/backup-and-restore-overview.md) (BR) を導入します。

PITR を使用して、次のビジネス要件を満たすことができます。

-   災害復旧の目標復旧時点 (RPO) を 20 分未満に短縮します。
-   エラー イベントの前の時点にデータをロールバックすることにより、アプリケーションからの誤った書き込みのケースを処理します。
-   履歴データの監査を実施し、法規制の要件を満たします。

このドキュメントでは、PITR の設計、機能、およびアーキテクチャについて紹介します。 PITR の使用方法を学習する必要がある場合は、 [PITR の使用シナリオ](/br/pitr-usage.md)を参照してください。

## ビジネスで PITR を使用する {#use-pitr-in-your-business}

[ブラジル](/br/backup-and-restore-overview.md)は PITR 機能を提供します。 BRでは、データのバックアップ（スナップショットバックアップ、ログバックアップ）、指定時点へのワンクリックリストア、バックアップデータの管理など、PITRのすべての操作を行うことができます。

ビジネスでPITRを使用する手順は次のとおりです。

![Point-in-Time Recovery](https://download.pingcap.com/images/docs/br/pitr-usage.png)

### バックアップデータ {#back-up-data}

PITR を達成するには、次のバックアップ タスクを実行する必要があります。

-   ログ バックアップ タスクを開始します。 `br log start`コマンドを実行して、ログ バックアップ タスクを開始できます。このタスクは、TiDB クラスターのバックグラウンドで実行され、KV ストレージの変更ログをバックアップ ストレージに自動的にバックアップします。
-   [スナップショット (フル) バックアップ](/br/br-usage-backup.md#back-up-tidb-cluster-snapshots)を定期的に実行します。 `br backup full`コマンドを実行して、指定した時点 (毎日 00:00 など) にクラスター スナップショットをバックアップ ストレージにバックアップできます。

### ワンクリックでデータを復元 {#restore-data-with-one-click}

PITR を使用してデータを復元するには、 `br restore point`コマンドを実行して復元プログラムを実行する必要があります。プログラムは、スナップショット バックアップとログ バックアップからデータを読み取り、指定された時点のデータを新しいクラスターに復元します。

`br restore point`コマンドを実行する場合、復元したい時点より前の最新のスナップショット バックアップ データを指定し、ログ バックアップ データを指定する必要があります。 BR は、最初にスナップショット データを復元し、次にスナップショット時点と指定された復元時点の間のログ バックアップ データを読み取ります。

### バックアップ データの管理 {#manage-backup-data}

PITR のバックアップ データを管理するには、バックアップ データを格納するためのバックアップ ディレクトリ構造を設計し、古くなったバックアップ データや不要になったバックアップ データを定期的に削除する必要があります。

-   バックアップ データを次の構造で編成します。

    -   スナップショット バックアップとログ バックアップを同じディレクトリに格納して、一元管理します。たとえば、 `backup-${cluster-id}`です。
    -   各スナップショット バックアップは、名前にバックアップ日付が含まれるディレクトリに保存します。たとえば、 `backup-${cluster-id}/snapshot-20220512000130`です。
    -   ログ バックアップを固定ディレクトリに保存します。たとえば、 `backup-${cluster-id}/log-backup`です。

-   古くなった、または不要になったバックアップ データを削除します。

    -   スナップショット バックアップを削除すると、スナップショット バックアップのディレクトリを削除できます。
    -   指定した時点より前のログ バックアップを削除するには、 `br log truncate`コマンドを実行します。

## 機能 {#capabilities}

-   PITR ログ バックアップは、クラスターに 5% の影響を与えます。
-   ログとスナップショットを同時にバックアップすると、クラスターへの影響は 20% 未満になります。
-   各 TiKV ノードで、PITR は 280 GB/h でスナップショット データを復元し、30 GB/h でログ データを復元できます。
-   PITR を使用すると、ディザスタ リカバリの RPO は 20 分未満になります。復元するデータのサイズに応じて、目標復旧時間 (RTO) は数分から数時間まで異なります。
-   BR は、600 GB/h の速度で古いログ バックアップ データを削除します。

> **ノート：**
>
> -   上記の機能仕様は、次の 2 つのテスト シナリオのテスト結果に基づいています。実際のデータは異なる場合があります。
> -   スナップショット データの復元速度 = スナップショット データのサイズ / (期間 * TiKV ノードの数)
> -   ログデータの復元速度 = 復元されたログデータのサイズ / (期間 * TiKV ノードの数)

テスト シナリオ 1 ( [TiDB Cloud](https://tidbcloud.com)で):

-   TiKV ノード数 (8 コア、16 GB メモリ): 21
-   リージョン数: 183,000
-   クラスターで作成された新しいログ: 10 GB/h
-   書き込み (挿入/更新/削除) QPS: 10,000

テスト シナリオ 2 (オンプレミス):

-   TiKV ノード数 (8 コア、64 GB メモリ): 6
-   リージョン数: 50,000
-   クラスターで作成された新しいログ: 10 GB/h
-   書き込み (挿入/更新/削除) QPS: 10,000

## 制限事項 {#limitations}

-   1 つのクラスターで実行できるログ バックアップ タスクは 1 つだけです。
-   空のクラスターにのみデータを復元できます。クラスターのサービスとデータへの影響を避けるために、PITR をインプレースまたは空でないクラスターで実行しないでください。
-   Amazon S3 または共有ファイルシステム (NFS など) を使用して、バックアップ データを保存できます。現在、GCS と Azure Blob Storage はサポートされていません。
-   クラスター レベルの PITR のみを実行できます。データベース レベルおよびテーブル レベルの PITR はサポートされていません。
-   ユーザー テーブルまたは権限テーブルのデータを復元することはできません。
-   バックアップ クラスタに TiFlash レプリカがある場合、PITR を実行した後、復元クラスタには TiFlash レプリカのデータが含まれません。 TiFlash レプリカからデータを復元するには、 [スキーマまたはテーブルで TiFlash レプリカを手動で構成する](/br/pitr-troubleshoot.md#after-restoring-a-downstream-cluster-using-the-br-restore-point-command-data-cannot-be-accessed-from-tiflash-what-should-i-do)を実行する必要があります。
-   アップストリーム データベースが TiDB Lightning の物理インポート モードを使用してデータをインポートする場合、ログ バックアップでデータをバックアップすることはできません。データのインポート後に完全バックアップを実行することをお勧めします。詳細については、 [アップストリーム データベースはTiDB Lightning Physical Mode を使用してデータをインポートします](/br/pitr-known-issues.md#the-upstream-database-imports-data-using-tidb-lightning-in-the-physical-import-mode-which-makes-it-impossible-to-use-the-log-backup-feature)を参照してください。
-   バックアップ プロセス中は、パーティションを交換しないでください。詳細については、 [PITR リカバリ中の Exchange パーティション DDL の実行](/br/pitr-troubleshoot.md#what-should-i-do-if-an-error-occurs-when-executing-the-exchange-partition-ddl-during-pitr-log-restoration)を参照してください。
-   一定期間のログバックアップデータを繰り返し復元しないでください。範囲`[t1=10, t2=20)`のログ バックアップ データを繰り返し復元すると、復元されたデータが不整合になる可能性があります。
-   その他の既知の制限については、 [PITR の既知の問題](/br/pitr-known-issues.md)を参照してください。