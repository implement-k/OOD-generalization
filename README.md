# OOD-generalization
keyword: Computer Vision, OOD Generalization, Robustness, Resnet-18, IBN-Net
26.02(연구실 과제)
---
### Problem
Pre-trained 모델 사용이 제한된 from-scratch 학습 조건에서, ResNet-18 모델 만으로 Photo 도메인에서 학습하고 Sketch 도메인으로 일반화해야 하는 과제

### Methodologies
·	서로 다른 데이터 분포에 강점이 있는 single view와 multi view 모델을 설계하고, 각 모델의 예측력을 결합하기 위해 앙상블 수행.
·	train dataset을 test dataset의 분포와 유사하게 정렬할 수록 일반화 성능이 향상될 것이라는 가설 하에 실험 설계 
  -	single view model에서는 물체를 구분하는데 필요 없는 배경정보와 물체 정보를 제거하기 위해 grabcut(배경 제거) + canny알고리즘을 사용.

·	multi view model에서는 grabcut+canny, transformed 원본, phase reconstruction, mixup, PROF, sobel을 적용한 6개의 모델에 대해 mdar, proto, subcon Loss를 사용하여 학습을 진행. 
  -	Pahse Reconstruction: 이미지 구조 정보 활용
  -	PROF: magnitude spectrum을 변주하여 모델이 다양한 도메인 환경 학습하도록 유도
  -	Sobel: canny 정보 손실 보완용
  -	Mdar loss: 각 뷰의 예측 분포 일치시켜 도메인에 무관한 판단 유도
  -	Proto loss: feature 공간 내에 스케치와 제일 비슷한 배경제거+canny를 기준점으로 하여 분포 수렴 유도.
  -	subcon: 동일 라벨 응집도를 높혀 클래스 분별력 강화

·	multi view model에서 한 번에 모든 뷰를 학습시키는 대신, 뷰를 순차적으로 추가하여 최적의 모델로부터 학습 진행.
·	스타일을 지우기 위해 Instance Normalization을 사용하는 IBN-Net 구조를 도입하고, 배경보다 물체에 집중시키기 위해 SE-block을 사용.
·	그 외에는 학습 기법과 데이터 증강은 결과에 대한 효과가 독립적이라고 가정하고 성능이 향상되는 최적의 조합을 선별.

### 코드 설명
·	sigle view 
