# Model-API

### 1. YOLOv5 Model
중고거래 상품의 종류를 판별하기 위해 YOLOv5 모델을 커스터마이징 한 후 Object Detection을 진행하였다. 
원 모델은 [ultralytics](https://github.com/ultralytics/yolov5 )의 YOLOv5s 모델을 사용하였고, 직접 수집한 데이터를 모델에 학습시켰다. 

### 2. 데이터 라벨링
이미지 데이터는 네이버에서 크롤링하여 수집하였다. 이미지 라벨링에는 makesense를 사용하였다.
https://www.makesense.ai/ <br>

class는 converse high, nike daybreak로 설정하였다.
추후 데이터 수와 클래스를 늘려가며 확장할 수 있을 것이라 기대된다.



### 3. 데이터셋 


https://drive.google.com/file/d/1fR5rPbnFA82-beYaUAZIq1oNlQGvSLky/view?usp=sharing <br>
https://drive.google.com/file/d/1fR5rPbnFA82-beYaUAZIq1oNlQGvSLky/view?usp=sharing <br>
https://drive.google.com/file/d/1gVNf6pIyMxyrRfLySIvLiV_XDk7viJb_/view?usp=sharing

### 4. 학습 진행

다음과 같은 과정으로 커스텀 데이터를 학습시켰다.

#### 1. yaml 파일 설정
custom_data.yaml 파일을 다음과 같이 설정하였다.
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

#### 2. 원 모델 설치 및 환경 설정
```python
!git clone https://github.com/ultralytics/yolov5  # clone
%cd yolov5
%pip install -qr requirements.txt  # install

import torch
from yolov5 import utils
display = utils.notebook_init()  # checks
```
#### 3. 커스텀 데이터 업로드 및 압축 해제
```python
!unzip -q ../train_data.zip -d ../content/
```

#### 4. 모델 학습 진행
```python
!python train.py --img 640 --batch 16 --epochs 700 --data custom_data.yaml --weights yolov5s.pt --cache
```

#### 5. test 이미지 detect
```python
!python detect.py --weights runs/train/exp/weights/last.pt --img 640 --conf 0.35 --source ../test/
```

아래처럼 detect 결과를 볼 수 있다. <br>
![1](https://i.postimg.cc/wv3qNrgZ/1-r.jpg)
![2](https://i.postimg.cc/v8g9Fqf0/2-r.png)
![3](https://i.postimg.cc/fRKSj6QR/3-r.jpg)
![4](https://i.postimg.cc/tRK6X3zp/4-r.png)

