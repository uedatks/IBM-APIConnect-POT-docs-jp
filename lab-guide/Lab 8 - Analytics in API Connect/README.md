# Lab 8	- API Connect での分析

このラボでは、ゲートウェイ・ノードでキャプチャーした情報を、分析を使用して視覚化する方法について概略的な理解を得ることを目的としています。API のイベント・データをフィルターに掛け、ソートし、集約して、その結果を相関関係のあるグラフ、表、およびマップに表示することで、サービス・レベルの管理、割り当て量の設定、コントロールの確立、セキュリティー・ポリシーのセットアップ、コミュニティーの管理、および傾向の分析に役立てることができます。

API 分析は、Kibana V4.3 というオープン・ソースの分析と視覚化のプラットフォーム上に構築されています。このプラットフォームは、Elasticsearch のリアルタイムの分散検索および分析のエンジンと連携して動作するように設計されています。

---
# Lab 8 - 学習目標
 
このラボの学習目標は以下のとおりです。

+ API Manager 内でカタログの分析を表示する方法の習得
+ カスタム・ダッシュボードの作成方法の習得
+ ダッシュボードへの可視化の追加方法の習得 
+ ダッシュボード内での可視化のカスタマイズおよび配置方法の習得

---
# Lab 8 - このチュートリアルで利用するケース・スタディー

このチュートリアルでは、ダミーの API に動作する一連の `curl` コマンドを呼び出すスクリプトを実行し、API Connect プラットフォームを通過するトラフィックをシミュレートします。スクリプトが完了したら、API Manager でダッシュボードのカスタマイズとシステム上のトラフックに対する分析を表示する方法を、実践をとおして学びます。

---
# Lab 8	- 手順

# 8.1	- API Connect で分析データにデータを入れる API 呼び出しのステージング

1. `Firefox Web Browser` が開いていない場合は、お気に入りメニューから起動します。

2.  `API Manager` ブックマークをクリックします。

3. 以下の認証情報を使用して `API Manager` サーバーにログインします。

	> Username: `student@think.ibm`
	
	> Password: `Passw0rd!`

4. `Sandbox` カタログのタイトルをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/api-mgr-dashboard-sandbox-tile.png)

5. `Sandbox` カタログの構成画面で、`Analytics` をクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-tab.png)

6. デフォルトのダッシュボードには、最もアクティブだった 5 個の製品や、最もアクティブだった 5 個の API などの一般的な情報がいくつか表示されます。これらも興味深い情報ですが、ダッシュボードをカスタマイズすることで、このほかにも多くの情報を表示させられます。`+ Add Visualization` アイコンをクリックして、新規の可視化を追加します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-add-visualization.png)

7. これによって、一部の標準の可視化のリストが表示されます。ストリングを入力して可視化にフィルターを掛けたり、矢印を使ってリスト内を移動したりできます。

8. `API Calls` 可視化をクリックしてダッシュボードに追加します。新規の可視化がダッシュボードの一番下に追加されます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/image21.png)

9. 新規の可視化が表示されるまでスクロールダウンします。右下の境界線をクリックし、ドラッグすることで、サイズを調整できます。さらに、ボックスをクリックし、ドラッグすることで、位置も調整できます。

	`API Calls` ボックスを「5 Most Active Products」ウィンドウと「5 Most Active API」ウィンドウの間に移動します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/image22.png)
	
10. 他の可視化もダッシュボードに追加し、自由に動かしてみてください。`Save Dashboard` ボタンをクリックすることでダッシュボードを保存することもできます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-save-dashboard.png)

11. すぐに使用できるダッシュボードもいくつか用意されているので、`Load Saved Dashboard` アイコンをクリックし、開いてみましょう。`api default` ダッシュボードを開きます。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-load-dashboard.png)

12. 処理された API トラフィックに関する情報が、さまざまなグラフや図で可視化されています。

13. 分析データはさまざまな期間でフィルターをかけられるほか、ウィジェットを自動最新表示させることもできます。デフォルトの期間である `Last 7 days` が設定されているカレンダー・アイコンをクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-calendar.png)
	
14. `Auto Refresh` をクリックします。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-auto-refresh.png)

15. 最新表示される時間を `5 seconds` に設定します。

	![](https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/lab8/analytics-refresh-5sec.png)

16. Chrome Web ブラウザーのコンシューマー・アプリケーションに戻ります。サイト内をナビゲートし、いくつかの機能を試して、ほかの API 呼び出しを生成してみてください。

17. 分析ビューに戻り、データが自動的に最新表示されていることを確認してください。

---
# Lab 8 - 修了

**Lab 8 の学習過程は以上です。**これですべてのラボを終了しました。

[important]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/important.png "Important!"
[info]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/info.png "Information"
[troubleshooting]: https://github.com/ibm-apiconnect/pot-onprem-docs/raw/master/lab-guide/img/common/troubleshooting.png "Troubleshooting"
