# Song-Title-Suggestion-Model

Mini project6



## 

## 한글 가사 기반 노래 제목 추천 모델 만들기

<br>

---

### 목   차

1. 프로젝트 기획(Proposal)
2. 전처리(Preprocessing)
3. 모델 학습(Train Model)
4. 결과 및 피드백(Implement)

<br>

---

## 1. 프로젝트 기획

<br>

### **1-1. 프로젝트 주제**


텍스트 모델을 활용해 가사를 입력값으로 넣어주면 노래 제목을 추천해주는 시스템을 만든다.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092975836723937280/image.png?width=1195&height=671" width="680">



<br>
<br>


### **1-2. 사용한 데이터**

음원 사이트에서 크롤링을 통해 수집

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092976936499826838/image.png?width=1197&height=671" width="680">

<br>
<br>


### **1-3. 프로젝트 기간 및 팀원 역할**

(1) 프로젝트 수행 기간 : 2022.9.13 ~ 9.20

(2) 팀 구성 및 역할

+ 김용재(조장) : 크롤링(블로그), Transformer설계, 디버깅
+ 국승용 : 크롤링(블로그), KoBART설계, PPT, 디버깅
+ 박혜정 : LSTM Attention 설계, 크롤링(멜론), 전처리, 노션정리, 디버깅
+ 한민재 : KoGPT2, 모델 핸들링, 코드정리, 디버깅
+ 황성연 : 크롤링(벅스), 전처리, KoBERT 설계, 디버깅


<br>
<br>

---

## 2. 전처리

<br>

### **2-1. 전처리 정리**

(1) 기본 전처리
+ 가사와 제목의 영어 제거
+ Tokenizer를 활용해 토큰화 및 임베딩
+ 토큰 길이 조절

<br>

(2) 추가 전처리
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092982463984316446/image.png" width="680">

<br>
<br>

### **2-2. 추가 전처리를 통한 기대 효과**

+ 학습 데이터의 이상치를 제거하여 보다 개선된 제목 추천 기대
+ 데이터 크기를 줄임으로써 한정된 컴퓨터 자원으로도 효율적인 모델 학습이 가능하도록 함

<br>
<br>

---

## 3. 모델학습

<br>

### **3-1. 사용 모델**

(1) 직접 설계
+ LSTM Attention
+ Transformer



(2) 사전학습 모델
+ KoBART (cosmoquester/bart-ko-small)
+ KoBERT (SKTBrain/koBERT)
+ KoGPT2 (skt/kogpt2-base-v2)


<br>
<br>


### **3-2. Best Model 선정**

(1) 선정된 모델 : KoGPT2

(2) 선정이유 : 사전 학습된 트랜스포머의 디코더를 장착한 모델인 GPT2를 활용해 노래 제목 추천에 적합한 추상적 문자생성 모델이 가장 좋은 결과를 보여줌.

<img src="https://media.discordapp.net/attachments/1002189622912221250/1092989205375877260/image.png" width="680">


<br>
<br>

---

## 4. 결과 및 피드백

<br>

### **4-1. 모델 학습 결과**

학습된 GPT2를 활용한 제목 예측
<img src="https://media.discordapp.net/attachments/1002189622912221250/1092995155566534846/image.png" width="680">


### **4-2. 모델 학습 결과 분석**

(1) 한계점
+ 만족스러운 제목을 얻기 위해서는 1~10 차례 예측 시도 요구됨
+ 학습 데이터에 사랑에 관한 노래가 많기 때문에, 추천된 노래 제목도 사랑에 대한 내용이 많음. → 다양한 주제의 균형된 데이터 추가 학습 필요
+ 전처리를 통해 영어를 제외했기 때문에 제외된 영어 토큰 앞뒤의 문맥 정보 일부 훼손 → 다국어 학습 모델 사용 또는 영어 포함 전처리 방안 모색

<br>

(2) KoGPT2 이외 모델의 낮은 성능 원인
+ LSTM, Transformer : Seq2Seq를 통해 생성요약을 시도했지만 여러 사전학습 모델과 비교했을 때, 학습하는 데이터의 수가 적기 때문에 뛰어난 성능을 보여주기 힘듬.
+ KoBERT : 추출 요약에는 적절할 수 있지만, 생성 요약을 해야 하는 제목 추출 모델에서는 인코더만 사용된 BERT가 아닌 디코더를 장착한 GPT 계열의 모델을 활용하는 것이 좋은 것으로 판단