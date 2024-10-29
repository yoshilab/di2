# 先端デジタル価値創造

## 本講義で学ぶこと
- 物体認識AIモデルを実際に使ってみる
- 物体認識モデルととして有名な Ultralytics の YOLO を使う
- 2024年に発表された YOLO v11 を使い、カメラからの画像に対して画像認識を行う

## 本講義の前提 (必要なもの)
- Windows 11 の PC (Mac でも可能かも知れないが未検証)
- Python 3.12 が python.org からダウンロード・インストールされている
- 2024/10/07 に公開された Python 3.13 では不具合が発生する
- Python のインストール時に、パッケージマネージャの pip が使えるように (PATHも通してある) なっていること
- PC にカメラがついていること (USBカメラでも良い)

## AI の先端技術～物体認識

AI (Artificial Intelligence: 人工知能) 技術の一つに、画像認識 (Image Recognition) 技術がある。

画像認識の代表的なものに下記のようなものがある。
- 物体認識 (Object Recognition) : 画像の中にある物体の位置を検出し、それが何かを認識する。
- 顔認識 (Face Recognition) : 画像の中の顔を検出し、それが誰か、あるいは、性別や年齢、表情、その他を認識する。

物体認識のモデルの中で、比較的広く使われているものに、YOLO (You Only Look Once) がある。2024年10月現在の最新版は、2024年に公開された v11 である。

## YOLOとは

YOLO は You Only Look Once を略したもので、 深層学習（Deep Learning) による一般物体認識モデルである。

```python
import numpy as np

a = 0
while a < 10:
  a = a + 1

print(a)
```
