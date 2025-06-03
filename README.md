# jetson_yolov5

yolov5部署至jetson nano使用CSI摄像头实现实时监测

---

## 配置环境

首先安装numpy，避免下面安装出问题

```markdown
sudo apt-get install libjpeg8 libjpeg62-dev libfreetype6 libfreetype6-dev

sudo pip install numpy==1.19.4
```

下载torch和torchvision，去英伟达官网下载所需版本

我用的 1.8.0，去下面下载

https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048

在torchvision文件夹的目录下进入终端，输入以下命令:

```markdown
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
```

```markdown
pip install torch-1.8.0-cp36-cp36m-linux_aarch64.whl
```

验证安装

import torch

成功与否时出现

core dump

执行下面命令


```markdown
export OPENBLAS_CORETYPE=ARMV8
```
按顺序执行以下代码

```markdown
cd torchvision
 
export BUILD_VERSION=0.9.0
 
sudo python setup.py install 
 
python setup.py build
 
python setup.py install
 
cd .. 
```
---
## 运行YOLOv5

```markdown
cd yolov5-5.0

pip install -r requirements.txt
```
opencv 编译时间较长，请耐心等待

运行

```markdown
python3 detect.py --weights yolov5s.pt 
```

如果没问题，执行下面命令

```markdown
python3 gen_wts.py --w yolov5s.pt
```

文件夹下生成 yolov5.wts 文件

---
## tensorRT部署yolov5

```markdown
cd tensorrtx-yolov5-v5.0

cd yolov5

mkdir build

cd build

cmake ..

make
```

将上面生成的yolov5.wts文件拖到tensorrtx-yolov5-v5.0/yolov5下,
执行以下命令

```markdown
sudo ./yolov5 -s yolov5s.wts yolov5s.engine s
```
在tensorrtx-yolov5-v5.0/yolov5下进行测试
文件tensorrtx-yolov5-v5.0/yolov5下

```markdown
mkdir test //test下放入测试图片

然后执行

sudo ./yolov5 -d yolov5s.engine ../test
```

最终运行成功！！！

---
## 接入CSI 摄像头

对yolov5.cpp 进行更改，更改代码在tensorrtx-yolov5-v5.0/yolov5下code.txt

更改后执行

```markdown
make

sudo ./yolov5 -v yolov5s.engine
```







