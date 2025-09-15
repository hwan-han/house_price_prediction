# 🏠 서울시 집값 예측 프로젝트  

## 📌 프로젝트 개요  
- **주제**: 서울시 아파트 매매가격 예측 (2023.07 ~ 2023.10)  
- **목표**: 주어진 학습 데이터와 외부 데이터를 활용해 **RMSE 최소화**  
- **기간**: 2025년 9월 1일 ~ 9월 12일  

---
```
## 📂 프로젝트 구조  
house-price-prediction/
│── .gitignore
│── data/
│ ├── train.csv, test.csv # 용량 문제로 test.csv만 존재
│ ├── bank.csv # 연도별 수치
│ ├── subway_feature.csv # 지하철 데이터
│ └── merged.csv # 전처리된 데이터(용량문제)
│
│── codes/
│ ├── ML_Advanced.ipynb # 베이스라인 코드
│ ├── ML_BANK.ipynb # 은행데이터 추가 코드
│ └── ML_subway.ipynb # 지하철 데이터 추가 코드
│
│── src/
│ ├── requirements.txt 
│ └── utils.py # 필수 라이브러리
│
│── outputs/
│ ├── figures/ # 시각화 이미지
│ └── submissions/ # 제출 파일
```



---

## 📊 데이터 설명  

- **기본 데이터**  
  - `train.csv`, `test.csv`: 아파트 거래 내역과 매매가  

- **외부 데이터**  
  - 한국은행 지표(ecos): 한국은행기준금리, 국고채(3년), 정부대출금리, 서울지가변동률   
  - 지하철 정보: 1km 반경 내 지하철 개수, 거리  

- **파생변수 생성**  
  - `top_5_pct`: target 값 기준 상위 5% 여부 - target값을 이용하는 도중 문제 발견 
  - `luxury_apt`: 연평균 20억 이상 아파트 여부 - target값을 이용하는 도중 문제 발견
  - `계약계절`, `이사철여부` 등 시계열 변수
  - `전용면적`: 4분위로 구간을 나눈 후 구간별 평균값 학습
  - `금리`: 당해 년도가 아닌 2년 후 값에 적용
  - `지하철역`: 역과의 거리에 따라 가중치 부과 

---

## ⚙️ 모델링 과정  

- **전처리**  
  - 카카오API를 이용한 위치 정보 정확히 맵핑 + 외부데이터 결측치 없이 추출 + 결측치가 많은 컬럼과 행 제거
  - 데이터 구조상 이상치 처리 과정 불필요
  - 카테고리 인코딩 (Label Encoding, category 타입 변환)  

- **교차검증**  
  - `TimeSeriesSplit(n_splits=5) CrossValidation`  

- **모델**  
  - **LightGBM**: GPU 활용, 파생변수  
  - **RandomForest**: 가중치 실험 (고가 아파트 중요도 반영)
  - **XGBoost**: GPU 활용, 파생변수

- **평가지표**  
  - RMSE (Root Mean Squared Error)  
<img width="412" height="140" alt="image" src="https://github.com/user-attachments/assets/152c2735-fe2a-448f-9209-fbfbf5d04c7f" />

---

## 📈 성능 비교  

| 모델                | RMSE (검증)  | 특징 |
|--------------------|-------------|------|
| Baseline (평균값)    | 48,385      | 베이스라인 코드 rmse |
| RandomForest       | 17,328      | 외부데이터 + 파생변수 + 하이퍼파라미터 최적화 |
| LightGBM           | 15,159      | 외부데이터 + 파생변수 + 하이퍼파라미터 최적화 |
| XGBoost            | 14,660      | 외부데이터 + 파생변수 + 하이퍼파라미터 최적화 |

---

## 📉 시각화  

- 금리와 집값의 상관관계  
- 지하철 개수/거리와 평균 집값 관계  
- 전용면적과 집값 관계
- 변수 중요도  

(→ `outputs/figures/`에 그래프 저장)  

---
