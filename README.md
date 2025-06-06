# Brazilian E-Commerce Public Dataset by Olist 분석
- 데이터 셋 : https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce/data

## 디렉토리 구조
```
├── 1_전처리
│   └── Brazilian_E_Commerce_Public_Dataset_by_Olist_1전처리완료코드_merge완료.ipynb
├── 2_EDA
│   └── Brazilian_E_Commerce_Public_Dataset_by_Olist_2EDA최종.ipynb
├── 3_결과
│   └── Brazilian_E_Commerce_Public_Dataset_by_Olist_3결과(C3).ipynb
└── README.md
```

---

## 1. 전처리

### 사용한 데이터

| 파일명                                 | 설명                          |
|----------------------------------------|-------------------------------|
| customers.csv                          | 고객 정보                     |
| sellers.csv                            | 셀러 정보                     |
| order_items.csv                        | 주문 상세                     |
| products.csv                           | 상품 정보                     |
| geolocation.csv                        | 우편번호별 위경도             |
| orders.csv                             | 주문 정보                     |
| order_reviews.csv                      | 리뷰 정보                     |
| order_payments.csv                     | 결제 정보                     |
| product_category_name_translation.csv | 카테고리 번역 정보            |

### 주요 처리 내용

| 항목             | 설명                                                                  |
|------------------|-----------------------------------------------------------------------|
| 우편번호 정제     | zip code prefix를 `zfill(5)`로 5자리 통일                             |
| 고객 중복 확인    | `customer_unique_id` 기준으로 여러 배송지를 쓰는 고객 구분             |
| 좌표 정제         | 브라질 외 위경도 제거 (GeoJSON 경계 기준)                             |
| 도시명 정리       | 동일한 지역명이 여러 표기로 들어온 경우 수동으로 통일                   |
| 데이터 병합       | 모든 테이블을 병합한 `merge_df` 생성 (모델링 및 분석용 기준 테이블)     |

---

## 2. EDA

### 카테고리, 지역, 셀러 중심 분석

| 항목                      | 설명                                                             |
|---------------------------|------------------------------------------------------------------|
| 카테고리별 제품 수         | 등록 상품 수 기준으로 경쟁 치열도 예측 지표로 활용 가능           |
| 카테고리별 단가 분석       | 평균/중앙값 단가를 기준으로 고가/저가 상품군 파악                |
| 카테고리별 총매출 분석     | 실제 판매 기준으로 수요가 많은 카테고리 구분 가능                 |
| 셀러 수 분석               | 전체 셀러 수 및 지역별 셀러 밀집도 확인                           |
| 지역별 고객 수 분석        | 브라질 27개 주 기준 고객 분포 시각화                             |
| 지역+카테고리 조합 분석    | 특정 지역에서 유리한 카테고리 조합 추천                          |

---

## 3. 결과

### 카테고리 경쟁도 분석 (Red Ocean / Blue Ocean)

#### 분석 목표

| 항목                         | 설명                                                                 |
|------------------------------|----------------------------------------------------------------------|
| 분석 단위                    | 카테고리 × 연도-분기                                                 |
| 주요 지표                    | 총 주문건수, 총 매출액, 판매 참여 셀러 수                           |
| 파생 지표                    | Seller 1인당 평균 주문건수 / 평균 매출액 계산                        |
| 분류 기준                    | 값이 낮을수록 경쟁 심함 (Red Ocean), 높을수록 경쟁 완화 (Blue Ocean) |

#### 분석 방법

- 각 카테고리의 **분기별 TotalOrder / TotalRevenue / Seller 수**를 기준으로,
  - `TotalOrder_NS = 총 주문건수 / 셀러 수`
  - `TotalRevenue_NS = 총 매출액 / 셀러 수` 계산
- 위 값들을 **0~1로 정규화**한 후 구간을 나눠 시각화
- 시각화는 Area Chart + 색상 그라데이션을 통해 Red~Blue Ocean의 정도를 표현

#### 예측 모델

| 항목          | 설명                                                       |
|---------------|------------------------------------------------------------|
| 사용 모델      | 선형회귀 (Linear Regression)                               |
| 예측 대상      | 다음 분기의 총 주문건수, 총 매출액, 셀러 수                |
| 파생 예측 지표 | Seller 1인당 평균 주문건수 및 매출 → Red/Blue Ocean 추정 |

---

### 주요 인사이트

- **같은 카테고리라도 시기별로 경쟁 환경이 바뀜** 분기별 추세 분석 가능
- **Red Ocean → Blue Ocean 전환 시기** 를 예측하면 셀러 진입 타이밍 파악 가능
- 이 방법은 **2025년 이후 신규 데이터셋** 에도 그대로 확장 가능

---

## 결론 및 활용 방향

- 이 분석은 Olist 데이터를 기반으로 **셀러 수/매출/상품 수** 등 주요 지표를 바탕으로 경쟁도와 시장성을 판단하고,
- **카테고리와 지역을 조합해 신규 셀러에게 유리한 진입 조건**을 제안하는 것이 목적이다.
- 이후에는 이 분석 결과를 바탕으로 **다음 분기 수요 예측**, **셀러 전략 수립**, **신규 셀러 유치 전략**으로 확장 가능하다.

---

## 팀원 및 역할 분담

| 작업 항목      | 참여자                     |
|----------------|-----------------------------|
| 전처리         | 김경호, 송은희, 최규선      |
| EDA 분석       | 김경호, 송은희, 최규선      |
| 결과 분석      | 손다혜 |
| PPT 자료 작성  | 김경호, 송은희, 최규선, 손다혜 |

## 프로젝트 회고

이번 프로젝트는 처음부터 좀 열심히 해보고 싶었고, 그래서 어려운 데이터셋을 골랐다.  
의욕은 있었는데, 경험이 부족하다 보니 이게 시간 안에 끝낼 수 있을지, 난이도가 어느 정도일지 잘 몰랐던 것 같다.  
솔직히 ‘할 수 있는 범위’보단 ‘내가 해보고 싶은 것’에 가까웠다.

그래도 조원들이랑 나눠서 조금씩 차곡차곡 해나가니까 결국 끝까지 만들 수 있었고, 결과물도 만족스럽다고 생각한다.  
특히 처음엔 막막했던 데이터 전처리부터 EDA, 추천 로직까지 전부 직접 손으로 만들었다는 게 의미 있었다.

다음엔 시간도 좀 고려해서, 계획적으로 역할 나누고 효율적으로 진행해보고 싶다.  
이번 팀원들이랑은 유익한 시간이었다.  
힘든 것도 있었지만 덕분에 재밌는 경험이었다고 생각한다.

