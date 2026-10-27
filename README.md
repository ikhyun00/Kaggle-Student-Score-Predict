🎓 Kaggle Student Score Prediction
<div align="center"> <img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"> <img src="https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white"> <img src="https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white"> <img src="https://img.shields.io/badge/XGBoost-EA4335?style=for-the-badge&logo=apache&logoColor=white"> <img src="https://img.shields.io/badge/statsmodels-4051B5?style=for-the-badge"> <img src="https://img.shields.io/badge/SHAP-FF0D57?style=for-the-badge"> </div>
📖 프로젝트 주제

시험 점수 예측 모델링: 통계분석 및 머신러닝 접근

학생의 학습 습관, 생활 패턴, 학습 환경 데이터를 기반으로 시험 점수를 예측하는 머신러닝 모델을 구축했습니다.
63만 건의 대용량 데이터에서 발생하는 p-value 인플레이션 문제를 고려하여, p-value 대신 효과크기(Effect Size) 중심으로 변수의 영향력을 해석했습니다.
프로젝트 기간: 2026.01.17 ~ 2026.01.29
1. Project Overview
주제: 학생의 학습 및 생활 데이터를 활용한 시험 점수 예측
데이터셋: Kaggle Playground Series - Student Exam Score (합성 데이터)
문제 유형: 회귀(Regression)
평가 지표: RMSE
핵심 목표
성적에 영향을 주는 요인을 효과크기 기준으로 해석
학습 시간·출석률·수면의 질에 영향을 주는 요인 분석
모델 비교 및 OOF 블렌딩으로 예측 성능 개선
2. Data Dictionary
규모: 630,000행 × 13개 컬럼 (결측치 없음)
변수명	설명	타입
exam_score	시험 점수 (Target)	수치형
study_hours	학습 시간	수치형
class_attendance	출석률 (%)	수치형
sleep_hours	수면 시간	수치형
age	나이	수치형
gender	성별	범주형
course	전공 과정 (b.sc, diploma, b.tech 등)	범주형
internet_access	인터넷 접근 여부 (yes/no)	범주형
study_method	학습 방법 (coaching, self-study, group study 등)	범주형
sleep_quality	수면의 질 (poor < average < good)	순서형
facility_rating	학습 시설 만족도 (low < medium < high)	순서형
exam_difficulty	시험 난이도 (easy < moderate < hard)	순서형
3. Problem Definition
데이터 특성
표본이 63만 건으로 매우 커서, 실질적 차이가 거의 없어도 대부분의 검정에서 p < 0.05가 나옴
수치형 5개, 범주형 7개 변수로 구성
분석 방향
통계 분석: p-value가 아닌 **효과크기(Spearman ρ, η², Cohen's d)**로 영향력 해석
머신러닝: 선형·부스팅 모델 비교 후 OOF 기반 블렌딩으로 성능 개선
4. Data Preprocessing
문자열 정규화
범주형 값 공백 제거 및 소문자 통일
순서형 변수(sleep_quality, facility_rating, exam_difficulty)에 순서 지정
Pipeline 구성 (모델별 분리)
구분	수치형 처리	범주형 처리
Ridge	중앙값 대체 + 결측 지시변수 + StandardScaler	최빈값 대체 + One-Hot Encoding
Tree (HGBR, XGB)	중앙값 대체 + 결측 지시변수 (스케일링 없음)	최빈값 대체 + One-Hot Encoding
ColumnTransformer + Pipeline으로 전처리와 모델을 묶어, 교차검증 각 fold 내부에서만 전처리를 학습하도록 하여 데이터 누수 방지
5. 통계분석 핵심 인사이트
성적과의 관계 (효과크기)
변수	지표	효과크기	해석
study_hours	Spearman ρ	0.770	큰 효과
class_attendance	Spearman ρ	0.352	중간 효과
sleep_hours	Spearman ρ	0.160	작은 효과
sleep_quality	η² (ANOVA)	0.056	중간에 가까운 효과
study_method	η² (ANOVA)	0.050	작은~중간 효과
facility_rating	η² (ANOVA)	0.036	작은 효과
age, gender, course, exam_difficulty	ρ / η²	≈ 0	효과 없음
추가 분석
인터넷 접근 여부 vs 학습 시간 (t-test)
균형 샘플링 및 200회 반복 샘플링 결과, 유의 확률 6.5%, Cohen's d 평균 −0.023
→ 실질적인 차이 없음
학습 시간 위계적 다중회귀
인구통계 → 시험 난이도 → 출석·수면 → 학습 방법·환경 순으로 추가했을 때 R²는 최대 1.66%
→ 관측된 변수로는 학습 시간을 거의 설명하지 못함
출석률 표준화 OLS 계수
학습 시간 +1SD(2.36시간) → 출석률 +1.42%p로 가장 큰 영향
수면의 질 순서형 로지스틱 회귀
학습 방법이 자습(vs 코칭)이면 좋은 수면의 질 오즈 −9.85%
6. 모델링 및 성능 비교
검증 방식: 5-Fold 교차검증 (KFold, shuffle, random_state=42)
Model	CV RMSE	블렌딩 가중치
Ridge	8.895	0.00
HistGradientBoosting	8.776	0.55
XGBoost	8.779	0.45
Blend (OOF)	8.769	-
OOF 블렌딩: 각 모델의 Out-Of-Fold 예측값으로 가중치를 0.05 간격 그리드 탐색하여 RMSE 최소 조합 선택
제출 처리: 예측값을 학습 타깃 범위로 clip, 반올림 미적용
구분	Baseline	Final	개선율
RMSE (CV)	8.807	8.769	0.44%
Kaggle Public Score	8.781	8.734	0.53%
7. Feature Importance (SHAP / Permutation)
중요 변수 TOP 3
학습 시간 (study_hours) — 값이 클수록 점수 상승
출석률 (class_attendance) — 학습 시간과 같은 방향, 영향 범위는 더 작음
수면의 질 (sleep_quality) — poor는 음(−), good은 양(+)의 기여
중요도가 낮은 변수
age, gender, course는 SHAP 값이 거의 0에 수렴
8. Conclusion
학습 시간이 성적과 가장 강하게 연결되며, 통계 분석(ρ=0.77)과 SHAP 결과가 일치했습니다.
대용량 데이터에서는 p-value보다 효과크기가 변수 해석에 더 적절함을 확인했습니다.
부스팅 모델 블렌딩으로 Kaggle Public Score를 0.53% 개선했습니다.
한계
Playground Series는 합성 데이터이므로, 결과를 실제 학생 집단에 일반화하는 데에는 한계가 있습니다.
기대 활용
학습 코칭 시스템
성적 위험군 조기 탐지
개인 맞춤형 학습 전략 수립
보고서
프로젝트 상세 내용은 PDF 보고서를 참고해 주세요
최종 보고서 : Kaggle 시험 점수 예측
통계 분석 코드 : Kaggle_시험점수예측_메인.ipynb
모델링·SHAP 코드 : kaggle_시험점수예측_SHAP_.ipynb
🔗 배지 및 이모지 공식 소스 링크
용도	사이트 이름	링크
배지 생성	Shields.io	https://shields.io/
로고/색상 검색	Simple Icons	https://simpleicons.org/
이모지 검색	Emoji Cheat Sheet	https://github.com/ikatyang/emoji-cheat-sheet
