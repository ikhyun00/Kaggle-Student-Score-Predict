# 🎓 Kaggle Student Score Prediction

<div align="center">
  <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white">
  <img src="https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white">
  <img src="https://img.shields.io/badge/statsmodels-4051B5?style=for-the-badge">
  <img src="https://img.shields.io/badge/XGBoost-EA4335?style=for-the-badge&logo=apache&logoColor=white">
</div>

---

## 📖 프로젝트 주제
**시험 점수 예측 모델링: 통계분석 및 머신러닝 접근**

- 학생의 **학습 습관, 생활 패턴, 학습 환경** 데이터를 기반으로
  **시험 점수를 예측하는 머신러닝 모델**을 구축했습니다.
- 63만 건의 대용량 데이터에서 발생하는 p-value 인플레이션을 고려해, **효과크기 중심으로 주요 변수**를 함께 분석했습니다.

---

## 1. Project Overview

- **주제**: 학생의 학습 및 생활 데이터를 활용한 시험 점수 예측
- **데이터셋**: Kaggle Playground Series - Student Exam Score (630,000행 × 13개 컬럼, 합성 데이터)
- **문제 유형**: 회귀(Regression), 평가 지표 RMSE
- **핵심 목표**
  - 시험 점수 예측 모델 개발
  - 효과크기 기반 주요 영향 변수 분석
  - 모델 성능 비교 및 OOF 블렌딩으로 성능 개선

---

## 2. Data Dictionary

| 변수명 | 설명 | 타입 |
| :--- | :--- | :--- |
| `exam_score` | 시험 점수 (Target) | 수치형 |
| `study_hours` | 학습 시간 | 수치형 |
| `class_attendance` | 출석률 | 수치형 |
| `sleep_hours` | 수면 시간 | 수치형 |
| `age` / `gender` / `course` | 나이 / 성별 / 전공 과정 | 수치형 / 범주형 / 범주형 |
| `internet_access` | 인터넷 접근 여부 (yes/no) | 범주형 |
| `study_method` | 학습 방법 (coaching, self-study 등) | 범주형 |
| `sleep_quality` | 수면의 질 (poor < average < good) | 순서형 |
| `facility_rating` | 학습 시설 만족도 (low < medium < high) | 순서형 |
| `exam_difficulty` | 시험 난이도 (easy < moderate < hard) | 순서형 |

---

## 3. Problem Definition

- **데이터 특성**
  - 결측치 없음, 수치형 5개 · 범주형 7개 변수로 구성
  - 표본이 매우 커서 실질적 차이가 거의 없어도 대부분 p < 0.05 (p-value 인플레이션)

- **분석 방향**
  - 통계 분석: p-value 대신 효과크기(Spearman ρ, η², Cohen's d)로 변수 영향력 해석
  - 머신러닝: 선형·부스팅 모델 비교 후 OOF 기반 블렌딩

---

## 4. Data Preprocessing

- **결측치 처리**
  - 수치형 변수: 중앙값 대체 + 결측 지시변수
  - 범주형 변수: 최빈값 대체

- **범주형 변수 처리**
  - 범주형 값 공백 제거 및 소문자 통일
  - 모든 범주형 변수: One-Hot Encoding

- **스케일링**
  - Ridge: 수치형 변수에 `StandardScaler` 적용
  - 트리 모델(HGBR, XGBoost): 스케일링 미적용

- **Pipeline 구성**
  - `ColumnTransformer` + `Pipeline`으로 전처리와 모델을 묶어 교차검증 fold 내부에서만 전처리를 학습 → 데이터 누수 방지

---

## 5. 통계분석 핵심 인사이트

- **학습 시간**은 시험 점수와 가장 강한 양의 관계를 보였습니다. (Spearman ρ = 0.77, 큰 효과)
- **출석률**이 높을수록 점수도 높게 나타났습니다. (ρ = 0.35, 중간 효과)
- **수면의 질**(η² = 0.056)과 **학습 방법**(η² = 0.050)은 작은~중간 수준의 효과를 보였습니다.
- **인터넷 접근 여부**는 200회 반복 샘플링 t-test에서 유의 확률 6.5%, Cohen's d −0.023으로 실질적 차이가 없었습니다.

---

## 6. 모델링 및 성능 비교

| Model | CV RMSE | 블렌딩 가중치 |
| :--- | :---: | :---: |
| Ridge | 8.895 | 0.00 |
| HistGradientBoosting | 8.776 | 0.55 |
| XGBoost | 8.779 | 0.45 |
| **Blend (OOF)** | **8.769** | - |

> 5-Fold 교차검증 기준으로 비교했으며, OOF 예측값으로 가중치를 탐색한 블렌딩이 가장 우수했습니다. (Kaggle Public Score 8.781 → 8.734, 0.53% 개선)

---

## 7. Feature Importance

- **중요 변수 TOP 3** (SHAP, XGBoost 기준)
  1. 학습 시간
  2. 출석률
  3. 수면의 질

- **중요도가 낮은 변수**
  - 나이, 성별, 전공은 SHAP 값이 거의 0에 수렴했습니다.

---

## 8. Conclusion

- 학생의 생활 및 학습 데이터를 바탕으로 **시험 점수를 예측할 수 있음**을 확인했습니다.
- 특히 **학습 시간, 출석률, 수면의 질**이 주요 예측 변수로 작용했으며, 통계 분석과 SHAP 결과가 일치했습니다.
- 대용량 데이터에서는 p-value보다 **효과크기**가 변수 해석에 더 적절함을 확인했습니다. (단, 합성 데이터이므로 실제 학생 집단으로의 일반화에는 한계가 있습니다.)

### 기대 활용
- 학습 코칭 시스템
- 성적 위험군 조기 탐지
- 개인 맞춤형 교육 전략 수립

---

# 보고서
- 프로젝트 상세 내용은 PDF 보고서를 참고해 주세요
- 최종 보고서 : [Kaggle 시험 점수 예측](./Kaggle%20시험%20점수%20예측.pdf)
- 분석 코드 : [통계 분석](./Kaggle_시험점수예측_메인.ipynb) / [모델링·SHAP](./kaggle_시험점수예측_SHAP_.ipynb)

---

# 🔗 배지 및 이모지 공식 소스 링크
| 용도 | 사이트 이름 | 링크 |
| :--- | :--- | :--- |
| **배지 생성** | Shields.io | https://shields.io/ |
| **로고/색상 검색** | Simple Icons | https://simpleicons.org/ |
| **이모지 검색** | Emoji Cheat Sheet | https://github.com/ikatyang/emoji-cheat-sheet |
