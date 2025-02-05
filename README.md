# Jupyter-Notebook_Machine-Learning-Training-2
# NSL-KDD 기반 네트워크 침입 탐지 시스템

본 프로젝트는 NSL-KDD 데이터셋을 활용하여 **머신러닝 및 딥러닝 기법을 적용한 네트워크 침입 탐지 시스템**을 구축하는 것을 목표로 합니다. 데이터 불균형 문제를 해결하기 위해 **SMOTE, 언더샘플링 기법을 적용**하였으며, 여러 분류 알고리즘(Random Forest, XGBoost, SVM, KNN, Logistic Regression 등)을 실험하여 성능을 비교 분석하였습니다.

---

## 🔰 데이터셋 개요

NSL-KDD 데이터셋은 네트워크 침입 탐지를 위한 공개 데이터셋으로, 다양한 공격 유형과 정상 트래픽을 포함합니다.  
본 연구에서는 **이진 분류(정상 vs. 공격) 및 다중 분류(공격 유형별 분류)** 를 수행하여 모델의 성능을 평가하였습니다.

데이터셋 구조:
- **41개의 피처(feature) + 1개의 라벨(label)**
- **정상(normal) vs. 공격(attack)** 분류 적용
- **SMOTE 및 언더샘플링을 이용한 데이터 균형 조정**

---

## 🔰 데이터 전처리 과정

1. **불필요한 컬럼 제거:** `num_outbound_cmds` 등 정보량이 적은 변수 제거  
2. **범주형 데이터(Label Encoding):** `protocol_type`, `service`, `flag` 등 문자열 데이터를 숫자로 변환  
3. **정규화(Standardization):** `StandardScaler`를 활용하여 데이터 정규화  
4. **클래스 불균형 해결:**  
   - **SMOTE**를 적용하여 소수 클래스를 오버샘플링  
   - **언더샘플링**을 이용하여 정상 데이터와 공격 데이터를 1:1 비율로 균형 조정  

```python
# SMOTE 적용
smote = SMOTE(random_state=42, k_neighbors=5)
X_train_resampled, y_train_resampled = smote.fit_resample(X_train, y_train)

# 언더샘플링 적용
undersampler = RandomUnderSampler(sampling_strategy={0: 150000, 1: 150000}, random_state=42)
X_train_balanced, y_train_balanced = undersampler.fit_resample(X_train_resampled, y_train_resampled)
