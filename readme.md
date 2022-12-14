# <a name="dp-203t00-data-engineering-on-azure"></a>DP-203T00:Data Engineering in Azure

DP-203: Data Engineering on Azure コースへようこそ。 このコースをサポートするため、コースの内容を更新して、コースで使用される Azure サービスを最新の状態に維持する必要があります。  ラボの手順とラボ ファイルは GitHub で公開しています。これにより、コースの作成者と MCT 間でのオープンな作業が可能となり、Azure プラットフォームの変更に合わせてコンテンツを最新の状態に保つことができます。

## <a name="lab-overview"></a>ラボの概要

各モジュールのラボの目的の概要を次に示します。

### <a name="day-1"></a>Day 1

#### <a name="module-00-lab-environment-setup"></a>[モジュール 00:ラボ環境のセットアップ](Instructions/Labs/LAB_00_lab_setup_instructions.md)

このコースのラボ環境のセットアップを完了します。

#### <a name="module-01-explore-compute-and-storage-options-for-data-engineering-workloads"></a>[モジュール 01:データ エンジニアリング ワークロードのコンピューティングおよびストレージ オプションを確認する](Instructions/Labs/LAB_01_compute_and_storage_options.md)

このラボでは、データ レイクを構成し、探索、ストリーミング、バッチ ワークロードに備えてファイルを最適化する方法を説明します。 バッチおよびストリーム処理によってファイルを変換するときに、データ レイクをデータの洗練度に応じて整理する方法について学習します。 また、Azure Synapse Analytics で Apache Spark を使用する経験も積みます。  データセットで CSV、JSON、Parquet ファイルのようなインデックスを作成し、これを使用して Hyperspace や MSSParkUtils などの Spark ライブラリでクエリやワークロード アクセラレーションを行う方法を学びます。

#### <a name="module-02-run-interactive-queries-using-azure-synapse-analytics-serverless-sql-pools"></a>[モジュール 02:Azure Synapse Analytics サーバーレス SQL プールを使用してインタラクティブなクエリを実行する](Instructions/Labs/LAB_02_queries_using_serverless_sql_pools.md)

このラボでは、Azure Synapse Analytics のサーバーレス SQL プールで T-SQL ステートメントを実行し、データ レイクと外部ファイル ソースに格納されているファイルを使用する方法を学びます。 学生は、データ レイクに格納されている Parquet ファイルと、外部データ ストアに格納されている CSV ファイルに対してクエリを実行します。 次に、Azure Active Directory セキュリティ グループを作成し、ロールベースのアクセス制御 (RBAC) とアクセス制御リスト (ACL) を介してデータ レイク内のファイルへのアクセスを強制します。

#### <a name="module-03-data-exploration-and-transformation-in-azure-databricks"></a>[モジュール 03:Azure Databricks でのデータの探索と変換](Instructions/Labs/LAB_03_data_transformation_in_databricks.md)

このラボでは、さまざまな Apache Spark DataFrame メソッドを使用して、Azure Databricks でデータを探索して変換する方法を説明します。 受講者は、標準的な DataFrame メソッドを実行してデータの探索と変換を行う方法を学びます。 また、重複データの削除や、日時の値の操作、列の名前変更、データの集計など、より高度なタスクを実行する方法を学習します。 選択した取り込み技術をプロビジョニングし、これを Stream Analytics と統合して、ストリーミング データで動作するソリューションを作成します。

### <a name="day-2"></a>Day 2

#### <a name="module-04-explore-transform-and-load-data-into-the-data-warehouse-using-apache-spark"></a>[モジュール 04:Apache Spark を使用してデータの探索と変換を行い、データ ウェアハウスに読み込む](Instructions/Labs/LAB_04_data_warehouse_using_apache_spark.md)

このラボでは、データ レイクに格納されているデータを探索し、データを変換して、リレーショナル データ ストアにデータを読み込む方法を学びます。 Parquet ファイルと JSON ファイルを探索し、階層構造を使用して JSON ファイルのクエリと変換を実行する技術を使用します。 その後、Apache Spark を使用してデータをデータ ウェアハウスに読み込み、データ レイクの Parquet データを専用 SQL プールのデータに統合します。

#### <a name="module-05-ingest-and-load-data-into-the-data-warehouse"></a>[モジュール 05:データ ウェアハウスにデータを取り込んで読み込む](Instructions/Labs/LAB_05_load_data_into_the_data_warehouse.md)

このラボでは、T-SQL スクリプトと Synapse Analytics 統合パイプラインを介してデータ ウェアハウスにデータを取り込む方法を説明します。 PolyBase を使用して Synapse 専用 SQL プールにデータを読み込む方法と、T-SQL を使用した COPY について学習します。 また、ワークロード管理を Azure Synapse パイプラインの Copy アクティビティと共に使用してペタバイト規模のデータ インジェストを行う方法についても学習します。

#### <a name="module-06-transform-data-with-azure-data-factory-or-azure-synapse-pipelines"></a>[モジュール 06:Azure Data Factory または Azure Synapse パイプラインでデータを変換する](Instructions/Labs/LAB_06_transform_data_with_pipelines.md)

このラボでは、データ統合パイプラインを構築して、複数のデータ ソースから取り込み、マッピング データ フローとノートブックを使用してデータを変換し、ひとつ以上のデータシンクにデータを移動する方法を説明します。

### <a name="day-3"></a>Day 3

#### <a name="module-07-integrate-data-from-notebooks-with-azure-data-factory-or-azure-synapse-pipelines"></a>[モジュール 07:ノートブックのデータを Azure Data Factory または Azure Synapse パイプラインと統合する](Instructions/Labs/LAB_07_integrate_data_from_notebooks.md)

このラボでは、過去 12 ヶ月間のユーザーのアクティビティと購入を照会するためにノートブックを作成します。 その後、新しいノートブック アクティビティを使用してノートブックをパイプラインに追加し、調整プロセスの一環としてマッピング データ フローの後でこのノートブックを実行します。 これを構成する間に、制御フローでダイナミック コンテンツを追加し、どのようにパラメーターを使用できるのか検証します。

#### <a name="module-08-end-to-end-security-with-azure-synapse-analytics"></a>[モジュール 08:Azure Synapse Analytics を使用したエンドツーエンドのセキュリティ](Instructions/Labs/LAB_08_security_with_synapse_analytics.md)

このラボでは、Synapse Analytics ワークスペースとその補助インフラストラクチャを保護する方法を学習します。 学生は、SQL Active Directory 管理者、IP ファイアウォール規則の管理、Azure Key Vault を使用したシークレットの管理、および Key Vault のリンク サービスとパイプラインアクティビティを使用したシークレットへのアクセスを行います。 専用 SQL プールを使用するときに、列レベルのセキュリティ、行レベルのセキュリティ、および動的データ マスキングを実装する方法を理解します。

#### <a name="module-09-support-hybrid-transactional-analytical-processing-htap-with-azure-synapse-link"></a>[モジュール 09:Azure Synapse Link を使用してハイブリッド トランザクション分析処理 (HTAP) に対応する](Instructions/Labs/LAB_09_htap_with_azure_synapse_link.md)

このラボでは、Azure Synapse Link によって Azure Cosmos DB アカウントを Synapse ワークスペースにシームレスに接続する方法を学習します。 Synapse Link を有効にして構成する方法、および Apache Spark プールと SQL サーバーレス プールを使用して Azure Cosmos DB 分析ストアのクエリを行う方法を学びます。
### <a name="day-4"></a>4 日目
#### <a name="module-10-real-time-stream-processing-with-stream-analytics"></a>[モジュール 10: Stream Analyticsを使用したリアルタイムストリーム処理](Instructions/Labs/LAB_10_stream_analytics.md)

このラボでは、Azure Stream Analytics を使用してストリーミング データを処理する方法を学習します。 車両のテレメトリ データを Event Hubs に取り込んだ後、Azure Stream Analytics のさまざまなウィンドウ化関数を使用してリアルタイムでそのデータを処理します。 データは Azure Synapse Analytics に出力されます。 最後に、スループットを増やすために Stream Analytics ジョブのスケーリングを行う方法を学びます。

#### <a name="module-11-create-a-stream-processing-solution-with-event-hubs-and-azure-databricks"></a>[モジュール 11: Event HubsとAzure Databrickを使用してストリーム処理ソリューションの作成](Instructions/Labs/LAB_11_stream_with_azure_databricks.md)

このラボでは、Azure Databricks で Event Hubs と Spark Structured Streaming を使用して大規模なストリーミング データの取り込みと処理を行う方法を学習します。 構造化ストリーミングの主な機能と使用方法について学びます。 スライディング ウィンドウを実装して、データのチャンクで集計を行い、基準値を適用して古いデータを削除します。 最後に、Event Hubs に接続して、ストリームの読み取りと書き込みを行います。

- **あなたは MCT ですか?** - [MCT 向けの GitHub ユーザー ガイド](https://microsoftlearning.github.io/MCT-User-Guide/)をご覧ください。
                                                                       
## <a name="how-should-i-use-these-files-relative-to-the-released-moc-files"></a>公開済みの MOC ファイルと一緒にこれらのファイルを使用する方法

- 講師ハンドブックと PowerPoint は、引き続きコースのコンテンツを教えるための主要なソースになるでしょう。

- GitHub にあるファイルは、受講者ハンドブックと組み合わせて使えるように設計されています。ただし、MCT とコースの作成者が最新のラボ ファイルのソースを共有できるように、中央リポジトリとして GitHub に置いてあります。

- 各モジュールのラボの手順は /Instructions/Labs フォルダーに含まれています。 このフォルダー内の各サブフォルダーは各モジュールを参照しています。 たとえば、Lab01 は module01 などに関連付けされています。ラボの手順の各フォルダーには、受講者が従うことのできる README.md ファイルがあります。

- 講師は、ラボを行うたびに、最新の Azure サービスに合わせて修正された個所がないか GitHub を確認し、最新のラボ用ファイルを取得してください。

- ラボの手順に掲載されている画像の中には、このコースで使用するラボの環境の状態を必ずしも反映していないものもあります。 たとえば、データ レイクでファイルを参照する際、実際の環境では存在しない追加フォルダーが画像に表示されている可能性があります。 これは意図的なもので、ラボの手順には影響しません。

## <a name="what-about-changes-to-the-student-handbook"></a>受講者ハンドブックの変更については?

- 受講者ハンドブックは四半期ごとに見直し、必要があれば通常の MOC リリースの手順を通して更新します。

## <a name="how-do-i-contribute"></a>貢献するには?

- MCT は、GitHub repro のコードまたはコンテンツに問題を送信できます。Microsoft とコース作成者は、必要に応じてコンテンツとラボのコード変更をトリアージして含めます。

## <a name="classroom-materials"></a>コース資料

MCT とパートナーが、これらの資料にアクセスし、学生に個別に提供することを強く推奨します。  進行中のクラスの一部としてラボ ステップにアクセスできるように、学生に直接 GitHub を指示するには、学生がコースの一部として別の UI にもアクセスする必要がありますが、これは混乱の原因となります。 個別のラボの手順を受け取る理由を学生に説明すると、クラウドベースのインターフェイスとプラットフォームが常に変化しているという性質を強調できます。 GitHub 上のファイルにアクセスするための Microsoft Learning サポートと GitHub サイトのナビゲーションのサポートは、このコースのみを教える MCT に限定されます。

## <a name="what-are-we-doing"></a>説明

- このコースをサポートするには、コースで使用される Azure サービスを最新の状態に保つために、コース コンテンツを頻繁に更新する必要があります。  ラボの手順とラボ ファイルは GitHub で公開しています。これにより、コースの作成者と MCT 間でのオープンな作業が可能となり、Azure プラットフォームの変更に合わせてコンテンツを最新の状態に保つことができます。

- これにより、今まで経験したことがないようなコラボレーション感がラボに生まれると期待しています。Azure が変更され、ライブ配信中にあなたがそれを最初に見つけた場合は、ラボ ソースで機能強化を行ってください。  仲間の MCT を支援しましょう。

## <a name="how-should-i-use-these-files-relative-to-the-released-moc-files"></a>リリースされた MOC のファイルに対してこれらのファイルを使用する方法

- 講師ハンドブックと PowerPoint は、引き続きコースのコンテンツを教えるための主要なソースになるでしょう。

- GitHub 上のこれらのファイルは学生ハンドブックと組み合わせて使用するように設計されています。ただし、中央リポジトリとして GitHub 内にあるので、MCT とコース作成者が最新のラボ ファイルの共有ソースを持っている可能性があります。

- 講師は、ラボを行うたびに、最新の Azure サービスに合わせて修正された個所がないか GitHub を確認し、最新のラボ用ファイルを取得してください。

## <a name="what-about-changes-to-the-student-handbook"></a>受講者ハンドブックの変更については?

- 受講者ハンドブックは四半期ごとに見直し、必要があれば通常の MOC リリースの手順を通して更新します。

## <a name="how-do-i-contribute"></a>貢献するには?

- すべての MCT は、GitHub repro のコードまたはコンテンツに pull request を送信できます。Microsoft とコース作成者は、必要に応じてコンテンツとラボ コードの変更をトリアージして追加します。

- バグ、変更、改善、アイデアを送信できます。  新しい Azure 機能を先に見つけたら、  新しいデモを送信してください!

## <a name="notes"></a>Notes

### <a name="classroom-materials"></a>コース資料

MCT とパートナーが、これらの資料にアクセスし、学生に個別に提供することを強く推奨します。  進行中のクラスの一部としてラボ ステップにアクセスできるように、学生に直接 GitHub を指示するには、学生がコースの一部として別の UI にもアクセスする必要がありますが、これは混乱の原因となります。 個別のラボの手順を受け取る理由を学生に説明すると、クラウドベースのインターフェイスとプラットフォームが常に変化しているという性質を強調できます。 GitHub 上のファイルにアクセスするための Microsoft Learning サポートと GitHub サイトのナビゲーションのサポートは、このコースを教える MCT のみに限定されています。
