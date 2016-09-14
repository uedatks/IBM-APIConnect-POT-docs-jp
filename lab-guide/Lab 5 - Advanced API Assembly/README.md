# Lab 5 - 高度な API アセンブリー

このラボでは、高度な API アセンブリーを作成する方法を学習します。グラフィカルなデザイン・ツールとソース・エディターを使用して、既存の SOAP サービスを RESTful API として公開する `financing` という名前の新規 API を作成します。その後、既存の公開サービスに接続し、アセンブリー・ツールを使用して必要なフォーマットへとマッピングする、`logistics` という名前の別の API を作成します。

---
# Lab 5 - 学習目標

このラボの学習目標は以下のとおりです。

+ オブジェクトの定義およびパスを含む、新規 API の作成方法の習得
+ API を既存の SOAP サービスにアクセスするよう構成する方法の習得
+ ソース・エディターを使用して既存の API 定義をインポートする方法の習得
+ 複数の API 呼び出しから取得されたデータを集約された応答へとマップする方法の習得
+ API アセンブリー内で gatewayscript を直接使用する方法の習得

---
# Lab 5 - このチュートリアルで利用するケース・スタディー

このチュートリアルでは、**ThinkIBM** の製品オファリングを拡大します。**ThinkIBM** では、在庫 API のほかにも、消費者のアプリケーションにファイナンシングと物流・発送情報を提供する API も作成したいと考えています。このチュートリアルの目的は、既存のエンタープライズ・アセットと公開アセットを活用してこのような API を作成することです。

---
# Lab 5	- 手順

## 5.1 - ファイナンシング API の作成 (REST から SOAP)

1. API Designer 画面が開いていない場合は、ターミナルを開き、以下のコマンドを実行して API Designer を起動します:

	```bash
	cd ~/ThinkIBM/inventory
	apic edit
	```

2. API Designer 画面がすでに開いている場合は、`All APIs` リンクをクリックし、API Designer のメイン画面に戻ります。

### 5.1.1 - API 定義の作成

1. `+ Add` ボタンをクリックし、`API` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_add_new.png)

2. API のフォームに値を入力し、`Next` ボタンをクリックして次へ進みます。

	> Title: `financing`
	
	"name": `financing`
	
	> Base Path: `/financing`
	
	> Version: `1.0.0`
	
	> Description: `Operations for calculating financing payments`

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_api_info.png)

3. `Don't add to a product` がデフォルトで選択された状態のまま `Add` ボタンをクリックします。

4. API Connect が、`financing` API の Swagger 定義ファイルを生成し、自動的に API エディター画面にロードします。API にはパスおよびデータ定義が一切含まれていないことを確認してください。パスおよびデータ定義は、今後の手順で追加していきます。

5. API エディターのメニューから、`Host` をクリックします。「Host」フィールドから `$(catalog.host)` を削除し、空欄にします。

	> ![][troubleshooting]
	> 
	> 「Host」フィールドに、入力値が必須であることを示す赤い線が表示されますが、無視して構いません。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_no_host.png)

6. `Schemes` セクションまでスクロールダウンするか、または API エディターのメニューからこのセクションを選択してください。`https` スキームを有効にします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_schemes.png)

7. スキームのセクションの下には、`Consumes` および `Produces` セクションがあります。それぞれのセクションで `application/json` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_consumes_produces.png)

8. 次に、新規 API のモデルを定義する必要があります。モデル定義には複数の用途があります。意図された入出力パラメーターを定義する資料として、開発者ポータルで公開することが最大の目的であるといえますが、データ・マッピング操作に使用することもできます。API Designer メニューから、`Definitions` をクリックします。

9. **Definitions** セクションの `+` アイコンをクリックして新規定義を作成します。次に、`new-definitions-1` をクリックして新規定義を編集します。

10. 定義の `Name` を `paymentAmount` に変更します。

11. 新規定義には、`new-property-1` という名前のサンプル・プロパティーがすでに追加されています。このプロパティー値を以下のとおりに編集します:

	> Property Name: `paymentAmount`
	
	> Description: `Monthly payment amount`
	
	> Type: `float`

	> Example: `199.99`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_definition_complete.png)

12. 定義ができたところで、パスを作成します。API Designer メニューから、`Paths` をクリックします。

13. `+` ボタンをクリックして新規パスを作成します。テンプレートによってパスが生成され、パスの下には GET リソースが入ります。今回の目的上ではこれで十分ですが、必要があれば他のリソースや REST verb をパスに追加することもできます。

14. デフォルト・パスの場所を `/calculate` に変更します。

	+ この API の基本パスは `/financing` であることを思い出してください。この新しいパスはこの基本パスに追加され、最終的には `/financing/calculate` というパスが作成されます。

15. `GET` 操作をクリックして、このリソースの構成オプションを展開します。

16. 各操作に操作 ID が必要です。操作 ID を`get.financingAmount` に設定します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_operation_id.png)

17.  次に、オプションで操作に要求パラメーターを追加できます。これにより、API 要求への入力が定義されます。今回は GET 要求であるため、URI の照会コンポーネントに必要な要求パラメーターを追加します。

	`Add Parameter` リンクをクリックして新規の照会パラメーターを作成します。次に、サブメニューから `Add new parameter` を選択します。

18. この操作には合計 3 つのパラメーターが必要なため、`Add Parameter` リンクをもう 2 回クリックして、パラメーター・テンプレートにパラメーターを追加します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_parameter_template.png)

19. パラメーターを編集し、以下の値を設定します:

	> Name: `amount`
	  
	> Located In: `Query`
	  
	> Description: `amount to finance`
	  
	> Required: *selected*
	  
	> Type: `float`
	  
	> ---
	  
	> Name: `duration`
	    
	> Located In: `Query`
	  
	> Description: `length of term in months`
	  
	> Required: *selected*
	  
	> Type: `integer`
	  
	> ---
	    
	> Name: `rate`
	    
	> Located In: `Query`
	  
	> Description: `interest rate`
	  
	> Required: *selected*
	  
	> Type: `float`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_parameter_complete.png)

20. 次に、応答のスキーマを設定します。`paymentAmount` 定義をすでに定義しているため、これをドロップダウン・リストから選択します。`paymentAmount` 定義はリストの一番上にあるはずです。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_resp_schema.png)

21. API 定義を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/save-icon.png)

22. 新規 API 定義の作成が完了しました。公開後、パスおよびモデル・データが開発者ポータルでコンシューマーに提供されます。

### 5.1.2 - ファイナンシング API アセンブリーの構築

1. `Assemble` タブをクリックし、アセンブリー・エディターを開きます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_assemble_tab.png)

2. `DataPower Gateway policies` フィルターを選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_gateway_policies.png)

3. ポリシーのリストで `activity-log` ポリシーをクリックし、処理パイプラインの先頭にドラッグします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_assembly_pipeline_1.png)

4. パイプライン上の 'activity-log` アクションをクリックし、ポリシーの構成オプションを開きます。

5. 「Content」フィールドで選択されている項目を `activity` から `payload` に変更します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_activity_log.png)

6. ポリシー・エディター・ウィンドウの `X` をクリックして閉じ、アセンブリー処理パイプラインに戻ります。

7. ここまで終わったら、REST API を SOAP へマップするために必要な残りのポリシーを追加します。

8. `activity-log` および `invoke` ポリシーの間に `map` ポリシーを追加します。

9. `invoke` ポリシーの後に、`gatewayscript` ポリシー、`xml-to-json` ポリシーの順に追加します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_assembly_pipeline_2_temp.png)

10. REST ベースの API から SOAP ベースのサービスを使用するには、`GET /calculate` 操作で定義した照会パラメーターの入力値を SOAP ペイロードにマップする必要があります。そのためには、パイプライン上の `map` ポリシーをクリックして、マップ・エディターを開きます。

11. `+` アイコンをクリックしてエディター・ウィンドウを全画面表示にします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_map_fullscreen.png)

12. **Input** 列で、`pencil` アイコンをクリックして入力エディターを開きます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/input-pencil-icon.png)

13. GET 操作には照会パラメーターが 3 つ必要であることを思い出してください (`amount`、`duration`、および `rate`)。`+ input` ボタンを 3 回クリックし、入力表にエントリーを追加します。

14. それぞれの入力パラメーターに、以下の値を入力します:

	> Context variable: `request.parameters.amount`
	
	> Name: `amount`
	
	> Content type: `none`
	
	> Definition: `float`
	
	> ---
	
	> Context variable: `request.parameters.duration`
	  
	> Name: `duration`
	  
	> Content type: `none`
	  
	> Definition: `integer`
	
	> ---
	
	> Context variable: `request.parameters.rate`
	  
	> Name: `rate`
	    
	> Content type: `none`
	    
	> Definition: `float`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_map_inputs.png)

15. `Done` ボタンをクリックしてマップ・エディターに戻ります。

16. 次に、**Output** 列で `pencil` アイコンをクリックし、出力定義を追加します。

17. `+ output` をクリックします。

18. `Content type` を `application/xml` に設定します。

19. `Definition` フィールドで、ドロップダウン・メニューをクリックし、`Inline schema` を選択します。

20. 時間短縮と入力ミス防止のために、サンプルのスキーマ・ファイルが用意してあります。

	`Sublime Text` アプリケーションをシステムのタスクバー、またはお気に入りメニューから開きます。
	
21. ファイル・ツリー・メニューを使用して `lab5/schema_financingSoap.yaml` ファイルを開きます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/open-schema-financing-soap.png)

22. `schema_financingSoap.yaml` ファイルのコンテンツをシステムのクリップボードにコピーします。

	```yaml
	$schema: 'http://json-schema.org/draft-04/schema#'
	id: 'http://services.think.ibm'
	type: object
	properties:
	  Envelope:
	    type: object
	    xml:
	      prefix: soapenv
	      namespace: 'http://schemas.xmlsoap.org/soap/envelope/'
	    properties:
	      Body:
	        type: object
	        properties:
	          financingRequest:
	            type: object
	            xml:
	              prefix: ser
	              namespace: 'http://services.think.ibm'
	            properties:
	              amount:
	                id: 'http://services.think.ibm/Envelope/Body/financingRequest/amount'
	                type: number
	                name: amount
	              duration:
	                id: 'http://services.think.ibm/envelope/body/financingRequest/duration'
	                type: integer
	                name: duration
	              rate:
	                id: 'http://services.think.ibm/envelope/body/financingRequest/rate'
	                type: number
	                name: rate
	            additionalProperties: false
	            required:
	              - amount
	              - duration
	              - rate
	            name: financingRequest
	        additionalProperties: false
	        required:
	          - financingRequest
	        name: Body
	    additionalProperties: false
	    required:
	      - Body
	    name: Envelope
	additionalProperties: false
	required:
	  - Envelope
	title: output
	```

23. `Firefox` ブラウザーに戻り、SOAP スキーマ定義をスキーマ・エディター・ウィンドウに貼り付け (`control+v`)、`Done` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_soap_schema.png)

24. 出力エディター・ウィンドウで `Done` ボタンをクリックし、マップ・エディターに戻ります。

25. `financingRequest: {}` エレメントの中括弧 (`{}`) をクリックし、ビューを展開し、SOAP の入力エレメントを確認します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_expand_operations.png)

26. それぞれの `Input` 照会パラメーターを、対応する SOAP の `Output` エレメントにマッピングします。

	入力フィールドから出力フィールドへマッピングするには、*ソース*・エレメントの横の丸をクリックしてから、*ターゲット*・エレメントの横の丸をクリックします。エレメント間を結ぶ線は、ソースからターゲットへマッピングされたことを意味します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_map_to_soap.png)

27. マップ・エディターの `X` ボタンをクリックし、ポリシー・パイプラインに戻ります。

28. `invoke` ポリシーをクリックし、エディターを開きます。

29. `Invoke URL` を `https://services.think.ibm:1443/financing` に設定します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_invoke_url.png)

30. スクロールダウンし、`HTTP Method` を `POST` に設定します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/fin_invoke_method.png)

31. `X` ボタンをクリックし、ポリシー・パイプラインに戻ります。

32. `gatewayscript` ポリシーをクリックし、エディターを開きます。以下の行を追加します: 

	```javascript
	session.name('_apimgmt').setVar('content-type', 'application/xml');
	```	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/gws-workaround.png)

33. `financing` API のアセンブリーが完成しました。アセンブリーによって以下のアクションが実行されます。

	+ 要求のログ記録
	+ REST 要求パラメーターの SOAP 本体へのマッピング
	+ SOAPAction ヘッダーの設定
	+ SOAP サービスの呼び出し
	+ SOAP サービス応答の JSON への変換

34. API 定義を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/save-icon.png)

## 5.2 - 物流 API の追加 (高度なアセンブリー)

このセクションでは、送料および最寄りの店舗の位置を計算するヘルパー・サービスを提供する `logistics` という名前の新規 API を追加します。
 
また一から API を作成しなおすのではなく、ソース・エディターを使用して、途中まで作成されている API 定義を貼り付ける方法を学習します。  

### 5.2.1 - 事前定義の API 定義のインポート

1. `All APIs` リンクをクリックし、API Designer のメイン画面に戻ります。

2. `+ Add` ボタンをクリックして新規 API を作成します。以下の値を指定します: 

	> Title: `logistics`
	  
	> Name: `logistics`
	  
	> Base Path: `/logistics`
	  
	> Version: `1.0.0`
	  
	> Description: `Provides logistics for sales`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-create.png)
	

3. `Don't add to a product` がデフォルトで選択された状態のまま `Add` ボタンをクリックします。

4. `Source` タブに移動し、API 定義を確認します。ソースへ直接行った編集は、`Design` タブに即時に反映されます。  

5. `Sublime Text` エディターから、`lab5/api_logistics.yaml` ファイルを開きます。

	ファイルのコンテンツをすべて**選択**し、クリップボードに**コピー**します。

6. ブラウザーに戻り、`Source` タブのコンテンツを選択して削除します。

7. クリップボードのコンテンツを貼り付けて API 定義を更新します。
  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-yaml.png)

8. `Design` ビューに戻り、インポートした変更を確認します。特に以下に注意してください。

	* Properties
	* Paths
	* Definitions

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-updated-definition.png)

### 5.2.2 - 物流 API アセンブリーの作成

1. `Assemble` タブに切り替え、`Create assembly` ボタンをクリックします。

2. `activity-log` ポリシーをアセンブリー・パイプラインに追加します。

3. `activity-log` ポリシーをクリックしてエディターを開き、**Content** フィールドで `payload` をログに記録するように構成します。

4. `operation-switch` ポリシーを activity-log ステップの右に追加します。

5. `operation-switch` ポリシーをクリックし、エディターを開きます。デフォルトで、1 つのケース `case 0` が作成されます。

6. `search operations...` をクリックして、使用可能な操作のドロップダウン・リストを表示させます。

7. `shipping.calc` 操作を選択します。

8. `+ Case` ボタンをクリックし、`get.stores` 操作の 2 つ目のケースを追加します。  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-operationswitch-cases.png)

9. `X` をクリックして operation-switch の構成エディターを閉じます。

	`operation-switch` ステップに、1 つのケースにつき 1 つ、つまり合計 2 つの処理パイプラインが新規に作成されているはずです:  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-twopipelines.png)

#### 5.2.2.1 - `shipping.calc` ケースの構成

この操作では、2 つの別々のバックエンド・サービスを呼び出し、各企業の送料情報を入手し、map アクションを活用して 2 つの別々の応答を 1 つの、統合されたメッセージにしてコンシューマーに提供します。

1. 以下のプロパティーを使用して `shipping.calc` ケースに invoke ポリシーを追加します:

	> Title: `invoke_xyz`
	  
	> Invoke URL: `$(shipping_svc_url)?company=xyz&from_zip=90210&to_zip={zip}`
	  
	> Response object variable (一番下へスクロールして表示): `xyz_response`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokexyz1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokexyz2.png)
	
	> ![][info]
	>
	> ここでの `{zip}` パラメーターは操作の入力として定義された `zip` パラメーターを指します。URL の `{zip}` の部分は、API コンシューマーによって実際に提供される zip コードで置き換えられます。

2. 以下のプロパティーを使用して `shipping.calc` ケースに 2 つ目の invoke ポリシーを追加します。

	> Title: `invoke_cek`
	  
	> Invoke URL: `$(shipping_svc_url)?company=cek&from_zip=90210&to_zip={zip}`
	
	> Response object variable: `cek_response`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokecek1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokecek2.png)
3. 2 つ目の invoke の後に `map` ポリシーを追加し、そのポリシーをクリックしてエディターを開きます。

4. `Input` の横の `pencil` アイコンをクリックし、以下の map プロパティーを指定します:
  
	> Title: `map_responses`
	  
	> Description: `Map responses from invoke_xyz and invoke_cek to output`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-map-properties.png)



5. `+ input` ボタンをクリックして入力を追加します。以下のように入力を構成します: 
  
	> Context variable: `xyz_response.body`
	  
	> Name: `xyz`
	  
	> Content type: `application/json`
	  
	> Definition: `Inline schema`

6. `Inline schema` を選択すると、「Provide a schema」というプロンプトが出ます。

	`Sublime Text` エディターを使用して `lab5/schema_shippingSvc.yaml` ファイルを開きます。
	
	コンテンツをクリップボードにコピーし、スキーマ・エディター・ウィンドウに貼り付けます。
	  
7. `Done` をクリックしてスキーマ・ウィンドウを閉じます。  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-map-xyz-schema.png)

8. `+ input` ボタンを再度クリックして、もう 1 つ入力を追加します。以下のように入力を構成します:
  
	> Context variable: `cek_response.body`
	  
	> Name: `cek`
	  
	> Content type: `application/json`
	  
	> Definition: `Inline schema`



9. 先の入力に使用したものと同じスキーマ定義を貼り付けます (ラボの目的上、配送サービスからの応答は同じフォーマットであるとしています。そうでないと同じスキーマは利用できません)。

10. `map` ポリシーに 2 つの入力を指定しました:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-map-responses-inputs.png)

11. `Done` ボタンをクリックしてエディターに戻ります。

12. `Output` の横の `pencil` アイコンをクリックし、`+ output` ボタンをクリックして出力フィールドに以下のプロパティーを追加します: 

	> Context variable: `message.body`
	  
	> Name: `output`
	  
	> Content type: `application/json`
	  
	> Definition: `#/definitions/shipping`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-map-responses-output.png)

13. `Done` ボタンをクリックしてエディターに戻ります。

14. マッピングを完成させます。入力フィールドから出力フィールドへマッピングするには、*ソース*・エレメントの横の丸をクリックしてから、*ターゲット*・エレメントの横の丸をクリックします。エレメント間を結ぶ線は、ソースからターゲットへマッピングされたことを意味します。  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-map-complete.png)

15. `X` をクリックしてマップ・エディターを閉じます。

	`shipping.calc` 操作のアセンブリー・ポリシーが完成しました。
	  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-shipping-calc-policy.png)

16. 変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/save-icon.png)

#### 5.2.2.2 - `get.stores` ケースの構成

この操作では、Google Geocode API を呼び出して、提供された zip コードについての位置情報を入手し、単純な gatewayscript をもとに応答を変更して、フォーマット設定された Google Maps リンクを提供します。

1. 以下のプロパティーを使用して `get.stores` ケースに invoke ポリシーを追加します:

	> Title: `invoke_google_geolocate`
	  
	> Invoke URL: `https://maps.googleapis.com/maps/api/geocode/json?&address={zip}`
	  
	> Response object variable (一番下へスクロールして表示): `google_geocode_response`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokegeolocate1.png)
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_invokegeolocate2.png)

2. 以下のプロパティーを使用して `gatewayscript` ポリシーを追加します:  
  
	> Title: `gws-format-maps-link`
	  
	> ポリシーのコード・セクションに `lab5/gws_formatMapsLink.js` を貼り付けます。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics-gws.png)
	
	> ![][info]
	> 
	>  5 行目に注目してください。gatewayscript ファイルが、`invoke` アクションの出力に指定された `google_geocode_response` 変数の本体部分を読み込んでいます。

3. `X` をクリックして gatewayscript エディターを閉じます。

4. `logistics` API のアセンブリーに 2 つの別々の操作ポリシーが含まれるようになりました:

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/logistics_assembly-complete.png)

5. 変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab5/save-icon.png)

6. タブまたはブラウザー・ウィンドウの `x` をクリックして Firefox ブラウザーを閉じます。

7. `Terminal Emulator` セッションに戻ります。

8. ブラウザーを閉じても、API Designer アプリケーションは動作し続けます。

	`control` と `c` のキーを同時に押し、API Designer セッションも終了します:
	
	```bash
	control+c
	```
	
	これで、コマンドライン・プロンプトに戻ります。

# Lab 5 - 検証

CLI と API Designer の両方を使用し、多くのファイルを作成および更新しました。次のラボへ進む前に、ソース・ファイルと完成したラボとを照らし合わせて検証するための簡単なスクリプトを用意しました。この手順を踏むことで、後に入力ミスに起因した問題の発生を防止することができます。

1. `Terminal Emulator` で以下を入力します:

	```bash
	validate_lab 5
	```
	
2. このスクリプトはプロジェクト・フォルダー (`~/ThinkIBM/inventory`) にある特定のファイルに対し一連の `diff` コマンドを実行します。

3. `validate_lab` スクリプトの出力に不一致があった場合、以下を入力し、訂正した変更をソース・ファイルに統合します:

	```bash
	merge_lab 5
	```

# Lab 5 - まとめ

**Lab 5 の学習過程は以上です。**2 つの新規 API に高度なアセンブリーを構成できました。次のラボでは、API を製品にバンドルし、コンシューマー・ポータルに公開します。

次は、[Lab 6 - API 製品の操作](../Lab%206%20-%20Working%20with%20API%20Products)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"
