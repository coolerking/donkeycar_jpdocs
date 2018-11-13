# donkeycar パッケージは、週末に２つのアップデートを行います

* 私個人の管理下にある GitHub リポジトリから [donkeycar リポジトリ](https://github.com/autorope/donkeycar)に移動しました
*　デフォルトのビルドに含まれていないpartを削除した新バージョン(2.5.8)のdonkeycarをリリースしました。これはすでに先進的なpartのいくつかを使用している人たちにとってはステップバックのように思えるかもしれませんが、新しいpartを作成して共有したい人のための方たちから私の作業を引き離してくれます。新しいpartは、独自のpythonパッケージでかつ `donkey createcar`ステップにインストールできるrepoでなければなりません。
*　削除されたパーツは、自分のリポジトリに移動できるようになるまで、一時的に [ここ](https://github.com/autorope/donkeyparts/blob/master/donkeyparts/controller.py) に置いておきます。
* @acbot は Donkey Carのための Raspberry Pi Hat(訳者注：おそらくDCDCコンバータのこと) をリリースしているので、piのための別のusbバッテリの必要はありません。ここでは、2番目の注文を取得したい人向けのアップデートを掲載します。jitx.com の duncan のおかげで、ボードの設計を自動化することができました。
* これらの変更でドキュメントを更新する必要があります。何か問題があると思われる場合は、pull request を提出してください。

現在まだpull requestを処理していますが、master==devであれば、これははるかに簡単になるでしょう。 
