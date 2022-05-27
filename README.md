# Model-API

### 📌 YOLOv5 Model

- 중고거래 상품의 종류를 판별하기 위해 YOLOv5 모델을 커스터마이징 한 후 Object Detection을 진행

- Yolov5 Model : https://github.com/ultralytics/yolov5 



### 📌 데이터 라벨링

- 크롤링을 통해 상품 이미지 데이터 수집
- 라벨링 Tool : [Makesense](https://www.makesense.ai/)
- converse high, nike daybreak 2개의 class로 학습


### 📌 데이터셋 

- cocoval2017 : https://drive.google.com/file/d/1fR5rPbnFA82-beYaUAZIq1oNlQGvSLky/view?usp=sharing <br>
- cocotest2017 : https://drive.google.com/file/d/1fR5rPbnFA82-beYaUAZIq1oNlQGvSLky/view?usp=sharing <br>
- 전체 데이터 : https://drive.google.com/file/d/1gVNf6pIyMxyrRfLySIvLiV_XDk7viJb_/view?usp=sharing

### 📌 학습 진행

다음과 같은 과정으로 커스텀 모델 학습 진행

#### - yaml 파일 설정

custom_data.yaml 파일을 다음과 같이 설정
```python
# Train/val/test sets as 1) dir: path/to/imgs, 2) file: path/to/imgs.txt, or 3) list: [path/to/imgs1, path/to/imgs2, ..]
path: ../train_data/  # dataset root dir
train: images/train  # train images (relative to 'path') 128 images
val: images/val  # val images (relative to 'path') 128 images
test:  # test images (optional)

# Classes
nc: 2  # number of classes
names: ['converse high', 'nike daybreak']  # class names
```

#### - 원 모델 설치 및 환경 설정
```python
!git clone https://github.com/ultralytics/yolov5  # clone
%cd yolov5
%pip install -qr requirements.txt  # install

import torch
from yolov5 import utils
display = utils.notebook_init()  # checks
```
#### - 커스텀 데이터 업로드 및 압축 해제
```python
!unzip -q ../train_data.zip -d ../content/
```

#### - 모델 학습 진행
```python
!python train.py --img 640 --batch 16 --epochs 700 --data custom_data.yaml --weights yolov5s.pt --cache
```

#### - test 이미지 detection
```python
!python detect.py --weights runs/train/exp/weights/last.pt --img 640 --conf 0.35 --source ../test/
```

#### - detection 결과 이미지

![1](https://i.postimg.cc/wv3qNrgZ/1-r.jpg)

![2](https://i.postimg.cc/v8g9Fqf0/2-r.png)


