# FAQ
---------


## どのようなタイプのRCカーが donkey プラットフォームで動作しますか？


ほとんどのホビーグレードのRC車は電子機器でうまく動作しますが、自分で天板やカメラを固定するロールバーなどを自作する必要があります。Donkey Car化可能かどうかについては、次の内容をチェックしてください。

* ESC(スピードコントローラ)とレシーバ(受信機)が別れていること。安価なRCカーの場合、同じモジュールとなっている場合があります。このような車の場合、ESCを別途用意して接続する必要があります（場合により、はんだ付け）。
* ESCから受信機へ接続するコネクタは3線式であること。もしそうであれば、簡単に接続することができます。
* ブラシ付きモータは、比較的低速なので簡単です。ブラシレスモータも同様に機能させることは可能です。

詳細については、[Roll Your Own](/roll_your_own.md) を参照してください。



## アメリカ在住ではない場合、どのような車を使用できますか？


最も簡単な方法は、自前のパーツを現地のRC/ホビーショップに持って行き、そのパーツで実装可能な車があるかを確認することです。アメリカ以外の国で、このような動作確認をしている人たちが使った車両をいくつか紹介します。


* オーストラリア: [KAOS](https://www.hobbywarehouse.com.au/hsp-94186-18694k-kaos-blue-rc-truck.html) (Exceed Magnetと機能的に同等)
* 中国: [HSP 94186](https://item.taobao.com/item.htm?spm=a1z02.1.2016030118.d2016038.314a2de7XhDszO&id=27037536775&scm=1007.10157.81291.100200300000000&pvid=dd956496-2837-41c8-be44-ecbcf48f1eac) (Exceed Magnetと機能的に同等)

あなたの国の製品をこのリストに追加してください（このドキュメントを編集するには左上隅のアイコンをクリックしてください）


## 自分のトラックを作るにはどうすればいいですか？


テープ、リボン、またはロープを使用するとよいでしょう。最も人気のあるトラックは4フィート幅で、2インチの白い枠線と黄色の中心線をひいたものです。オークランドのコースは中心線の周り約70フィートのコースです。主なレースのコース特徴としては、直線を含み、左右のヘアピンターンやスタート/フィニッシュラインを集会させています。


## 別のハードウェアで動きますか？


はい。すべてPythonで実装されているため、Pythonが動作すればどのシステムでも実行できます。通常、Donkey の移植の難しい部分は、ハードウェアを動かす部分です。これは、人々が試したり話したりしたシステムがいくつかあります。

* NVIDA TX2 - ウェブカメラ実装しモーター/サーボを制御させることができました。
* Pi-Zero - テストされていませんが、OpenCVとTensorflowがインストールされていれば動くでしょう。

＃# 自分でRaspberry Piディスクを作成するにはどうすればいいですか？

4時間ほど時間がかかります。

### メモリカード上の最小限のスペースを使用し簡単にアップグレードしてソースを変更します。



##### This uses minimal space on your memory card, is easy to upgrade and make changes to source

* [raspian lite をダウンロードします](https://downloads.raspberrypi.org/raspbian_lite_latest)
* Windowsの場合 [ディスクイメージャをダウンロードします](https://sourceforge.net/projects/win32diskimager/files/latest/download)
* Mac や Linux の場合 [Etcher をダウンロードします](https://etcher.io)
* 次の手順でイメージをメモリカードへ書き込みます:
      * Win32DiskImager [ビデオ](https://www.youtube.com/watch?v=SdWr-aolCSA)
      * Win32DiskImager [ライトアップ](https://codeyarns.com/2013/06/21/how-to-write-a-disk-image-using-win32-disk-imager/)
      * Etcher [ビデオ](https://www.youtube.com/watch?v=I6F2HoTeiFc)
      * Etcher [ライトアップ](https://www.raspberrypi.org/magpi/pi-sd-etcher/)
      * Multiple methods [ライトアップ](http://elinux.org/RPi_Easy_SD_Card_Setup)
* Raspberry Piにカードを指してブートします

* ブート完了後、プロンプト表示されたら、ユーザ名 ```pi``` を入力します。

* パスワード ```raspberry``` を入力します。

* `raspi-config` を実行し、いくつかの便利なオプションをセットアップします:
    `sudo raspi-config`
    * hostname を変更します。
    * パスワードを変更します。
    * インターフェイスオプション
        * カメラの有効化
        * SSHの有効化
        * I2Cの有効化

* リブート

* パッケージ更新による最新を実行します:
``` bash
sudo apt-get update
sudo apt-get upgrade
```

* パッケージのインストール:

``` bash
sudo apt-get install git
sudo apt-get install python3 python3-pip python3-virtualenv python3-dev virtualenv
sudo apt-get install build-essential gfortran libhdf5-dev
```

* 最新 donkey コードを入手します:

``` bash
git clone https://github.com/wroscoe/donkey
cd donkey
```

* Anacondaをインストールします:

``` bash
wget http://repo.continuum.io/miniconda/Miniconda3-latest-Linux-armv7l.sh
bash Miniconda3-latest-Linux-armv7l.sh
source ~/.bashrc
```

* python 環境をセットアップします

``` bash
conda env create -f envs/rpi.yml
source activate donkey
```


* numpyを更新します。最新のコンパイルを含むので時間がかかります。

``` bash
pip install --upgrade numpy
```

* TensorFlowのセットアップ:

``` bash
wget https://github.com/samjabrahams/tensorflow-on-raspberry-pi/releases/download/v1.1.0/tensorflow-1.1.0-cp34-cp34m-linux_armv7l.whl
pip install tensorflow-1.1.0-cp34-cp34m-linux_armv7l.whl
```

* donkey のセットアップ

``` bash
pip install -e .
```

* データ取得のために初期設定ファイルやディレクトリをセットアップします。[Get Driving](guide/get_driving.md) のオプションを参照してください。


---
### 再起動後、プロンプトの前に（Donkey）が表示されず、実行時にPythonエラーが表示されます。

1. 上記のディスク設定ガイドを使用した場合は、仮想環境を管理するためにcondaを使用しました。あなたは conda環境 donkey を以下のように活性化する必要があります：

```bash
source activate donkey
```

2. オプションで `~/.bashrc` の最後の行に行追加して、ログインするたびにその行をアクティブにさせることもできます。


----
## 最新の Donkey ソースの入手方法

1. 最新の更新情報を得ることができます。githubリポジトリから直接インストールした場合、最新の入手は簡単です： 

     ```bash
    cd donkey
    git pull origin master
    ```
2. テンプレートファイルがmanage.pyに影響を与える修正によって変更されることもあります。新しいユーザディレクトリを作成してテストすることができます。 [インストールセクション](guide/install_software.md) で作成したのと同じオプションを使用しますが、新しいパスを使用します。例えば： 

    ```bash
    donkey createcar --path ~/d2_new
    ```

---
