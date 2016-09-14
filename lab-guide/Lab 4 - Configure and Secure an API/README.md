# Lab 4 - API の構成と保護

このラボでは、LoopBack アプリケーションの作成時に作成された `inventory` API を構成、保護する方法を学習します。API Designer のグラフィカルなデザイン・ツールを使用して、`oauth` という名前の OAuth 2.0 プロバイダー API を作成し、このプロバイダーを使うように `inventory` API を更新します。API エディターのアセンブル・ビューで API のランタイム動作を指定します。

---
# Lab 4 - 学習目標

このラボの学習目標は以下のとおりです。

+ OAuth 2.0 プロバイダーを作成する方法、具体的には、リソース所有者パスワードの権限付与タイプを使用した作成方法の習得
+ 新規作成した OAuth 2.0 プロバイダーを使用して、既存の API を保護する方法の習得
+ API のプロパティーに対してカタログ固有の値を追加する方法の習得
+ activity-log、set-variable と invoke ポリシーを使用した API 実装のアセンブリー方法の習得

<!---
FIXME: Because of a limitation in the proxy policy not being able to pass
the Host header, we'll need to use the Invoke policy. One downfall though
of using invoke is that the filter parameters don't work.
-->


---
# Lab 4 - このチュートリアルで利用するケース・スタディー

このチュートリアルでは、在庫 API にセキュリティーを適用し、**ThinkIBM** が公開するリソースを保護します。有効な OAuth トークンを取得し、提示してからでないと、API のコンシューマーが 在庫 API を呼び出せないようにします。

---
# Lab 4	- 手順

## 4.1 - API Designer を使用した在庫 API の編集

1. プロジェクト・ディレクトリーに以下のコマンドを入力し、API Designer を起動します:

	```bash
	cd ~/ThinkIBM/inventory/
	apic edit
	```

	ブラウザーで API Designer が開きます。ドラフト API に関する通知メッセージが表示されます。(ただし、API Designer を初めて起動する時に表示される一回限りのメッセージです。) `Got it!` をクリックしてメッセージを消し、API の作成にうつりましょう。

	API ビューで、リストに API が 1 つ表示されているはずです。`inventory` API は、LoopBack アプリケーションの作成時に自動的に作成された API です。後続のステップで、この API を編集します。
	
	![alt text](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/startapidesigner.png)

## 4.2 - 新規 OAuth 2.0 プロバイダー API の追加

1. `+ Add` ボタンをクリックし、メニューから `OAuth 2.0 Provider API` を選択します。

2. 以下のプロパティーを指定し、`Next` ボタンをクリックして続行します。

	> Title: `oauth`
	
	> Name: `oauth`
	
	> Base Path: `/oauth20`
	
	> Description: `API for Obtaining Access Tokens`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/newoauthprops-1of2.png)



3. `Don't add to a product` がデフォルトで選択された状態のまま `Add` ボタンをクリックします。

	API エディターが起動します。初めて API エディターを起動する場合、通知メッセージが表示されます。`Got it!` ボタンをクリックしてメッセージを消し、作業を続行しましょう。  
	
	新規作成した `oauth` API が API エディターで開きます。画面左のビューには、情報、ホスト、基本パスなど、API 定義のあらゆるエレメントへのショートカットがあります。API エディターでは、デフォルトで、API が見やすく、編集しやすい `Design` ビューが開きます。ほかにも `Source` および `Assemble` のタブがあります。後に、これらのビューでも操作をしてみます。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/newoauth-start.png)

4. API の `Host` セクションにナビゲートします。「Host」 フィールドから `$(catalog.host)` を削除し、空欄にします。



1. `OAuth 2` セクションにナビゲートします。

	後続のステップで、クライアント・タイプ (公開または機密)、スコープに対する有効なアクセス権限、サポートされる権限付与タイプなど、OAuth 固有のオプションを設定します。ここで設定する各プロパティーの詳細情報は [OAuth 2.0 仕様](http://tools.ietf.org/html/rfc6749) に記述されています。

2. クライアント・タイプのフィールドで、ドロップダウン・メニューから `Confidential` を選択します。

3. OAuth プロバイダー API 作成時に、3 つのスコープが生成されました (`scope1`、`scope2`、`scope3`)。

4. `scope1` の値を編集し 、以下のとおりフィールドを設定します:

	> Name: `inventory`
	
	> Description: `Access to Inventory API`

5. スコープ定義の右にあるゴミ箱アイコンをクリックし、`scope2` と `scope3` を削除します。

6. このプロバイダーが、リソース所有者パスワード資格情報の権限付与タイプ*のみ*をサポートするよう設定します。`Implicit`、`Application` および `Access Code` の権限を選択解除し、`Password` のみにチェックを残します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/oauthgranttypes.png)

7. 残りの OAuth 2 の設定には以下の値を入力します:	> Collect credentials using: `basic`
	
	> Authenticate application users using: `Authentication URL`
	
	> Authentication URL: `https://services.think.ibm:1443/auth`
	
	> TLS Profile: `tls-profile-4` を**削除し**フィールドを空欄に
	
	> `Enable revocation URL` オプションは選択解除
	
	完成した設定は以下のようになっているはずです。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/edit-oauthoauth2.png)

	> ![][important]
	> 
	> ここで定義したスコープは、`inventory` API にこの OAuth 設定を利用するように指示する後続のステップで定義するスコープと完全に一致しなければなりません。後に発生する問題のほとんどが、大文字と小文字を区別しないことが原因で起こります。このような問題を防ぐために、スコープの定義ではすべて**小文字**を採用しましょう。

8. `Paths` セクションにナビゲートします。生成されたパスが `/oauth2` で始まっていることがわかります。先ほど、基本パスを `/oauth20` に設定したため、許可パスおよびトークン・パスを短縮します。

9. `/oauth2/authorize` パスを `/authorize` に変更します。  

10. `/oauth2/token` パスを `/token` に変更します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/edit-oauthpaths.png)

11. エディターの右上にある `Save` アイコンをクリックし、変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/save-icon.png)

## 4.3 - 在庫 API の構成と保護

1. API エディターの左上にある `All APIs` リンクをクリックし、API のリスト・ビューに戻ります。

2. `inventory` リンクをクリックします。

	在庫 API が API エディターで開きます。ここで必要な設定変更を行うことができます。後続のステップで、この API を先ほど作成した OAuth プロバイダーを使用するよう設定します。  
	

1. `trashcan` アイコンをクリックして `x-any` 定義を消去します。プロンプトが出たら、`OK` ボタンをクリックし、操作を確定します。

2. `Base Path` セクションにナビゲートします。

	基本パスを `/api` から `/inventory` に変更します。

3. API の `Host` セクションにナビゲートします。`$(catalog.host)` の値を**削除**します。

	先ほど作成した OAuth プロバイダー API と同様、このフィールドは空欄にします。

4. `Security Definitions` セクションにナビゲートします。

	**Security Definitions** セクションにある `+` アイコンをクリックし、メニューから `OAuth` を選択します。  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inv-addoauthsecuritydef.png)
	
	`oauth-1 (OAuth)` という名前の新しいセキュリティー定義が作成されているのがわかります。

5. 少しスクロールダウンし、新しく作成されたセキュリティー定義を編集します。

	以下のプロパティーを設定します:
	
	> Name: `oauth`
	
	> Description: `Resource Owner Password Grant Type`
	
	> Flow: `Password`
	
	> Token URL: `https://api.think.ibm/sales/sb/oauth20/token`

6. **Scopes** セクションにある `+` アイコンをクリックし、新しいスコープを作成します。以下のプロパティーを設定します:	> Scope Name: `inventory`
	
	> Description: `Access to all inventory resources`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inv-oauth.png)7. `Security` セクションへナビゲートし、`oauth (OAuth)` チェック・ボックスにチェックを入れます。  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inv-security-addoauth.png)
	
	OAuth プロバイダーを使って API が保護されたところで、API 呼び出し時の動作を定義しましょう。続く 2 つのセクションでは、`inventory` API が Lab 3 の最後に公開した在庫アプリケーションを呼び出せるように設定します。

## 4.4 - API プロパティーの追加

`inventory` API を処理するポリシーを追加し、構成する前に、いくつかの API プロパティーを設定する必要があります。プロパティーは API に必要なパラメーターを格納します。パラメーターの値は API が実行されるカタログによって異なります。

例えば、`inventory` API はバックエンドのサービスを呼び出すように構成します。API のデプロイ先であるカタログに応じて、このサービスのさまざまなインスタンスを呼び出せるようにするのが狙いです。

1. `Properties` セクションへナビゲートし、`+ Add` アイコンをクリックしてプロパティーを新規作成します。以下の属性を設定します:	> Name: `app-server`
	
	> Description: `Catalog-specific app server URL`
	
	> Default Value: `http://localhost/`

2. `Add value` リンクをクリックし、カタログ固有の値を追加します。以下のカタログ名と値を指定します:

	> Catalog: `Sandbox`
	
	> Value: `http://appsvr.think.ibm`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inventory_appserver_property.png)

	> ![][important]
	> 
	> カタログ名と値のフィールドは厳密に指定する必要があります。大文字と小文字を区別し、上記のとおり正確に入力する必要があります。カタログのドロップダウン・リストには、`sb` のデフォルト値が用意されていますが、上記にあるとおり、「Catalog」には必ず `Sandbox` と名前を入力する必要があります。さもなければ、後に問題発生の原因となります。

3. `+` アイコンをクリックして、プロパティーをもう 1 つ作成し、`app-id` と命名します:

	> Name: `app-id`

	> Description: `Inventory application id`

	> Default Value: `null`

4. OS のタスクバーの右上にあるメモ帳のアイコンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/notepad.png)

5. メモ帳から `Host header` をコピーします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/copy_app-id.png)

6. `app-id` プロパティーに新しい値を追加します:

	> Catalog: `Sandbox`
	
	> Value: `app-id` の値を貼り付け
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inventory_appid_property.png)

	> ![][troubleshooting]
	> 
	> app-id を貼り付けると、UI が意図せず値を追加してしまうことがあります。その場合、ゴミ箱アイコンをクリックし、余計な入力値を消去してください。

7. 変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/save-icon.png)

## 4.5 - API の処理動作の定義
API アセンブリーは、API ゲートウェイで適用、実行される複数のポリシーを提供します。ポリシーにはロギング動作の変更やメッセージのコンテンツやヘッダーを修正するためのアクションが含まれます。また、製品組み込みのポリシーがお客様固有のニーズに対応できない場合、独自のポリシーを作成し、API Connect の UI から API デザイナーに提供することができます。

1. `Assemble` タブに切り替えます。簡単なアセンブリーが用意されています。  

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inv-assembly-start.png)

2. DataPower Gateway ポリシーを利用するように、アセンブリーを変更します。

	`Filter` ラベルの右にあるメニュー・アイコンをクリックし、`Filter` メニューを展開します。 
	
	`DataPower Gateway policies` のラジオ・ボタンを選択します。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/filter-datapowerpolicies.png)

3. アセンブリーに `activity-log` ポリシーを追加し、API ペイロードのログを記録するように設定します。  

	利用可能なポリシーの一覧から、アセンブリーにて作成済みの `invoke` ポリシーの左へ `activity-log` ポリシーをドラッグします。

4. 新規に追加した `activity-log` ステップを選択します。画面の右にプロパティー・メニューが開きます。

	`Content` のドロップダウン・リストから `payload` を選択します。  
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/inv-assembly-logpayload.png)

5. `X` をクリックして activity-log エディターのメニューを閉じます。

6. `set-variable` ポリシーをアセンブリーの activity-log ステップの右に追加します。

7. `set-variable` ポリシーをクリックしてエディターにアクセスし、要求の `Host` ヘッダーを設定します。Liberty ランタイムは、この `Host` ヘッダーを使って、要求を適切に在庫アプリケーションへルーティングします。

8. `+ Action` ボタンをクリックします。

	アクションのプロパティーを編集し、ヘッダーの値を設定します:	
	> Action: `Set`
	
	> Set: `message.headers.Host`
	
	> Type: `string`
	
	> Value: `$(app-id)`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/set_app-id_host_header.png)



9. `X` をクリックして set-variable エディターのメニューを閉じます。

10. `invoke` ポリシーをクリックし、エディターを開きます。

11. デフォルトの `Invoke URL` パスを変更します。

	URL を次のとおり更新します: `$(app-server)$(request.path)`
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/invoke_url.png)
	
	> ![][info]
	> 
	> `$(request.path)` 変数は、完全な API Connect の要求パスから組織とカタログのコンポーネントを自動的に削除します。加えて、先頭にあるスラッシュ `/` は変数の値に既に含まれています。

12. `X` をクリックして invoke ポリシー・エディターのメニューを閉じます。

13. 変更を保存します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab4/save-icon.png)

14. タブまたはブラウザー・ウィンドウの `x` をクリックして Firefox ブラウザーを閉じます。

15. `Terminal Emulator` セッションに戻ります。

16. ブラウザーを閉じても、API Designer アプリケーションは動作し続けます。

	`control` と `c` のキーを同時に押し、API Designer セッションも終了します:
	
	```bash
	control+c
	```
	
	これで、コマンドライン・プロンプトに戻ります。

# Lab 4 - 検証

CLI と API Designer の両方を使用し、多くのファイルを作成および更新しました。次のラボへ進む前に、ソース・ファイルと完成したラボとを照らし合わせて検証するための簡単なスクリプトを用意しました。この手順を踏むことで、後に入力ミスに起因した問題の発生を防止することができます。

1. `Terminal Emulator` で以下を入力します:

		```bash
	validate_lab 4
	```
	
2. このスクリプトはプロジェクト・フォルダー (`~/ThinkIBM/inventory`) にある特定のファイルに対し一連の `diff` コマンドを実行します。

3. `validate_lab` スクリプトの出力に不一致があった場合、以下を入力し、訂正した変更をソース・ファイルに統合します:

	```bash
	merge_lab 4
	```

# Lab 4 - まとめ

**Lab 4 の学習過程は以上です。**これで、在庫 API を構成し、保護できました。後続のラボで、この API を利用します。

次は、[Lab 5 - 高度な API アセンブリー](../Lab%205%20-%20Advanced%20API%20Assembly)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"
