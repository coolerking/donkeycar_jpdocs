> 本ドキュメントは [donkey blue README.md](https://github.com/autorope/donkeypart_bluetooth_game_controller) を勝手に翻訳したものです。


# Bluetooth ゲームコントローラ

Donkey CarにWii-U(およびその互換デバイス）Bluetooth ゲームコントローラと接続するためのライブラリです。

## インストール

ライブラリをインストールします。

```bash
git clone https://github.com/autorope/donkeypart_bluetooth_game_controller.git
pip install -e ./donkeypart_bluetooth_game_controller
```

# Bluetooth コントローラをRaspberry Piに接続

* Raspberry PiのBluetooth bashツールを起動します

```bash
sudo bluetoothctl
power on
scan on
```

* コントローラをスキャンモードで起動し、Bluetoothスキャン結果の中からコントローラ名を探します。

* コントローラIDを見つけたら、そのID(自分のコントローラの場合は、`8C:CD:E8:AB:32:DE`)を使ってコントローラを接続します。これらのコマンドを何回か実行する必要があります。

```bash
pair 8C:CD:E8:AB:32:DE
connect 8C:CD:E8:AB:32:DE
trust 8C:CD:E8:AB:32:DE
```

* コントローラ側に接続したことを示す動作を確認します（たとえば、点滅していたLEDが点灯状態にかわる、など）。

* `part.py` を実行して、動作確認を行います。実行すると、次のようなすべてのボタンのPWM値が表示されます。

```bash
python ./donkeypart_bluetooth_game_controller/donkeyblue/part.py 


LEFT_STICK_Y 0.00234375 
LEFT_STICK_Y 0.0015625 
LEFT_STICK_Y 0.00078125 
A 1 
A 0 
Y 1 
Y 0 
X 1 
X 0
```

* ボタン出力を確認できたら、`manage.py`にDonkeycar用コントローラを次のようにプラグインすることができます。

```python
from donkeyblue import BluetoothGameController

# カレントのコントローラと差し替える
ctl = BluetoothGameController()
```

* 微調整は ボタンマッピング [コード](https://github.com/autorope/donkeypart_bluetooth_game_controller/blob/master/donkeyblue/part.py#L86) をチェックしてください。