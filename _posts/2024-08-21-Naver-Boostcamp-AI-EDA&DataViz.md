---
layout: single
title:  "EDA&DataViz Lecture 3"
categories: Naver_Boostcamp_AI
tag: [EDA&DataViz]
toc: true
---
# 3강 정형데이터

### 목차
1. 범주형 데이터
2. 명목형 데이터
3. 순서형 데이터
4. 수치형 데이터
5. 다양한 전처리

## 범주형 데이터

범주형 데이터 - 순서형 / 명목형
수치형 데이터 - 이산형 / 연속형

- 범주형 데이터
	- 명목형 데이터
		- 순서가 상관없이 항목으로 구분되는 데이터
			- 성별, 국가, 과일 종류 등
	- 순서형 데이터
		- 각 값이 우위 등의 순서가 존재하는 데이터
			- 리커트 척도, 영화 별점표 등
		- 수치형인지 순서형인지 고민된다? -> 산술연산의 의미를 둬보자 (별점 4점이 2점보다 2배좋은가?)
		- 경우에 따라 순서형도 수치형처럼 치환해 계산해볼 수 있다. (평균 별점)
	- 집단 간 분석에 용이
		- 분류 기준을 통해 범주 별 통계 지표 확인
			- 국가별 평균 임금, 지하철 노선별 사용자 연령층 분포 등
		- 범주 별 관계 분석에 용이  (추천 알고리즘)
			- 성별에 따른 구매품목 통계 비교, 영화 카테고리 추천 등
		- 그룹 별 어떤 값을 뽑아볼 수 있을까?

### 대푯값

- 대푯값
	- 데이터 집단의 비교를 위해 추출하는 대표 값
		- 평균/총합
			- 이산적인 합산 그리고 전체 갯수로 나눠서 얻는 값
			- 평균이 항상 좋은 것은 아님
		- 기댓값(가중평균)
			- 각 값이 나올 확률에 대해, 또는 중요도에 대해 계산하여 구한 값 (ex. 수능 표준점수)
		- 최빈값
			- 가장 많이 관측된 값
		- 중앙값
			- 정렬된 데이터의 가운데 값
			- 홀수개면 중간, 짝수개면 중앙에서 이웃된 2개 값의 평균으로 계산
		- 사분위값
			- 중앙값을 포함하여 1/4 위치의 값, 3/4 위치의 값
		- 절사평균
			- 관측값의 양끝의 일부를 제외한 값에 대한 평균
			- 예외치가 너무 큰 경우 사용
	- 대푯값 예시
		- 모기업의 임직원 연 임금 평균은 2억. 다만 CEO의 연 임금은 1000억이다.
		  ->중앙값, 사분위값
		- 사과가 100개, 포도 30개, 귤 20개에 대해 평균 구할 수 있나?
		  ->최빈값
		- 심사 결과가 80점 부근의 평가가 다수 있었으나, 악의적인 심사가 0점, 내부자가 100점을 주면
		  ->절사평균
	- 도메인에 따라, 데이터 분포에 따라 대푯값 선택할 수 있고, 이러한 대푯값을 뽑는데 그룹으로 묶는게 중요하다면 범주형 데이터를 사용해 구분하는게 좋음

## 명목형 데이터

- 명목형 데이터
	- 일반적으로는 값이 텍스트로 구분됨
	- 국가 / 혈액형 / MBTI / 과일 / 성별 등
	- pandas에서는 어떻게 진행할 수 있나?
		- groupby(), apply(cunstomFunction)를 통해 그룹별 분석이 가능
	- AI 모델에 넣기 위해서는 수치로 변환 해야 함
- 명목형 데이터 전처리
	- Label Encoding
		- 가장 쉬운 전처리는 1대 1 매핑
			- ```sklearn.preprocessing.OrdinalEncoder()```
			- 국가 [한국, 중국, 일본, 미국] => [0, 1, 2, 3]
		- 주의점
			- 없는 레이블에 대해 미리 전처리 필요 (기본 값 -1)
				- ```sklearn.preprocessing.OrdinalEncoder(encoded_missing_value=-1)```
			- [0, 1, 2]처럼 2개 이상의 레이블을 만들었을 때 자체적인 순서가 존재하게 됨
			  모델에 바이어스가 생길 수 있음
	- One-Hot Encoding
		- 순서 정보를 모델에 주입하지 않기 위한 방법
			- ```sklearn.preprocessing.OneHotEncoder()```
			- [한국, 중국, 일본, 미국] => [1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]
			- [여자, 남자, 무응답] = > [1,0,0],[0,1,0],[0,0,1]
		- 여러 범주에 동시 포함될 수 있는 경우에도 효과적
		- 주의점
			- 변수에 따라 데이터가 커질 수 있고, 학습 속도와 퀄리티에 악영향을 줄 수 있음
			- 인코딩해야 할 데이터가 많은 경우 학습속도에 악영향
			- 인코딩 대상 데이터의 수와 데이터의 차원이 비례하여 커짐
		- 빈도 기반으로 범주를 줄이는 방법 존재
			- 국가 [한국, 그외] 이런식으로
	- Binary Encoding
		- 레이블링을 하고
		- 이진수로 변환하고
		- 이를 컬럼으로 변환
		- 순서 정보를 없애며, 개수가 많은 범주에 대해 효과적이나
		- 범주의 의미가 사라짐
	- Embedding
		- 임베딩 모델을 사용하여 임베딩
	- Hashing
		- 값들의 간격에 대해 램덤하여 값들의 랜덤성을 강하게 줄 수 있으나 바이어스도 강하게 줄 수 있음 
	- 특정 값에 따른 인코딩
		- 해당 범주가 가진 통계값의 사용
		- ex) 빈도수로 한다면 여자가 30명, 남자가 70명 -> [여자, 남자] => [30,70]
		- 범주에 대한 정보와 수치에 대한 정보를 동시에 전달해 효과적인 머신러닝 만들 수 있다는 가정
		- 표본집단의 데이터가 모집단의 데이터와 통계가 일부 다른 경우 잘못된 바이어스를 주입해 결과에 악영향 줄 수 있음 -> 범용적인 모델 만들 때 주의해야 하는 부분
	
## 순서형 데이터

- 순서형 데이터
	- 명목형 데이터에 순서 정보가 추가
	- 앞선 인코딩 방법 모두 가능
	- 범주 또한 그룹별로 묶는 것도 훨씬 용이함
- 순환형 데이터 (Cyclical Data)
	- 순서는 있지만, 순서의 우위가 없는 경우(범주형/수치형 모두 포함)
		- 월, 요일, 각도 등
	- 삼각함수 등 값의 크기에 따라 순환되는 값을 사용할 수도 있음(시계열 등에서도 사용 가능)
	- NeRF 에서는 각도 값의 표현력을 넓히기 위해 한 값을 여러개의 sin, cos값으로 치환해 사용

## 수치형 데이터

- 수치형 데이터
	- 이산형 데이터(Discrete)
		- 일반적으로 정수 형태
		- 인구 수, 제품 수, 횟수
	- 연속형 데이터(Continuous)
		- 일반적으로 실수 형태
		- 키, 몸무게, 온도
	 
	- 구간형 데이터
		- 온도, 시간
		- 기준을 정하면 비율형 데이터로 만들 수 있음
	- 비율형 데이터
		- 인구수, 횟수, 밀도
- 데이터에 대한 해석
	- 대푯값은 모델에게도 인간에게도 언제나 잘못된 정보를 제공할 수 있음
	- 수치형 데이터 분석을 할 때는 단순히 대푯값만 볼 게 아니라 데이터의 분포, 밀도차 등 비교하고 구성을 봐야함
	- 이런 것을 보기 위해 테이블이 아닌 시각화 활용
	- 시각화의 필요성
		- 다양한 요인에 따라 데이터는 여러 정보를 담고 있음
		- 이를 형태로 확인하여 데이터에 대한 인사이트를 도출할 수 있음
			- 왜도(skewness): 치우친 정도
			- 첨도(Kurtosis): 뾰족한 정도
			- 형태
	- 모델이 어떤 형태의 데이터에서 잘 동작하는지 생각해보고 우리의 데이터를 어떻게 가꿔나갈지 생각해보기

## 수치형 데이터 전처리

- 정규화와 표준화
	- 모델에 따라 범위 이슈가 있음
	- 대부분의 알고리즘의 정확도는 수렴 속도에도 영향
	- 정규화(Normalization)
		- 데이터의 범위를 [0,1] 또는 [-1,1]과 같은 특정 범위로 변환
		- 정규화 방법
			- 최솟값이 0, 최댓값이 1로 치환 되어야함
			- 모든 값에 최솟값을 빼주고
			- 각 값에 (최댓값-최솟값)으로 나눔
	- 표준화(Standardization)
		- 데이터의 평균을 0, 표준편차를 1로 만들어, 데이터를 표준 정규 분포 형태로 변환
		- 표준화 방법
			- 각 값에서 평균을 빼주고(전체의 평균을 0으로 만드는 작업)
			- 그 결과를 표준편차로 나눠준다(전체의 표준편차를 1로 만드는 작업)
- 대칭성을 위한 방법들
	- 오른쪽으로 치우친 데이터라면?
		- 큰 값에 값들이 모여있음
		- 큰 값의 구간을 작은 값보다 더 효과적으로 크게 만들자
	- 왼쪽으로 치우친 데이터라면?
		- 작은 값에 값들이 모여있음
		- 작은 값의 구간을 큰 값보다 더 효과적으로 크게 만들자
	- ![[Pasted image 20240821113926.png]]
	- Negative skew
		- 큰 값이 서로 더 차이가 크게 되기 위해선?
			- Square/Power Transformation: 제곱 변환 또는 거듭 제곱
			- Exponential Transformation: 지수 함수
			- 부호에 따라 영향 받을 수 있으니 표준화 등을 통한 전처리 이후 연산하기
	- Positive skew
		- 작은 값이 서로 더 차이가 크게 되기 위해선?
			- Log Transformation: 로그
			- Square-root Transformation: 제곱근
			- 로그는 0이상의 실수, 제곱근은 양수라는 조건이 있으므로 전처리 이후 연산하기
	- Positive Skewness: Box-Cox Transformation
		- 범용적인 Log Transformation 변형 방법론
			- 대략적으로 값에 따라 다음 변환 형태를 가짐
				- 0인 경우, Log 변환
				- 0.5인 경우, 제곱근 변환
				- 1인 경우, 유지
				- -1인 경우, 역수 변환
				- ![[Pasted image 20240821114624.png]]
