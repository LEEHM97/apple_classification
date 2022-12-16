# 사과 이미지를 이용한 사과 품종 분류 - AI CONNECT 모의경진대회

<br>

## 1. Dataset

- Input: 512X512 크기의 이미지 데이터
- Target: 4개의 사과 종(HJ, HR, SG, AR)
- n_train: 10000
- n_test: 5000
- Data directory structure:

    ```bash
    DATA/  
    ├── train/  # 학습 데이터
    │     ├── xxx.jpg
    │     ├── yyy.jpg
    │     ├── zzz.jpg
    │     └── ...
    │ 
    ├── test/  # 추론 데이터
    │     ├── xxx.jpg
    │     ├── yyy.jpg
    │     ├── zzz.jpg
    │     └── ...
    │
    ├── train.csv
    └── test.csv 
    ```

<br>

## 2. Evalutation Metric

- F1 Score: 정밀도와 재현율의 조화 평균 <br>
![image](https://user-images.githubusercontent.com/67961082/208139536-d73cd7a9-4bfc-4546-afac-701bedf93897.png)

<br>

## 3. Modeling

### 3-1. Augmentations
- ToTensor
- Normalize
- RandomHorizontalFlip
- RandomVerticalFlip
- RandomRotation

### 3-2. Models
- Efficientnet_B4
- Inception_V4
- Seresnext101_32x4d
- **Ensemble(best)**
    - Efficientnet_B4_Kfold01
    - Seresnext101-32x4d_Kfold02
    - Inception_V4_Kfold01
 
<br>

## 4. Model Evaluation

| Models        | Loss          | Acc        | F1-score    |
| ------------- |:-------------:| ----------:| -----------:|
| Efficientnet_B4      | 0.1039       |    0.9628 | 0.9628 |
| Inception_V4         | 0.0630       |    0.9768 | 0.9574 |        
| Seresnext101_32x4d   | 0.0614       |    0.9818 | 0.9702 |
| Ensemble             | 0.0189       |    0.9952 | 0.9924 |

<br>

## 5. Result
- F1-score Private Score: 0.9576
- 5th / 137 teams

<br>

## 6. Review
- 코로나 등의 이슈로 시간이 많이 부족해 계획했던 실험을 못해서 아쉬웠던 대회
- 4:2:1:1의 비율로 편향되어있는 데이터를 처리하는 방법에 대해 생각해볼 수 있어 좋았다.
    1. 비율이 낮은 데이터만 augmentation 적용
    2. 외부 데이터가 허용된 대회이므로 크롤링으로 데이터 보충
    3. 비율이 높은 데이터를 삭제하여 비율을 맞춤(오히려 성능이 하락 됨을 확인)
- 추가로 시도해볼 수 있는 실험
    1. 원본 이미지가 화면에 꽉찬 사과이기 때문에 Augmentation진행 시 없어지는 주변 픽셀 처리
    2. 더 많은 모델 사용
    3. TTA
