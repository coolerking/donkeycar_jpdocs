# コントローラパーツ

## Local Web Controller

携帯電話やブラウザからの車の運転を実現するデフォルトのコントローラ。このコントーローラは、カメラのWebライブプレビューを使っています。制御オプションには以下が含まれます：

1. 仮想ジョイスティック。
2. （サポート対象の加速度計を備えたモバイルデバイスを使用している場合）デバイスの傾きを操作に使用する。
3. Webアダプタを使用した物理的なジョイスティック。サポートはブラウザ、OS、およびジョイスティックの組み合わせによって異なる。

## Physical Joystick Controller

物理的なジョイスティックを運転操作で使用するには、デフォルトWebコントローラを1行をこのコントローラに置き換えます。これはデフォルトで OS デバイス `/dev/input/js0` を使用します。理論的には、OSがこのキャラクタデバイスにマウントされているジョイスティックデバイスを使用することになります。実際には、ジョイスティック（Sonyまたはノックオフ）のモデル、またはXBoxコントローラとそれをサポートするために使用されるBluetoothドライバの種類によって動作が変わります。デフォルトのコードは、 [SonyブランドのPS3 Sixaxisコントローラ]((https://www.amazon.com/Dualshock-Wireless-Controller-Charcoal-playstation-3)) で作成され、テストされています。他のコントローラも機能するかもしれませんが、Bluetoothの代替インストールが必要で、正しい軸とボタンのためにソフトウェアを調整する必要があります。

これらはUSBケーブルで接続することができますが、デフォルトコードとosドライバはこの設定をポーリングするバグを内包しています。ワイヤレスで応答性の高いコントロールのためにBluetoothを設定するほうがずっと安定していて便利です。

### config.py の変更、もしくは --js オプションをつけて実行

```
python manage.py drive --js
```
上記のコマンドを実行することで、ジョイスティックで運転することができます。これにより、カメラのライブプレビューとWebページの機能が無効になります。 `config.py` を変更して `USE_JOYSTICK_AS_DEFAULT = True` にすると、 `--js` を指定して実行する必要はありません。

### Bluetooth のセットアップ

[このガイド](https://pythonhosted.org/triangula/sixaxis.html) に従ってください。`Accessing the SixAxis from Python` セクションの手順を無視することができます。リンクが古くなった場合にそなえて、ここにも手順を残しておきます。

``` bash
sudo apt-get install bluetooth libbluetooth3 libusb-dev
sudo systemctl enable bluetooth.service
sudo usermod -G bluetooth -a pi
```

ユーザグループを変更したら、リブートします。

PS3コントローラをUSBケーブルで接続します。 中央の PS ロゴボタンを押します。コマンドラインペアリングツールを入手して、ビルドし、実行します：
```bash
wget http://www.pabr.org/sixlinux/sixpair.c
gcc -o sixpair sixpair.c -lusb
sudo ./sixpair
```

`bluetoothctl` を使ってペアリングを行います。
```bash
bluetoothctl
agent on
devices
trust <MAC アドレス>
default-agent
quit
```

USBケーブルを外します。中央の PS ロゴボタンを押します。

PS3 コントローラがリモート接続サれていることをテストするには、 `/dev/input/js0` が存在していることを確認します。
```bash
ls /dev/input/js0
```

### PS3 6軸ジョイスティックのチャージ

何かの問題があり、このジョイスティックは、有効な Bluetooth コントロールやOSドライバを持たないUSBポートでの充電がうまくいきません。これは、電話機のUSB充電器が機能しないばかりか、Windowsマシンからの充電までうまくいかないことを意味します。

Raspberry Pi からは充電が可能です。ジョイスティックを Raspberry Pi に接続し、ACアダプタやPCから Raspberry Pi へ給電して充電してください。

### PS3 6軸ジョイスティックのバッテリ交換

コントローラがかなり古くバッテリが弱っている場合は、 [この新しいバッテリーへのリンク](http://a.co/5k1lbns) を確認してください。カバーを取り外すときは注意してください。5本のネジを外します。ハンドグリップの上半分にタブがあります。あなたは分割/正面から開いて、下を前方に引っ張ってみてください。そうしない場合は、タブを壊してしまいます。

