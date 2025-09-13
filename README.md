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
