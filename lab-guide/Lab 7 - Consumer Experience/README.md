# Lab 7 - コンシューマーによる使用

このラボでは、開発者組織向けに公開された API のコンシューマーによる使用について学習します。

開発者ポータルを使用して、まずは管理者ユーザーとして Think IBM のカスタム・テーマをロードします。次に、開発者としてログインし、アプリケーションを登録し、API にサブスクライブして API をテストします。

---
# Lab 7 - 学習目標

このラボの学習目標は以下のとおりです。

+ 開発者ポータルの全体的なルック・アンド・フィールを変更するためにカスタム・テーマを追加する方法の習得
+  開発者ポータルでのアプリケーションの登録方法の習得
+ API を使用するためにプランにサブスクライブする方法の習得
+ 開発者ポータルから API をテストする方法の習得
+ サンプルのテスト・アプリケーションから API を使用する方法の習得

---
# Lab 7 - このチュートリアルで利用するケース・スタディー

このチュートリアルでは、独自のルック・アンド・フィールを適用することで開発者ポータルを自社用にカスタマイズしたいと考えている **ThinkIBM** の要望に応えます。また、同社は API の潜在的なコンシューマーであるアプリケーション開発者に、セルフサービス体験を提供したいとも考えています。 

---
# Lab 7	- 手順

## 7.1 - 開発者ポータルのカスタマイズ

このセクションでは、事前構築されたカスタム・テーマを活用します。テーマ・ファイルは、事前定義ファイル一式を格納した zip ファイルとしてパッケージされています。このテーマが入った zip ファイルを開発者ポータルにロードします。カスタム・テーマの作成については、既存のカスタム・テーマを入手して、名前を変更し、自社のカスタマイズ要件に合わせて下表に説明されたとおりにファイルを変更するのが最適な方法です。

|File 名|説明|
|-----------------|-----------|  
|`overrides.css`|このスタイル・シートは、基本の  IBM API Connect テーマから継承されたフォント、色、ならびにその他のデフォルト設定をオーバーライドします。この css ファイルの名前は、theme.info ファイルで指定されます。|
|`screenshot.png`|このファイルには、カスタム・テーマのホームページのスクリーンショットが格納されます。このファイルを外観の設定で使用することで、有効化したいテーマを簡単に探し、デフォルトのテーマとして設定できます。overrides.css の編集が完了し、ウェルカム・バナーの設定に満足したら、Snagit などのスクリーン・キャプチャー・ツールで開発者ポータルのホームページのスクリーンショットをキャプチャーし、ファイルをテーマ・ファイルの最上位ディレクトリーに配置します。このスクリーンショット・ファイルの名前は、theme.info ファイルで指定されます。|
|`templete.php`|template.php ファイルには、Drupal のデフォルト・マークアップを操作するためのサブテーマの関数が格納されます。Drupal テーマを作成または変更するときに最も役立つファイルの 1 つです。このファイルを使用することで、プリプロセス関数やプロセス関数を使用して任意のテーマのフック変数を変更したり、独自の変数を追加したりできます。任意のテーマ関数をオーバーライド (モジュールのデフォルトのテーマ関数を、独自に作成した関数に置き換え) したり、フックを変更する関数を呼び出し、フォームの render element を含む Drupal 内部のさまざまな部分を変更したりできます。これらの関数に関して詳しくは、<http://api.drupal.org> を参照してください。| 
|`thinkibm_connect_theme.info`|この .info ファイルは、テーマを定義・構成するための静的テキスト・ファイルです。この .info ファイルの各行はキーと値のペアになっており、等号 (=) の左側のキーと右側の値が対にされます (例: name = my_theme)。.info ファイルの命名規則として、ファイル名にテーマ名が含まれていなければなりません。テーマの名前は .info ファイル内の project キーに含まれています。|
|`theme-settings.php`|Drupal テーマには、いくつかの無料で使える構成可能な設定オプションがあります。例えば、ほとんどのテーマで、検索ボックス、サイト・スローガン、ユーザー画像などを選択するためのトグルを用意してます。同様に、ほとんどのテーマに、カスタム・ロゴやファビコンを追加するためのファイル・アップロード・ウィジェットが付いています。これらの設定は簡単です。設定は、Drupal によってデフォルトでテーマの構成ページに追加されるため、追加作業は必要ありません。独自のカスタム設定を作成する際に、テーマの構成フォームへ現行リリース情報を含む非表示フィールドを追加するためには、新規ファイル `theme-settings.php` をテーマに追加する必要があります。このファイル内で指定される関数名は、テーマ名 `thinkibm_connect_theme` が接頭辞に付いている必要があります。|
|`thinkibm_welcome_banner.png`|このファイルはウェルカム・バナーに表示される画像を指定します。ウェルカム・バナーはテーマ・ファイルの zip に含める必要はありませんが、他のテーマ・ファイルと一緒に置いておくとよいでしょう。|
|`favicon.ico`|Web 開発を行う場合、訪問者のブラウザーのブックマーク・フォルダー内とアドレス・バーの近くに表示される小さいロゴを指定できます。このロゴは「ファビコン」と呼ばれます。Drupal からは、知名度のある水滴の形をしたデフォルトのロゴが用意されています。この Drupal ロゴを使用しても問題ありませんが、サイトを目立たせるには、独自のロゴを使用することをお勧めします。ファビコン・ファイルは .ico 形式で、サイズはとても小さく、デフォルトの Drupal のファビコンは高さ 32 ピクセル × 幅 32 ピクセルで、同じファイルに格納できる 16 x 16 ピクセルのバージョンを使用するブラウザーも多くあります。というのも、ファビコンはアドレス・バーおよびお気に入り (ブックマーク) リストに表示するためだけのアイコンであり、通常これらの場所はスペースが限られているためです。作成するファビコンは、同等のサイズにとどめましょう。|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
|`logo.png`|開発者ポータル・ページの左上に表示されるデフォルトのロゴです。|

### 7.1.1 - カスタム・テーマのインストールと構成

カスタム・テーマ・ファイルの内容について説明が終わったところで、カスタム・テーマのロードにうつります。

1. Firefox ブラウザーのツールバーで `Portal` ブックマークをクリックして開発者ポータルを起動します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/DeveloperPortalFirefoxBookmark.png)

2. ユーザー名に `admin`、パスワードに `Passw0rd!` を使用して管理者として開発者ポータルへログインします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAdminLogin.png)

3. 管理者メニューから、`Appearance` を選択し、`Install new theme` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/InstallNewPortalTheme.png)

4. `Browse` ボタンから、`/home/student/lab_files/lab7/` ディレクトリーにナビゲートし、`think_ibm_connect_theme.zip file` を選択して `Open` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/open-theme-zip.png)

5. `Install` ボタンをクリックし、**ThinkIBM** カスタム・テーマをインストールします。   

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/InstallPortalThemeZipFile.png)
    
6. カスタム・テーマのインストールが完了したら、`Enable newly added themes` リンクをクリックします。
   
7. `thinkibm_connect 7.43` テーマが表示されるまで、テーマのリストをスクロールダウンします。`Enable and set default link` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/EnableandSetDefaultThinkIBMTheme.png)
       
8. テーマの設定に行き、構成を保存する必要があります。テーマの下の `Settings` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ThinkIBMSettings.png)
   
9. 設定ページの最下部までスクロールダウンし、**Toggle display** セクションを見つけます。**Toggle display** を展開して、画面一番下の `Save Configuration` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ThinkIBMSaveConfiguration.png)
	
10. `Home` アイコンをクリックしてホームページに戻ります。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/CloseThemeSettings.png)

### 7.1.2 - ウェルカム・バナーのカスタマイズ
デフォルトのウェルカム・バナーを、カスタムのバナー画像に変更します。 

1. 管理メニューから `Content > Blocks` を選択して ブロックのコンテンツ・ページにナビゲートします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ThinkIBMBlocks.png)

2. ウェルカム・バナー (Welcome Banner) ブロックの右の操作の下の `Edit` を選択します。 

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/EditWelcomeBannerBlock.png)
	   
3. バナーの編集ページをスクロールダウンし、**Image** の下の `Browse` ボタンをクリックし、ファイルを開きます。

4. `/home/student/lab_files/lab7/think_ibm_connect_theme` ディレクトリーにナビゲートし、`thinkibm_welcome_banner.png` ファイルを選択します。`Open` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/open-welcome-banner-img.png)

5. `Upload` ボタンをクリックして新規のウェルカム・バナー・イメージをアップロードします。

6. ページの一番下までスクロールダウンし、`Save` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SaveWelcomeBanner.png)

7. コンテンツ・ブロックの設定を閉じます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/CloseContentBlockSettings.png)

### 7.1.3 - コンテンツ・ブロックの地域設定の変更

一部のコンテンツ・ブロックの地域設定がリセットされたため、これらの設定を元に戻します。

1. 管理メニューから `Structure > Blocks` を選択して、ブロックのコンテンツ・ページの地域設定にナビゲートします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ThinkIBMBlockRegionSettings.png)

2. **Sidebar first** ブロックを見つけます。`Navigation`、`Support`、および `User login` のエントリーがあることを確認してください。

3. `Navigation` と `User Login` エントリーは**いずれ**も、ドロップダウン・メニューから `- None -` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SetBlockRegionstoNone.png)

4. `Support` セクションのエントリーのみが残っているはずです。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/sidebar-first-complete.png)

5. ページの一番下までスクロールダウンし、`Save Blocks` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SaveBlockRegions.png)

6. ブロックの地域設定を閉じます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/CloseContentBlockSettings.png)

7. 開発者ポータルのカスタマイズが完了しました。このラボではすべて紹介しきれませんが、他にもたくさんのカスタマイズ方法があります。開発者ポータルからログアウトします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAdminLogout.png)

## 7.2 - 開発者としてのアプリケーションの登録 

このセクションでは、アプリケーション開発者としてポータルにログインし、API を使用するために必要なアプリケーションを登録します。

1. ユーザー名に `developer@consumer.ibm`、パスワードに `Passw0rd!` を使用してアプリケーション開発者として開発者ポータルへログインします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalDeveloperLogin.png)

2. `Apps` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAppsList.png)

3. `Register new Application` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/RegisterNewApplication.png)

4. 以下のとおりにアプリケーションのタイトルおよび説明を入力し、`Submit` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SubmitNewApplication.png)

5. 後に Web アプリケーションで使用するために、クライアント・シークレットとクライアント ID をテキスト・エディターに取り込む必要があります。ページ上部にあるクライアント・シークレットの横の `Show Client Secret` チェック・ボックス、ならびにクライアント ID の横の `Show` チェック・ボックスを選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ShowClientSecretandID.png)

6. 以下のとおりに `Notes` アプリケーションを起動します。 

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SelectNotesApp.png)
	
7. ブラウザーに戻り、クライアント ID とクライアント・シークレットをコピーし、以下のように Notes アプリケーション・ウィンドウに貼り付けます。クライアント ID とクライアント・シークレットの値がそれぞれわかるように、メモを付記します。この値は後のセクションで必要になるため、Notes アプリケーションは閉じないでください。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SaveClientIDandSecret.png)

## 7.3 - ThinkIBM API のプランへのサブスクライブ 
このセクションでは、Think IBM Web Consumer アプリケーションを使用して ThinkIBM API のプランへサブスクライブします。

1. `API Products` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAPIProductsList.png)

2. `think (v1.0.0)` API 製品へのリンクをクリックします。                                                          

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SelectThinkIBMProduct.png)

3. 左側のナビゲーション・メニューの `Plans` リンクをクリックし、以下のとおり **Silver** プランの下の `Subscribe` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SubscribetoThinkSilverPlan.png)
	
	> ![Alt text][info]
	> 
	> Gold プランでは、所定時間内の要求数は制限されませんが、すべてのサブスクリプションの要求に対して API プロバイダーによる承認が必要です。Silver プランの要求数は 1 時間につき 100 件に制限されますが、サブスクリプションの要求に API プロバイダーの承認は必要ありません。    

4. `Think IBM Web Consumer` トグルを選択し、`Subscribe` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SubscribeThinkIBMWebApplicationtoPlan.png)

5. これで、`Think IBM Web Consumer` Web アプリケーションが `think` API 製品の Silver プランにサブスクライブされました。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ThinkIBMWebApplicationSubscribed.png)

6. `think v1.0.0` API 製品の Silver プランにサブスクライブしたアプリケーションを検証しましょう。`Apps` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAppsList.png)

7. `Think IBM Web Consumer` アプリケーション・リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SelectThinkIBMWebConsumerApp.png)

8. 以下のとおり、`Think IBM Web Consumer` アプリケーションが `think` API 製品の Silver プランにサブスクライブされています。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/ValidateSubscribedtoThinkSilverPlan.png)

## 7.4 - 開発者ポータルからの `think` 製品 API のテスト

このセクションでは、開発者ポータルを使用して think 製品 API の 1 つをテストします。このテストは、アプリケーション開発者が、アプリケーション開発を完成する前に API を**_試用_**してみる場合や、または単に API への入力に対して期待される応答が得られるかどうかを確認したい場合に役立ちます。このラボの後のセッションでは、これらと同じ API を使用する、開発済みの実際のアプリケーションを実行します。開発者ポータルから `logistics` API をテストします。

1. `API Products` リンクをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalAPIProductsList.png)

2. `think (v1.0.0)` API 製品へのリンクをクリックします

  ![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SelectThinkIBMProduct.png)

3. 左側のナビゲーション・メニューで `logistics 1.0.0` リンクをクリックし、`GET /shipping` パスの横の上向き矢印をクリックして展開します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalExpandShipping.png)

4. `GET /shipping` パスの **Try this operation** セクションまでスクロールダウンします。任意の zip コードを入力し、`Call Operation` ボタンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalTryShippingOperation.png)

5. `Call operation` ボタンの下へスクロールダウンします。`200 OK` と、以下のような応答本体が表示されるはずです。
  
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/PortalTryShippingOperationResultsSuccess.png)

## 7.5 - コンシューマー・アプリケーションの構成と実行

このセクションでは、`Think IBM Web Consumer` アプリケーションの実装の構成ファイルを編集します。このアプリケーションは、API Connect 環境、および先ほど作成した API とやりとりできるように事前構成されています。

このアプリケーションは、OAuth のトークン交換を処理してくれるほかにも、資格の検証用にすべての API 呼び出しに対してクライアント ID とクライアント・シークレットを提供します。  

1. `Sublime Text` エディター・ウィンドウがすでに開いている場合はタスクバーから、または以下のようにお気に入りメニューから起動します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/launch-sublime-text.png)

2. `Sublime Text` エディター・ファイルのツリー・メニューで、`lab7 > consumer_app > config` フォルダーを展開し、`default.json` ファイルを選択してソースを編集します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/open-default-json.png)

3. Notes アプリケーションのクライアント ID とクライアント・シークレットの値をコピーして、以下のとおり、`default.json` ファイルの `"client_id":""` および `"client_secret":""` 値の引用符の間に貼り付けます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/EditOpenThinkIBMWebConsumerConfigFile.png)

4. Sublime Text エディター・ウィンドウの `default.json` タブで、アプリケーション・メニューから `File > Save` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/SaveOpenThinkIBMWebConsumerConfigFile.png)

5. ここまで終わったら Terminal Emulator の新規ウィンドウを開き、`Think IBM Web Consumer` アプリケーションを開始できます。

  お気に入りメニューをクリックして以下のとおり `Terminal Emulator` を選択します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/launch-teminal-emulator.png)

6. 以下のとおり `cd` コマンドを入力して、home/student/lab_files/lab7/consumer_app/` ディレクトリーにナビゲートます。

	```bash
	cd ~/lab_files/lab7/consumer_app/
	```
                                                                                                                                                                                                                                                                                                                                                                                                                    
7. 以下のとおり `npm start` コマンドを入力して、`Think IBM Web Consumer` アプリケーションを開始します。アプリケーションは **Node.js** アプリケーションです。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         

	```bash
	npm start
	```

8. `Think IBM Web Consumer` アプリケーションがブラウザー・ウィンドウで開きます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/think-consumer-home.png)

## 7.6 - Think IBM Web Consumer アプリケーションからの API のテスト 

1. Lab 4 で OAuth を要求することで `inventory` API を保護したことを思い出してください。ログインせずに品目の在庫を表示しようとすると、ユーザー名とパスワードを求められます。`Browse Item Inventory` ボタン、または `Log In` リンクをクリックし、ログイン・プロンプトを開きます。

2. ユーザー名とパスワードを入力し、`Log In` ボタンをクリックします。サンプルの認証サービスはいかなる認証情報が入力されても受け入れるダミーのリポジトリーです。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/consumer-login.png)

	> ![][info]
	> 
	> Consumer アプリケーションは、OAuth トークン URL に接続し、標準的な OAuth 手順でトークン交換を処理します。トークンを確認したい場合は、Consumer アプリケーションを起動したターミナルに移動し、ログを確認できます。以下のような行を探してください。

	```text
	Using OAuth Token: AAEkZGEwZDgxNDMtMzIxMS00ZDgyLWE3MGYtMTJmY2UyYjk1YmUyxzqSMdEr7wM8XU_edO3dmxGDmC1ZGaqUC9Ibh-rOVNlflzd6blfDq4CGaNsD1qn-KESw6Q6RN_TPA0IfMdIn1nHY4ZpGRYa5N0f7mgY2Jg4Tfhm0IlhCUq5HRvoo7c4SIX7SAS3rL998_BvMVBST_g
	```

3. ログインが完了したら、Consumer アプリケーションが品目の在庫のページにリダイレクトします。ページに表示されているデータは、`inventory` API によって取得されたものです。Consumer アプリケーションは API を呼び出し、それが LookBack アプリケーションに送信されます。そして、LookBack アプリケーションが、在庫データが保存されている MySQL データ・ソースへの接続を処理します。 

4. ページのさまざまな箇所をクリックしてみてください。レビュー・フォームを使用して品目に対するレビューを残します。品目のレビューは、MongoDB に格納された別のデータ・モデルですが、LoopBack アプリケーションを通して品目と関連付けられていることを思い出してください。

	新しいレビューが掲載されるとともに、製品の評価も自動的に更新されることを確認してください。これは Lab 3 で LoopBack アプリケーションにカスタム・コードを追加したことによる動作です。
	
	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/sample-reviews.png)

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/updated-rating.png)

5. Consumer アプリケーション機能のテストを続けます。自宅の zip コードを入力して配送データを取得してみましょう。これは、Lab 5 で高度なアセンブリー手法を使用して構築した `logistics` API によって実現する機能です。入力した zip コードによって、見積額は変わります。

	最寄りの店舗へのリンクも提示されたことを確認してください。このリンクをクリックすると、Google Maps へのリンクが開きます。これも Lab 5 のアセンブリーで組み込んだ機能です。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/shipping-results.png)

6. 最後に、`Calculate Monthly Payment` リンクをクリックしてみてください。これは、Lab 5 で REST から SOAP へのアセンブリーを使用して構築した `financing` API によって実現する機能です。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab7/financing-results.png)

7. いくつかの製品で上記の手順を繰り返しアナリティクス・データベースにデータを作成します。

# Lab 7 - まとめ

**Lab 7 の学習過程は以上です。**これで Lab 7 は完了です。

次は、[Lab 8 - API Connect での分析](../Lab%208%20-%20Analytics%20in%20API%20Connect)

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"
