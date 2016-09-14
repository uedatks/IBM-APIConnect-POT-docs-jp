# Lab 6 - API 製品の操作

API Connect で公開された API は、**製品**と呼ばれるオブジェクトにバンドルされます。製品とは、1 つ以上の API と 1 つ以上のプランの組み合わせです。

**プラン**とは、API プロバイダーと API コンシューマー間の契約のようなものです。プランは、一定期間中に許可される API 呼び出しのレートを規定します。

---
# Lab 6 - 学習目標

このラボの学習目標は以下のとおりです。

+ 製品の作成方法の習得
+ API を製品に付加する方法の習得
+ プランの作成方法の習得
+ 製品の公開方法の習得

---
# Lab 6 - このチュートリアルで利用するケース・スタディー

アプリケーション開発者/API デザイナーとしての作業は完了しました。今度は API 製品マネージャーとして作業を行っていきます。API 製品マネージャーの役割は、go-to-market 戦略をもとに開発済みのアセットをバンドルにまとめることです。

**ThinkIBM** の例でいうと、これまでに作成したすべての API を、API コンシューマー用に 1 つの製品オファリングとしてまとめて公開します。これに加えて、2 つのプランを作成して、異なるレベルの API アクセスを定めます。

---
# Lab 6	- 手順

## 6.1 - API 製品の作成

1. API Designer 画面が開いていない場合は、ターミナルを開き、以下のコマンドを実行して API Designer を起動します:

	```bash
	cd ~/ThinkIBM/inventory
	apic edit
	```

2. `Products` タブに切り替えます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/products.png)

3. `inventory` 製品へのリンクをクリックします。
	
	> ![][info]
	> 
	> この製品は、LoopBack 在庫アプリケーションの作成時に自動的に作成されたものです。

4. 製品の情報および問い合わせ先を編集します:

	> Title: `think`
	
	> Name: `think`
	
	> Description: `The **think** product will provide really awesome APIs to your application.`
	
	> Contact Name: `Thomas Watson`
	
	> Contact Email: `thomas@think.ibm`
	
	> Contact URL: `http://www.ibm.com`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-infocontact.png)

5. 「License」および「Terms of Service」を以下のように指定します:

	> License Name: `The MIT License (MIT)`
		
	> License URL: `https://opensource.org/licenses/MIT`
		
	> Terms of Service: `lab6/license.txt` ファイルのコンテンツを貼り付け
	  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-licensetos.png)
	
6. 「Visibility」を変更し、`think` 製品が `Authenticated users` のみに表示されるようにします。
  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-visibility.png)
	
7. API セクションにナビゲートします。`+` ボタンをクリックし、すべての新規 API を製品に追加します。

8. `financing`、`logistics` および `oauth` のチェック・ボックスを選択し、`inventory` も選択されていることを確認します。
	 
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-apis.png)

9. `Apply` ボタンをクリックします。

10. 「Plans」セクションにナビゲートします。デフォルトのプランを選択して変更します。以下のプロパティーを指定します:

	> Title: `Silver`
	
	> Name: `silver`
	
	> Description: `Limited access to these APIs`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-silverplan.png)

11. `+` ボタンをクリックして新規プランを作成します。以下のプロパティーを指定します:

	> Title: `Gold`
	
	> Name: `gold`
	
	> Description: `Unlimited access to these APIs for approved users`
	
	> Rate limit: `Unlimited`
	
	> Approval:  check `Require subscription approval`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-goldplan.png)

12. 変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/save-icon.png)

## 6.2 - API 製品の公開

1. `Publish` アイコンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/think-publish.png)

2. メニューから `Add and Manage Targets` を選択します。

3. `Add a different target` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-adddifferent.png)

4. IBM API Connect 管理サーバーへ接続するための接続情報を入力し、`Sign in` ボタンをクリックします: 

	> API Connect host address: `mgr.think.ibm`
	
	> Username: `student@think.ibm`
	
	> Password: `Passw0rd!`  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-target-signin.png)

5. **Select an organization and catalog** 画面で、`Sandbox` カタログを選択し、`Next` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-sandbox-catalog.png)

6. **Select an App** 画面では、`Inventory` アプリケーションを選択し、`Save` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-inventory-app.png)
	
	> ![][info]
	> 
	> これで、オフラインのツールキット環境を中央管理サーバーと対話させるための設定が整いました。
	> 
	> 在庫アプリケーションは Lab 3 ですでに公開しているため、ここで公開が必要なのは、API を含む関連する製品だけです。

7. `Publish` ボタンを再度クリックし、ターゲットを選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-target.png)

8. `Stage or Publish products` ボックスにチェックを入れます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-product.png)

	> ![][info]
	> 
	> ここで公開する API を指定することができます。例えばプロジェクトで複数の API 製品を扱っている場合などに、公開したい特定の API を選択することができます。
	> 
	> また、API をステージングにとどめるオプションもあります。「ステージングのみ」のアクションでは、構成を管理サーバーにプッシュしますが、使用に向けて公開はしません。ステージングのみを実行する理由としては、ユーザー権限がステージングに限定されている、もしくは製品の公開は違うグループが担当しているなどが考えられます。

9. `Publish` ボタンをクリックすると、開発者ポータルから API が使用できるようになり、API が API ゲートウェイに適用されます。

10. 製品の公開が完了するまでしばらくお待ちください。完了すると、公開に成功したことを示す `Success` メッセージが表示されます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab6/publish-success.png)

11. Firefox Web ブラウザーを閉じます。

12. `Terminal Emulator` で `control+c` のショートカット・キーを使用して API Designer のプロセスを終了します。

# Lab 6 - 検証

CLI と API Designer の両方を使用し、多くのファイルを作成および更新しました。次のラボへ進む前に、ソース・ファイルと完成したラボとを照らし合わせて検証するための簡単なスクリプトを用意しました。この手順を踏むことで、後に入力ミスに起因した問題の発生を防止することができます。

1. `Terminal Emulator` で以下を入力します:

	```bash
	validate_lab 6
	```
	
2. このスクリプトはプロジェクト・フォルダー (`~/ThinkIBM/inventory`) にある特定のファイルに対し一連の `diff` コマンドを実行します。

3. `validate_lab` スクリプトの出力に不一致があった場合、以下を入力し、訂正した変更をソース・ファイルに統合します:

	```bash
	merge_lab 6
	```
	4. 統合が必要な場合は、API 製品を再公開して、API Connect サーバーに最新の変更を反映させる必要があります。

5. `apic edit` コマンドを実行して API Designer を起動し、先ほどの手順を繰り返して製品を再公開します。

# Lab 6 - まとめ

**Lab 6 の学習過程は以上です。**これで Lab 6 は完了です。

このラボで学習した内容:

+ API 製品の概要
+ API 製品の公開方法の習得

次は、[Lab 7 - コンシューマーによる使用](../Lab%207%20-%20Consumer%20Experience)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"
