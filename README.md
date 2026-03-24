# 🎓 Kaggle Student Score Prediction

<div align="center">
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white">
  <img src="https://img.shields.io/badge/lightgbm-02569B?style=for-the-badge&logo=lightgbm&logoColor=white">
</div>

---

## 📖 프로젝트 주제
**시험 점수 예측 모델링: 통계분석 및 머신러닝 접근**

- 학생의 **학습 습관, 생활 패턴, 배경 정보**를 기반으로
  **시험 점수를 예측하는 머신러닝 모델**을 구축했습니다.
- 단순 예측을 넘어, **성적에 영향을 주는 주요 변수**를 함께 분석했습니다.

---

## 1. Project Overview

- **주제**: 학생의 학습 및 생활 데이터를 활용한 시험 점수 예측
- **데이터셋**: Kaggle Playground Series - Student Performance
- **문제 유형**: 회귀(Regression)
- **핵심 목표**
  - 시험 점수 예측 모델 개발
  - 주요 영향 변수 분석
  - 모델 성능 비교 및 최적 모델 선정

---

## 2. Data Dictionary

| 변수명 | 설명 | 타입 |
| :--- | :--- | :--- |
| `exam_score` | 시험 점수 (Target) | 수치형 |
| `study_hours_per_day` | 하루 공부 시간 | 수치형 |
| `attendance_rate` | 출석률 | 수치형 |
| `sleep_hours` | 수면 시간 | 수치형 |
| `physical_activity` | 신체 활동 여부 | 범주형 |
| `internet_usage` | 인터넷 사용 시간 | 수치형 |
| `parental_education` | 부모 학력 | 범주형 |
| `family_income` | 가구 소득 수준 | 순서형 |

---

## 3. Problem Definition

- **데이터 특성**
  - 다양한 학습 습관 및 생활 패턴 변수 포함
  - 일부 변수 간 상관관계 존재

- **분석 방향**
  - 통계 분석을 통해 변수 영향력 해석
  - 머신러닝 모델을 통해 예측 성능 비교

---

## 4. Data Preprocessing

- **결측치 처리**
  - 수치형 변수: 평균값 대체
  - 범주형 변수: 최빈값 대체

- **범주형 변수 처리**
  - 일반 범주형 변수: One-Hot Encoding
  - 순서형 변수: Ordinal Encoding

- **스케일링**
  - 수치형 변수에 대해 `StandardScaler` 적용

- **Pipeline 구성**
  - 전처리와 모델을 하나의 파이프라인으로 구성하여 데이터 누수 방지

---

## 5. 통계분석 핵심 인사이트

- **공부 시간**은 시험 점수와 가장 강한 양의 관계를 보였습니다.
- **출석률**이 높을수록 전반적인 점수 수준도 높게 나타났습니다.
- **수면 시간**은 일정 수준까지 학업 성과에 긍정적인 영향을 주었습니다.
- **과도한 인터넷 사용 시간**은 점수 하락과 관련된 경향을 보였습니다.

---

## 6. 모델링 및 성능 비교

| Model | RMSE | MAE | R² |
| :--- | :---: | :---: | :---: |
| Linear Regression | 8.21 | 6.45 | 0.71 |
| Decision Tree | 7.98 | 6.20 | 0.73 |
| Random Forest | 7.42 | 5.81 | 0.78 |
| XGBoost | 7.21 | 5.65 | 0.80 |
| **LightGBM** | **7.05** | **5.52** | **0.82** |

> 검증 데이터 기준으로 비교했으며, LightGBM이 가장 우수한 성능을 보였습니다.

---

## 7. Feature Importance

- **중요 변수 TOP 3**
  1. 공부 시간
  2. 출석률
  3. 인터넷 사용 시간

- **중요도가 낮은 변수**
  - 일부 생활 패턴 변수는 제거 후에도 성능 변화가 크지 않았습니다.

---

## 8. Conclusion

- 학생의 생활 및 학습 데이터를 바탕으로 **시험 점수를 예측할 수 있음**을 확인했습니다.
- 특히 **공부 시간, 출석률, 인터넷 사용 시간**이 주요 예측 변수로 작용했습니다.
- Feature Selection 이후에도 성능 저하가 크지 않아, **모델 경량화 가능성**도 확인했습니다.

### 기대 활용
- 학습 코칭 시스템
- 성적 위험군 조기 탐지
- 개인 맞춤형 교육 전략 수립

---

## 📁 프로젝트 구조

├── data/
├── analysis/
│   └── ml_pipeline_cleaned.ipynb
├── report/
│   └── 프로젝트보고서.pdf
└── README.md
---

# 보고서
- 프로젝트 상세 내용은 PDF 보고서를 참고해 주세요
- 최종 보고서 : [당뇨병 예측 모델링: 통계분석 및 머신러닝 접근](report/프로젝트보고서.pdf)
- 분석 코드 : [분석 코드](/analysis/ml_pipeline_cleaned.ipynb)

---

# 🔗 배지 및 이모지 공식 소스 링크
| 용도 | 사이트 이름 | 링크 |
| :--- | :--- | :--- |
| **배지 생성** | Shields.io | https://shields.io/ |
| **로고/색상 검색** | Simple Icons | https://simpleicons.org/ |
| **이모지 검색** | Emoji Cheat Sheet | https://github.com/ikatyang/emoji-cheat-sheet |
