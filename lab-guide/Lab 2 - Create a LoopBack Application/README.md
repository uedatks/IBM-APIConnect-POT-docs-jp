# Lab 2	- LoopBack アプリケーションの作成

このラボでは、IBM API Connect (APIC) ソリューションのアーキテクチャー、機能および開発コンセプトについて概略的な理解を得ることを目的としています。このラボでは、APIC コマンドライン・インターフェースや、直感的な Web ベースのユーザー・インターフェースを使用し LoopBack アプリケーションを作成し、RESTful ベースのサービスの構成と操作に関するあらゆる面を探求する機会を全工程を通して設けています。

このラボでは、いくつかの API リソース操作を介して製品在庫にアクセスするための新規アプリケーションを作成します。

---
# Lab 2 - 学習目標

このラボの学習目標は以下のとおりです。

+ 複数モデルの LoopBack アプリケーションの作成方法の習得
+ API Designer を使用した Representational State Transfer (REST) API 定義の作成方法の習得
+ コマンドラインを使用した Representational State Transfer (REST) API 定義の作成方法の習得
+ LoopBack MySQL コネクターの使用方法の習得
+ LoopBack MongoDB コネクターの使用方法の習得
+ REST API をテストする方法の習得
+ モデル間の関係を構築する方法の習得

---
# Lab 2 - このチュートリアルで利用するケース・スタディー

**ThinkIBM** は、歴史的価値がある希少な IBM 機器を取り扱う販売会社です。 **ThinkIBM** は 複数の API を利用して、製品在庫データベースへのアクセスを簡素化したいと考えていました。また、このアプリケーションに購入者がレビューを残す機能を持たせることも必要です。このラボの受講者の目的はアプリケーション開発者として、この製品在庫へアクセスするためのアプリケーションを作成することです。

---
# Lab 2 - 手順

## 2.1 - 作業ディレクトリーの作成

1. タスクバーから `ターミナルエミュレーター` に戻ります。閉じてしまった場合は、お気に入りメニューから再度開いてください。

2. 受講者のホームフォルダー配下に ThinkIBM というフォルダーを作成します。ターミナルに以下を入力します:

	```bash
	mkdir ~/ThinkIBM
	```

3. 以下を入力して、新規作成した `ThinkIBM` ディレクトリーに移動します:

	```bash
	cd ~/ThinkIBM
	```

## 2.2 - 在庫アプリケーションの作成

API Connect Developer ツールキットに含まれる LoopBack フレームワークを使って在庫アプリケーションを作成します。Web フレームワークである Express フレームワーク上で稼働し、Swagger 2.0 仕様に対応している LoopBack を使用することで、スケーラブルな API を迅速に作成できます。LoopBack は、高い機能拡張性を持った、オープンソースの Node.js のフレームワークです。

* ほとんど、あるいは全くコードを書かずに、動的なエンド・ツー・エンドの REST API を作成できます。
* リレーショナル・データベース、NoSQL データベース、SOAP や REST API のデータにアクセスできます。
* モデル間の関係と複雑な API へのアクセス制御を統合できます。

LoopBack は以下で構成されます。

* Node.js モジュールのライブラリー
* アプリケーションをサポートするための開発ツール

1. コマンドライン・ターミナルに以下のコマンドを入力し、`inventory` アプリケーションを作成します: 

	```bash
	apic loopback
	```

2. アプリケーション名を入力するように求めるプロンプトが出ます。

	```
	? What's the name of your application? (ThinkIBM) inventory
	```

	`inventory`と入力し、`Enter` もしくは `Return` キーを押下します。

3. 次は、アプリケーションを作成するディレクトリーの名前を入力するように求めるプロンプトが出ます。

  LoopBack はデフォルトでアプリケーション名をプロジェクト・ディレクトリー名に使用します。
  
  `Enter` もしくは `Return` キーを押下し、デフォルトの `inventory` を使用します。

4. 次に、作成するアプリケーションの種類を選択するように求めるプロンプトが出ます。

	矢印キーを使用して `empty-server` を選択してから `Enter` もしくは `Return` キーを押下します。 

	```bash
	❯ empty-server (An empty LoopBack API, without any configured models or datasources) 
	```

5. これで、プロジェクト・ビルダーが Node.js アプリケーションの主な依存関係をインストールします。

  `Next steps:` と表示されるまでお待ちください。

6. 新規作成した `inventory` ディレクトリーに移動します: 

	```bash
	cd inventory
	```

## 2.3 - MySQL に接続するためのデータ・ソース・コネクターの作成

API はデータ・ソースを使ってバックエンドのデータ・レポジトリーとやりとりします。今回は、MySQL を使用し、在庫品目のデータを保管します。

この作成方法は二部構成になっており、まず、バックエンド・システムへの接続方法を定義します。次に、MySQL へ接続するために必要な実際の LoopBack コネクターをダウンロードします。コネクターは ODBC や JDBC コネクターに類似しています。

1. ターミナルで現在のディレクトリーが `~/ThinkIBM/inventory` であることを確認してください。プロンプトは `student@xubuntu-vm:~/ThinkIBM/inventory` と出ているはずです。

	もしくは、ターミナルに `pwd` と入力すれば、`/home/student/ThinkIBM/inventory` と返ってくるはずです。

2. ターミナルに以下を入力します: 

	```bash
	apic create --type datasource
	```

	ターミナルが、新規データ・ソースの構成ウィザードを呼び出します。構成ウィザードでは、いくつかの質問が表示されます。テキスト入力を必要とする質問のほか、事前に定義された選択肢から回答を選ぶような質問もあります。
	
	以下のデータを使用して質問に回答してください: 

	```bash
	? Enter the data-source name:  mysql-connection
	? Select the connector for mysql-connection:
		> MySQL (supported by StrongLoop)
	Connector-specific configuration:
	? Connection String url to override other settings (eg: mysql://user:pass@host/db):
	? host:  mysql.think.ibm
	? port:  3306
	? user:  student
	? password:  Passw0rd!
	? database:  think
	? Install loopback-connector-mysql@^2.2 (Y/n)  Y
	```
	
	> ![][info]
	> 
	> `Install loopback-connector-mysql@^2.2` という質問に Y (Yes) と入力すると、MySQL コネクターがダウンロードされ、自動的にプロジェクトに保存されます。 
	> `~/ThinkIBM/inventory/server/datasources.json` ファイルに接続プロファイルが作成されます。実質的には、以下を実行し、コネクターをインストールするのと同じことです:
	>
	> `npm install loopback-connector-mysql --save`
	>
	> MySQL に接続するための LoopBack コネクターに関する詳しい情報は以下リンクを参照してください:
	>
	> <https://www.npmjs.com/package/loopback-connector-mysql>

## 2.4 - API Designer の起動

1. 現在のディレクトリーが `~/ThinkIBM/inventory` であることを確認し、以下のコマンドを入力します:

	```bash
	apic edit
	```

	Firefox Web ブラウザーが起動し、API Designer 画面が自動的にロードされます。

2. API Designer が稼働すると、スタート・ページに `inventory` API があるはずです。

	> ![][info]
	> 
	> この API は LoopBack アプリケーション作成時に生成された成果物です。

## 2.5 - 在庫品目 (item) のモデルの作成

このセクションでは、`inventory` API の `item` データ・モデルを定義し、MySQL データ・ソースに関連付けます。LoopBack はデータ・モデル駆動のフレームワークです。データ・モデルのプロパティーが、API の要求および応答ペイロードの JSON エレメントになります。

1. `モデル` タブをクリックします。

	![](../img/lab2/api-designer-model-design-page_jp.png)
	
2. `+ 追加` ボタンをクリックします。

	![](../img/lab2/api-designer-model-design-page-add-button_jp.png)
	
3. 「New LoopBack Model」ダイアログに、モデル名として `item` を入力し、`新規作成` ボタンをクリックします。

	![](../img/lab2/api-designer-model-design-page-new-model_jp.png)

4. item モデルのモデル編集ページが表示されたら、データ・ソースに先ほど作成した`mysql-connection` データ・ソースを選択します。

	![](../img/lab2/api-designer-model-datasource_jp.png)


## 2.6 - `item` モデルのプロパティーの作成

MySQL データベースの item 表には、6 の列があり、これらもマッピングが必要です。item モデルのプロパティーを作成する手順は以下のとおりです: 

1. **Properties** セクションの `+` ボタンをクリックします。

2. `item` データ・モデルは 7 つのプロパティーで構成されます。以下のデータを使用して各プロパティーを追加します:

	> 必須: `yes`
	
	> プロパティー名: `id`
	
	> タイプ: `number`
	
	> **ID: `yes`**
	
	> 説明: `id`
	
	> ---
	> 必須: `yes`
	
	> プロパティー名: `name`
	
	> タイプ: `string`
	
	> 説明: `item name`
	
	> ---
	
	> 必須: `yes`
	
	> プロパティー名: `description`
	
	> タイプ: `string`
	
	> 説明: `item description`
	
	> ---
	
	> 必須: `yes`
	
	> プロパティー名: `img`
	
	> タイプ: `string`
	
	> 説明: `location of item image`
	
	> ---
	
	> 必須: `no`
	
	> プロパティー名: `img_alt`
	
	> タイプ: `string`

	> 説明: `item image title`
	
	> ---
	
	> 必須: `yes`
	
	> プロパティー名: `price`
	
	> タイプ: `number`
	
	> 説明: `item price`
	
	> ---
	
	> 必須: `no`
	
	> プロパティー名: `rating`
	
	> タイプ: `number`
	
	> 説明: `item rating`

	![](../img/lab2/api-designer-model-property_jp.png)



3. ページの最上部までスクロールし、`Save` ボタンをクリックし、データ・モデルを保存します。

	![](../img/lab2/api-designer-model-design-page-model-properties-save_jp.png)

4. `すべてのモデル` リンクをクリックし、API Designer のメイン・ページに戻ります。

## 2.7 - API の検証

API が正しくマッピングされ、MySQL データ・ソースとインターフェースできることを確認するために、サーバーを実行し API のテストを実施します。

1. `再生` ボタンをクリックし、`inventory` LoopBack アプリケーションを起動します。同時に、MicroGateway も起動されます。

	![](../img/lab2/api-designer-run_jp.png)

2. サーバーが起動するまでしばらくお待ちください。以下の画面が表示されたら、次の手順に進んでください。

	![](../img/lab2/api-designer-running_jp.png)

3. `探索` ボタンをクリックし、API を確認します。 

	![](../img/lab2/api-designer-explore_jp.png)

4. ページの左側に、`inventory` API 用のパスの一覧が表示されます。これは、LoopBack フレームワークに `item` データ・モデルを追加したことにより、自動的に作成されたパスおよび操作です。これらの操作により、ユーザーは接続された (MySQL) データ・ソースからデータ・モデルを作成、更新、削除、照会ができます。

5. `GET /items` 操作をクリックします。

	![](../img/lab2/api-designer-explore-page-get-items-api_jp.png)

6. `GET /items` 操作をクリックすることで、これに応じた正しい場所がウィンドウに表示されるよう画面が遷移します。中央のウィンドウには、操作の要約や、任意のパラメーターおよび応答が表示されます。

	cURLサンプル・コードのほか、ページ下には、応答例、URL、API 識別情報や API パラメーターも表示されています。

7. `操作の呼び出し` ボタンが表示されるまでゆっくりとスクロールダウンします。

	![](../img/lab2/api-designer-explore-page-call-operation_jp.png)

8. `操作の呼び出し` ボタンをクリックし、API を呼び出します。


	> ![][troubleshooting]
	> 
	> The first time you invoke the API, you may receive an error. The error occurs becuase the browser does not trust the self-signed certificate from the MicroGateway. To resolve the error, click on the link in the response window and accept the certificate warning.

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab2/cert-error.png)

9. 終了したら、API Explorer に戻り、`Call operation` ボタンを再度クリックします。

10. スクロールダウンし、`Request` および `Response` ヘッダーを確認します。 

	```text
	Request
	GET https://localhost:4002/inventory/items
	APIm-Debug: true
	Content-Type: application/json
	Accept: application/json
	X-IBM-Client-Id: default
	X-IBM-Client-Secret: SECRET
	```

	```text
	Response
	Code: 200 OK
	Headers:
	content-type: application/json; charset=utf-8
	x-ratelimit-limit: 100
	x-ratelimit-remaining: 99
	x-ratelimit-reset: 3599999
	```

11. さらに下へスクロールすると、GET 要求に対し返されたペイロードが表示されています。

	```json
	[
		{
			"name": "Dayton Meat Chopper",
			"description": "Punched-card tabulating machines and time clocks...",
			"img": "images/items/meat-chopper.jpg",
			"img_alt": "Dayton Meat Chopper",
			"price": 4599.99,
			"rating": 0,
			"id": 5
		},
		...
	]
	```

12. 同じ手順で、`GET /items/count` 操作もテストします。12 件の在庫品目がカウントとして返されるはずです。

	```json
	{
	  "count": 12
	}
	```

13. `Run` の画面に戻り、`Stop` ボタンをクリックして、在庫アプリケーションと MicroGateway を停止します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab2/stop-application.png)

## 2.8 - MongoDB データ・ソースの作成

ここまでで、MySQL データベースに保存された在庫品目に関する API を提供する LoopBack アプリケーションを作成しました。

次のセクションでは、品目に対するレビューのデータ・モデルを作成します。レビュー・データは MongoDB に保存されます。

まずは、MongoDB データ・ソースを作成します。

1. Firefox のタブもしくはウィンドウの `x` ボタンをクリックし、ブラウザーを閉じます。

2. タスクバーから `Terminal Emulator` を選択し、コマンドラインを開きます。

3. ブラウザーを閉じても、API Designer アプリケーションは動作し続けます。

	`control` と `c` のキーを同時に押し、API Designer セッションも終了します:
	
	```bash
	control+c
	```
	
	これで、コマンドライン・プロンプトに戻ります。

4. 以下のコマンドを入力し、MongoDB のデータ・ソースを作成します:

	```bash
	apic create --type datasource
	```

5. ターミナルが、新規データ・ソースの構成ウィザードを呼び出します。構成ウィザードでは、いくつかの質問が表示されます。テキスト入力を必要とする質問のほか、事前定義済みの選択肢から回答を選ぶような質問もあります。
		
	以下のデータを使用して質問に回答してください:
	
	```bash
	? Enter the data-source name:  mongodb-connection
	? Select the connector for mongodb-connection:
		> MongoDB (supported by StrongLoop)
	? host: mongo.think.ibm
	? port: 27017
	? user: 
	? password: 
	? database: think
	? Install loopback-connector-mongodb@^1.4 (Y/n) Y

	```

	> ![][info]
	> 
	> Install loopback-connector-mongo@^1.4 という質問に Y (Yes) と入力すると、MongoDB コネクターがダウンロードされ、自動的にプロジェクトに保存されます。 
	> `~/ThinkIBM/inventory/server/datasources.json` ファイルに接続プロファイルが作成されます。
	>
	> 実質的には、以下を実行し、コネクターをインストールするのと同じことです:
	>
	> `npm install loopback-connector-mongodb --save`
	>
	> MongoDB に接続するための LoopBack コネクターに関する詳しい情報は以下リンクを参照してください:
	> 
	> <https://www.npmjs.com/package/loopback-connector-mongodb>

## 2.9 - レビュー用のモデルの作成

`review` データ・モデルは、購入者が残した品目に対するレビューを保管するために使用します。レビューは MongoDB に保管されます。

先の手順では、API Designer を使用してデータ・モデルを作成しました。今回は、コマンドラインを使用して、`review` モデルを作成します。

1. 以下のコマンドを入力し、`review` データ・モデルを作成します:

	```bash
	apic create --type model
	```

2. `review` モデルのプロパティーを入力します:

	> ![][important]
	> 
	> review モデルは REST API として公開**しません**。後ほど品目とレビューを関連付けますが、この作業の一環で使用する REST API を作成します。

	```bash
	? Enter the model name:  review
	? Select the data-source to attach review to:
		> mongodb-connecton (mongodb)
	? Select models base class:
		> PersistedModel
	? Expose review via the REST API? (Y/n):  N
	? Custom plural form (used to build REST URL):
	? Common model or server only?
		> common
	```
	

3. そのままウィザードを使って `review` モデルにプロパティーを追加します。

4. 最初に `date` プロパティーを追加します:

	```text
	Enter an empty property name when done.
	? Property name: date
	? Property type:
		> date
	?Required? Y
	?Default value [leave blank for none]: <leave blank>

	```

5. 次は、`reviewer_name` プロパティーを追加します:

	```text
	Let's add another review property.
	Enter an empty property name when done.
	? Property name: reviewer_name
	? Property type:
		> string
	? Required? N
	? Default value [leave blank for none]: <leave blank>
	```

6. 次は、`reviewer_email` プロパティーを追加します:

	```text
	Let's add another review property.
	Enter an empty property name when done.
	? Property name: reviewer_email
	? Property type:
		> string
	? Required? N
	? Default value [leave blank for none]: <leave blank>

	```

7. 次は `comment` プロパティーを追加します:

	```text
	Let's add another review property.
	Enter an empty property name when done.
	? Property name: comment
	? Property type:
		> string
	? Required? N
	? Default value [leave blank for none]: <leave blank>
	```

8. 最後に品目の `rating` プロパティーを追加します:

	```text
	Let's add another review property.
	Enter an empty property name when done.
	? Property name: rating
	? Property type:
		> number
	? Required? Y
	? Default value [leave blank for none]: <leave blank>
	```

9. ウィザードを閉じるには、次にレビュー・プロパティーを追加するか聞かれたときに `Enter` または `Return` を押してください。

## 2.10 - `item` と `review` データ・モデルの関連付け

このラボの次のステップでは、`item` モデルと `review` モデルの間の関係を作成します。それぞれのモデルは、まったく異なるデータベースのエンティティーを参照しますが、API Connect はモデル間に論理的な関係を構築することができます。この論理的な関係は、item モデルの追加操作として公開できます。

1. ターミナル・セッションに以下のコマンドを入力します:

	```bash
	apic loopback:relation
	```

2. 関係の詳細を以下のとおり入力します:

	```text
	? Select the model to create the relationship from:
		> item
	? Relation type:
		> has many
	? Choose a model to create a relationship with:
		> review
	? Enter the property name for the relation:  reviews
	? Optionally enter a custom foreign key: <leave blank>
	? Require a through model? No
	```
	
## 2.11 - 関係の確認

作成した関係を確認するために、API Connect Designer を起動し、Explore ページで操作を確認します。

1. ターミナル・セッションに以下のコマンドを入力し、API Connect Designer ウィンドウを起動します:

	```bash
	apic edit
	```

2. 「APIs」タブの `inventory` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab2/inventory-link.png)

3. API 定義の `Paths` セクションまでスクロールダウンします。

	品目のレビュー・データにアクセスするための API パスが ３ 件新規作成されていることを確認します:
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab2/new-paths.png)

4. Firefox のタブもしくはウィンドウの `x` ボタンをクリックし、ブラウザーを閉じます。

5. タスクバーから `Terminal Emulator` を選択し、コマンドラインを開きます。

6. ブラウザーを閉じても、API Designer アプリケーションは動作し続けます。

	`control` と `c` のキーを同時に押し、API Designer セッションも終了します:
	
	```bash
	control+c
	```
	
	これで、コマンドライン・プロンプトに戻ります。

# Lab 2 - 検証

CLI と API Designer の両方を使用し、多くのファイルを作成および更新しました。次のラボへ進む前に、ソース・ファイルと完成したラボとを照らし合わせて検証するための簡単なスクリプトを用意しました。この手順を踏むことで、後に入力ミスに起因した問題の発生を防止することができます。

1. `Terminal Emulator` で以下を入力します:

	```bash
	validate_lab 2
	```
	
2. このスクリプトはプロジェクト・フォルダー (`~/ThinkIBM/inventory`) にある特定のファイルに対し一連の `diff` コマンドを実行します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab2/validate-lab-2.png)
	
3. 出力結果が上記イメージと一致したら、問題がないので、Lab 3 に進んでください。もし、いずれかのファイルで上記と違う結果が出た場合、それが後に問題を引き起こす可能性があるか詳細を調べましょう。

4. 以下の例では、`server/datasources.json` ファイルの問題が特定されました。

	```bash
	Running validation check on file: server/datasources.json 
	12c12
	<     "host": "mongo.thinkibm",
	---
	>     "host": "mongo.think.ibm",
	```

5. 1 行目には、2 つのファイルの間の相違点が特定された行数が示されています。この例では、両ファイルの 12 行目に相違点があると特定されています。

6. その下には、相違内容が示されています。

	`<` 記号で始まる行は作成されたソース・ファイルに属します。
	
	`>` 記号で始まる行は用意された正しいソース・ファイルに属します。
	
7. こちらの例では、MongoDB サーバーのホストに入力ミスがありました。アプリケーションが間違ったホスト・アドレスに接続しようとしてしまうので、後に問題の原因となります。

8. このラボでは、検証が必要なファイルは主に 4 つあります。

	* common/models/item.json **および** review.json
		- これらのファイル内の重要な要素は、エレメント名とデータ・タイプです。
	* definitions/inventory.yaml
		- item.json または review.json ファイルに不一致があると、こちらのデータ・モデル定義で露呈します。
	* server/datasources.json
		- このファイルのエレメントのほとんどが正確でないといけません。

9. もし 1 つ以上のファイルに不一致が見つかった場合、修正作業は簡単なのでご安心ください。以下のコマンドを実行し、修正内容をソース・ファイルに統合します:

	```bash
	merge_lab 2
	```

# Lab 2 - まとめ

**Lab 2 の学習過程は以上です。**このラボで学習した内容:

+ マルチモデルの LoopBack アプリケーションの作成方法の習得
+ IBM Connect API Designer を使用した Representational State Transfer (REST) API 定義の作成方法の習得
+ IBM Connect コマンドラインを使用した Representational State Transfer (REST) API 定義の作成方法の習得
+ LoopBack MySQL コネクターの使用方法の習得
+ LoopBack MongoDB コネクターの使用方法の習得
+ REST API をテストする方法の習得
+ モデル間の関係を構築する方法の習得

Lab 3 では、これまで作成した成果物をもとにフックの処理や、API を API Manager に公開する方法を説明します。

次は、[Lab 3 - アプリケーションのカスタマイズとデプロイメント](../Lab%203%20-%20Customize%20and%20Deploy%20an%20Application)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"

