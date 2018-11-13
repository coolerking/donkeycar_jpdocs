# Donkey コマンドラインユーティリティ

`donkey` コマンドは、donkeycar Pythonパッケージをインストールする際に作成されます。これはいくつかの重要な機能を追加するPythonスクリプトです。ここでの操作は車両に依存しないため、すべてのハードウェア構成で動作する必要があります。

## Create Car

このコマンドは、ロボットを手動運転し、トレーニングをおこなうために必要なファイルを含むディレクトリを新規作成します。

使い方:
```bash
donkey createcar --path <ディレクトリ> [--overwrite] [--template <donkey2>]
```

* このコマンドはどのディレクトリでも実行できます。
* ホストコンピュータもしくはロボット上で実行します。
* 生成先ディレクトリを指定する場合 `--path` を使用します。もしそこに `.py` ファイルが存在する場合は、オプションとして `--overwrite` が設定されるまでそれらを上書きしません。
* オプション `--template` は、使用するテンプレートファイルを指定します。テンプレートのリストは、 `donkeycar/templates` ディレクトリを確認してください。

## Find Car

`nmap` を使ってローカルネットワークに接続された donkey car を検索するコマンドです。

使い方:
```bash
donkey findcar
```

* ホストコンピュータで実行します。
* ホストコンピュータのIPアドレスと車のIPアドレスがあれば表示します。
* `nmap` ユーティリティが必要です：
```bash
sudo apt install nmap
```

## Calibrate Car

手動で値を入力してPWM値をインタラクティブに設定し、ロボットの応答方法を試すことができるコマンドです。詳細は [こちら](/guide/calibrate/) を参照してください。

使い方:
```bash
donkey calibrate --channel <0-15 チャネルID>
```

* ホストコンピュータで実行します。
* `--channel` よって指定されたPWMチャネルを開きます。
* 整数値を入力してPWM値を指定し、`Enter` キーを押します。
* `Ctrl+C` を押して終了します。


## Clean data in Tub

tub から不良データを削除するWebサーバを開きます。

使い方:
```bash
donkey tubclean <tubデータを含むディレクトリ>
```

* Raspberry Pi またはホストコンピュータで実行します。
* 悪いデータ(脱輪や壁にぶつかるまでのデータ)を削除するためにWebサーバを開きます。
* `Ctrl+C` を押して終了します。


## Make Movie from Tub

Tubデータのイメージから動画ファイルを作成するコマンドです。

使い方:
```bash
donkey makemovie <tubへのパス> [--out=<tub_movie.mp4>] [--config=<config.py>]
```

* ホストコンピュータまたはロボットで実行します。
* 引数 `--tub` にて指定したディレクトリパスのイメージレコードを使用します。
* 引数 `--out` によって与えられたムービーを作成します。コーデックは* ファイル拡張子から推測されます。デフォルト：`tub_movie.mp4`
* オプションとして `config.py` 以外のものを引数として指定します。デフォルト：config.py


## Check Tub

任意の/すべてのタブに含まれるレコードの数を確認できます。また、各レコードを開き、データが読み取り可能であり、かつ完全であることを保証します。そうでない場合は、破損したレコードを削除することができます。


使い方:
```bash
donkey tubcheck <tubへのパス> [--fix]
```

* ホストコンピュータまたはロボットで実行します。
* レコード数の要約と、各タブごとに記録されたチャンネルが表示されます。
* 読み取り中に例外をスローするレコードを出力します。
* オプション `--fix` を指定すると、問題のあるレコードを削除します。


## Histogram

指定された tub データのヒストグラムをポップアップウィンドウを表示します。


使い方:
```bash
donkey tubhist <tubへのパス> --rec=<"user/angle">
```

* ホストコンピュータで実行します。
* `--tub` を省略すると、デフォルトの　`data` ディレクトリ内のすべてのタブがチェックされます。


## Plot Predictions

This command allows you plot steering and throttle against predictions coming from a trained model.
トレーニング済みモデルによる予測に対するステアリング値・スロットル値を描画します。

使い方:
```bash
donkey tubplot <tubへのパス> [--model=<modelファイルへのパス>]
```

* このコマンドは `~/mycardir` から実行できます。
* ホストコンピュータで実行します。
* トレーニング済みモデルからのNN予測と比較して、指定されたtubのステアリング値のプロットを示すポップアップウィンドウを表示します。
* `--tub` を省略すると、デフォルトの `data` ディレクトリ内のすべてのタブデータがチェックされます。


## Simulation Server

[Donkey シミュレータ](/guide/simulator.md) 上の車両に対して、自動運転機能を提供します。

使い方:
```bash
donkey sim --model=<モデルファイルへのパス> [--type=<linear|categorical>] [--top_speed=<スピード>] [--config=<config.py>]
```

* このコマンドは `~/mycardir` から実行できます。
* ホストコンピュータで実行します。
* モデルを使用して、シミュレータからのイメージと遠隔測定に基づいて予測を行います。
* `--type` を使って、モデルが舵角出力をカテゴリとして扱う必要があるかどうかを指定できます。
* さまざまな目標速度で安定性を確認するために最高速度を変更することができます。

