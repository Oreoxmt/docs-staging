---
title: SQL Tuning Overview
---

# SQLチューニングの概要 {#sql-tuning-overview}

SQLは宣言型言語です。つまり、SQLステートメントは、順番に実行する一連のステップではなく*、最終結果がどのようになるか*を記述します。 TiDBは実行を最適化し、説明されているように最終結果を正しく返すという条件で、クエリの一部を任意の順序で実行することを意味的に許可されます。

SQL最適化との有用な比較は、GPSナビゲーションを使用したときに何が起こるかを説明することです。あなたが提供した住所、 *2955 Campus Drive San Mateo CA 94403*から、GPSソフトウェアはあなたをルーティングするための最も時間効率の良い方法を計画します。これは、以前の旅行などのさまざまな統計、制限速度などのメタデータ、および最近の場合は交通情報のライブフィードを利用する場合があります。これらのアナロジーのいくつかはTiDBに変換されます。

このセクションでは、クエリの実行に関するいくつかの概念を紹介します。

-   [クエリ実行プランを理解する](/explain-overview.md)は、TiDBがステートメントの実行をどのように決定したかを理解するために`EXPLAIN`ステートメントを使用する方法を紹介します。
-   [SQL最適化プロセス](/sql-optimization-concepts.md)は、クエリ実行パフォーマンスを向上させるためにTiDBが使用できる最適化を示しています。
-   [実行計画の管理](/control-execution-plan.md)は、実行プランの生成を制御する方法を紹介します。これは、TiDBによって決定された実行プランが最適ではない場合に役立ちます。