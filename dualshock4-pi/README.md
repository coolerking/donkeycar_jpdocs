> https://github.com/macunixs/dualshock4-pi のREADME.mdを翻訳しました。

# デュアルショック 4 + Raspberry Pi 3

<b>これは、Dualshock 4をRaspberry Pi 3で稼働させるために必要なコマンドのコンパイルを単純化したものです。</b>

ガイドで使用したリンク/Webサイトのクレジット：

https://github.com/retropie/retropie-setup/wiki/PS4-Controller

https://github.com/chrippa/ds4drv

https://www.piborg.org/joyborg

<h1>デュアルショック4用のLinuxドライバをRaspberry Pi 3にインストールする手順</h1>

すべてのコマンドを<b>コピー</b> アンド <b>ペースト</b> してインストールを完了させます。
インストールした後でBluetooth接続を確認するために、デュアルショック 4 コントローラが充電されていることを確認してください。

<b>コマンドリスト:</b>

<h2>ステップ 0:</h2> 

<code>sudo apt update</code>

<h2>ステップ 1:</h2> 

ジェネリック（汎用）ジョイスティックドライバが最初にインストールされていることを確認するには、次のコマンドを実行する必要があります：
<code>sudo apt -y install joystick</code>

<h2>ステップ 2:</h2> 

次に、jstestを次のように実行します：

<code>jstest /dev/input/js0</code>

<b>注意: 上記のコマンド<code>jstest</code>を最初に実行すると、次のエラーが表示されます： <code>jstest: No such file or directory</code> これは過去に一度もジョイスティックを接続していないためです。心配しないで、ステップ3に進んでください。</b>

<h2>ステップ 3:</h2> 

<code>sudo apt install python-dev python3-dev python-pip python3-pip</code>

<h2>ステップ 4:</h2> 

<i>(以下のどちらかの行を実行してください)</i> 

<b>Python 3でインストール</b>:
<code>sudo pip3 install ds4drv</code>

<b>Python 2でインストール</b>:
<code>sudo pip install ds4drv</code>

<h2>ステップ 5:</h2>  

<b>非rootユーザにds4drv ジョイスティック操作を許可させます： Location:<code>/etc/udev/rules.d/</code></b>

<code>sudo wget https://raw.githubusercontent.com/chrippa/ds4drv/master/udev/50-ds4drv.rules -O /etc/udev/rules.d/50-ds4drv.rules</code>

<code>sudo udevadm control --reload-rules</code>

<code>sudo udevadm trigger</code>


<h4>これでドライバのインストールは完了したのでデュアルショック4を使ってペアリングをテストします。</h4>

<h4>Raspberry PiのBluetoothをオンになっていることを確認します。そして、デュアルショック4の<code>PS ボタン</code> + <code>Share ボタン(十字キーの右上)</code> を押し続けます。
LEDがすばやく点滅するまで押し続けます。</h4>

<h2>ステップ 6:</h2> 

<b>ターミナルで次のコマンドを実行しドライバが正常動作していることを確認します:</b>

<code>sudo ds4drv</code>

<b>もしドライバをバックグラウンド(デーモン)実行したいのであれば、末尾に <code>&</code>をつけて実行してください：</b>

<code>sudo ds4drv &</code>

<b>すべて正常であれば、Piはコントローラを発見します。 <code>ステップ 2</code> を再実行してデュアルショック4の入力をテストします。新しいターミナルを開きしんきセッションで次のコマンドを実行してみましょう。</b>

<code>jstest /dev/input/js0</code>

<b>______________________________________________________________________________________________________________________</b>

<h2>ステップ 7:</h2> 

<b>[重要] Now configure ds4drv to run at startup by editing the rc.local file:</b>

<code>sudo nano /etc/rc.local</code>

<b> <code># By default this script does nothing.</code> という行が表示されたあと、次のような新規の行が追加されます:</b>

<code>/usr/local/bin/ds4drv &</code>

<h2>ステップ 8:</h2> 

<b>今度はPiをリブートするだけで、起動時にドライバが起動されます。</b>

<b>一旦リブートしたら、ステップ9のコマンドを実行して、<code>ds4drv</code> ドライバがrootユーザでバックグラウンドとして実行していることを確認します。</b>

<h2>ステップ 9:</h2> 

<code>ps aux | grep python</code>

