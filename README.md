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
- 画像分類 (Image Classification) : 画像に何が写っているかを判定（画像の種類を判定）。狭い意味で画像認識と言う場合、これを指すことがある。
- 物体認識 (Object Recognition) : 画像の中にある物体の位置を検出し、それが何かを認識する。画像内の対象(Object)の位置と種類を検出する。画像内に複数の対象があっても動作するものもある。
- 顔認識 (Face Recognition) : 画像の中の顔を検出し、それが誰か、あるいは、性別や年齢、表情、その他を認識する。個人を高い精度で判別し、顔の認証に用いるものも一般的である。

物体認識のモデルの中で、比較的広く使われているものに、YOLO (You Only Look Once) がある。2024年10月現在の最新版は、2024年に公開された v11 である。

## YOLOとは

YOLO は You Only Look Once を略したもので、Ultralytics が公開する深層学習（Deep Learning) による一般物体認識モデルである。

物体認識のアルゴリズムとして、1ステージのものと2ステージのものに分けられる。2ステージのものは、まず対象の位置の検出を画像全体で行った後、検出されたそれぞれの対象について種類（内容）の識別を行うのに対し、1ステージのものは、一度をそれを行う。YOLOは1ステージで行うタイプのものである。

Ultralytics による YOLO の公式資料は、[https://docs.ultralytics.com/ja](https://docs.ultralytics.com/ja) にある。

簡単なインストール方法、使い方は、[https://docs.ultralytics.com/quickstart/#use-ultralytics-with-python](https://docs.ultralytics.com/quickstart/#use-ultralytics-with-python) にある。

# Windows 11 へのインストール

Python の pip の機構（pip installs packages or pip installs python : Python にインストールされていない機能をダウンロードしインストールし追加する機能。こうした追加の機能のことをライブラリと呼んだり、簡単にダウンロード・インストールできるようまとめてあるものをパッケージと呼ぶ) で、YOLO をインストールすることができる。

コマンド（Windows の PowerShell やコマンドプロンプト）で、下記を実行する。

```
pip install ultralytics
```

ネットワークの通信状況にもよるが、やや時間がかかるので、しばらく待つ。

インストールが終わったら、自分のホームディレクトリの下に yolo11 というフォルダを作成し、そこへ移る。

```
ホームディレクトリ ＝ \\Users\ユーザ名 が通常なので、

cd \\Users\ユーザ名 (既にそこに居る場合は良い)

mkdir yolo11

cd yolo11
```

エディタ (VS Code 等) を使い、新たにファイル predict.py を作成する。

次にその内容を下記のようにする。
```python
from ultralytics import YOLO
from ultralytics import settings
import cv2
import time
from datetime import datetime
import os

# 古い認識内容のあるディレクトリを削除
import shutil
shutil.rmtree("runs\\detect\\predict")


# Load a model
model = YOLO("yolo11m.pt")



# Webカメラを起動
cap = cv2.VideoCapture(0)

ret, frame = cap.read()
results = model(frame, show=True, save=True, save_txt=True)
print(results[0].names)

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break

    # YOLOでフレームを解析、結果を画像とテキストで保存することを有効化
    results = model(frame, show=True, save=True, save_txt=True)

    # 現在時刻を取得
    timenow = datetime.now()
    # 20241030-085000 という形式にフォーマット変更
    timenow_str = timenow.strftime("%Y%m%d-%H%M%S")
    # ファイルのリネーム runs\detect\predict に image0.jpg と保存されるため
    # ファイル名に時刻を付けて保存（リネーム）
    os.rename("runs\\detect\\predict\\image0.jpg", "runs\\detect\\predict\\" + timenow_str + ".jpg")
    os.rename("runs\\detect\\predict\\labels\\image0.txt", "runs\\detect\\predict\\labels\\" + timenow_str + ".txt")

    # 10秒停止
    time.sleep(10)
    

cap.release()
cv2.destroyAllWindows()
```
