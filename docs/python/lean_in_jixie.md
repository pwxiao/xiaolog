
# 智能零售柜商品识别


下载yolov5：

``` bash
git clone https://github.com/ultralytics/yolov5
```

进入yolov5文件夹,下载依赖文件

```bash
pip install -r requirements.txt
``` 

深度学习第一步
准备好数据集：

附件中的train.zip。

图片数据集为稀疏商品图片，
格式为jpg，标注文件符合VOC数据集类型，文件格式为xml。

数据集分为
训练集，验证集，测试集，

## 数据集分割

首先从数据集中抽取一百张图片以及对应的标签文件，作为测试集

接下来是对训练集和验证集进行分类

以下是分类的代码
```python
import xml.etree.ElementTree as ET
import pickle
import os
from os import listdir, getcwd
from os.path import join
import random
from shutil import copyfile

classes = ['3+2-2', '3jia2', 'aerbeisi', 'anmuxi', 'aoliao', 'asamu', 'baicha', 'baishikele', 'baishikele-2', 'baokuangli', 'binghongcha', 'bingqilinniunai', 'bingtangxueli', 'buding', 'chacui', 'chapai', 'chapai2', 'damaicha', 'daofandian1', 'daofandian2', 'daofandian3', 'daofandian4', 'dongpeng', 'dongpeng-b', 'fenda', 'gudasao', 'guolicheng', 'guolicheng2', 'haitai', 'haochidian', 'haoliyou', 'heweidao', 'heweidao2', 'heweidao3', 'hongniu', 'hongniu2', 'hongshaoniurou', 'jianjiao', 'jianlibao', 'jindian', 'kafei', 'kaomo_gali', 'kaomo_jiaoyan', 'kaomo_shaokao', 'kaomo_xiangcon', 'kebike', 'kele', 'kele-b', 'kele-b-2', 'laotansuancai', 'liaomian', 'libaojian', 'lingdukele', 'lingdukele-b', 'liziyuan', 'lujiaoxiang', 'lujikafei', 'luxiangniurou', 'maidong', 'mangguoxiaolao', 'meiniye', 'mengniu', 'mengniuzaocan', 'moliqingcha', 'nfc', 'niudufen', 'niunai', 'nongfushanquan', 'qingdaowangzi-1', 'qingdaowangzi-2', 'qinningshui', 'quchenshixiangcao', 'rancha-1', 'rancha-2', 'rousongbing', 'rusuanjunqishui', 'suanlafen', 'suanlaniurou', 'taipingshuda', 'tangdaren', 'tangdaren2', 'tangdaren3', 'ufo', 'ufo2', 'wanglaoji', 'wanglaoji-c', 'wangzainiunai', 'weic', 'weitanai', 'weitanai2', 'weitanaiditang', 'weitaningmeng', 'weitaningmeng-bottle', 'weiweidounai', 'wuhounaicha', 'wulongcha', 'xianglaniurou', 'xianguolao', 'xianxiayuban', 'xuebi', 'xuebi-b', 'xuebi2', 'yezhi', 'yibao', 'yida', 'yingyangkuaixian', 'yitengyuan', 'youlemei', 'yousuanru', 'youyanggudong', 'yuanqishui', 'zaocanmofang', 'zihaiguo', '']

TRAIN_RATIO = 99  


def clear_hidden_files(path):
    dir_list = os.listdir(path)
    for i in dir_list:
        abspath = os.path.join(os.path.abspath(path), i)
        if os.path.isfile(abspath):
            if i.startswith("._"):
                os.remove(abspath)
        else:
            clear_hidden_files(abspath)

wd = os.getcwd()
data_base_dir = os.path.join(wd, "train/")
if not os.path.isdir(data_base_dir):
    os.mkdir(data_base_dir)
work_sapce_dir = os.path.join(data_base_dir, "train/")
if not os.path.isdir(work_sapce_dir):
    os.mkdir(work_sapce_dir)
annotation_dir = os.path.join(work_sapce_dir, "xml/")
if not os.path.isdir(annotation_dir):
    os.mkdir(annotation_dir)
clear_hidden_files(annotation_dir)
image_dir = os.path.join(work_sapce_dir, "img/")
if not os.path.isdir(image_dir):
    os.mkdir(image_dir)
clear_hidden_files(image_dir)
yolo_labels_dir = os.path.join(work_sapce_dir, "labels/")
if not os.path.isdir(yolo_labels_dir):
    os.mkdir(yolo_labels_dir)
clear_hidden_files(yolo_labels_dir)
yolov5_images_dir = os.path.join(data_base_dir, "images/")
if not os.path.isdir(yolov5_images_dir):
    os.mkdir(yolov5_images_dir)
clear_hidden_files(yolov5_images_dir)
yolov5_labels_dir = os.path.join(data_base_dir, "labels/")
if not os.path.isdir(yolov5_labels_dir):
    os.mkdir(yolov5_labels_dir)
clear_hidden_files(yolov5_labels_dir)
yolov5_images_train_dir = os.path.join(yolov5_images_dir, "train/")
if not os.path.isdir(yolov5_images_train_dir):
    os.mkdir(yolov5_images_train_dir)
clear_hidden_files(yolov5_images_train_dir)
yolov5_images_test_dir = os.path.join(yolov5_images_dir, "val/")
if not os.path.isdir(yolov5_images_test_dir):
    os.mkdir(yolov5_images_test_dir)
clear_hidden_files(yolov5_images_test_dir)
yolov5_labels_train_dir = os.path.join(yolov5_labels_dir, "train/")
if not os.path.isdir(yolov5_labels_train_dir):
    os.mkdir(yolov5_labels_train_dir)
clear_hidden_files(yolov5_labels_train_dir)
yolov5_labels_test_dir = os.path.join(yolov5_labels_dir, "val/")
if not os.path.isdir(yolov5_labels_test_dir):
    os.mkdir(yolov5_labels_test_dir)
clear_hidden_files(yolov5_labels_test_dir)

train_file = open(os.path.join(wd, "yolov5_train.txt"), 'w',encoding='utf-8')
test_file = open(os.path.join(wd, "yolov5_val.txt"), 'w',encoding='utf-8')
train_file.close()
test_file.close()
train_file = open(os.path.join(wd, "yolov5_train.txt"), 'a',encoding='utf-8')
test_file = open(os.path.join(wd, "yolov5_val.txt"), 'a',encoding='utf-8')
list_imgs = os.listdir(image_dir)  # list image files
prob = random.randint(1, 100)
for i in range(0, len(list_imgs)):
    path = os.path.join(image_dir, list_imgs[i])
    if os.path.isfile(path):
        image_path = image_dir + list_imgs[i]
        voc_path = list_imgs[i]
        (nameWithoutExtention, extention) = os.path.splitext(os.path.basename(image_path))
        (voc_nameWithoutExtention, voc_extention) = os.path.splitext(os.path.basename(voc_path))
        annotation_name = nameWithoutExtention + '.xml'
        annotation_path = os.path.join(annotation_dir, annotation_name)
        label_name = nameWithoutExtention + '.txt'
        label_path = os.path.join(yolo_labels_dir, label_name)
    prob = random.randint(1, 100)
    if (prob < TRAIN_RATIO):  # train dataset
        if os.path.exists(annotation_path):
            train_file.write(image_path + '\n')
            convert_annotation(nameWithoutExtention)  # convert label
            copyfile(image_path, yolov5_images_train_dir + voc_path)
            copyfile(label_path, yolov5_labels_train_dir + label_name)
    else:  # test dataset
        if os.path.exists(annotation_path):
            test_file.write(image_path + '\n')
            convert_annotation(nameWithoutExtention)  # convert label
            copyfile(image_path, yolov5_images_test_dir + voc_path)
            copyfile(label_path, yolov5_labels_test_dir + label_name)
train_file.close()
test_file.close()
```

分割好的数据集文件夹结构：

```md
- train
    - labels
        - train
        - val
    - images
        - train
        - val
```

为了符合coco数据集格式，需要将xml标签文件转化为txt格式：

```python
def convert_annotation(image_id):
    in_file = open('train/train/xml/%s.xml' % image_id) # xml路径
    out_file = open('train/train/labels/%s.txt' % image_id, 'w')
    tree = ET.parse(in_file)
    root = tree.getroot()
    size = root.find('size')
    w = int(size.find('width').text)
    h = int(size.find('height').text)

    for obj in root.iter('object'):
        difficult = obj.find('difficult').text
        cls = obj.find('name').text
        if cls not in classes or int(difficult) == 1:
            continue
        cls_id = classes.index(cls)
        xmlbox = obj.find('bndbox')
        b = (float(xmlbox.find('xmin').text), float(xmlbox.find('xmax').text), float(xmlbox.find('ymin').text),
             float(xmlbox.find('ymax').text))
        bb = convert((w, h), b)
        out_file.write(str(cls_id) + " " + " ".join([str(a) for a in bb]) + '\n')
    in_file.close()
    out_file.close()

def convert(size, box): # 边界框的转换
    dw = 1. / size[0]
    dh = 1. / size[1]
    x = (box[0] + box[1]) / 2.0
    y = (box[2] + box[3]) / 2.0
    w = box[1] - box[0]
    h = box[3] - box[2]
    x = x * dw
    w = w * dw
    y = y * dh
    h = h * dh
    return (x, y, w, h)
```


## 配置YAML文件

```yaml
architecture: yolov5s  ##使用yolov5预训练
batch_size: 16
epochs: 50
lr: 0.001
names: ##类别
- 3+2-2
- 3jia2
- aerbeisi
- anmuxi
- aoliao
- asamu
- baicha
- baishikele
- baishikele-2
...
nc: 113  ##类别数目
train: train/images/train ##训练集路径
val: train/images/val ##验证集路径
```

## 训练模型：

```bash
python train.py --img 640 --batch 16 --epochs 50 --data ./data/my.yaml --cfg ./models/yolov5s.yaml --weights yolov5s.pt  
```
训练好的模型存在`runs/exp/best.pt`

## TensorRT加速

下载好tensorrt之后

```bash
python export.py --weights best.pt --data data/my.yaml --include engine --device 0 --half    
```
将best.pt导出为best.engine

## 目标识别
```bash
python detect.py --source train/images/val --data data/my.yaml --weights best.engine --device 0
```


















