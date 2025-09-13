# 🏠 서울시 집값 예측 프로젝트  

## 📌 프로젝트 개요  
- **주제**: 서울시 아파트 매매가격 예측 (2023.07 ~ 2023.10)  
- **목표**: 주어진 학습 데이터와 외부 데이터를 활용해 **RMSE 최소화**  
- **기간**: 2025년 9월 1일 ~ 9월 12일  

---

## 📂 프로젝트 구조  
house-price-prediction/
│── data/
│ └── sample.csv # 작은 예시 데이터만
│
│── notebooks/
│ ├── 01_EDA.ipynb # 탐색적 데이터 분석
│ ├── 02_Feature_Engineering.ipynb # 파생변수 생성
│ └── 03_Modeling_LGBM.ipynb # LightGBM 학습 실험
│
│── src/
│ ├── preprocess.py # 데이터 전처리 코드
│ ├── feature_engineering.py # 파생변수 생성 코드
│ ├── train.py # 학습 실행 (LightGBM, RF)
│ └── utils.py # 공통 함수
│
│── results/
│ ├── figures/ # 시각화 이미지
│ └── submissions/ # 제출 파일 (샘플만)
│
│── requirements.txt # 필요 라이브러리
│── README.md # 프로젝트 설명
│── .gitignore # 버전관리 제외 규칙




---

## 📊 데이터 설명  

- **기본 데이터**  
  - `train.csv`, `test.csv`: 아파트 거래 내역과 매매가  

- **외부 데이터**  
  - 한국은행 지표: 기준금리, 국고채(3년), 정부대출금리 등  
  - 지하철 정보: 반경 내 지하철 개수, 거리  

- **파생변수 생성**  
  - `top_5_pct`: target 값 기준 상위 5% 여부  
  - `luxury_apt`: 연평균 20억 이상 아파트 여부  
  - `계약계절`, `이사철여부` 등 시계열 변수  

---

## ⚙️ 모델링 과정  

- **전처리**  
  - 결측치 처리, 이상치 제거 (단, 고가 아파트는 유지)  
  - 카테고리 인코딩 (Label Encoding, category 타입 변환)  

- **교차검증**  
  - `TimeSeriesSplit(n_splits=5)`  

- **모델**  
  - **LightGBM**: GPU 활용, 파생변수 포함  
  - **RandomForest**: 가중치 실험 (고가 아파트 중요도 반영)  
  - **LSTM (실험)**: 시계열 특성 반영  

- **평가지표**  
  - RMSE (Root Mean Squared Error)  

---

## 📈 성능 비교  

| 모델               | RMSE (검증) | 특징 |
|--------------------|-------------|------|
| Baseline (평균값) | 40,000      | 단순 평균 예측 |
| RandomForest       | 20,000      | 기본 피처 사용 |
| LightGBM           | 18,000      | 외부데이터 + 파생변수 |
| LSTM (실험)        | 19,500      | 시계열 특성 반영 |

---

## 📉 시각화  

- 금리와 집값의 음의 상관관계  
- 지하철 개수/거리와 평균 집값 관계  
- 고가 아파트 분포  

(→ `results/figures/`에 그래프 저장)  

---

## 🚀 실행 방법  

```bash
# 1. 라이브러리 설치
pip install -r requirements.txt

# 2. 전처리
python src/preprocess.py

# 3. 파생변수 생성
python src/feature_engineering.py

# 4. 학습 (LightGBM)
python src/train.py --model lgbm

# 5. 학습 (RandomForest)
python src/train.py --model rf

