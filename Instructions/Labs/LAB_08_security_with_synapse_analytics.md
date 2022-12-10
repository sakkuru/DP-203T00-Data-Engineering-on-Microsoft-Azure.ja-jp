---
lab:
  title: Azure Synapse Analytics を使用したエンドツーエンドのセキュリティ
  module: Module 8
---

# <a name="lab-8---end-to-end-security-with-azure-synapse-analytics"></a>ラボ 8 - Azure Synapse Analytics を使用したエンドツーエンドのセキュリティ

このラボでは、Synapse Analytics ワークスペースとその補助インフラストラクチャを保護する方法を学習します。 SQL Active Directory Admin の観察、IP ファイアウォール ルールの管理、Azure Key Vault を使用したシークレットの管理、Key Vault にリンクされたサービスとパイプライン アクティビティによるシークレットへのアクセスを実行します。 専用 SQL プールを使用する際の列レベルのセキュリティ、行レベルのセキュリティ、動的データ マスクの実装方法を学びます。

このラボを完了すると、次のことができるようになります。

- *インフラストラクチャをサポートするセキュアなAzureSynapse Analytics
- Azure Synapse Analyticsワークスペースとマネージドサービスの保護
- Azure Synapse Analyticsワークスペースデータの保護

このラボでは、Azure Synapse Analytics のエンドツーエンドのセキュリティを対象とした複数のセキュリティ関連の手順を説明します。 このラボのキー ポイントのいくつかは以下のとおりです。

1. Azure Key Vault を利用すると機密の接続情報を格納できます (アクセス キー、リンク サービスやパイプラインのパスワードなど)。

2. 潜在的な機密データの開示という観点から SQL プール内に含まれているデータを調べます。 機密データを代表する列を識別し、列レベルのセキュリティを追加して保護します。 特定のユーザー グループからどのデータを非表示にすべきかについてテーブル レベルで判断し、セキュリティ述語を定義して、テーブルで行レベルのセキュリティ (フィルター) を適用します。 希望する場合は、動的データ マスクを適用して、列ごとにクエリで返された機密データをマスキングするオプションもあります。

## <a name="lab-setup-and-pre-requisites"></a>ラボの構成と前提条件

このラボを開始する前に、「**ラボ 4: *Apache Spark を使用してデータの探索と変換を行い、データ ウェアハウスに読み込む***」のセットアップ手順を少なくとも完了する必要があります。

このラボでは、前のラボで作成した専用 SQL プールを使用します。 前のラボの最後で SQL プールを一時停止しているはずなので、次の手順に従って再開します。

1. Azure Synapse Studio を開きます (<https://web.azuresynapse.net/>)。
2. **[管理]** ハブを選択します。
3. 左側のメニューで **[SQL プール]** を選択します。 **SQLPool01** 専用 SQL プールが一時停止状態の場合は、名前の上にマウスを動かして、**[&#9655;]** を選択します。

    ![専用 SQL プールで再開ボタンが強調表示されています。](images/resume-dedicated-sql-pool.png "再開")

4. プロンプトが表示されたら、**[再開]** を選択します。 プールが再開するまでに、1 ～ 2 分かかります。
5. 専用 SQL プールが再開する間、続行して次の演習に進みます。

> **重要:** 開始されると、専用 SQL プールは、一時停止されるまで Azure サブスクリプションのクレジットを消費します。 このラボを休憩する場合、またはラボを完了しないことにした場合は、ラボの最後にある指示に従って、SQL プールを一時停止してください。

## <a name="exercise-1---securing-azure-synapse-analytics-supporting-infrastructure"></a>演習 1 - Azure Synapse Analytics の補助インフラストラクチャを保護する

Azure Synapse Analytics (ASA) は、作成して管理する多くのリソースのセキュリティに対応する強力なソリューションです。 ただし、ASA を実行するには、基本的なセキュリティ対策を講じて、依存するインフラストラクチャのセキュリティを確保する必要があります。 この演習では、ASA の補助インフラストラクチャの保護について説明します。

### <a name="task-1---observing-the-sql-active-directory-admin"></a>タスク 1 - SQL Active Directory 管理者を観察する

 SQL Active Directory 管理者は、ユーザー (既定) またはグループ (複数のユーザーに強化を提供できるのでベスト プラクティス) のセキュリティ プリンシパルです。 これを割り当てられたプリンシパルには、ワークスペースに含まれている SQL プールへの管理権限が与えられます。

1. Azure portal (<https://portal.azure.com>) で、ラボのリソース グループを参照し、リソース リストから Synapse ワークスペースを開きます (Synapse Studio を起動しないでください)。

2. 左側のメニューで **[Azure Active Directory]** を選択し、SQL Active Directory 管理者としてリストされているのが、ユーザーであるのか、グループであるのかを確認します。

    ![[SQL Active Directory 管理者] 画面の左側のメニューで、SQL Active Directory 管理者が選択されており、Active Directory 管理者が強調表示されています。](images/lab5_workspacesqladadmin.png)

### <a name="task-2---manage-ip-firewall-rules"></a>タスク 2 - IP ファイアウォール規則を管理する

強健なインターネット セキュリティは、あらゆる技術システムの必須要素です。 インターネットの脅威ベクターを緩和するひとつの方法が、IP ファイアウォール規則を使用してAzure Synapse Analytics ワークスペースにアクセスできるパブリック IP アドレスの数を減らすことです。 その後、Azure Synapse Analytics ワークスペースは、SQL プールと SQL サーバーレス エンドポイントなど、ワークスペースのあらゆるマネージド パブリック エンドポイントに同じ規則を付与します。

1. Azure portal の Synapse ワークスペースのブレードで **[ネットワーク]** を選択します。

2. **[すべて許可]** の IP ファイアウォール規則が既にラボ環境で作成されていることがわかります。 特定の IP アドレスを追加する必要がある場合は、**[+ クライアント IP の追加]** をタスクバー メニューで選択します (すぐに実行する必要はありません)。

    ![Synapse ワークスペース画面で [+ クライアント IP の追加] ボタンがツールバー メニューで選択されています。](images/lab5_synapsefirewalladdclientipmenu.png)

    > **注**:ローカル ネットワークから Synapse に接続する際、特定のポートを開く必要があります。 Synapse Studio の関数に対応できるよう、送信 TCP ポート 80、443、1433、および UDP ポート 53 が開いていることを確認してください。

## <a name="exercise-2---securing-the-azure-synapse-analytics-workspace-and-managed-services"></a>演習 2 - Azure Synapse Analytics ワークスペースとマネージド サービスを保護する

### <a name="task-1---managing-secrets-with-azure-key-vault"></a>タスク 1 - Azure Key Vault を使用してシークレットを管理する

外部データ ソースとサービスへの接続を行う際、パスワードやアクセス キーなどの機密接続情報を適切に扱う必要があります。 このような情報は、Azure Key Vault で格納することが推奨されます。 Azure Key Vault を使用すると、シークレットを保護できるだけでなく、中心的な事実のソースにもなります。つまり、シークレットの値を更新する必要がある場合 (ストレージ アカウントでアクセス キーのサイクリングを行う場合など)、一か所で変更することができ、このキーを使用するすべてのサービスは即時、新しい値を利用できます。 Azure Key Vault は、256 ビット AES 暗号化 (FIPS 140-2 準拠) を利用して透過的に情報を暗号化および復号化します。

1. Azure portal で、このラボのリソース グループを開き、リソースのリストから **[キー コンテナー]** リソースを選択します。

    ![リソース グループでキー コンテナーが強調表示されています。](images/resource-group-key-vault.png "Key Vault")

2. 左側のメニューの [設定] で、**[アクセス ポリシー]** を選択します。

3. 使用している Synapse ワークスペースを表すマネージド サービス ID (MSI) (名前は **asaworkspace*xxxxxxx*** のようになります) が既に [アプリケーション] に一覧表示されており、選択された 4 つのシークレットの管理操作があることを確認します。

    ![Synapse ワークスペース アカウントと、割り当てられたシークレットのアクセス許可が強調表示されています。](images/key-vault-access-policies.png "アクセス ポリシー")

4. **[シークレット管理操作]** で **[4 つ選択済み]** というドロップダウンを選択し、**Get** (ワークスペースでキー コンテナーからシークレットの値を取得することが可能) と **List** (ワークスペースでシークレットを列挙することが可能) が設定されていることを確認します。

### <a name="task-2---use-azure-key-vault-for-secrets-when-creating-linked-services"></a>タスク 2 - リンク サービスの作成時にシークレット向けに Azure Key Vault を使用する

リンク サービスは Azure Synapse Analytics の接続文字列の同意語です。 Azure Synapse Analytics リンク サービスでは、Azure ストレージ アカウントから Amazon S3 にいたるまで 100 種類近くの外部サービスに接続することが可能です。 外部サービスに接続する際は、ほぼ必ず接続情報に関するシークレットが含まれています。 このようなシークレットの格納に最適な場所が Azure Key Vault です。 Azure Synapse Analytics には、Azure Key Vault からの値を使用してあらゆるリンク サービス接続を構成する機能が備わっています。

リンク サービスで Azure Key Vault を利用するには、まず、リンク サービスとして、キー コンテナー リソースを Azure Synapse Analytics で追加する必要があります。

1. Azure Synapse Studio で、左側のメニューから **[管理]** ハブを選択します。

    ![管理ハブが選択されています。](images/manage-hub.png "管理ハブ")

2. **[外部接続]** で **[リンク サービス]** を選択し、キー コンテナーをポイントするリンク サービスが環境内で作成されていることを確認します。

    ![キー コンテナーのリンク サービスが強調表示されています。](images/key-vault-linked-service.png "Key Vault のリンクされたサービス")

Azure Key Vault はリンク サービスとして設定されているため、新しいリンク サービスを定義する際にこれを活用できます。 新しいリンク サービスすべてに、Azure Key Vault からシークレットを取得するオプションが含まれています。 フォームでは、Azure Key Vault リンク サービス、シークレット名のほか、オプションでシークレットの特定のバージョンを選択するよう求められます。

![新しいリンク サービスのフォームが表示され、Azure Key Vault の設定が、前の段落で説明されたフィールドとともに強調表示されています。](images/lab5_newlinkedservicewithakv.png)

### <a name="task-3---secure-workspace-pipeline-runs"></a>タスク 3 - ワークスペース パイプライン実行を保護する

パイプラインの一部であるシークレットは Azure Key Vault に格納するよう推奨されています。 このタスクでは、Web アクティビティを使用し、これらの値を取得してその仕組みを示します。 このタスクの 2 番目の部分では、パイプラインで Web アクティビティを使用し、Key Vault からシークレットを取得します。

1. Azure Portal に戻ります。

2. **asakeyvault*xxxxxxx*** Azure Key Vault リソースのブレードの左側のメニューで、 **[シークレット]** を選択します。 次に、上部のツールバーで **[+ 生成/インポート]** を選択します。

   ![Azure Key Vault では [シークレット] が左側のメニューで選択され、上部のツールバーでは [+ 生成/インポート] が選択されています。](images/lab5_pipelinekeyvaultsecretmenu.png)

3. `PipelineSecret` という名前のシークレットを作成し、`IsNotASecret` の値を割り当てて、 **[作成]** ボタンを選択します。

   ![[シークレットの作成] フォームが表示され、指定された値が読み込まれています。](images/lab5_keyvaultcreatesecretforpipeline.png)

4. 作成したばかりのシークレットを開き、現在のバージョンまで進み、"シークレット識別子" フィールドの値をコピーします。 テキスト エディターでこの値を保存するか、クリップボードに維持して今後の手順で使用できるようにします。

    ![[シークレット バージョン] フォームで、"シークレット識別子" テキストフィールドの横にある [コピー] アイコンが選択されています。](images/lab5_keyvaultsecretidentifier.png)

5. Synapse Studio に戻り、左側のメニューで **[統合]** ハブを選択します。

    ![[統合] ハブ。](images/integrate-hub.png "[統合] ハブ")

6. **[統合]** ペインの [ **+** ] メニューで、 **[パイプライン]** を選択します。

    ![[統合] ブレードで [+] ボタンが展開されており、その下でパイプライン項目が選択されています。](images/new-pipeline.png)

7. **[パイプライン]** タブの **[アクティビティ]** ペインで「**Web**」を検索し、**Web** アクティビティのインスタンスを設計領域にドラッグします。

    ![[アクティビティ] ペインで、検索フィールドに「Web」と入力されています。 [全般] の検索結果に Web アクティビティが表示されています。 矢印は、パイプラインのデザイン領域へのアクティビティのドラッグ アンド ドロップの動きを示しています。 Web アクティビティは、デザイン領域に表示されています。](images/lab5_pipelinewebactivitynew.png)

8. **Web1** Web アクティビティを選択し、**[設定]** タブを選択します。次のようにフォームに入力します。

    1. **URL**: 上記の手順 4 でコピーした Key Vault シークレット識別子の値を貼り付け、`?api-version=7.1` をこの値の最後に**追加**します。 たとえば、次のようになります: `https://asakeyvaultNNNNN.vault.azure.net/secrets/PipelineSecret/f808d4fa99d84861872010f6c8d25c68?api-version=7.1`。
  
    2. **メソッド**: **[Get]** を選択します。

    3. **[認証]** で **[マネージド ID]** を選択する Synapse ワークスペースのマネージド サービス ID 向けのアクセス ポリシーはすでに確立されています。つまり、パイプライン アクティビティには、HTTP 呼び出しでキー コンテナーにアクセスする許可があります。
  
    4. **リソース**: 「**<https://vault.azure.net>**」と入力します

        ![[Web アクティビティ設定] タブが選択されており、上記の値がフォームに読み込まれています。](images/lab5_pipelineconfigurewebactivity.png)

9. [アクティビティ] ペインで、**[Set variable]** アクティビティをパイプラインの設計領域に追加します。

    ![矢印は、アクティビティの変数設定項目からパイプライン キャンバスに向かっています。](images/pipeline-activities-set-variable.png "アクティビティ:変数の設定")

10. パイプラインの設計領域で **[Web1]** アクティビティを選択し、**[成功]** アクティビティのパイプライン接続 (緑色のボックス) を **[Set variable1]** アクティビティにドラッグします。

11. デザイナーで選択したパイプラインを選択した状態で (どちらのアクティビティも選択されていない場合など)、 **[変数]** タブを選択し、 **`SecretValue`** という名前の新しい**文字列**変数を追加します。

      ![パイプラインのデザイン領域が表示され、新しいパイプラインの矢印は Web1 と変数の設定アクティビティを接続しています。 パイプラインが選択されており、デザイン領域の下では [変数] タブが選択され、SecretValue という名前の変数が強調表示されています。](images/lab5_newpipelinevariable.png)

12. **[Set variable1]** アクティビティを選択し、**[変数]** タブを選択します。次のようにフォームに入力します。

    1. **[名前]** : **SecretValue** を選択します (作成したばかりの変数)。

    2. **値**: 「`@activity('Web1').output.value`」と入力します

        ![パイプライン デザイナーで、[Set Variable1] アクティビティが選択されています。 デザイナーの下で [変数] タブが選択されており、フォームは以前に指定された値で設定されています。](images/lab5_pipelineconfigsetvaractivity.png)

13. ツールバー メニューで **[デバッグ]** を選択し、パイプラインのデバッグを実行します。 実行時には、パイプラインの **[出力]** タブで両方のアクティビティの入力と出力を確認します。

    ![パイプライン ツールバーが表示され、デバッグ項目が強調表示されています。](images/lab5_pipelinedebugmenu.png)

    ![パイプラインの出力で、[Set variable1] アクティビティが選択されており、その入力が表示されています。 入力には、SecretValue パイプライン変数に割り当てられているキー コンテナーからプルされた NotASecret の値が示されています。](images/lab5_pipelinesetvariableactivityinputresults.png)

    > **注**: **[Web1]** アクティビティの **[全般]** タブには、**[セキュリティで保護された出力]** チェックボックスがあります。このチェックボックスが選択されている場合、シークレット値のログはプレーン テキストで記録されません。たとえば、パイプライン実行で、キー コンテナーから取得した実際の値の代わりにマスキングされた値 ***** が表示されます。 この値を使用するアクティビティでは、**[セキュリティで保護された入力]** チェックボックスも選択されています。

### <a name="task-4---secure-azure-synapse-analytics-dedicated-sql-pools"></a>タスク 4 - Azure Synapse Analytics 専用 SQL プールを保護する

Transparent Data Encryption (TDE) は SQL サーバーの機能で、保存中のデータ (データベース、ログ ファイル、バックアップなど) の暗号化と復号化を行います。 Synapse Analytics 専用 SQL プールでこの機能を使用する場合は、プールによって提要された組み込み対象データベース暗号化キー (DEK) を使用します。 TDE を使用すると、格納されているデータがすべてディスクで暗号化されます。データを要請すると、TDE がこのデータをページ レベルで復号化します (メモリに読み込まれているため)。また、ディスクに再び書き込む前にメモリ内のデータを暗号化します。 名前と同様、これは透過的に行われ、アプリケーション コードには影響しません。 Synapse Analytics を介して専用 SQL プールを作成する際、Transparent Data Encryption は有効ではありません。 このタスクの最初の部分には、この機能を有効にする方法が示されます。

1. **Azure Portal** でリソース グループを開き、**SqlPool01** 専用 SQL プール リソースを見つけて開きます。

    ![SQLPool01 リソースがリソース グループで強調表示されています。](images/resource-group-sqlpool01.png "リソース グループ:SQLPool01")

2. **[SQL プール]** リソース画面の左側のメニューで、**[Transparent data encryption]** を選択します。 データ暗号化は**有効にしない**でください。

   ![SQL プール リソース画面でメニューから Transparent data encryption が選択されています。](images/tde-form.png)

    既定で、このオプションはオフになっています。 この専用 SQL プールでデータ暗号化を有効にすると、TDE が適用される間、プールは数分間、オフラインになります。

## <a name="exercise-3---securing-azure-synapse-analytics-workspace-data"></a>演習 3 - Azure Synapse Analytics ワークスペース データを保護する

### <a name="task-1---column-level-security"></a>タスク 1 - 列 レベルのセキュリティ

機密情報を保持しているデータ列を特定することが重要です。 機密の種類は、社会保障番号、メール アドレス、クレジットカード番号、財務合計などです。 Azure Synapse Analytics では、許可を定義し、ユーザーまたはロールが特定の列の権限を選択することを防ぎます。

1. **Azure Synapse Studio** の **[開発]** ハブで、**[SQL スクリプト]** セクションを展開し、**[列レベルのセキュリティ]** を選択します。
2. ツールバーで、**SQLPool01** データベースに接続します。
3. クエリ ウィンドウで、ステップのステートメントを強調表示し、ツールバーから **[実行]** ボタンを選択して (または **F5** キーを押して)、**各ステップを個々に実行**します。
4. [スクリプト] タブを閉じます。プロンプトが表示されたら、**[すべての変更を破棄する]** を選択します。

### <a name="task-2---row-level-security"></a>タスク 2 - 行レベルのセキュリティ

1. **[開発]** ハブの **[SQL スクリプト]** セクションで、**[列レベルのセキュリティ]** を選択します。
2. ツールバーで、**SQLPool01** データベースに接続します。
3. クエリ ウィンドウで、ステップのステートメントを強調表示し、ツールバーから **[実行]** ボタンを選択して (または **F5** キーを押して)、**各ステップを個々に実行**します。
4. [スクリプト] タブを閉じます。プロンプトが表示されたら、**[すべての変更を破棄する]** を選択します。

### <a name="task-3---dynamic-data-masking"></a>タスク 3 - 動的データ マスク

1. **[開発]** ハブの **[SQL スクリプト]** セクションで、**[動的データ マスク]** を選択します。
2. ツールバーで、**SQLPool01** データベースに接続します。
3. クエリ ウィンドウで、ステップのステートメントを強調表示し、ツールバーから **[実行]** ボタンを選択して (または **F5** キーを押して)、**各ステップを個々に実行**します。
4. [スクリプト] タブを閉じます。プロンプトが表示されたら、**[すべての変更を破棄する]** を選択します。

## <a name="important-pause-your-sql-pool"></a>重要:SQL プールを一時停止する

これらの手順を実行して、不要になったリソースを解放します。

1. Synapse Studio で **[管理]** ハブを選択します。
2. 左側のメニューで **[SQL プール]** を選択します。 **SQLPool01** 専用 SQL プールにカーソルを合わせ、 **[||]** を選択します。

    ![専用 SQL プールで一時停止ボタンが強調表示されています。](images/pause-dedicated-sql-pool.png "一時停止")

3. プロンプトが表示されたら、**[一時停止]** を選択します。

## <a name="reference"></a>リファレンス

- [IP ファイアウォール](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-ip-firewall)
- [Synapse ワークスペース マネージド ID](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-managed-identity)
- [Synapse マネージド VNet](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-managed-vnet)
- [Synapse マネージド プライベート エンドポイント](https://docs.microsoft.com/azure/synapse-analytics/security/synapse-workspace-managed-private-endpoints)
- [Synapse ワークスペースのセキュリティ保護](https://docs.microsoft.com/azure/synapse-analytics/security/how-to-set-up-access-control)
- [プライベート リンクを使用して Synapse ワークスペースに接続する](https://docs.microsoft.com/azure/synapse-analytics/security/how-to-connect-to-workspace-with-private-links)
- [データ ソースへのマネージド プライベート エンドポイントを作成する](https://docs.microsoft.com/azure/synapse-analytics/security/how-to-create-managed-private-endpoints)
- [ワークスペースのマネージド ID にアクセス許可を付与する](https://docs.microsoft.com/azure/synapse-analytics/security/how-to-grant-workspace-managed-identity-permissions)

## <a name="other-resources"></a>その他の参照情報

- [ワークスペース、データ、およびパイプラインへのアクセスを管理する](https://docs.microsoft.com/azure/synapse-analytics/sql/access-control)
- [Apache Spark を使用して分析を行う](https://docs.microsoft.com/azure/synapse-analytics/get-started-analyze-spark)
- [Power BI でデータを視覚化する](https://docs.microsoft.com/azure/synapse-analytics/get-started-visualize-power-bi)
- [SQL オンデマンドのストレージ アカウント アクセスを制御する](https://docs.microsoft.com/azure/synapse-analytics/sql/develop-storage-files-storage-access-control)
