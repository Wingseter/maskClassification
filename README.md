# maskClassification

Ai Stages 에서 진행하는 마스크 분류 프로젝트
</br>
</br>
---
## 프로젝트 설명
COVID-19의 확산은 전 세계 사람들의 큰 문제이며 이를 해결하기 위해 다양한 노력이 있지만
강력한 전염성으로 인해 여전히 우리를 곤란하게 하고 있다. 코로나의 감염 확산을 막기위해 무
엇보다 중요한 것은 모든 사람이 마스크로 코, 입을 가려 전파 경로를 차단하는 것이다. 이를 위
해 우리가 개발하려는 것은 카메라로 비춰진 사람 얼굴 이미지 만으로 이 사람이 마스크를 쓰고
있는지, 쓰지 않았는지, 정확히 쓴 것이 맞는지 자동으로 가려낼 수 있는 시스템 이다.
</br>
![classes](https://github.com/Wingseter/maskClassification/blob/main/classes.png)
</br>
최종적으로 우리가 만드는 시스템은 마스크의 착용 여부(Wear, Incorrect, Not Wear), 성별
(Male, Female), 나이(<30, >=30 and <60, >=60)를 분류해 총 18개의 class를 분류하게 된다.


## 	프로젝트 수행 절차 및 방법
</br>

* EDA
  
 시각화 도구(Excel, MatplotLib)들을 사용해서 제공된 이미지들을 분석해 본 결과 데이터 불균형 문제가 심각하다는 것을 깨달았다. 마스크 여부(Mask, Incorrect Mask, Not wear Mask)의 비율은 5:1:1 이었으며, 나이(<30, >=30 and <60, >=60) 의 비율은 1:1:6이었다. 또한 남자의 데이터 양이 여성에 비해 0.6배가 되어 이를 해결할 방법을 찾아야만 했다.
</br>
* Data

 데이터 불균형 문제를 해결하기 위해 가장 먼저 시도한 것은 Over Sampling 이었다. 우리는 색감, 채도, 밝기, 회전, Crop 등 다양한 방법으로 데이터가 적은 Class의 데이터를 늘려 비율을 통일시켰고. 최종적으로 가장 효과가 좋은 Random Crop, Center Crop, Random Horizontal Flip 조합이 채택되었다. 여기에 더해 나이를 10단위로 나누어 Augmentation을 진행했을 때 가장 좋은 결과를 얻었다. TTA, SMOTE 방법 또한 시도해 보았지만 큰 차이가 없었다.
</br>

* Model 

  직접 만든 CNN 모델을 사용했지만 좋은 성능을 보이지 못해 Pretrained 모델을 사용하기로 했다. vgg-16, vgg-19, Resnet-18, Resnet-50, Resnet-152, Efficient-Net-b3, b7를 시도했고 이전과 비교해 성능이 향상되었지만 각각의 모델의 성능은 큰 차이가 없었다. 이후 RandWireNN, Auto-Pytorch, Optuna등 AutoML을 사용해 자동으로 최적화된 모델을 직접 찾으려 했지만 원하는 성능이 나오지 않아 마지막에는 초기에 시도했던 Resnet을 사용했다.
</br>

* Hyper Parameter 튜닝 

 대회 후반에 베이스라인 코드를 이용해 다양한 Hyper Parameter을 시도해 보면서 조금이라도 성능을 올리려 했다.
 </br>
 
* Environment

 자동화 시킬 수 있는 부분은 터미널에서 베이스라인 코드를 활용했고, 분석이 필요한 내용들은 주피터 노트북을 통해 interactive하게 결과를 확인해가면서 나아갈 부분들을 고민했다.
</br>
## 프로젝트 결과
우리는 Resnet-18모델에 10 단위로 나눈 데이터를 Random Crop, Center Crop, Random Horizontal
Flip을 이용한 Augmentation을 사용했을 때 아래와 같은 가장 좋은 결과를 얻을 수 있었다.

Evaluation set) F1_Score: 0.754, Accuracy: 0.790
Test Set) F1 Score: 0.723, Accuracy: 0.774

주로 모델 보다는 데이터가 성능에 유의미한 영향을 줬으며, 데이터를 10단위로 나눈 것이 많은
영향을 주었다. 실제 데이터 에서 50대 후반과 60대를 잘 구별하지 못했는데 이를 나누어서 분류
함으로서 30~60세 사이의 데이터를 판단하는데 큰 도움이 준 것으로 생각한다.
또한 Crop을 통해서 결정에 방해가 되는 배경이 제거가 되었고 이것이 성능에 많은 영향을 준
것으로 판단된다.
