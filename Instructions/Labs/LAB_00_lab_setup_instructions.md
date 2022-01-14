---
lab:
    title: 'プレインストールされた仮想マシンを使用したラボ環境のセットアップ'
    module: 'モジュール 0'
---

# モジュール 0 - プレインストールされた仮想マシンを使用したラボ環境のセットアップ

次の手順により、学習者は後続のモジュール用にラボ環境を準備できます。モジュール 1 を開始する前に、これらの手順を実行してください。

**所要時間**: 以下の手順を実行して自動設定スクリプトを開始するには、約 5 分かかります。スクリプトが完了するまでに 1 時間以上かかる場合があります。

> **注**: これらの手順は、コースに提供されているプレインストールされた仮想マシンで使用するように設計されています。

## 要件

セットアップを開始する前に、Azure Synapse ワークスペースを作成する権限を備えた Azure アカウントが必要です。

> **Azure Pass サブスクリプションを使用する場合の重要な注意**
>
> Azure Pass サブスクリプションの有効期限がすでに切れたアカウントを使用している場合は、同じ名前の複数の Azure サブスクリプションにアカウントが関連付けられている可能性があります (*Azure Pass - スポンサー プラン*)。セットアップ手順を開始する前に、次の手順に従って、この名前の最新の*アクティブな*サブスクリプションのみが有効になっていることを確認してください。
>
> 1. `https://portal.azure.com` で Azure portal を開き、サブスクリプションに関連付けられているアカウントを使用してサインインします。
> 2. ページ上部のポータル ツール バーで、「**ディレクトリとサブスクリプション**」ボタンを選択します。
> 3. 「**既定のサブスクリプション フィルター**」ドロップダウン リストで、**(無効になっている) Azure Pass - スポンサー プラン** サブスクリプションの*選択を解除し*、使用するアクティブな **Azure Pass - スポンサー プラン** サブスクリプション<u>のみ</u>が選択されていることを確認します。

## セットアップの手順

次のタスクを実行して、ラボ用の環境を準備します。

1. Windows の**検索**ボックスを使用して、**Windows PowerShell** を検索し、管理者として実行します。

    > **注**: Windows PowerShell ISE では<u>なく</u>、**Windows Powershell** を実行していることを確認してください。必ず管理者として実行してください。

2. Windows PowerShell で、次のコマンドを実行して、必要なコース ファイルをダウンロードします。これには数分かかる場合があります。

    ```
    mkdir c:\dp-203

    cd c:\dp-203

    git clone https://github.com/microsoftlearning/dp-203-data-engineer.git data-engineering-ilt-deployment
    ```

3. Windows PowerShell で、次のコマンドを実行して実行ポリシーを設定し、ローカルの PowerShell スクリプト ファイルを実行できるようにします。

    ```
    Set-ExecutionPolicy Unrestricted
    ```

    > **注**: 「信頼できないリポジトリからモジュールをインストールしようとしています。」というプロンプトが表示された場合は、「**A**」を入力して、「*すべてはい*」オプションを選択します。

4. Windows PowerShell で、次のコマンドを使用して、ディレクトリを自動化スクリプトを含むフォルダーに変更します。

    ```
    cd C:\dp-203\data-engineering-ilt-deployment\Allfiles\00\artifacts\environment-setup\automation\
    ```
    
5. Windows PowerShell で、次のコマンドを入力してセットアップ スクリプトを実行します。

    ```
    .\dp-203-setup.ps1
    ```

6. Azure にサインインするように求められ、ブラウザーが開きます。資格情報を使用してサインインします。サインインした後、ブラウザーを閉じて Windows PowerShell に戻ると、アクセスできる Azure サブスクリプションが表示されます。

7. プロンプトが表示されたら、Azure アカウントに再度サインインします (これは、スクリプトが Azure サブスクリプションのリソースを管理できるようにするために必要です。必ず以前と同じ資格情報を使用してください)。

8. 複数の Azure サブスクリプションがある場合は、プロンプトが表示されたら、サブスクリプションの一覧にその番号を入力して、ラボで使用するサブスクリプションを選択します。

9. プロンプトが表示されたら、SQL データベースの適切に複雑なパスワードを入力します (後で必要になる場合に備えて、このパスワードをメモしてください)。

スクリプトの実行中に、講師がコースの最初のモジュールを提示します。最初のラボを開始するときは、環境の準備が整っている必要があります。

> **注**: スクリプトが完了するまでに約 45 - 60 分かかります。スクリプトは、ランダムに生成された名前で Azure リソースを作成します。スクリプトが "停止" しているように見える場合 (10分 間新しい情報が表示されない場合)、Enter キーを押して、エラー メッセージを確認します。多くの場合、スクリプトは問題なく続行されます。  まれに、同じリソース名がすでに使用されている場合、ランダムに選択されたリージョン内の特定のリソースに容量の制約がある場合、一時的なネットワークの問題が発生する場合があります。その場合は、エラーが発生します。これが発生した場合は、Azure portal を使用して、スクリプトによって作成された **data-engineering-synapse-*xxxxxx*** リソース グループを削除し、スクリプトを再実行します。
>
> Azure Pass サブスクリプションの**テナント ID** を指定する必要があることを示すエラーが表示された場合は、上記の「**要件**」セクションの手順に従って、使用する Azure Pass サブスクリプションのみを有効にしてください。