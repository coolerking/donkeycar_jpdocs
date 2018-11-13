# Part(パーツ)とはなにか

`Part` は、車両の機能コンポーネントをラップするPythonクラスです。`Part` には、次のような機能コンポーネントが含まれています:
* センサ - カメラ、レーダ、速度計、GPSなど
* アクチュエータ - モータコントローラなど
* パイロット - 車線検知、運転を模倣するモデルなど
* コントローラ - Webベースのもの、Bluetoothベースのもの
* ストア - `Tub` やデータ保存

次のコードは、`PiCamera` パーツを使って、各運転ループ上で　`'cam/img'` チャネルを提供するサンプルです。
```python
V = dk.Vehicle()

# カメラパーツの初期化
cam = PiCamera()

# 車両へパーツを追加
V.add(cam, outputs=['cam/img'])

V.start()
```

## パーツの解剖

車両駆動ループにて実行できるように、すべての part が共通の構造を持ちます。ここでは、数値を受け取り、乱数でそれを掛けて結果を返すサンプルパーツを紹介します。
```python
import random

# 特定のベースクラスを継承しなくてもOK
class RandPercent:
    # 車両駆動ループに追加された順にrun()が実行されます。
    # 引数・戻り値は、addした際の引数input/outputの順番
    # に従います。
    def run(self, x):
        return x * random.random()
```

このパーツを Vehicle へ追加し、実行するコードです：
```python
V = dk.Vehicle()

# チャネル値を初期化
V.mem['const'] = 4

# 同じチャネルを読み書きするパーツを追加
V.add(RandPercent, inputs=['const'], outputs=['cost'])

V.start(max_loops=5)
```


### パーツのスレッド化

車両がうまく動作するためには、ドライブループは1秒間に10〜30回実行する必要があります。処理の遅いパーツは、運転ループの終了待ちによるロックを避けるためにスレッド化する必要があります。

スレッド化されたパーツは、別スレッドで実行される関数を定義する必要があり、呼び出す関数自体は直近の値をすばやく返します。

A threaded part needs to define the function that runs in the separate thread
and the function to call that will return the most recent values quickly.

Here's an example how to make the RandPercent part threaded if the run
function too a second to complete.

```python
import random
import time

class RandPercent:
    self.in = 0.
    self.out = 0.
    # 処理結果を得た後１秒待機
    def run(self, x):
        return x * random.random()
        time.sleep(1)

    def update(self):
        # 自身のスレッド内でrun関数を実行
        while True:
            self.out = self.run(self.in)

    def run_threaded(self, x):
        self.in = x
        return self.out

```

* `part.run` : パーツを実行するために使用される関数
* `part.run_threaded` : 部品がスレッド化されている場合、駆動ループ機能が実行する
* `part.update` : スレッド化された関数
* `part.shutdown`

