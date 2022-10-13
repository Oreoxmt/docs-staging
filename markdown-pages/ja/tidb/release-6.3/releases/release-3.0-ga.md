---
title: TiDB 3.0 GA Release Notes
---

# TiDB 3.0 GA リリースノート {#tidb-3-0-ga-release-notes}

発売日：2019年6月28日

TiDB バージョン: 3.0.0

TiDB アンシブル バージョン: 3.0.0

## 概要 {#overview}

2019 年 6 月 28 日に、TiDB 3.0 GA がリリースされました。対応する TiDB Ansible のバージョンは 3.0.0 です。 TiDB 2.1 と比較すると、このリリースでは次の点が大幅に改善されています。

-   安定性。 TiDB 3.0 は、最大 150 以上のノードと 300 TB 以上のストレージを備えた大規模なクラスターで長期的な安定性を実証しています。
-   使いやすさ。 TiDB 3.0 では、標準化されたスロー クエリ ログ、十分に開発されたログ ファイル仕様、ユーザーの運用コストを節約する`EXPLAIN ANALYZE`や SQL トレースなどの新機能など、使いやすさが多面的に改善されています。
-   パフォーマンス。 TiDB 3.0 のパフォーマンスは、TPC-C ベンチマークで TiDB 2.1 の 4.5 倍、Sysbench ベンチマークで 1.5 倍以上です。ビューのサポートのおかげで、TPC-H 50G Q15 は正常に実行できるようになりました。
-   ウィンドウ関数、ビュー (**実験的**)、分割テーブル、プラグイン フレームワーク、ペシミスティック ロック (<strong>実験的</strong>)、および`SQL Plan Management`を含む新機能。

## TiDB {#tidb}

-   新機能
    -   サポートウィンドウ機能; `NTILE` 、 `LEAD` 、 `LAG` 、 `PERCENT_RANK` 、 `NTH_VALUE` 、 `CUME_DIST` 、 `FIRST_VALUE` 、 `LAST_VALUE` 、 `RANK` 、 `DENSE_RANK` 、および`ROW_NUMBER`を含む、MySQL 8.0 のすべてのウィンドウ関数と互換性があります。
    -   サポート ビュー (**実験的**)
    -   テーブル パーティションの改善
        -   サポート範囲パーティション
        -   ハッシュパーティションをサポート
    -   IP ホワイトリスト ( **Enterprise** ) や監査ログ ( <strong>Enterprise</strong> ) などのプラグインをサポートするプラグイン フレームワークを追加します。
    -   クエリの安定性を確保するために SQL 実行計画バインディングを作成するための SQL 計画管理機能をサポートします (**実験的**)。
-   SQL オプティマイザー
    -   `NOT EXISTS`のサブクエリを最適化し、それを`Anti Semi Join`に変換してパフォーマンスを向上させる
    -   `Outer Join`の定数伝播を最適化し、 `Outer Join`除去の最適化ルールを追加して、効果のない計算を減らし、パフォーマンスを向上させます。
    -   `IN`のサブクエリを最適化して、集計後に`Inner Join`を実行し、パフォーマンスを向上させます
    -   より多くのシナリオに適応するための最適化`Index Join`
    -   Range Partition の Partition Pruning 最適化ルールを改善
    -   クエリ ロジックを`_tidb_rowid`に最適化して、テーブル全体のスキャンを回避し、パフォーマンスを向上させます。
    -   フィルターに関連する列がある場合、複合インデックスのアクセス条件を抽出するときに、より多くのインデックスのプレフィックス列を一致させてパフォーマンスを向上させます
    -   列間の順序相関を使用してコスト見積もりの精度を向上させる
    -   貪欲な戦略と動的計画法アルゴリズムに基づいて最適化`Join Order`を行い、複数のテーブルの結合操作を高速化します。
    -   Skyline Pruning をサポートし、クエリの安定性を向上させるために実行計画が統計に過度に依存するのを防ぐいくつかのルールを使用します。
    -   NULL 値を持つ単一列インデックスの行数推定の精度を向上
    -   各リージョンでランダムにサンプリングする`FAST ANALYZE`をサポートして、完全なテーブル スキャンを回避し、統計収集でパフォーマンスを向上させます
    -   統計収集でパフォーマンスを向上させるために、単調に増加するインデックス列に対するインクリメンタル Analyze 操作をサポートします。
    -   `DO`ステートメントでのサブクエリの使用をサポート
    -   トランザクションで`Index Join`を使用するサポート
    -   パラメーターなしの DDL ステートメントをサポートするために`prepare`を最適化し`execute`
    -   `stats-lease`変数の値が 0 の場合に統計を自動ロードするようにシステムの動作を変更します。
    -   履歴統計のエクスポートをサポート
    -   ヒストグラムの`dump` `load`をサポート
-   SQL 実行エンジン
    -   ログ出力の最適化: トラブルシューティングを容易にするために、 `EXECUTE`つはユーザー変数を出力し、 `COMMIT`はスロー クエリ ログを出力します。
    -   SQLチューニングの使いやすさを向上させる機能を`EXPLAIN ANALYZE`サポート
    -   次の行の ID を取得する`admin show next_row_id`コマンドをサポート
    -   6 つの組み込み関数を追加: `JSON_QUOTE` 、 `JSON_ARRAY_APPEND` 、 `JSON_MERGE_PRESERVE` 、 `BENCHMARK` 、 `COALESCE` 、および`NAME_CONST`
    -   チャンク サイズの制御ロジックを最適化して、クエリ コンテキストに基づいて動的に調整し、SQL の実行時間とリソース消費を削減します。
    -   `TableReader` 、 `IndexReader` 、 `IndexLookupReader`の 3 つの演算子でメモリ使用量の追跡と制御をサポート
    -   空の`ON`条件をサポートするように Merge Join 演算子を最適化する
    -   列が多すぎる単一テーブルの書き込みパフォーマンスを最適化する
    -   逆順でのデータのスキャンをサポートすることで、 `admin show ddl jobs`のパフォーマンスを向上させます
    -   ホットスポットの問題を軽減するためにテーブルリージョンを手動で分割する`split table region`のステートメントを追加します。
    -   `split index region`のステートメントを追加して、インデックスリージョンを手動で分割し、ホットスポットの問題を軽減します
    -   ブロックリストを追加して、式をコプロセッサーにプッシュすることを禁止します
    -   `Expensive Query`のログを最適化して、構成された実行時間またはメモリの制限を超えたときにログに SQL クエリを出力します。
-   DDL
    -   文字セット`utf8`から`utf8mb4`への移行のサポート
    -   デフォルトの文字セットを`utf8`から`utf8mb4`に変更します
    -   データベースの文字セットと照合順序を変更するステートメントを`alter schema`追加します。
    -   サポート ALTER アルゴリズム`INPLACE` / `INSTANT`
    -   サポート`SHOW CREATE VIEW`
    -   サポート`SHOW CREATE USER`
    -   誤って削除されたテーブルの高速リカバリをサポート
    -   ADD INDEX の同時実行数の動的な調整をサポート
    -   `CREATE TABLE`ステートメントを使用してテーブルを作成するときにリージョンを事前に割り当てる`pre_split_regions`オプションを追加して、テーブル作成後の大量の書き込みによって発生する書き込みホット リージョンを軽減します。
    -   ホットスポットの問題を軽減するために、SQL ステートメントを使用して指定されたテーブルのインデックスと範囲によるリージョンの分割をサポートします
    -   `ddl_error_count_limit`のグローバル変数を追加して、DDL タスクの再試行回数を制限します
    -   ホットスポットの問題を軽減するために、列に AUTO_INCREMENT 属性が含まれている場合に行 ID を分散するために`SHARD_ROW_ID_BITS`を使用する機能を追加します
    -   無効な DDL メタデータの有効期間を最適化して、TiDB クラスターのアップグレード後に DDL 操作の通常の実行を回復する速度を上げます
-   取引
    -   ペシミスティック トランザクション モードのサポート (**実験的**)
    -   トランザクション処理ロジックを最適化して、より多くのシナリオに適応します。
        -   デフォルト値`tidb_disable_txn_auto_retry`を`on`に変更します。これは、自動コミットされていないトランザクションが再試行されないことを意味します
        -   `tidb_batch_commit`のシステム変数を追加して、トランザクションを複数のトランザクションに分割して同時に実行する
        -   `tidb_low_resolution_tso`システム変数を追加して、バッチで取得する TSO の数を制御し、トランザクションが TSO を要求する回数を減らして、一貫性の要件が比較的低いシナリオでのパフォーマンスを向上させます。
        -   分離レベルが SERIALIZABLE に設定されている場合にエラーを報告するかどうかを制御する`tidb_skip_isolation_level_check`の変数を追加します。
        -   `tidb_disable_txn_auto_retry`システム変数を変更して、再試行可能なすべてのエラーで機能するようにします。
-   権限管理
    -   `ANALYZE` 、 `USE` 、 `SET GLOBAL` 、および`SHOW PROCESSLIST`ステートメントに対して権限チェックを実行する
    -   役割ベースのアクセス制御 (RBAC) をサポート (**実験的**)
-   サーバ
    -   スロー クエリ ログを最適化します。
        -   ログ形式の再構築
        -   ログの内容を最適化する
        -   ログ クエリ メソッドを最適化して、メモリ テーブルの`INFORMATION_SCHEMA.SLOW_QUERY`ステートメントと`ADMIN SHOW SLOW`ステートメントを使用してスロー クエリ ログをクエリできるようにします。
    -   ツールによる収集と分析を容易にするために、ログシステムを再構築した統一ログフォーマット仕様を開発する
    -   SQL ステートメントを使用した TiDB Binlogサービスの管理をサポートします。これには、ステータスのクエリ、 TiDB Binlogの有効化、TiDB Binlog戦略の維持と送信が含まれます。
    -   `unix_socket`を使用したデータベースへの接続をサポート
    -   SQL ステートメントのサポート`Trace`
    -   トラブルシューティングを容易にするために、 `/debug/zip`の HTTP インターフェイスを介して TiDB インスタンスの情報を取得することをサポートします。
    -   トラブルシューティングを容易にするために監視項目を最適化します。
        -   実際のデータ量と統計に基づく推定データ量の差を監視する監視項目を`high_error_rate_feedback_total`追加
        -   データベース ディメンションに QPS 監視項目を追加する
    -   システム初期化プロセスを最適化して、DDL 所有者のみが初期化を実行できるようにします。これにより、初期化またはアップグレードの起動時間が短縮されます。
    -   `kill query`の実行ロジックを最適化して、パフォーマンスを改善し、リソースが適切に解放されるようにします
    -   起動オプション`config-check`を追加して、構成ファイルの有効性を確認します
    -   `tidb_back_off_weight`システム変数を追加して、内部エラーの再試行のバックオフ時間を制御します
    -   `wait_timeout`および`interactive_timeout`システム変数を追加して、許容される最大アイドル接続を制御します。
    -   接続確立時間を短縮するために、TiKV 用の接続プールを追加します。
-   互換性
    -   `ALLOW_INVALID_DATES` SQLモードをサポート
    -   MySQL 320 Handshake プロトコルをサポート
    -   自動インクリメント列として符号なし BIGINT 列をマニフェストするサポート
    -   `SHOW CREATE DATABASE IF NOT EXISTS`構文をサポート
    -   CSV ファイルのフォールト トレランスを`load data`に最適化する
    -   フィルタリング条件にユーザー変数が含まれている場合、述語プッシュダウン操作を放棄して、ウィンドウ関数をシミュレートするためにユーザー変数を使用する MySQL の動作との互換性を向上させます。

## PD {#pd}

-   単一ノードからのクラスターの再作成をサポート
-   リージョンメタデータを etcd から go-leveldb ストレージ エンジンに移行して、大規模なクラスターの etcd でのストレージのボトルネックを解決します
-   API
    -   Tombstone ストアをクリアする`remove-tombstone`の API を追加します。
    -   `ScanRegions`の API を追加して、リージョン情報をバッチ クエリします
    -   `GetOperator`の API を追加して、実行中のオペレーターを照会します
    -   `GetStores` API のパフォーマンスを最適化する
-   構成
    -   構成アイテムのエラーを回避するために構成チェック ロジックを最適化する
    -   リージョンマージの方向を制御するには、 `enable-two-way-merge`を追加します。
    -   `hot-region-schedule-limit`を追加して、ホット リージョンのスケジュール レートを制御します
    -   複数のしきい値に連続してヒットした場合、ホットスポットを識別するために`hot-region-cache-hits-threshold`を追加します
    -   1 分間に許可されるバランスリージョンオペレーターの最大数を制御する`store-balance-rate`の構成項目を追加します
-   スケジューラの最適化
    -   店舗ごとにオペレーターの速度を個別に制御するための店舗制限メカニズムを追加します
    -   `waitingOperator`のキューをサポートして、異なるスケジューラ間のリソース競合を最適化します
    -   スケジュール操作を TiKV にアクティブに送信するためのスケジュール レート制限をサポートします。これにより、1 つのノードで同時に実行されるスケジューリング タスクの数が制限されるため、スケジューリング レートが向上します。
    -   制限メカニズムに拘束されないように`Region Scatter`のスケジューリングを最適化する
    -   `shuffle-hot-region`のスケジューラーを追加して、ホットスポットのスケジューリングが不十分なシナリオで TiKV の安定性テストを容易にします
-   シミュレーター
    -   データ インポート シナリオ用のシミュレーターを追加する
    -   ストアの異なるハートビート間隔の設定をサポート
-   その他
    -   etcd をアップグレードして、一貫性のないログ出力形式、事前投票でのリーダー選択の失敗、およびリースのデッドロックの問題を解決します。
    -   ツールによる収集と分析を容易にするために、ログシステムを再構築した統一ログフォーマット仕様を開発する
    -   スケジューリング パラメーター、クラスター ラベル情報、PD が TSO 要求を処理するために消費する時間、ストア ID およびアドレス情報などを含む監視メトリックを追加します。

## TiKV {#tikv}

-   分散 GC と同時ロック解決をサポートして、GC パフォーマンスを向上
-   `raw_scan`と`raw_batch_scan`を逆にサポート
-   マルチスレッドの Raftstore とマルチスレッドの適用をサポートして、単一ノード内でのスケーラビリティ、同時実行容量、およびリソースの使用を改善します。同じレベルのプレッシャーの下でパフォーマンスが 70% 向上
-   Raftメッセージのバッチ送受信をサポートし、書き込みが集中するシナリオで TPS を 7% 向上
-   書き込み停止を回避するためにスナップショットを適用する前に RocksDB レベル 0 ファイルのチェックをサポート
-   値のサイズが 1KiB を超えるシナリオの書き込みパフォーマンスを向上させ、書き込み増幅をある程度軽減するキー値プラグインである Titan を導入します。
-   ペシミスティック トランザクション モードのサポート (**実験的**)
-   HTTP 経由での監視情報の取得をサポート
-   キーがない場合にのみプリライトが成功するように、 `Insert`のセマンティクスを変更します。
-   ツールによる収集と分析を容易にするために、ログシステムを再構築した統一ログフォーマット仕様を開発する
-   構成情報とキー バインド クロッシングに関連するパフォーマンス メトリックを追加します。
-   RawKV でローカル リーダーをサポートしてパフォーマンスを向上させる
-   エンジン
    -   メモリ管理を最適化して、メモリの割り当てとコピーを`Iterator Key Bound Option`に減らす
    -   異なる列ファミリー間で`block cache`の共有をサポート
-   サーバ
    -   コンテキスト スイッチのオーバーヘッドを`batch commands`から減らす
    -   `txn scheduler`を削除
    -   `read index`に関連する監視項目を`GC worker`
-   ラフトストア
    -   RaftStore からの CPU 消費を最適化するための Hibernate Regions のサポート (**実験的**)
    -   ローカル リーダー スレッドを削除する
-   コプロセッサー
    -   計算フレームワークをリファクタリングして、ベクトル演算子、ベクトル式を使用した計算、およびベクトル集計を実装してパフォーマンスを向上させます
    -   TiDB の`EXPLAIN ANALYZE`ステートメントのオペレーター実行ステータスの提供をサポート
    -   `work-stealing`スレッド プール モデルに切り替えてコンテキスト スイッチのコストを削減する

## ツール {#tools}

-   TiDB Lightning
    -   データ テーブルのリダイレクト レプリケーションをサポート
    -   CSVファイルのインポートをサポート
    -   SQL から KV ペアへの変換のパフォーマンスを向上させる
    -   単一テーブルのバッチ インポートをサポートしてパフォーマンスを向上させる
    -   TiKV-importer のパフォーマンスを向上させるために、大きなテーブルのデータとインデックスを個別にインポートすることをサポートします
    -   新しいファイルで列データが欠落している場合に、 `row_id`またはデフォルトの列値を使用して欠落している列を埋めることをサポートします
    -   SSTファイルを`TIKV-importer`にアップロードする際の速度制限の設定をサポート
-   Binlog
    -   Drainerに`advertise-addr`の構成を追加して、コンテナー環境でブリッジ モードをサポートします。
    -   Pumpに`GetMvccByEncodeKey`関数を追加して、トランザクション ステータスのクエリを高速化します。
    -   コンポーネント間の通信データの圧縮をサポートして、ネットワーク リソースの消費を削減します。
    -   Kafka からの binlog の読み取りをサポートする Arbiter ツールを追加し、データを MySQL にレプリケートします。
    -   Reparoによるレプリケーションを必要としないファイルの除外をサポート
    -   生成された列の複製をサポート
    -   `syncer.sql-mode`の構成アイテムを追加して、さまざまな SQL モードを使用して DDL クエリを解析することをサポートします
    -   レプリケートされないテーブルのフィルタリングをサポートする`syncer.ignore-table`の構成アイテムを追加します
-   同期差分インスペクター
    -   チェックポイントをサポートして検証ステータスを記録し、再起動後に最後に保存されたポイントから検証を続行します
    -   チェックサムを計算してデータの整合性をチェックする構成項目を`only-use-checksum`追加します
    -   より多くのシナリオに適応するために比較のためにチャンクを分割するために TiDB 統計と複数の列を使用するサポート

## TiDB アンシブル {#tidb-ansible}

-   次の監視コンポーネントを安定したバージョンにアップグレードします。
    -   Prometheus V2.2.1 から V2.8.1 へ
    -   V0.4.0からV0.7.0へのプッシュゲートウェイ
    -   V0.15.2からV0.17.0へのNode_exporter
    -   V0.14.0からV0.17.0へのAlertmanager
    -   V4.6.3 から V6.1.6 への Grafana
    -   V2.5.14 から V2.7.11 までの Ansible
-   TiKV サマリー監視ダッシュボードを追加して、クラスターのステータスを便利に表示します
-   TiKV トラブルシューティング監視ダッシュボードを追加して、重複するアイテムを削除し、トラブルシューティングを容易にします
-   TiKV 詳細監視ダッシュボードを追加して、デバッグとトラブルシューティングを容易にします
-   ローリング更新中にバージョンの一貫性を確認する同時チェックを追加して、更新のパフォーマンスを向上させます
-   TiDB Lightningの展開と運用をサポート
-   テーブルごとのリーダー分布の表示をサポートするために`table-regions.py`のスクリプトを最適化します
-   TiDB 監視を最適化し、SQL カテゴリ別にレイテンシー関連の監視項目を追加
-   オペレーティング システムのバージョン制限を変更して、CentOS 7.0+ および Red Hat 7.0+ オペレーティング システムのみをサポートするようにします。
-   クラスタの最大 QPS を予測するための監視項目を追加します (デフォルトでは非表示)