🎓 Kaggle Student Score Prediction
<div align="center"> <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white"> <img src="https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white"> <img src="https://img.shields.io/badge/lightgbm-02569B?style=for-the-badge&logo=lightgbm&logoColor=white"> </div>
📖 프로젝트 주제
📊 시험 점수 예측 모델링: 통계분석 및 머신러닝 접근
학생의 학습 습관, 생활 패턴, 배경 정보를 기반으로
시험 점수를 예측하는 머신러닝 모델 구축
단순 예측을 넘어 성적에 영향을 주는 주요 요인 분석까지 수행
1. Project Overview
주제 : 학생의 학습 및 생활 데이터를 활용한 시험 점수 예측
데이터셋 : Kaggle Playground Series (Student Performance)
문제 유형 : 회귀 (Regression)
핵심 목표
시험 점수 예측 모델 개발
주요 영향 변수(Feature) 분석
모델 성능 비교 및 최적 모델 선정
2. Data Dictionary (주요 변수)
변수명	설명	타입
exam_score	시험 점수 (Target)	수치형
study_hours_per_day	하루 공부 시간	수치형
attendance_rate	출석률	수치형
sleep_hours	수면 시간	수치형
physical_activity	신체 활동 여부	범주형
internet_usage	인터넷 사용 시간	수치형
parental_education	부모 학력	범주형
family_income	가구 소득 수준	순서형
3. Problem Definition
데이터 특성
다양한 생활 습관 변수 포함
변수 간 상관관계 존재
분석 방향
통계 분석을 통한 변수 영향력 확인
머신러닝 모델을 통한 예측 성능 최적화
4. Data Preprocessing
결측치 처리
수치형: 평균값 대체
범주형: 최빈값 대체
범주형 변수 처리
One-Hot Encoding
Ordinal Encoding (순서형 변수)
스케일링
StandardScaler 적용
Pipeline 구성
전처리 + 모델 통합
데이터 누수(Data Leakage) 방지
5. 통계분석 핵심 인사이트
공부 시간(study_hours) → 시험 점수에 가장 큰 영향
출석률(attendance_rate) → 점수와 강한 양의 상관관계
수면 시간(sleep_hours) → 적정 범위에서 성적 향상에 기여
과도한 인터넷 사용 → 점수 감소와 관련
6. 모델링 및 성능 비교
Model	RMSE	MAE	R²
Linear Regression	8.21	6.45	0.71
Decision Tree	7.98	6.20	0.73
Random Forest	7.42	5.81	0.78
XGBoost	7.21	5.65	0.80
LightGBM	7.05	5.52	0.82

LightGBM이 가장 우수한 성능을 보임

7. Feature Importance
중요 변수 TOP 3
공부 시간
출석률
인터넷 사용 시간
중요도 낮은 변수
일부 생활 패턴 변수 → 제거 후 성능 변화 미미
8. Conclusion
학생의 생활 데이터만으로도 시험 점수 예측 가능
학습 습관이 성적에 미치는 영향이 매우 큼
Feature Selection 이후에도 성능 유지 → 모델 경량화 가능

👉 실무 활용 가능성

학습 코칭 시스템
성적 위험군 조기 탐지
개인 맞춤형 교육 전략 수립
📁 프로젝트 구조
├── data/
├── notebooks/
├── models/
├── report/
├── README.md
📄 보고서
📘 최종 보고서 : report/프로젝트보고서.pdf
💻 분석 코드 : analysis/ml_pipeline_cleaned.ipynb
🔗 참고 링크
용도	사이트	링크
배지 생성	Shields.io	https://shields.io/

아이콘	Simple Icons	https://simpleicons.org/

이모지	Emoji Cheat Sheet	https://github.com/ikatyang/emoji-cheat-sheet
