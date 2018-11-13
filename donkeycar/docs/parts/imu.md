# IMU パーツ


IMU(Initial Measurement Unit：慣性計測ユニット)は、ロボットの慣性力を感知するためのパーツです。それらはセンサによって異なりますが、一般的に線形および回転性の迎え角を取得することができます。それらは時々、磁力計を組み込んで、地球の方位を向ける世界的なコンパスを与えることができます。感度に影響を与えるため、これらから温度が頻繁に得られます。

## MPU6050
MPU6050 は、安く、小さく、適度に正確なIMUです。[Amazon](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Dindustrial&field-keywords=MPU6050) で一般に入手可能です。


* 通常はI2Cインタフェースを使ってデフォルトのPWM PCA9685ボードに接続します。この構成でも電源が供給されます。
* 加速度X、Y、Z、ジャイロスコープX、Y、Z、および温度を出力
* チップ内蔵16ビットADコンバータ、16ビットデータ出力
* ジャイロスコープの範囲：+/- 250 500 1000 2000度/秒
* 加速範囲：±2±4±8±16g

##### ソフトウェアのセットアップ

`smbus` をインストールします:

パッケージから:
``` bash
 sudo apt install python3-smbus
```

ソースコードから:
```bash
sudo apt-get install i2c-tools libi2c-dev python-dev python3-dev
git clone https://github.com/pimoroni/py-smbus.git
cd py-smbus/library
python setup.py build
sudo python setup.py install
```

mpu6050 用 pip ライブラリをインストールします：
```bash
pip install mpu6050-raspberrypi
```

