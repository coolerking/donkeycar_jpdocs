# donkeypart_ps3_controller パッケージ README.md 日本語訳

## PS3 ジョイスティック コントローラ

物理的なジョイスティックpartを入力デバイスとして使用するには、デフォルトのWebコントローラをジョイスティックコントローラに置き換えます。これはデフォルトでOSデバイス /dev/input/js0 を使用します。 理論的には、OSがこのようにマウントされたジョイスティックデバイスを使用することができます。 実際には、ジョイスティック（Sonyまたはノックオフ）モデル、またはXBoxコントローラとそれをサポートするために使用されるBluetoothドライバの種類によって動作が変わります。 デフォルトのコードは、 [SonyブランドのPS3 Sixaxisコントローラ](https://www.amazon.com/Dualshock-Wireless-Controller-Charcoal-playstation-3) で作成され、テストされています。
 
他のコントローラも動作するかもしれませんが、代わりのBluetoothのインストールが必要になります。正しいaxisやボタンのためにソフトウェアを調整してください。

これらはUSBケーブルで接続することができますが、デフォルトコードとosドライバはこの設定をポーリングするバグが存在します。ワイヤレスで応答性の高いコントロールのためにBluetoothを設定するほうが、ずっと安定していて便利です。

### インストール

1. Bluetooth　コントローラを Raspberry Pi へ接続します。 下記の Bluetooth 章を参照してください。

2. parts python パッケージをインストールします。
    ```bash
    pip install git+https://github.com/autorope/donkeypart_ps3_controller.git
    ```

3. `manage.py` の先頭部分にてpartクラスをインポートします。
    ```python
    from donkeypart_ps3_controller import JoystickController
    ```   

4. JoysticController をpartsとして組み込むには、`manage.py` のコントローラ parts を生成しているコードを置き換えてください。    
    ```python
    ctr = JoystickController(
       max_throttle=cfg.JOYSTICK_MAX_THROTTLE,
       steering_scale=cfg.JOYSTICK_STEERING_SCALE,
       throttle_axis=cfg.JOYSTICK_THROTTLE_AXIS,
       auto_record_on_throttle=cfg.AUTO_RECORD_ON_THROTTLE
    )

     V.add(ctr,
          inputs=['cam/image_array'],
          outputs=['user/angle', 'user/throttle', 'user/mode', 'recording'],
          threaded=True)
    ```

5. 必要な設定パラメータを `config.py` ファイルに追加します。 編集後のファイルは次のようになります。
    ```python
    #JOYSTICK
    JOYSTICK_MAX_THROTTLE = 0.25
    JOYSTICK_STEERING_SCALE = 1.0
    JOYSTICK_THROTTLE_AXIS = 'rz'
    AUTO_RECORD_ON_THROTTLE = True
    ```
6. コマンド `python manage.py drive` を実行して、走行を開始します。 

### Bluetooth セットアップ

[このガイド](https://pythonhosted.org/triangula/sixaxis.html) に従ってください。「PythonからSixAxosへのアクセス」章の手順は省略可能です。リンクが古くなった場合のために、手順をここにも残しておきます。
   ```bash
   sudo apt-get install bluetooth libbluetooth3 libusb-dev
   sudo systemctl enable bluetooth.service
   sudo usermod -G bluetooth -a pi
   ```

ユーザグループ設定変更後、リブートしてください。

USBケーブルでPS3を接続します。 センターのPSロゴボタンを押します。 コマンドラインペアリングツールを入手してビルドします。 それを実行します：
   ```bash
   wget http://www.pabr.org/sixlinux/sixpair.c
   gcc -o sixpair sixpair.c -lusb
   sudo ./sixpair
   ```

`bluetoothctl` コマンドをつかってペアリングを実行します。
   ```bash
   bluetoothctl
   agent on
   devices
   trust <MAC ADDRESS>
   default-agent
   quit
   ```

USBケーブルをはずします。そしてPSロゴボタンを押します。

PS3 コントローラがリモート接続されているかテストするには、 `/dev/input/js0` の有無で確認できます。
   ```bash
   ls /dev/input/js0
   ```

### PS3 sixaxis ジョイスティックの充電

何らかの理由で、PS3 sixaxis ジョイスティックは、BluetoothコントロールやOSドライバが有効でない場合USBポートで充電することができません。 これは、電話機などのUSB充電器ではうまく機能せず、Windowsマシンからの充電がうまくいかないことを意味しています。

Raspberry Pi からはいつでも充電することができます。 ジョイスティックをPiに差し込んで、充電器またはPCを使用してPiに電源を入れるだけで、良いです。

### PS3 sixaxis ジョイスティックのバッテリ交換

古いコントローラは時としてバッテリが弱っている事があると思います。このリンク先に [新しいバッテリ](http://a.co/5k1lbns) があります。カバーを外すときは注意してください。5つのスクリューをはすす必要があります。ハンドグリップの上半分にタブがあります。 蓋をとる/開く場合は、正面から見て、下の部分を前方に引っ張ってみてください。そうしないと、タブを壊してしまいます。


### ジョイスティックの操作方法

* 左アナログスティック - ステアリングを左右に調整
* 右アナログスティック - 前に倒すと全身スロットルを増加します
* 後進するには右アナログスティックを２回引きます

> Userモードの場合スロットル値が0ではない間、データを記録します！

* Selectボタンでモード切り替え - "User, Local Angle, Local(操舵およびスロットル)"
* △ - 最大スロットル値を増加
* ✕ - 最大スロットル値を減少
* ◯ - 記録の切り替え (デフォルトでは無効、で取るとではスロットルオン中の自動記録は有効)
* 上 - スロットル値の増加
* 下 - スロットル値の減少
* 左 - ステアリング値の増加
* 右 - ステアリング値の減少
