# Fall_Detection

[소프트웨어융합캡스톤디자인]

## 1. Overview

한국은 빠른 고령화 속도로 인해 2025년에 초고령 사회 진입할 것으로 예측되고 있다. 이로 인해 노약자 사고 발생 예측, 예방을 위한 스마트 돌봄 서비스 수요가 증가한다는 것을 알 수 있다. 노약자에게 발생하는 사고 중 뇌졸중 등으로 인한 낙상 사고가 가장 빈번하고 낙상으로 인한 2차 피해 발생 가능성이 높다. 이를 해결하고자 본 과제에서는 pose line을 그린 이미지와 action recognition 모델을 이용하여 낙상 감지 모델 성능을 비교하고자 한다.

## 2. Dataset

- URFD Dataset

  ADL: 40 clip, Fall: 30 clip
  
  - RGB
  
  <img src="/img/urfd.png" width="400" height="250">
  
  - Openpose
  
  <img src="/img/urfd_pose.png" width="400" height="250">
  
- AI Hub 시니어 이상행동 영상

  ADL: 700 clip, Fall: 700 clip
  
  - RGB
  
  <img src="/img/aihub.png" width="400" height="250">
  
  - Openpose

  <img src="/img/aihub_pose.png" width="400" height="250">

## 3. Model

아래 2개의 모델에 RGB 이미지와 Openpose 라이브러리를 이용하여 pose line을 그린 이미지를 넣는다.(I3D는 Optical Flow도)

### 1) C3D

<img src="/img/c3d.png">

- Input shape: (16, 112, 112, 3)
- Optimizer: Adam
- 모든 Conv layer은 3x3x3 kernel을 사용한다.
- Max pooling layer를 사용하며 첫 번째 Pooling layer는 1x2x2 kernel과 stride를 사용하고 나머지 Pooling layer는 2x2x2 kernel과 stride를 사용한다.
  - 첫 번째만 다른 이유는 시간축 정보가 빨리 머지되는 것을 방지하기 위함이다.

### 2) I3D

<img src="/img/i3d.png">

- Input shape: (64, 224, 224, 3)
  - frame을 (256, 256)로 resize 후 (224, 224)로 random crop
- Optimizer: SGD
- Momentum:0.9
- Imagenet pretrained Inception-V1 module을 사용한다.
- C3D와 동일한 이유로 두 번째 Max pooing layer까지는 1x2x2 kernel과 stride를 사용하고 나머지 Max pooling layer는 2x2x2 kernel과 stride를 사용한다.

## 4. Training

### 1) C3D

- URFD(RGB)

  <img src="/img/urfd/c3d_rgb_epoch100.png" width="500" height="300">

- URFD(Openpose)

  <img src="/img/urfd/c3d_pose_epoch100.png" width="500" height="300">

- AI Hub(RGB)

  <img src="/img/aihub/c3d_rgb_epoch10.png" width="500" height="300">

- AI Hub(Openpose)

  <img src="/img/aihub/c3d_pose_epoch10.png" width="500" height="300">

### 2) I3D(pretrained on Imagenet)

- URFD(RGB)

  <img src="/img/urfd/i3d_imagenet_rgb_lr0.01_epoch100.png" width="500" height="300">

- URFD(Optical Flow)

  <img src="/img/urfd/i3d_imagenet_flow_lr0.01_epoch100.png" width="500" height="300">

- URFD(Openpose)

  <img src="/img/urfd/i3d_imagenet_pose_lr0.01_epoch100.png" width="500" height="300">

- AI Hub(RGB)

  <img src="/img/aihub/i3d_imagenet_rgb_lr0.0025_epoch4.png" width="500" height="300">

- AI Hub(Openpose)

  <img src="/img/aihub/i3d_imagenet_pose_lr0.0025_epoch4.png" width="500" height="300">

||C3D|I3D|
|----|----|----|
|URFD(RGB)|1|1|
|URFD(Optical Flow)|-|1|
|URFD(Openpose)|1|1|
|AI Hub(RGB)|0.97|0.92|
|AI Hub(Openpose)|0.97|0.97|


## 4. Conclusion

URFD Dataset의 Fall 영상이 한 장소에서 촬영되어 있기 때문에 배경까지 학습되어 정확도가 1이 나온 것으로 보인다. 이를 보완하고자 추후 새로운 데이터셋을 수집하여 테스트할 계획이다.
Openpose를 이용하여 이미지에 pose line을 그린 경우 성능이 최대 5%까지 향상되는 것을 확인할 수 있다. AI Hub 데이터의 경우 배경이 다양하고 차지하는 비율이 높아 사람을 감지하기 어렵다. 본 과제에서 Openpose를 이용하여 사람을 미리 감지하기 때문에 성능이 향상되는 것으로 보인다.

## Link

[발표자료](https://drive.google.com/file/d/1FVipTl91Kh3r8U1L-D2zCsWObi-QkAS6/view?usp=sharing)
