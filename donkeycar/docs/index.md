# Donkeyとは

Donkeyは、Pythonで書かれた高水準の自動運転ライブラリです。迅速な実験と簡単な貢献を可能にすることに重点を置いて開発されました。 

---------

### 独自Donkey Car(Donkey2)の製作

Donkey2は、ほとんどの人が最初に構築する標準的な車です。部品の価格は250〜300ドル(訳者注：日本では約４万円)程度で、組み立てに2時間かかります。自分の車を作るための主な手順は次のとおりです。 

1. [ハードウェアの組み立て](guide/build_hardware.md)
2. [ソフトウェアのインストール](guide/install_software.md)
3. [車のキャリブレーション(調整)](guide/calibrate.md)
4. [車の運転](guide/get_driving.md)
5. [オートパイロットのトレーニング](guide/train_autopilot.md)
6. [シミュレータでの体験](guide/simulator.md)

---------------



### Hello World.

Donkey Carは車に新しい部品を簡単に追加できるように設計されています。ここでは、カメラの画像をキャプチャして保存するサンプルのカーアプリケーションを紹介します。 

```python
import donkey as dk

# 乗り物の初期化
V = dk.Vehicle()

# カメラパートの追加
cam = dk.parts.PiCamera()
V.add(cam, outputs=['image'], threaded=True)

# イメージを記録するtubパートの追加
tub = dk.parts.Tub(path='~/mycar/gettings_started',
                   inputs=['image'],
                   types=['image_array'])
V.add(tub, inputs=inputs)

# 乗り物の運転ループを開始
V.start(max_loop_count=100)
```
----------------

### インストール

Linux / OSユーザの場合、masterブランチを複製して最新バージョンを入手します。 
```bash
git clone -b master https://github.com/wroscoe/donkey donkeycar
cd donkeycar
pip install -e .
```

[Windowsへインストールする方法](guide/install_software.md)

-----------------------

### なんでDonkey(ろば)なの？

このプロジェクトの究極の目標は何か役に立つものをつくることです。Donkey（家畜）は最初に家畜化された動物であり、彼らは悪名高く頑固で、子供は安全です。車がある都市の出発地点から目的地へナビゲートできるようになるまで、天体の存在つぎにおもいついた名前をつけました。
