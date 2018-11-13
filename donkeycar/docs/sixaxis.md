正直、だれも使わない情報かもしれませんが...

> この文章は https://pythonhosted.org/triangula/sixaxis.html を翻訳し、一部加筆・修正を加えたものです。


------

# プレイステーション3コントローラの設定

もしあなたがロボットを作っているなら、おそらくどこかで手動で操作する方法が必要になるとおもいます。SixAxisとも呼ばれるPlaystation3コントローラは、Bluetoothを介して接続し、さまざまなボタン、スティック、モーションセンサーのバンドルを持ち、そしてすぐに利用できるのです。Raspberry Piで使えるようにする方法についてはGoogleがこたえてくれると考えるはずです。もしあなたが経験した内容が私の想像通りなら、おそらくオンラインガイドはすべて a）元のとあるドキュメントのコピーであることに気づき、そして b）そのとおりやっても動作しなかったのではないでしょうか。私はこの問題を解決しました。私、素敵やん。私が費やした時間を無駄にしてはならないと、解決方法をここに書いておくことにしました。

## ペアリングに関するメモ

使いやすい SixAxisが、使い勝手ほど簡単ではない理由の1つとして、ペアリングがあります。通常の Bluetooth デバイスは、デバイスとホストとの間のリンクを一度確立すると、ホストはこの以前に記憶された情報を用いて接続を開始することができます。しかしSixAxisの場合、実際にはプロセスを開始するのはコントローラなので、事前にセットアップを行う必要があるのです。コントローラに接続しようとする Bluetooth ホストを指示する必要があり、ホスト（Pi）にコントローラの接続を許可する必要があることを伝えなければなりません。

## ハードウェア

このガイドでは、Raspberry Pi での利用を前提としています(Pi 2を使用しています、古いPiでは動作しません)。また、USB Bluetooth ドングルと SixAxis コントローラが必要です。私はSONYの純正で試しただけであり、あなたがオンラインで見つけた安価なコントローラの多くはクローン(非純正)だとおもいます。YMMV(自分の特定の用途にはこれがピッタリだったけど、あなたの用途に合うかどうかは保証ないけどネ)で、動作するはずです。

## Bluetooth ドングル

一部の人々から、このガイドの方法では機能しないという報告があがっています。私はこれらの問題の多くが Bluetooth ドングルにあると考えています。私が使用しているのは Asus USB-BT400 ですが、これは小型で、現在すべての Bluetooth 標準をサポートしています。別のドングルで動かすことができるのであれば、Twitter @approx_eng_に知らせてください。このリストに追加します：

* Asus USB-BT 400

## ソフトウェア

>注意1 
> `git` を設定して、githubで公開鍵をインストールした場合、作業をおこなう必要はありません。そうでない場合は `git` コマンドのいくつかを変更する必要があります。https://help.github.com/articles/generating-ssh-keys/#platform-all の指示に従って公開鍵を設定してください。

　

>注意2
> このガイドでは最新のJessieベースのRaspbianのクリーンインストールから始めることを前提としています。他のディストリビューションでは、devライブラリなどのさまざまな組み合わせが必要かもしれません。テストのために、私は最小限のインストール(ファイル名： `2015-11-21-raspbian-jessie-lite.zip`)を使用ましたが、これらの手順では最新バージョンに対して適用する必要があります。いつものように `sudo apt-get update && sudo apt-get upgrade` を実行するのも悪い考えではありません。あなたの使いたいディストリビューションを構築してからパッケージへの変更を取得してください。

最初にいくつかのパッケージをインストールして、Bluetooth サービスを有効にする必要があります：

```bash
pi@raspberrypi ~ $ sudo apt-get install bluetooth libbluetooth3 libusb-dev
pi@raspberrypi ~ $ sudo systemctl enable bluetooth.service
```

あなたが使っているユーザ（ここでは `pi` ）を bluetooth グループに追加します。

```bash
pi@raspberrypi ~ $ sudo usermod -G bluetooth -a pi
```

ここまで作業したら *Piの電源を入れ直す必要があります*。再起動ではなく、実際にシャットダウンして、一旦電源を切ってから、数秒待ってから再接続してください。これはやりすぎの可能性がありますが、次のステップが成功するためには、これが一番良い方法でした。

## ペアリング

コマンドラインペアリングツール `sixpair.c` のコードを入手してビルドします。

```bash
pi @ raspberrypi〜$ wget http://www.pabr.org/sixlinux/sixpair.c 
pi @ raspberrypi〜$ gcc -o sixpair sixpair.c -lusb
```

> 訳者注
> 社内LANのから実行する場合は、環境変数 `HTTP_PROXY` 、`HTTPS_PROXY` を設定してから `wget` してください。

 まず、コントローラに Bluetooth ドングルのMacアドレスを伝える必要があります。これを行うには、mini-USB ケーブルを使用してコントローラをPiに接続する必要があります。接続前に、Piに外部電源がきちんと供給されていることを確認してください。コントローラを接続するときに必要な余分な電力は、ラップトップのUSBソケットでは足りなくなり、ランダムなエラーが発生するか、プロセスがまったく機能しません。rootユーザとして実行される '`sixpair`'コマンドは、コントローラのBluetoothマスターアドレスを更新します：

```bash
pi@raspberrypi ~ $ sudo ./sixpair
Current Bluetooth master: 5c:f3:70:66:5c:e2
Setting master bd_addr to 5c:f3:70:66:5c:e2
```

実行すると、コントローラのBluetoothマスターアドレスが変更されたことを示すメッセージが表示されます（変更するアドレスを指定できます。何らかの理由で複数のプラグインを持っていない限り、デフォルトでは最初にインストールされたBluetoothアダプタを使用する必要はありません）。PSボタンを押すとコントローラは Bluetooth ドングルに接続しようとします（まだ実行しないでください、動作しません）。上記の例では、この特定のコントローラが以前はドングルとペア設定されていたため、変更はありませんでしたが、2つの異なるアドレスが表示されています。最初はコントローラが信頼していたアドレス、2番目は信頼するアドレスです。

次に、Piの Bluetooth ソフトウェアをコントローラからの接続を受け入れるように設定する必要があります。

コントローラをUSBポートから切り離し、通常のユーザとして '`bluetoothctl`'コマンドを実行します（`root`である必要はありません）：

```bash
pi@raspberrypi ~ $ bluetoothctl
[NEW] Controller 5C:F3:70:66:5C:E2 raspberrypi [default]
... (他のBluetoothハードウェアを使用している場合、他のメッセージがここに表示されることがあります)
```

今すぐ mini-USB ケーブルでコントローラをPiへ再接続してください。端末に何か接続されていることを示すメッセージが表示されるはずです（次のステップで役立つ情報が表示されているので、そうならなくても心配はありません）。

ターミナルにに `device` と入力します。SixAxis コントローラを含む、可能なデバイスのリストが表示されます。次のステップのためにコントローラのMACアドレスを記録する必要があります。

```bash
[bluetooth]# devices
Device 60:38:0E:CC:OC:E3 PLAYSTATION(R)3 Controller
... (他のデバイスがある場合はここに表示されます)
```

`agent on` とタイプしてから `trust MAC` (MACの部分は、前の手順でメモしたMACアドレスを置き換える)とタイプしてください。（これは以下の例と同じではないはずです）。完了したらツールを終了します。

```bash
[bluetooth]# agent on
Agent registered
[bluetooth]# trust 60:38:0E:CC:0C:E3
[CHG] Device 60:38:0E:CC:0C:E3 Trusted: yes
Changing 60:38:0E:CC:0C:E3 trust succeeded
[bluetooth]# quit
Agent unregistered
[DEL] Controller 5C:F3:70:66:5C:E2
```

コントローラの物理接続を外したら、ワイヤレスで接続できるはずです。これを確認するには、接続を外す前にまず `/dev/input` 以下のすべてを表示します：

```bash
pi@raspberrypi ~ $ ls /dev/input
by-id  by-path  event0  event1  event2  event3  event5  mice  mouse0
```

PSボタンを押すと、コントローラ前面のライトが数秒間点滅し、停止して1つのライトが点灯します。`/dev/ input` の内容をもう一度参照すると、おそらく '`js0`'のような新しいデバイスが表示されます。

```bash
pi@raspberrypi ~ $ ls /dev/input
by-id    event0  event2  event4  js0   mouse0
by-path  event1  event3  event5  mice
```

新しいデバイスが登場したら、ドングルとSixAxisコントローラーのペアリングが成功しています。これはリブートの間中も持続するので、今度はコントローラのPSボタンを押すだけで接続できます。このボタンを押し続けると、コントローラがシャットダウンします。タイムアウトがない場合は、コントローラをしばらく使用しないときは必ずオフにしてください。

## Python から SixAxis へのアクセス

`/dev/input` にジョイスティックデバイスがありますが、これをPythonコードでどのように使えばいいのでしょうか？

私が試した2つの異なるアプローチがあります。ひとつは PyGame を使用することです。これは既にあなたが利用したことがあるかもしれないという利点があります（最も簡単な解決策です）。これはシステム上のPythonに既にインストールされています。欠点もあります、ディスプレイが必要です。気づいていましたが、これは満足できるものではありません。2番目の選択肢は evdev 用のPythonバインディングを使うことです。これは軽量ですが、OSXのようなUNIX系のシステムであっても使用するのが複雑で、Linux上でしか動作しないという欠点があります PyGame は一般にクロスプラットフォームでの使用に適しています。私は Pi でのみ実行したいだけなので、ヘッドレス環境できれいに動作する必要が本当に必要ということもあり、私は evdev を使いましたが、両方の論争がいまだ存在しています。

実際に evdev を使うことは簡単ではありません。私が持っている最良のドキュメントは、それを処理するために書いたサンプルコードです。私はPythonクラスtriangula.input.SixAxisとこれに対応するリソースtriangula.input.SixAxisResourceを作成して、これを簡単におこなえるようにしました。このクラスはasyncoreを使用してevdevデバイスをポーリングし、オブジェクト内の内部状態を更新します。また、呼び出されるボタンハンドラを登録し、センタリング、ホットゾーン（1.0または-1.0にクランプするaxis範囲の領域）、および不感帯（0.0にクランプする中心点付近の領域）を登録することもできます。

次のコードの例ではコントローラに接続しています（接続されていない場合は例外が発生）。2つのアナログスティックの値を表示しています：

```python
from triangula.input import SixAxis, SixAxisResource

# 後で□ボタンにバインドされるボタンハンドラ
def handler(button):
  print 'Button {} pressed'.format(button)

# ジョイスティックを取得します。これはSIXAXISコントローラがアクティブに
# されていない限り、失敗します。
# bind_defaults 引数は、SELECTやSTART ボタンへのアクションをバインドして、
# コントローラを中心にしてキャリブレーションをリセットします。
with SixAxisResource(bind_defaults=True) as joystick:
    # □ボタンのボタンハンドラを登録します
    joystick.register_button_handler(handler, SixAxis.BUTTON_SQUARE)
    while 1:
        # 左ハンドスティックのX軸およびY軸を読み取る
        # 右ハンドスティックの場合は2と3
        x = joystick.axes[0].corrected_value()
        y = joystick.axes[1].corrected_value()
        print(x,y)
```

Triangulaのライブラリを勉強することは大歓迎です。半定期的にPyPiにアップロードしています（`pip install tr​​iangula`で取得できます）し、githubからも入手できます。いずれの場合でも、最初に1つの特定のパッケージをインストールする必要があります(下のコマンド参照のこと)。それ以外の場合、evdev モジュールはビルドされません。

```bash
pi @ raspberrypi〜$ sudo apt-get install libpython2.7-dev
```

Triangula コードを github から取得し、 triangula.input モジュールを取得するためにビルドすれば、独自のコードでこれを使用できます（特にTriangula固有のものはありません）。

```bash
pi@raspberrypi ~ $ git clone git@github.com:basebot/triangula.git
pi@raspberrypi ~ $ cd triangula/src/python
pi@raspberrypi ~/triangula/src/python python setup.py develop
```

これは、開発モードでライブラリを設定し、Pythonインストールへのシンボリックリンクを作成します（仮想環境を使用している前提です。そうしていない場合、 いくつかのコマンドをrootユーザとして実行する必要があります）。

