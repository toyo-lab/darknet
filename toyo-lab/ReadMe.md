# コンパイル環境
toyo-lab の z390 用に Makefile を変えてあります。

    GPU=1
    OPENCV=1

GPU は cuda の 10.2 を入れてあります。
    /usr/local/cuda-10.2

OpenCV は make してインストールしてあります。
Version 3.4.3
    /usr/local/lib/libopencv*

# 2019 年研究用の cfg

- cfg2019/yolov2-voc.cfg  
じゃんけんの為の cfg そのため classes = 3

- cfg2019/yolov3-voc.cfg  
テニスの為の cfg   
classes = 9
