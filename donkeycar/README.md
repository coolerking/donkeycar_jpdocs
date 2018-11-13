> * donkeycar パッケージ 日本語ドキュメント
>
>以下のコマンドを実行すると、siteフォルダ以下にコンテンツが格納されます。
>   ```bash
>   pip install mkdocs
>   pip install mkdocs-material
>   mkdocs build
>   ```
>
> 以下のコマンドを実行すると、 http://127.0.0.1:8000 から参照可能になります。
>   ```bash
>   mkdocs serve
>   ```

# donkeycar: python 自動運転ライブラリ

[![Build Status](https://travis-ci.org/autorope/donkeycar.svg?branch=dev)](https://travis-ci.org/autorope/donkeycar)
[![CodeCov](https://codecov.io/gh/autoropoe/donkeycar/branch/dev/graph/badge.svg)](https://codecov.io/gh/autorope/donkeycar/branch/dev)
[![PyPI version](https://badge.fury.io/py/donkeycar.svg)](https://badge.fury.io/py/donkeycar)
[![Py versions](https://img.shields.io/pypi/pyversions/donkeycar.svg)](https://img.shields.io/pypi/pyversions/donkeycar.svg)


Donkeycarは、Pythonで記述された必要最小限のモジュールで構成された自動運転ライブラリです。 これは、ホビーイストや学生がすぐに実験をおこなうことができ、コミュニティの貢献を簡単にすることに焦点を当てて開発されています。

#### クイックリンク
* [Donkeycar 更新情報 & サンプル](http://donkeycar.com)
* [構築手順およびソフトウェアドキュメント](http://docs.donkeycar.com)
* [Slack / チャット](https://donkey-slackin.herokuapp.com/)

![donkeycar](./docs/assets/build_hardware/donkey2.PNG)

#### よければ Donkey を使ってみてください:
* 自動運転するRCカーを作成。
* [DIY Robocars](http://diyrobocars.com) のような自動運転レースで競争。
* コンピュータビジョンによるマッピングやニューラルネットワークによる自動運転を経験。
* センサデータをロギング (イメージ、ユーザによる入力、センサ読み込み)。
* Webもしくはゲームコントローラ経由で運転。
* 運転データ提供によるコミュニティへ影響を与える。
* 既存のCADモデルの設計を更新して使う。

### 手動運転する
Donkey2を構築後スイッチを入れ、 http://localhost:8887 を開いて運転します。

### 運転の振る舞いを変更する
donkeycar は一連のイベントにより操作されます。

```python
# 画像を1秒間に10回撮影して記録する車両を定義

from donkeycar import Vehicle
from donkeycar.parts.camera import PiCamera
from donkeycar.parts.datastore import Tub


V = Vehicle()

# カメラ part の追加
cam = PiCamera()
V.add(cam, outputs=['image'], threaded=True)

# tub part を追加してイメージを記録
tub = Tub(path='~/mycar/get_started',
          inputs=['image'],
          types=['image_array'])
V.add(tub, inputs=['image'])

# 10 Hz で運転ループを開始
V.start(rate_hz=10)
```

[ホームページ](http://donkeycar.com), [docs](http://docs.donkeycar.com) を参照のこと。
また [Slack チャネル](http://www.donkeycar.com/community.html) にて詳細を確認のこと。
