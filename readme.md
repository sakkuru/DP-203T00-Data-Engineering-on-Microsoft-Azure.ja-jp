# <a name="dp-203t00-data-engineering-on-azure"></a>DP-203T00:Data Engineering in Azure

Welcome to the course DP-203: Data Engineering on Azure. To support this course, we will need to make updates to the course content to keep it current with the Azure services used in the course.  We are publishing the lab instructions and lab files on GitHub to allow for open contributions between the course authors and MCTs to keep the content current with changes in the Azure platform.

## <a name="lab-overview"></a>ラボの概要

各モジュールのラボの目的の概要を次に示します。

### <a name="day-1"></a>Day 1

#### <a name="module-00-lab-environment-setup"></a>[モジュール 00:ラボ環境のセットアップ](Instructions/Labs/LAB_00_lab_setup_instructions.md)

このコースのラボ環境のセットアップを完了します。

#### <a name="module-01-explore-compute-and-storage-options-for-data-engineering-workloads"></a>[モジュール 01:データ エンジニアリング ワークロードのコンピューティングおよびストレージ オプションを確認する](Instructions/Labs/LAB_01_compute_and_storage_options.md)

This lab teaches ways to structure the data lake, and to optimize the files for exploration, streaming, and batch workloads. The student will learn how to organize the data lake into levels of data refinement as they transform files through batch and stream processing. The students will also experience working with Apache Spark in Azure Synapse Analytics.  They will learn how to create indexes on their datasets, such as CSV, JSON, and Parquet files, and use them for potential query and workload acceleration using Spark libraries including Hyperspace and MSSParkUtils.

#### <a name="module-02-run-interactive-queries-using-azure-synapse-analytics-serverless-sql-pools"></a>[モジュール 02:Azure Synapse Analytics サーバーレス SQL プールを使用してインタラクティブなクエリを実行する](Instructions/Labs/LAB_02_queries_using_serverless_sql_pools.md)

DP-203: Data Engineering on Azure コースへようこそ。

#### <a name="module-03-data-exploration-and-transformation-in-azure-databricks"></a>[モジュール 03:Azure Databricks でのデータの探索と変換](Instructions/Labs/LAB_03_data_transformation_in_databricks.md)

このコースをサポートするため、コースの内容を更新して、コースで使用される Azure サービスを最新の状態に維持する必要があります。

### <a name="day-2"></a>Day 2

#### <a name="module-04-explore-transform-and-load-data-into-the-data-warehouse-using-apache-spark"></a>[モジュール 04:Apache Spark を使用してデータの探索と変換を行い、データ ウェアハウスに読み込む](Instructions/Labs/LAB_04_data_warehouse_using_apache_spark.md)

ラボの手順とラボ ファイルは GitHub で公開しています。これにより、コースの作成者と MCT 間でのオープンな作業が可能となり、Azure プラットフォームの変更に合わせてコンテンツを最新の状態に保つことができます。

#### <a name="module-05-ingest-and-load-data-into-the-data-warehouse"></a>[モジュール 05:データ ウェアハウスにデータを取り込んで読み込む](Instructions/Labs/LAB_05_load_data_into_the_data_warehouse.md)

This lab teaches students how to ingest data into the data warehouse through T-SQL scripts and Synapse Analytics integration pipelines. The student will learn how to load data into Synapse dedicated SQL pools with PolyBase and COPY using T-SQL. The student will also learn how to use workload management along with a Copy activity in a Azure Synapse pipeline for petabyte-scale data ingestion.

#### <a name="module-06-transform-data-with-azure-data-factory-or-azure-synapse-pipelines"></a>[モジュール 06:Azure Data Factory または Azure Synapse パイプラインでデータを変換する](Instructions/Labs/LAB_06_transform_data_with_pipelines.md)

このラボでは、データ統合パイプラインを構築して、複数のデータ ソースから取り込み、マッピング データ フローとノートブックを使用してデータを変換し、ひとつ以上のデータシンクにデータを移動する方法を説明します。

### <a name="day-3"></a>Day 3

#### <a name="module-07-integrate-data-from-notebooks-with-azure-data-factory-or-azure-synapse-pipelines"></a>[モジュール 07:ノートブックのデータを Azure Data Factory または Azure Synapse パイプラインと統合する](Instructions/Labs/LAB_07_integrate_data_from_notebooks.md)

In the lab, the students will create a notebook to query user activity and purchases that they have made in the past 12 months. They will then add the notebook to a pipeline using the new Notebook activity and execute this notebook after the Mapping Data Flow as part of their orchestration process. While configuring this the students will implement parameters to add dynamic content in the control flow and validate how the parameters can be used.

#### <a name="module-08-end-to-end-security-with-azure-synapse-analytics"></a>[モジュール 08:Azure Synapse Analytics を使用したエンドツーエンドのセキュリティ](Instructions/Labs/LAB_08_security_with_synapse_analytics.md)

In this lab, students will learn how to secure a Synapse Analytics workspace and its supporting infrastructure. The student will observe the SQL Active Directory Admin, manage IP firewall rules, manage secrets with Azure Key Vault and access those secrets through a Key Vault linked service and pipeline activities. The student will understand how to implement column-level security, row-level security, and dynamic data masking when using dedicated SQL pools.

#### <a name="module-09-support-hybrid-transactional-analytical-processing-htap-with-azure-synapse-link"></a>[モジュール 09:Azure Synapse Link を使用してハイブリッド トランザクション分析処理 (HTAP) に対応する](Instructions/Labs/LAB_09_htap_with_azure_synapse_link.md)

This lab teaches you how Azure Synapse Link enables seamless connectivity of an Azure Cosmos DB account to a Synapse workspace. You will understand how to enable and configure Synapse link, then how to query the Azure Cosmos DB analytical store using Apache Spark and SQL Serverless.
### <a name="day-4"></a>4 日目
#### <a name="module-10-real-time-stream-processing-with-stream-analytics"></a>[モジュール 10: Stream Analyticsを使用したリアルタイムストリーム処理](Instructions/Labs/LAB_10_stream_analytics.md)

This lab teaches you how to process streaming data with Azure Stream Analytics. You will ingest vehicle telemetry data into Event Hubs, then process that data in real time, using various windowing functions in Azure Stream Analytics. You will output the data to Azure Synapse Analytics. Finally, you will learn how to scale the Stream Analytics job to increase throughput.

#### <a name="module-11-create-a-stream-processing-solution-with-event-hubs-and-azure-databricks"></a>[モジュール 11: Event HubsとAzure Databrickを使用してストリーム処理ソリューションの作成](Instructions/Labs/LAB_11_stream_with_azure_databricks.md)

This lab teaches you how to ingest and process streaming data at scale with Event Hubs and Spark Structured Streaming in Azure Databricks. You will learn the key features and uses of Structured Streaming. You will implement sliding windows to aggregate over chunks of data and apply watermarking to remove stale data. Finally, you will connect to Event Hubs to read and write streams.

- <bpt id="p1">**</bpt>Are you a MCT?<ept id="p1">**</ept> - Have a look at our <bpt id="p1">[</bpt>GitHub User Guide for MCTs<ept id="p1">](https://microsoftlearning.github.io/MCT-User-Guide/)</ept>.
                                                                       
## <a name="how-should-i-use-these-files-relative-to-the-released-moc-files"></a>公開済みの MOC ファイルと一緒にこれらのファイルを使用する方法

- 講師ハンドブックと PowerPoint は、引き続きコースのコンテンツを教えるための主要なソースになるでしょう。

- GitHub にあるファイルは、受講者ハンドブックと組み合わせて使えるように設計されています。ただし、MCT とコースの作成者が最新のラボ ファイルのソースを共有できるように、中央リポジトリとして GitHub に置いてあります。

- このラボでは、データ レイクを構成し、探索、ストリーミング、バッチ ワークロードに備えてファイルを最適化する方法を説明します。

- 講師は、ラボを行うたびに、最新の Azure サービスに合わせて修正された個所がないか GitHub を確認し、最新のラボ用ファイルを取得してください。

- バッチおよびストリーム処理によってファイルを変換するときに、データ レイクをデータの洗練度に応じて整理する方法について学習します。

## <a name="what-about-changes-to-the-student-handbook"></a>受講者ハンドブックの変更については?

- 受講者ハンドブックは四半期ごとに見直し、必要があれば通常の MOC リリースの手順を通して更新します。

## <a name="how-do-i-contribute"></a>貢献するには?

- MCT は、GitHub repro のコードまたはコンテンツに問題を送信できます。Microsoft とコース作成者は、必要に応じてコンテンツとラボのコード変更をトリアージして含めます。

## <a name="classroom-materials"></a>コース資料

また、Azure Synapse Analytics で Apache Spark を使用する経験も積みます。

## <a name="what-are-we-doing"></a>説明

- データセットで CSV、JSON、Parquet ファイルのようなインデックスを作成し、これを使用して Hyperspace や MSSParkUtils などの Spark ライブラリでクエリやワークロード アクセラレーションを行う方法を学びます。

- We hope that this brings a sense of collaboration to the labs like we've never had before - when Azure changes and you find it first during a live delivery, go ahead and make an enhancement right in the lab source.  Help your fellow MCTs.

## <a name="how-should-i-use-these-files-relative-to-the-released-moc-files"></a>リリースされた MOC のファイルに対してこれらのファイルを使用する方法

- 講師ハンドブックと PowerPoint は、引き続きコースのコンテンツを教えるための主要なソースになるでしょう。

- GitHub 上のこれらのファイルは学生ハンドブックと組み合わせて使用するように設計されています。ただし、中央リポジトリとして GitHub 内にあるので、MCT とコース作成者が最新のラボ ファイルの共有ソースを持っている可能性があります。

- 講師は、ラボを行うたびに、最新の Azure サービスに合わせて修正された個所がないか GitHub を確認し、最新のラボ用ファイルを取得してください。

## <a name="what-about-changes-to-the-student-handbook"></a>受講者ハンドブックの変更については?

- 受講者ハンドブックは四半期ごとに見直し、必要があれば通常の MOC リリースの手順を通して更新します。

## <a name="how-do-i-contribute"></a>貢献するには?

- すべての MCT は、GitHub repro のコードまたはコンテンツに pull request を送信できます。Microsoft とコース作成者は、必要に応じてコンテンツとラボ コードの変更をトリアージして追加します。

- You can submit bugs, changes, improvement and ideas.  Find a new Azure feature before we have?  Submit a new demo!

## <a name="notes"></a>Notes

### <a name="classroom-materials"></a>コース資料

このラボでは、Azure Synapse Analytics のサーバーレス SQL プールで T-SQL ステートメントを実行し、データ レイクと外部ファイル ソースに格納されているファイルを使用する方法を学びます。
