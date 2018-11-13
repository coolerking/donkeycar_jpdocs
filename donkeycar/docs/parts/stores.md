# ストアパーツ

Stores are parts that record and replay vehicle data produced by other parts.

ストアは、他のパーツが作成した車両データを記録し、ロードするためのパーツです。

## Tub

Tub は、標準 Donkey データストアです。ROSBAG の後でモデル化されました。


> TODO: Tubパーツの構造は理想的ではないので、変更する必要があります。

> * 型を指定する必要がなく、最初のループで検査され保存されます。

生成の例
```python
import donkey as dk

T = dk.parts.Tub(path, inputs, types)

```

### 受け入れ可能なタイプ
* `float` - レコードとして保存される
* `int` - レコードとして保存される

