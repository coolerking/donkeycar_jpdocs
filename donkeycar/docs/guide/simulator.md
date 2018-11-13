# Donkey Simulator

シミュレータ上で(手動)運転したDonkey Carデータをトレーニングすることができます。このシミュレータはUnityゲームプラットフォームの物理エンジンやグラフィックを使って構築されました。そしてdonkey python プロセスと接続し、独自の訓練済みモデルを使ってシュミレートされた Donkey Carを自動運転にかける事ができます。

### Simulator のダウンロード

プラットフォーム別にダウンロード先が異なります：

* Ubuntu 16.04 [ダウンロード](https://drive.google.com/open?id=0BxSsaxmEV-5Yc3VVdVQ4UUZOc0k)
* Mac 10.10.5+ [ダウンロード](https://drive.google.com/open?id=0BxSsaxmEV-5YRnFEZmpRaks5NXM)
* Windows 7+ [ダウンロード](https://drive.google.com/open?id=0BxSsaxmEV-5YRC1ZWHZ4Y1dZTkE)

ダウンロードした圧縮ファイルを展開すると、実行可能ファイルを含むフォルダが作成されます。その実行可能ファイルをダブルクリックして、 Simulator を起動します。

#### Mac の場合 追加ステップ

ログが生成されていない場合は、信頼できないアプリケーションをサンドボックス化するOS Xのバージョンが動作している可能性があります。これにより、Simulatorがディスクに書き込めなくなります。解決するには、アプリケーションフォルダ内の実行可能ファイルを移動します。

### 記録データ

このシミュレータは Donkey データを donkey tub 形式で作成できます。これは実行可能ファイルの横にあるフォルダのルートにある `log` ディレクトリに格納されます。Mac では、このフォルダを参照するためにパッケージの内容を参照する必要があります。このフォルダがない場合、データは記録されません。


Simulator では、2つのシーンを提供しています。

## Generated Road シーン

道路をランダムに作成します。生成するごとに異なる路面を提供し、コースは数マイルのカーブをふくむ非周回コースとなります。1つのコースで訓練をして、同様のものや全く異なる路面でテストが可能です。


## Warehouse シーン

これは、Oakland DIYRobocars Meetup のプライマリトラックとして使用されている実際のコースと少し似ている特定のトラックを作成します。

____

### メニュー オプション:

##### Joystick/Keyboard No Rec

ジョイスティックやキーボードでDonkeyを動かします。私はPS2ジョイスティックとPS3ジョイスティックを使用しました。キーボードコントロールは矢印キーで操作します。このモードでは、データは記録されません。

Drive the donkey with a joystick or keyboard. I used a PS2 Joystick and a PS3 Joystick. Keyboard controls use arrow keys to steer. In this mode, no data is recorded.

> 注意：キーボードを使う場合、ステアリング情報を段階状(例：-1, 0, 1)に生成するため、トレーニングデータとしては難しいデータになる可能性があります。ジョイスティックのセットアップ手順を確認してください。


##### Joystick/Keyboard w Rec

Donkey Car をジョイスティックやキーボードで動かします。このオプションでは tub donkey フォーマットとしてデータを保存します。

##### Auto Drive No Rec

道路情報を使ってDonkey Carをコースにしたがってガイドします。ステアリング操作のためにPIDコントローラを使うため、時々振動(左右にブルブル動かす)します。このオプションでは走行データを記録しません。

##### Auto Drive w Rec

道路情報を使ってDonkey Carをコースにしたがってガイドします。ステアリング操作のためにPIDコントローラを使うため、時々振動(左右にブルブル動かす)します。このオプションでは走行データをtub donkey フォーマットで記録します。

##### Next Track

Generated road scene の場合、路面やトラック幅が切り替わります。

##### Regen Track

Use the current surface type, but generate a new random path and road.
現在の路面タイプを継続して使用しますが、新しくランダムに道路コースが生成されます。

____

## PID コントロール

##### Max Speed

This setting determines the target speed during the PID auto drive. It will also affect the speed when driving by keyboard controls (not recommended).
PID 自動運転中のターゲットスピードを定義します。キーボードによる運転時の速度に影響します(非推奨)。

##### Prop

Propotation (比例) の略です。偏差に比例してステアリングを経路に合わそうとするPIDのPにあたります。

##### Diff

PIDのDにあたり、脱輪を引き起こすような進路をステアリング操作で制限し、行き過ぎを制限するように設計されています。

##### Max Steering

> 注意 - Max Steering は重要な補正で、分類トレーニングに非常に強く影響します。ステアリングデータが書き込み時に正規化され、Pythonから来た後に乗算されるので、舵角はトレーニングとシミュレーションで一定でなくてはなりません。この値を変更する場合は注意してください。Max Steering 設定ごとにデータとモデルを分離します。


`Max Steering` は、 `Auto Drive No Rec` 使用時にのみ調整できます。また、ジョイスティックとキーボードの操作範囲にも影響しますので、保存して再ロードする必要があります。

デフォルトのカテゴリカルモデルは 16 個のbin(バイナリ：0か1)、もしくは 16分類を保持しており、最大ステアリングが `+-16` 、各binが角度の2度を表現しています。トレーニングとサンプルデータを比較して、いかにデータフィットさせるかについて知ることは有用です。

____

## 一般的な利用方法

* シミュレータを開始します
* `log` ディレクトリが存在し、かつ中身が空であることを確認します
* シーンを選択します
* `Auto Drive w Rec` ボタンを押します
* Max Speed、Prop、Diffそれぞれのスライダーを変更して、運転スタイルを調整します
* データが 10KB以上貯まるまで、10から15分ほど待機します
* `Stop` ボタンを押します
* `Exit` ボタンを押します
* `log` ディレクトリを `~/mycar/data/` ディレクトリ(通常データを配置する場所)へ移動し、 `~/mycar/data/log` を作成します。
* いつもどおり、トレーニングを実行します。

> Note: I had problems w default categorical model. Linear model worked better for me.

> 注意：デフォルトのカテゴリカルモデルを使用するとうまくいきません。私が試した際には、線形モデルのほうが良い動作をしました。

``` bash
python manage.py train --tub=data/log --model=models/mypilot
```

* Simulator サーバの開始

``` bash
donkey sim --model=models/mypilot
```

`wsgi starting up on http://0.0.0.0:9090` と表示されるまで待機します。

* Simulator 上でシーンを選択します
* `NN Steering w Websockets` ボタンを押します
* Donkey Car が自動運転を開始します。画面左上のステアリング値、スロットル値を確認しましょう。
______

## ジョイスティック セットアップ

キーボード入力は学習信号としては不十分です。ジョイスティックを使用して手動の運転データを提供することをお勧めします。

##### Linux におけるジョイスティック セットアップ

Unity on Linux は、SDLライブラリを使用してジョイスティックを表示します。そして、特にGamePad APIですが、デフォルトでは設定されていません。使用するには次の手順を行う必要があります：

```bash
git clone https://github.com/Grumbel/sdl-jstest

sudo apt-get install cmake
sudo apt-get install libsdl1.2-dev
sudo apt-get install libsdl2-dev
sudo apt-get install libncurses5-dev
cd sdl-jstest
mkdir build
cd build
cmake ..
make install


./sdl2-jstest -l
```

`Joystick GUID: 030000004f04000008b1000000010000` を探します。

GUID は、デバイスにより異なります。

[https://github.com/gabomdq/SDL_GameControllerDB/blob/master/gamecontrollerdb.txt] を開きます。

そしてLinux の商のGUIDを探します。1行1デバイスタイプです。デバイス情報を特定するために `environment` を更新します：

```bash
sudo -H gedit /etc/environment
```

`SDL_GAMECONTROLLERCONFIG=` 行を追加して、開始時と終了時に必ず引用符を追加してください。すなわち、


`SDL_GAMECONTROLLERCONFIG="030000004f04000008b1000000010000, ...` (以降の長い行は `gamecontrollerdb.txt` からコピー)

* リブートします
* `sim` を開始します
* `drive w joystick` を選択します
* ジョイスティックを操作します
* ハッピーダンスを踊ります


