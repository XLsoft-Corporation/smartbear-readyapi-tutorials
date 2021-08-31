# ReadyAPIハンズオンセミナー

本ドキュメントはSmartBear社のAPIテストツールReadyAPIのハンズオン用の資料です。

ReadyAPIユーザーガイドの中で、クイックスタート／チュートリアルの部分を抜粋して翻訳したものです。

インストールファイルの入手、各機能の詳細を含んだ最新バージョンのReadyAPIのユーザーガイドは、SmartBear社の下記のサイトで参照できます。

[https://support.smartbear.com/readyapi/docs/get-started.html](https://support.smartbear.com/readyapi/docs/get-started.html)





## Web API の基礎

### API とは

API（Application Programming Interface）とは、コンピュータ・プログラムの開発者が、自作したプログラムを他の開発者に使ってもらうときの「手続き・使い方・決まり」の総称です。プログラム自体を指すことも、その開発文書のことを指すことも、プログラムの設計理念を指すこともあります。どうにも抽象的な用語ですが、スマートフォンのアプリ操作にも当たり前に使われる技術です。

「ぐるなび」や「食べログ」で行きたいお店を探すとき、所在地を確認するために、わざわざ別のブラウザで地図を検索したりはしないでしょう。グルメサイトの画面内で「地図」メニューを開けば、そこにはGoogle Mapの地図が表示されますし、現在地からの道順も簡単に検索できます。

でもこの地図、もちろんグルメサイトが作成したものではありません。地図情報はGoogleが提供しています。グルメサイトは提供されたデータを表示するだけです。このようにAPIサービスは、あるアプリが他のアプリの機能を呼び出す「代理」をしてくれます。

> 出展/抜粋：[APIマーケット – データ流通市場の歩き方](https://www.j-dex.co.jp/datamarketguide/archives/tag/api%E3%83%9E%E3%83%BC%E3%82%B1%E3%83%83%E3%83%88)

例：

- Google Map API
- [OpenWeather API](https://openweathermap.org/api)
- AWS, Azure, GCPの各種API（言語解析、画像解析、AI、DeepLearningなど）
- Twitter, Facebook, Instagramなど各種SNSのAPI




### 基本概念

> [Basic Concepts \| ReadyAPI Documentation](https://support.smartbear.com/readyapi/docs/functional/tutorial/your-first-test/basic-concepts.html) の翻訳です。

ReadyAPIを使用してテストを作成および実行するには、Webサービステクノロジーとテストの原則を一般的に理解している必要があります。ReadyAPIをより早く使い始めるのに役立つ概要をまとめました。

用語：

- Webサービスは、クライアントとサーバーがHTTPプロトコルまたはHTTPに基づくその他のプロトコルを介してWeb経由でデータを交換するクライアント/サーバーアプリケーションです。このようなアプリケーションの例には、ナビゲーションソフトウェア、オンラインバンククライアント、気象監視システムなどが含まれます。
- クライアントがリクエストを送信するURLには、テスト対象のサーバー（**ホスト**）、通信に使用されるポート番号、およびリクエストされたサーバー**リソース**（ページやファイルパスなど）に関する情報が含まれています。

![](images/bc_1-01.png)

- クライアントがサーバーに送信する**リクエスト**の構造は次のとおりです。
    - HTTPメソッド（GET、POST、DELETEなど）、ターゲットURL、およびプロトコルバージョンを指定する開始行。
    - レスポンスデータの予想される形式、またはリクエストデータのサイズと形式などの追加情報を渡すヘッダー。
    - （オプション）リクエストボディ。一部のリクエストタイプはそれを使用しません。

![](images/bc_1-02.png)

- レスポンスの構造は似ています。
    - レスポンスコードとメッセージの開始行。頻繁に使用されるコードには、`200 OK`（成功）と`404 Not Found`（失敗、要求されたリソースが見つからなかった）があります。
    - レスポンスデータ形式を説明し、Cookie、サーバー情報などの追加値を含むヘッダー。
    - レスポンスボディ。たとえば、要求されたデータ、画像、ファイルなどを含む配列。

![](images/bc_1-03.png)

- リクエストとレスポンスのボディで頻繁に使用される形式は、JSONとXMLです。
- クライアントが実行のためにサーバーに送信するコマンドは、サービスアーキテクチャスタイル（SOAPまたはREST、以下を参照）に応じて、**アクション**、**メソッド**、または**オペレーション**と呼ばれます。
- Webサービスの2つの一般的なアーキテクチャスタイルは、SOAPとRESTです。
    - SOAPサービスは、HTTP上に構築されたSOAPプロトコルを使用します。これらのサービスは、POSTタイプのHTTPリクエストを使用し、リクエストとレスポンスのボディでXML形式のデータを渡します。すべてのリクエストは同じURLに送信され、実行される操作は、リクエストボディの特別なリクエストヘッダーまたはXML要素によって指定されます。    
    SOAPサービスは、サービスでサポートされる操作、それらのパラメータータイプ、およびデータ形式を厳密に記述するWSDL定義を使用します。
    - RESTサービスはHTTP上で機能します。実行する操作は、HTTPメソッドと要求されたリソース名の組み合わせによって設定されます。たとえば、オンラインペットショップのRESTfulサービスには、`/pets`リソースを含めることができます。`POST http://petstore.io/pets`リクエストはペットに関する情報をデータベースに追加でき、`GET http://petstore.io/pets`リクエストは利用可能なペットに関する情報を取得できます。    
    RESTサービス定義には、OpenAPI（Swagger）、WADLなどのいくつかの形式があります。ただし、一部の開発者は、RESTfulサービスの定義を提供していません。






## 機能テスト

[1\. Create Functional Test \| ReadyAPI Documentation](https://support.smartbear.com/readyapi/docs/functional/tutorial/your-first-test/create-project.html) を見ながら、SwaggerのサンプルであるPetstoreのAPIを利用して機能テストを作成してみましょう。

操作の流れ：

- 「API Definition」を選択し、Petstoreの定義ファイルを読み込みAPIを作成
- プロジェクトを確認
    - 送信するJSONのサンプルは以下を参照してください。
- 「Functional Test」の `/pet/findByStatus-Request 1 Test Case` を修正
- テストを実行
- Assertionを追加

addPetの例：

```json
{
  "id": 54407875,
  "category": {
    "id": 1,
    "name": "dog"
  },
  "name": "inu chan",
  "photoUrls": [
    "string"
  ],
  "tags": [
    {
      "id": 0,
      "name": "dog"
    }
  ],
  "status": "available"
}
```

> Assertionについては [Working With Assertions in Functional Tests \| ReadyAPI Documentation](https://support.smartbear.com/readyapi/docs/functional/assertions.html) を参照してください。






## データドリブンテスト

[Text Files as Data Sources \| ReadyAPI Documentation](https://support.smartbear.com/readyapi/docs/testing/data-driven/tutorials/text/index.html)をベースにした日本語ドキュメントです。




### 1) プロジェクト作成

最初にプロジェクトを作成します。

「File＞New Functional test」を選択して、新しい機能テストを作成します。

![](images/ddt_1-01.png)

「Endpoint」を選択します。

![](images/ddt_1-02.png)

次のURLを入力します。

```
https://maps.googleapis.com/maps/api/geocode/json?key=AIzaSyCKZwfQKwzSVDMu791w5N1tQro9MPOfHAE&address=
```

![](images/ddt_1-03.png)

> Google Goecording APIを利用するにはGoogle Developerアカウントとクレジットカードの登録が必要なため、今回は私が用意したAPI Keyを使用します。

「Next」をクリックします。

ウィザードの次のページで、追加したURLに対して新しいプロジェクトを作成するか、既存のプロジェクトに追加するかを選択します。

![](images/ddt_1-04.png)

「Next」をクリックして続行します。

ウィザードの次のページで、新しいリクエストにアサーションを追加できます。このチュートリアルでは提案されたアサーションを使用しないため、それらの選択を解除して「Next」をクリックします。

![](images/ddt_1-05.png)

次のダイアログでは、データソースを追加するように求められます。このチュートリアルでは手動で作成するので、ダイアログを閉じて続行します。

![](images/ddt_1-06.png)

次のステップでは、作成したテストケースを変更しファイルのデータソースを追加します。




### 2) データソース追加

データドリブンテストを実行するには、[データソース](https://support.smartbear.com/readyapi/docs/functional/steps/data-source.html)のテストステップを使用する必要があります。テストケースに追加しましょう。

作成したテストケースを開きます。

![](images/icon_datasource.png) をクリックして、[データソース](https://support.smartbear.com/readyapi/docs/functional/steps/data-source.html)のテストステップを追加します。

![](images/ddt_2-01.png)

ReadyAPIを使用すると、[パラメーター値を自動的に生成](https://support.smartbear.com/readyapi/docs/testing/data-driven/using.html#automatic)し、[データソースループ](https://support.smartbear.com/readyapi/docs/functional/steps/data-source-loop.html)のテストステップを作成できます。「Yes」をクリックします。

![](images/ddt_2-02.png)

「Create Data-Driven Loop」ダイアログで「Request 1」の「address」だけにチェックをして「OK」をクリックします。

![](images/ddt_2-03.png)

ReadyAPIは、リクエストから取得したプロパティを使用してデータソースのテストステップを作成します。

![](images/ddt_2-04.png)

ReadyAPIは、リクエストパラメータの値として[プロパティ拡張](https://support.smartbear.com/readyapi/docs/testing/properties/expansion.html)も追加します。プロパティ拡張は、データソース内のプロパティを参照します。

「key」「address」の2つが登録されています。

![](images/ddt_2-05.png)

次のステップは、データソースを設定する方法を示します。




### 3) データソースの設定

テキストファイルをデータソースとして使用するように、追加された[データソース](https://support.smartbear.com/readyapi/docs/functional/steps/data-source.html)のテストステップを構成しましょう。

「DataSource」から「File」を選択します。

![](images/ddt_3-01.png)

[sample_data_source.txt](./sample_data_source.txt)をダウンロードして、データソースとして使用します。

> 以下の内容でファイルを作成しても構いません。
> 
> ```text
> Boston:Boston, MA, USA
> Galway:Galway, Ireland
> Coconut Creek:Coconut Creek, FL, USA
> Tokyo:Tokyo, Japan
> ```

「Browse」をクリックして、ダウンロードしたファイルを指定します。

ReadyAPIを使用すると、プロパティをインポートできます。このチュートリアルではそれらを手動で追加するので「No」をクリックします。

![](images/ddt_3-02.png)

データソースにはすでに「address」プロパティが含まれています。![](images/icon_add.png) をクリックして`Expected result`プロパティを追加します。

![](images/ddt_3-03.png)

サンプルファイルでは、コロン":"がセパレーターとして利用されているため、「Separator」フィールドに`:`を入力します。

![](images/ddt_3-04.png)

> 日本語を含むデータを利用する場合は、適切に「Charset」を指定してください。`UTF-8`を利用すると良いでしょう。

データソースが正しく構成されているかどうかをテストするには、![](images/icon_run.png) をクリックして取得する行数を指定します。

> データソースをテストすると、取得した値がそのプロパティに割り当てられます。チュートリアルの後半で必要になります。

ReadyAPIは、取得した値をデータログに表示します。

![](images/ddt_3-05.png)

データソースが正しく機能することを確認したら、そのデータを使用するようにテストステップを構成できます。




### 4) リクエストの設定

データソースからのデータを使用するには、リクエストテストステップを変更する必要があります。

「Request 1」ステップエディタを開き、「[Request](https://support.smartbear.com/readyapi/docs/requests/rest/index.html)」タブに切り替えます。以前にデータソースループを作成した場合、リクエストには次の `${DataSource#address}` の値が含まれます。

![](images/ddt_4-01.png)

「address」のパラメータに別の値がある場合は、`${DataSource#address}` と入力するか、「Get Data」コマンドを使用してウィザードでプロパティを選択します。

> 1. 「Value」セルを選択します。
> 1. 「…」ボタンをクリックするか、セル内を右クリックして、コンテキストメニューから「Get Data」を選択します。これにより、「Get Data」ダイアログが呼び出されます。    
> ![](images/ddt_4-02.png)
> 1. 「Get Data」ダイアログで、「Data Source」テストステップと「address」プロパティを選択します。
> 1. 「Add」をクリックします。

エディターツールバーの ![](images/icon_send-request.png) をクリックして、リクエストを実行します。

右側にレスポンスデータが表示されます。

![](images/ddt_4-03.png)

> 前の手順でデータソースをテストしなかった場合、プロパティには値がないため、リクエストは有効なパラメータを送信しません。

これで、リクエストテストステップにアサーションを追加して、レスポンスデータを検証できます。




### 5) テストの実行と検証

#### テストの実行

これでテストステップの準備が整いました。テストケースツールバーの ![](images/icon_run.png) をクリックして、テストを実行します。

![](images/ddt_5-01.png)

テスト実行の詳細を確認するには、[トランザクションログ](https://support.smartbear.com/readyapi/docs/functional/results/transaction.html)を確認してください。ログを開くには、テストケースツールバーの「Transaction Log」をクリックします。

![](images/ddt_5-02.png)

トランザクションログには、次のテスト実行の詳細を示しています。

![](images/ddt_5-03.png)

> リクエストの検証にアサーションを使用しなかったため、リクエストのアサーションステータスは `UNKNOWN` です。「Status」列に表示されます。データソースとデータソースループは、それらをアサートすることが不可能であるため、常に `PASS` です。

実行中に行われた特定の要求に対する応答を確認するには、それぞれの行で「Open」をクリックします。

![](images/ddt_5-04.png)


#### アサーションの追加

サーバーの応答を検証するためにアサーションを追加しましょう。

「Request 1」テストステップをクリックし「Add Assertion」をクリックします。

![](images/ddt_5-05.png)

「Property Content」の「JsonPath Match」アサーションを選択します。必要なアサーションをすばやく見つけるには、「Search」フィールドに名前を入力します。

![](images/ddt_5-06.png)

ReadyAPIは、「JSONPath Match Assertion」ダイアログを表示します。

![](images/ddt_5-07.png)

「JSONPath Expression」の ![](images/icon_declare.png) をクリックし、表示されるダイアログで`formatted_address`をクリックし、`$['results'][0]['formatted_address']`ノードを選択します。

![](images/ddt_5-08.png)

「Expected Result」フィールドを右クリックし、コンテキストメニューから「Get Data」を選択します。

「Get Data」ダイアログで使用している「Test suite」「Test Case」、「Test Step」は「Data Source」、「Properties」で「Expected result」を選択します。

![](images/ddt_5-09.png)

「Add」をクリックします。

「Save」をクリックしてアサーションを作成します。

追加されたアサーションは、「Assertion」パネルにあります。

![](images/ddt_5-10.png)

テストケースを開き、もう一度 ![](images/icon_run.png) をクリックします。テストの実行が終了したら、トランザクションログのアサーションステータスを調べます。

![](images/ddt_5-11.png)

このチュートリアルでは、テキストファイルをデータソースとして使用するデータドリブンテストを作成する方法を学習しました。






## 負荷テスト

[Creating Your First Load Test \| ReadyAPI Documentation](https://support.smartbear.com/readyapi/docs/performance/tutorial/your-first-test/index.html) を日本語化したドキュメントです。

このチュートリアルではLoad Testエディターのインターフェイスについて説明し、ReadyAPIを使った負荷テストの作成方法を示します。このチュートリアルのセクションは、ReadyAPIでの負荷プロジェクトの作成方法、簡単なテストの実行、およびテスト結果の解析に関する説明をします。テストはサンプルの機能テストをエミュレートします。

> このチュートリアルは、「templates」を使って新規の負荷テストを作成します。これには、ReadyAPI Proまたは評価版のライセンスが必要です。これらを持っていない場合は、URLより新しいテストを手動で作成します。





### 1) 新規負荷テストの作成

ReadyAPIで負荷テストを作成するには

「File＞New Load Test」を選択します。

![](images/lt_1-01.png)

または、ダッシュボードで「[New Test](https://support.smartbear.com/readyapi/docs/dashboard/new-test.html)」タイル内の「All Tests」を選択し、「Load Test」をクリックします。

![](images/lt_1-02.png)

> 画面に表示される「New Test」タイルは、タイルの内容がお持ちのReadyAPIライセンスに依存するため、上記のタイルと異なる場合があります。 詳細については、[タイルの説明](https://support.smartbear.com/readyapi/docs/dashboard/new-test.html#requirements)を参照してください。

「URL」を選択します。

![](images/lt_1-03.png)

現在のワークスペースに他のプロジェクトがある場合、ReadyAPは 既存のプロジェクトのテストケースからテストを作成することを提案します。このチュートリアルでは、URLから負荷テストを作成する必要があります。

下記のURLを入力し、「Next」をクリックします。

```
https://petstore.swagger.io/v2/pet
```

![](images/lt_1-04.png)

「Default」テンプレートを選択し、「Configure Load Test「をクリックします。

![](images/lt_1-05.png)

他のテンプレートを使用する場合は、ReadyAPI Performanceの評価版または製品ライセンスが必要です。

「Max response time」を300に設定し「Finish」をクリックします。

![](images/lt_1-06.png)

ReadyAPIが負荷テストを作成し、「Run Load Test」または「Add [Local](https://support.smartbear.com/readyapi/docs/performance/distributed/intro/create.html) or [Cloud](https://support.smartbear.com/readyapi/docs/performance/distributed/intro/cloud.html) Agent」を選択する画面が表示されます。テストを実行する前にこのウィンドウを閉じ、負荷テストを変更することができます。このチュートリアルでは、ウィンドウを閉じテストを開始する前にテストを変更します。

作成した負荷テストを負荷テストエディターで見てみましょう。




### 2) 負荷テストエディターインターフェースの概要

負荷テストエディターを使用して、ReadyAPIで負荷テストを表示、編集、および実行します。負荷テストエディターのサンプルビューは次のとおりです。

![](images/lt_2-01.png)

負荷テストエディターツールバー

エディターのツールバーでは、[テストの継続時間](https://support.smartbear.com/readyapi/docs/performance/configure/test-duration.html)を設定します。ReadyAPIは対象となるWebサーバーへのリクエストを指定したタイムアウトが発生するまで、またはその他の理由でテストが停止するまでシミュレートします。タイムアウトが指定されていない場合は、テストを手動で停止する必要があります。

エディターの上部にあるプログレスバーは、テストの進行状況を表示します。

「Running」フィールドには、使用中の仮想ユーザーおよび使用可能な仮想ユーザーの最大数が表示されます。

#### 負荷テストエディターページ

エディターにはいくつかのページがあります。

|ページ|説明|
|---|---|
|Load|このページでは、テストの[Load Type](https://support.smartbear.com/readyapi/docs/performance/configure/load-type.html)およびシナリオの[Load Profile](https://support.smartbear.com/readyapi/docs/performance/configure/profiles/index.html)の選択、テスト結果を 検証する[アサーション](https://support.smartbear.com/readyapi/docs/performance/configure/assertions/about.html)の作成などの負荷テストの設定を行います。テスト設定に関する詳細については「[Setting Up Load Tests](https://support.smartbear.com/readyapi/docs/performance/configure/index.html)」セクションを参照してください。|
|Scheduler|負荷テスト中にシナリオを使用する時間を設定します。詳細については「[Load Scheduler](https://support.smartbear.com/readyapi/docs/performance/configure/load-scheduler.html)」を参照してください。|
|Log|特定の条件に応じてリクエストを記録します。詳細については「[Transaction Log Page](https://support.smartbear.com/readyapi/docs/performance/results/log/about.html)」を参照してください。|
|Distribution|リモートコンピューターでテストを実行するためのリモートエージェントを設定します。詳細については 「[About Distributed Load Testing](https://support.smartbear.com/readyapi/docs/performance/distributed/index.html)」を参照してください。|
|Monitoring|テスト実行時におけるサーバーのモニタリング設定をします。収集データは他のテスト結果と一緒に保存されます。詳細については「[About Server Monitoring](https://support.smartbear.com/readyapi/docs/performance/monitor/index.html)」を参照してください。|
|Statistics|対象となるWebサービスにおける、テスト間のパフォーマンスメトリックの変化を確認できます。詳細については「[Test Results](https://support.smartbear.com/readyapi/docs/performance/results/statistics/index.html)」を 参照してください。|
|Scripts|テストの開始時および終了時に使用されるスクリプトを設定します。詳細については「[Setup and Teardown Scripts](https://support.smartbear.com/readyapi/docs/performance/configure/script.html)」を 参照してください。

基本機能では、「Load」ページにのみアクセスできます。他のページを操作するには、[ReadyAPI Performanceライセンス](http://smartbear.com/product/ready-api/loadui/pricing/#_ga=2.248384768.640695114.1630330748-92759339.1628599618)が必要です。

#### 「Load Scenarios」パネル

このパネルでは、負荷タイプを選択し、負荷テストのシナリオを設定できます。

「Load Type」ドロップダウンリストで、負荷テストで使用するload typeを選択します。

- VUs – ReadyAPIは、対象となるWebサービスにリクエストを送信する仮想ユーザーをシミュレートします。
- Rate – ReadyAPIは、指定された割合で対象となるWebサービスにリクエストを送信します。

シミュレートした負荷を編集するには、編集する負荷シナリオをクリックし、Editorパネルの「Scenario」セクションで値を編集します。

アサーションを作成するには、アサーションを作成するシナリオまたは「TestStep」をクリックし、Editorパネルで作成します。

#### 「Metrics」パネル

Global Metricsパネルでは、全体的なテスト結果のグラフが表示されます。リクエストの処理にかかる最大時間を確認できます。これらのグラフに表示されるメトリックのテストアサーションも確認できます。

Test Steps Metricsパネルは、各TestStepの詳細メトリックを表示します。たとえば、ステップを完了するのに要する最短および最長時間を確認できます。

また、ドロップダウンリストを使って、テスト実行中に使用した別のシナリオのメトリックに切り替えることもできます。

> これらのパネルに表示される結果は、進行中のテストのみです 。過去のテスト情報、またはその他のメトリックを参照するには、[Statistics](https://support.smartbear.com/readyapi/docs/performance/results/statistics/index.html)ページのグラフを使用します。

#### 「Inspector」パネル

シナリオ、TestCase、またはTestStepレスポンスをクリックすると、Inspectorパネルが表示されます。このパネルでは、選択したオブジェクトを編集できます。たとえば、負荷シナリオの負荷プロファイルを設定したり、TestStepレスポンスにアサーションを追加したりできます。




### 3) 作成した負荷テストの実行

デフォルトでは、新規作成した負荷テストは選択したテンプレート設定を使用します。

![](images/lt_3-01.png)

テスト継続時間を1分に変更するため、負荷時間フィールドに **1** を入力します。

必要なプロセスをスタートするのに時間がかかるため、最初の数秒間はリクエストに対するサーバーのレスポンスが正しく反映されません。そのような結果を記録することを回避するために、テストのウォームアップに要する時間を指定します。

![](images/lt_3-02.png)

Scenariosパネルの任意の場所をクリックして負荷テストエディターを開きます。「Warm-up Time」フィールドに 5 秒と入力します。

![](images/lt_3-03.png)

![](images/icon_run.png) をクリックしてテストを実行します。

テストの実行は、ユーザーの操作なしで行われます。ReadyAPIは、基になる機能テストケースのテストステップをシミュレートします（エディターの「Targets」セクションで名前を確認できます）。対象となるWebサービスにリクエストを送信し、レスポンスを取得します。テストを開始すると、[テスト結果が表示](https://support.smartbear.com/readyapi/docs/performance/tutorial/your-first-test/viewing-test-results.html)されます。ReadyAPIは、テスト実行中とテスト実行終了時の両方で結果を表示します。

![](images/lt_3-04.png)

テス トの実行が終了すると、ReadyAPIはConfirmationダイアログを表示し、エラーの確認、レポートの作成、または統計の調査を提案します。このチュートリアルでは、手動で行うため、ダイアログを閉じます。

![](images/lt_3-05.png)




### 4) テスト結果の表示

ReadyAPIが負荷テストを実行中に、サービスパフォーマンスのデータが収集されます。収集されたデータは、Global Metricsグラフ、Test Steps Metricsテーブル、およびStatisticsページで表示されます。

![](images/lt_4-01.png)

「Global Metrics」グラフでは、対象となるWebサービスのさまざまなパフォーマンスメトリックの経時的な変化が表示されます。**VUs/s**（1秒ごとにサーバーに到達するユーザー数）、**Time taken**（各リクエストに要した時間）、**TPS**（1秒あたりのトランザクション数）および**BPS**（1秒あたりのバイト数）メトリックを分析し、対象となるサービスのパフォーマンス情報を得られます。テスト中にエラーが発生すると、ReadyAPIはグローバルメトリックグラフの下にメッセージを表示します。メッセージをクリックして、「Performance Log」タブでエラーの詳細な説明を表示します。

![](images/lt_4-02.png)

「TestSteps Metrics」テーブルは、各ステップをシミュレートするのに要した最長および最短時間など、個々のTestStepsにおけるパフォーマンス値を表示します。

テストの他の統計を表示、確認するには「Statistics」ページに切り替えます。（この操作にはeadyAPI Performanceのライセンスが必要となります。）

> ヒント：このページでは、サーバーのモニターを設定し（[configure server monitors](https://support.smartbear.com/readyapi/docs/performance/monitor/quickstart.html)）、サーバーメトリックを解析することもできます。

![](images/lt_4-03.png)

「Statistics」ページへのメトリックの追加についての詳細は、[Adding and Configuring Statistics Groups](https://support.smartbear.com/readyapi/docs/performance/results/statistics/configuring.html)をご覧ください。

各種メトリックの意味については[Statistics and Metrics Reference](https://support.smartbear.com/readyapi/docs/performance/results/metrics/index.html)をご覧ください。




### 5) 負荷テストの変更

最初の負荷テストの実行に成功しました。いくつかのパラメーターを変更し、異なる負荷プロファイルでテストを再実行してみましょう。

「Load Test」エディターの「Load」ページに切り替えます。

「Load Type」ドロップダウンリストで、「VUs (simulate user behavior)」を選択します。この[負荷タイプ](https://support.smartbear.com/readyapi/docs/performance/configure/load-type.html)を選択すると、負荷プロファイル設定で同時に動作する仮想ユーザーの人数を指定できます。

![](images/lt_5-01.png)

負荷シナリオをクリックすると、「Editor」パネルが右側に表示されます。

![](images/lt_5-02.png)

「Load profile」ドロップダウンリストで、「Burst」プロファイルを選択します。プロファイルのアイコンが表示するとおりに、テスト実行中に異なる負荷をシミュレートします。

![](images/lt_5-03.png)

「Base VUs」には、たとえば 5 ユーザーのように、シミュレートする仮想ユーザーの最少人数を入力します。

「Burst VUs」では、シミュレートする仮想ユーザーの最多人数が表示されます。負荷テストの設定から計算されます。これを増やすには、シミュレートするVUの総数またはVUのシェアを増やします。

このチュートリアルでは、一つのシナリオしかないため、すべての仮想ユーザーをカウントします。たとえば、シミュレートする仮想ユーザーの最多人数を 10 ユーザーと設定します。

![](images/lt_5-04.png)

> ReadyAPI の基本バージョンでは、最高 10 人までの仮想ユーザーを同時にシミュレートできます。さらに多くの人数をシミュレートするには、ReadyAPI Performanceのライセンスが必要となります。

「Base duration」には、たとえば 1 秒のように、ReadyAPIが最少人数の仮想ユーザーをシミュレートする時間を入力します。

「Burst duration」には、たとえば 5 秒のように、ReadyAPIが最多人数の仮想ユーザーをシミュレートする時間を入力します 。

「Wait time」には、たとえば 1 秒のように、各仮想ユーザーのテストを再実行するまでのReadyAPIの待ち時間を入力します。

再実行の必要な理由：何人かの仮想ユーザーのシミュレーションが終了すると、同時使用ユーザーの人数が減ります。この人数がプロファイルに指定された人数より
少ない場合、ReadyAPIは必要人数に達するまで新規の仮想ユーザーを増やします。

テストを実行します。テスト実行中に、ReadyAPIで指定された最多人数に達するまで仮想ユーザーの人数が増えます。その後、ReadyAPIはテストが終了するまで仮想ユーザーの人数を増減し続けます。

![](images/lt_5-05.png)

テスト結果をもとに、対象となるWebサービスのパフォーマンスの負荷下における変化を確認します。




### 6) 負荷テストへのアサーションの追加

[アサーション](https://support.smartbear.com/readyapi/docs/performance/configure/assertions/about.html)は、テスト実行中に対象となるWebサービスの機能を確認するテスト結果に適用される検証規則です。

1 つのTest Stepの平均シミュレーション時間が制限時間を超過しないことを確認するアサーションを作成しましょう。

TestCaseを広げ、アサーションを作成するTest Stepの「Response」をクリックします。「Request」エディターが表示されます。

![](images/lt_6-01.png)

「Request」インスペクターの「Assertions」セクションで ![](images/icon_add.png) をクリックします。

![](images/lt_6-02.png)

次の「Add Assertion」ダイアログの「Statistics:」では「Time Taken]、「Type:」では「Median」をそれぞれのドロップダウンリストから選択します。

![](images/lt_6-03.png)

その他のアサーションパラメーターを設定します。

「Maximum」エディットボックスにTest Stepが平均して要する最多回数を入力します。

「Tolerance」セクションで、エラーをログに記録する前にアサーションがトリガーされる最多回数を設定します。

「OK」をクリックすると、ReadyAPIの負荷テストにアサーションが追加されます。

![](images/lt_6-04.png)

再度テストを実行します。レスポンスに時間がかかりすぎると、ReadyAPIはエラーをレポートします。アサーションエラーは、「Global Metrics」グラフおよび「TestSteps Metrics」テーブルで表示されます。

![](images/lt_6-05.png)










