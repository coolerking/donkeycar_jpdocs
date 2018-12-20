# Bluetooth ゲームコントローラ

[Wii-U](https://www.amazon.com/gp/product/B01GJBUNTG/ref=as_li_ss_tl?ie=UTF8&psc=1&linkCode=ll1&tag=donkeycar-20&linkId=a7fc2ff3e6489b9e6dd267a7f8b2ff19&language=en_US) (やその他の）Bluetooth ゲームコントローラでDonkey Carを操作するためのライブラリです。

### インストール

ライブラリをインストールします。
   ```bash
   git clone https://github.com/autorope/donkeypart_bluetooth_game_controller.git
   pip install -e ./donkeypart_bluetooth_game_controller
   ```


### BluetoothコントローラをRaspberry Piに接続

1. Raspberry Pi上で Bluetooth bashツールを開始します。
   ```bash
   sudo bluetoothctl
   power on
   scan on
   ```

2. コントローラをスキャンモードでスイッチを入れ、bluetoothctl スキャン結果の中からコントローラ名を探します。

3. IDを発見したらそのID(私のコントローラの場合は、`8C:CD:E8:AB:32:DE`)を使ってコントローラを接続します。これは数回実行し無くてはならないこともあります。
   ```bash
   pair 8C:CD:E8:AB:32:DE
   connect 8C:CD:E8:AB:32:DE
   trust 8C:CD:E8:AB:32:DE
   ```

4. コントローラが接続されたことを確認します（すなわち、点滅していたライトが点灯状態に変わる）。

5. `part.py` スクリプトを実行して、動作するかどうかを確認します。次のように、すべてのボタンの値が表示されます。
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


6. ボタン出力を表示できたら、あなたのDonkey Carアプリディレクトリ内の `manage.py` スクリプトにプラグインすることができます...
   ```python
   from donkeyblue import BluetoothGameController
   
   # then replace your current controller with...
   ctl = BluetoothGameController()
   
   ```

7. [コード](https://github.com/autorope/donkeypart_bluetooth_game_controller/blob/master/donkeyblue/part.py#L86) 上のボタンマッピングをチェックします。
