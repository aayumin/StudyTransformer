# 🧠 Transformer Architecture 완전 정복

## 📚 목차

1. Transformer는 왜 등장했을까? (배경 간략 요약)
2. Transformer란 무엇인가?
3. 전체 구조: Encoder & Decoder
4. Transformer의 핵심 구성요소
    - Positional Encoding
    - Self-Attention
    - Cross-Attention
    - Multi-Head Attention
    - Feed Forward Layer
    - Add & Norm (Residual + Normalization)
5. Transformer의 효과와 현재 트렌드

---

## ✨ 1. Transformer는 왜 등장했을까?

- 이전에는 자연어 처리에서 **RNN, LSTM, GRU** 같은 순환 신경망이 주로 쓰였어요.
- 하지만 이 방식은 **입력 순서를 따라가야 해서 병렬처리가 어렵고**, **긴 문장**일수록 **기억을 잃는 문제**가 있었습니다.
- 이 문제를 해결하기 위해 **Attention Mechanism**이 등장했고,
- 그 Attention을 구조 전체에 적용한 모델이 바로 **Transformer**입니다!

---

## 🚀 2. Transformer란?

> Transformer는 입력 문장을 **한 번에 다 보고**, 각 단어 간의 관계를 파악해서 번역하거나 문장을 생성하는 **완전한 Attention 기반 구조**입니다.

- RNN처럼 순차적으로 처리하지 않고, **전체 문장을 동시에 처리**합니다.
- 구조는 Encoder-Decoder 방식이며, **입력 → 출력**을 단계적으로 바꿔줍니다.

비유 🔎  
- RNN: 책을 한 줄씩 순서대로 읽는 학생  
- Transformer: 책 전체를 동시에 펼쳐놓고, 필요한 부분을 자유롭게 참고하는 학생  


---

## 🏗️ 3. 전체 구조: Encoder & Decoder


Transformer는 **Encoder-Decoder** 구조를 기반으로 합니다.

- **Encoder**: 입력 문장을 받아 의미(정보)를 인코딩  
- **Decoder**: 인코딩된 정보를 활용해 출력 문장을 생성  



### 🧩 Encoder
- 입력 문장을 처리하는 파트입니다.
- 여러 개의 층으로 쌓여 있고, 각 단어 간의 관계를 **Self-Attention**으로 학습합니다.

### 🧩 Decoder
- Encoder의 출력을 받아서, 새로운 문장(예: 번역문)을 생성하는 파트입니다.
- 디코더도 여러 층으로 되어 있으며, **Self-Attention + Cross-Attention** 구조를 갖습니다.



[이미지 part: 인코더-디코더 구조를 블록 다이어그램처럼 그린 그림. 입력 문장이 왼쪽에서 들어와 디코더를 거쳐 번역 결과로 나오는 모습.]

---

## 🧱 4. Transformer의 구성요소

### 🔢 4-1. Positional Encoding
> 각 단어마다 그 단어의 위치 정보까지 추가
Transformer는 단어를 한꺼번에 보기 때문에, **단어 순서 정보**가 사라집니다.  
그래서 각 단어에 **위치 정보를 더해주는 역할**을 합니다.  
→ `나는`과 `는나`는 단어는 같아도 위치가 다르므로 의미도 달라져야 하니까요!

### 🌀 4-2. Self-Attention
> 입력 시퀀스 내의 단어들이 서로 어떤 관련이 있는지 계산  
- "나는 사과를 먹었다"에서 "사과"와 "먹었다"의 연결 관계를 파악  
- 각 단어가 다른 단어를 **참고(참조)**하며 표현을 강화

예: "나는 학교에 간다"라는 문장에서 "간다"는 "학교"와 더 관련이 깊죠?  
Self-Attention은 이런 **관련성 점수**를 계산해서, **중요한 단어에 더 집중**하게 합니다.

### ↔️ 4-3 Cross-Attention
- **Decoder**에서 사용되는 Attention  
- Decoder가 현재까지 번역된 단어와, Encoder가 인코딩한 전체 문장을 연결  
- 즉, 번역할 때 원문 문장 전체를 다시 참고  


### 🧠 4-4. Multi-Head Attention
> Self-Attention을 여러 번 병렬로 다양하게 수행하는 구조

- 여러 시각에서 문장을 해석하는 것과 같아요.
- 예: 어떤 Head는 문법에 집중, 다른 Head는 감정에 집중 등
- 이 덕분에 더 풍부한 표현이 가능해집니다.

### ⚙️ 4-5. Feed Forward Layer
- Attention 결과를 바탕으로 **각 단어를 독립적으로 처리하는 작은 MLP(완전연결 신경망)**입니다.
- 표현을 비선형적으로 더 복잡하게 변환합니다.

### 🧩 4-6. Add & Norm (잔차 연결과 정규화)
- 각 Layer 블록은 **잔차 연결(residual connection)**을 통해 원래 입력을 더합니다.
  - 원래는 y = f(x) 이지만, 잔차 연결을 통해 y = x + f(x) 로 계산합니다.
  - 이를 통해 학습 안정성이 강화됩니다.
- 그리고 **Layer Normalization**으로 학습 안정성과 속도를 향상시킵니다. (학습 시 값이 튀지 않도록 정규화)

🛡️ **비유**
Residual은 “원래 답안을 지우지 않고 메모를 추가”하는 것,  
Normalization은 “모든 메모의 크기를 일정하게 정리”하는 것과 같음.


---

## 🎯 5. Self-Attention vs Cross-Attention

| 구분             | 설명 |
|------------------|------|
| **Self-Attention** | 하나의 문장 내에서 단어들 간의 관계를 파악 |
| **Cross-Attention** | 인코더 출력과 디코더 입력 간의 관계를 파악 |

예:  
- Self → "나는 학교에 간다" 안에서 단어들끼리 주고받음  
- Cross → 원문(입력)과 번역문(출력) 사이에서 관계를 형성

---

## 📈 6. Transformer의 효과와 트렌드

### ✅ 성능 및 효과
- **빠름**: 병렬 연산 덕분에 GPU 성능 활용이 뛰어남
- **정확함**: 긴 문장도 잘 처리하고, 번역 품질 향상
- **범용성**: NLP뿐 아니라 음성, 이미지까지 다양한 분야에 적용

### 📊 트렌드
- BERT, GPT, T5 등 대부분의 최신 모델은 Transformer 구조 기반
- 심지어 ViT처럼 **이미지 처리 모델**도 Transformer 구조를 사용
- 현재 AI의 **표준 구조**라고 할 수 있음

---

