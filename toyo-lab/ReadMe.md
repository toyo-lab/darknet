# コンパイル環境
toyo-lab の z390 用に Makefile を変えてあります。

    GPU=1
    OPENCV=1

GPU は cuda の 10.2 を入れてあります。
    /usr/local/cuda-10.2

OpenCV は make してインストールしてあります。
Version 3.4.3
    /usr/local/lib/libopencv*

## 自前でコンパイル
以下の方法で出来るはず。./darknet が出来れば成功。GPU がないときや
OpenCV をインストールしていないときは Makefile を書き換えて
GPU=0 や OPENCV=0 とする。-pthread でエラーが出たら適当に削除。
```
> git clone -b shirouzu https://github.com/toyo-lab/darknet
> cd darknet
> make 
```

# 2019 年研究用の cfg とデータ

## じゃんけん用
classes = 3 になっている
- cfg2019/yolov2-janken.cfg  
- data/janken.data
- data/janken.names

## テニス用
classes = 9 になっている。
- cfg2019/yolov3-tennis.cfg  
- data/tennis.data
- data/tennis.names

# 評価
そのためには学習済みデータが必要
テニスの場合は yolov3-voc_tennis.weights で 235M と巨大なためにここには含まず。
(voc になっているがvoc とは関連ない。間違い)

-rw-r--r-- 1 nobody nobody 246477708 1月  16 21:50 yolov3-voc_tennis.weights

じゃんけんは yolov3-voc_janken.weights なるものが存在するが
上記の yolov2-janken.cfg と整合性があるのか不明(バージョンが違う)
恐らく生成の過程で v2, v3 両方実験しているのでしょう。
再度、興味のある人が学習をした方がよい。

```
./darknet.exe detector test tennis.data toyo-lab/cfg2019/yolov3-tennis.cfg toyo-lab/yolov3-voc_tennis.weights toyo-lab/tennis_17444.jpg
<実行結果中略>
Loading weights from toyo-lab/yolov3-voc_tennis.weights...Done!
toyo-lab/images/tennis_17444.jpg: Predicted in 9.927605 seconds.
(Center,Left,Right): 100%
```

## 失敗例
./darknet を detect で動作させると "cfg/coco.data" を参照するため
motorbike: 100% などと間違ったラベル名を出力してしまう。
以前は coco.data を書き換えるという事をしていたようだが、
coco.data は意味のあるデータなので書き直してはいけない。

```
> ./darknet.exe detect toyo-lab/cfg2019/yolov3-voc.cfg toyo-lab/yolov3-voc_tennis.weights toyo-lab/tennis_17444.jpg 
<実行結果中略>
Loading weights from toyo-lab/yolov3-voc_tennis.weights...Done!
toyo-lab/images/tennis_17444.jpg: Predicted in 9.886351 seconds.
motorbike: 100%
```
