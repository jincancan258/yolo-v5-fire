# yolo-v5-fire-detection-for-video

  此项目为通过yolo v5训练模型实现视频火焰检测，包括对本地视频以及摄像头读取到的实时视频流中的火焰将进行检测。
  主要内容包括yolo v5环境搭建、数据集的收集、模型训练以及调用模型实现检测。

一、 环境搭建  
  操作系统：Ubuntu20.04  
  CUDA版本：10.2  
  Pytorch版本：1.9.1  
  TorchVision版本：0.10.1  
  IDE：PyCharm  
  硬件：RTX1660S  
  其他环境依赖参考requirement.txt  

二、数据集
  数据集为网上收集到的火焰检测数据，经转化为yolo v5适配的txt格式，放在coco目录下,组织文件结构如下：
![2021-10-20_17-24](https://user-images.githubusercontent.com/67082200/138066628-77f010b2-0615-4774-a832-b0366951c976.png)

三、模型训练
  1. 下载预训练模型
  本次项目对模型精度要求不高而对检测速度要求较高，因此选择较小的yolo v5s模型进行训练。预训练模型yolo v5s.pt已下载好，放在weight目录下。

  2. 修改配置文件
  进入yolov5/data目录下，修改coco128.yaml文件内容。建议复制一份coco128.yaml，然后更名为fire_detection.yaml，修改内容如下：声明train和val文件的路径，类别数nc以及类别名names（由于检测目标只有火焰一类，所以类别数为1，名称为fire）
![image](https://user-images.githubusercontent.com/67082200/138062732-af6c9771-4272-484c-8cb3-18b6c58f8079.png)
  进入yolov5/models目录下，修改yolov5s.yaml文件内容。这里，我们可以先复制一份yolov5s.yaml，然后更名为fire_yolov5s.yaml，只需要修改类别数nc:1
  
  3. 开始训练
  终端进入到yolo v5文件夹下，使用yolo v5s权重进行训练  
  python train.py --img 640 --batch 16 --epochs 30 --data ./data/fire.yaml --cfg ./models/fire_detect_yolov5s.yaml --weights ./weights/yolov5s.pt  
  训练结束的模型权重以及训练结果都保存在runs/train目录下
  v![image](https://user-images.githubusercontent.com/67082200/138064537-ed484fd0-e170-4d72-aeb4-7bb31cdc7618.png)
  
  4. 调用模型进行检测  
  python detect.py --source posVideo1.868.avi --weight runs/train/exp3/weights/best.pt  
  待检测的视频和检测权重路径根据自己实际情况修改。
  
