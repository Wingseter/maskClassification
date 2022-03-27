# maskClassification
---
## 프로젝트 설명
COVID-19의 확산은 전 세계 사람들의 큰 문제이며 이를 해결하기 위해 다양한 노력이 있지만
강력한 전염성으로 인해 여전히 우리를 곤란하게 하고 있다. 코로나의 감염 확산을 막기위해 무
엇보다 중요한 것은 모든 사람이 마스크로 코, 입을 가려 전파 경로를 차단하는 것이다. 이를 위
해 우리가 개발하려는 것은 카메라로 비춰진 사람 얼굴 이미지 만으로 이 사람이 마스크를 쓰고
있는지, 쓰지 않았는지, 정확히 쓴 것이 맞는지 자동으로 가려낼 수 있는 시스템 이다.

![classes](https://github.com/Wingseter/maskClassification/blob/main/classes.png)

최종적으로 우리가 만드는 시스템은 마스크의 착용 여부(Wear, Incorrect, Not Wear), 성별
(Male, Female), 나이(<30, >=30 and <60, >=60)를 분류해 총 18개의 class를 분류하게 된다.


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
