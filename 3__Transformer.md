# 🧠 Transformer Architecture

## 📚 목차

1. Transformer는 왜 등장했을까? 
2. Transformer란?
3. 전체 구조: Encoder & Decoder
4. Transformer의 핵심 구성요소
    - Positional Encoding
    - Self-Attention
    - Cross-Attention
    - Multi-Head Attention
    - Feed Forward Layer
    - Add & Norm
5. Transformer 성능 및 트렌드

</br> </br>

---

## ✨ 1. Transformer는 왜 등장했을까?

- 이전에는 자연어 처리에서 **RNN, LSTM, GRU** 같은 순환 신경망이 주로 쓰였어요.
- 하지만 이 방식은 **입력 순서를 따라가야 해서 병렬처리가 어렵고**, **긴 문장**일수록 **기억을 잃는 문제**가 있었습니다.
- 이 문제를 해결하기 위해 **Attention Mechanism**이 등장했고,
- 그 Attention을 구조 전체에 적용한 모델이 바로 **Transformer**입니다!

</br> </br>

---

## 🚀 2. Transformer란?

> Transformer는 입력 문장을 **한 번에 다 보고**, 각 단어 간의 관계를 파악해서 번역하거나 문장을 생성하는 **완전한 Attention 기반 구조**입니다.

- RNN처럼 순차적으로 처리하지 않고, **전체 문장을 동시에 처리**합니다.


비유 🔎  
- RNN: 책을 한 줄씩 순서대로 읽는 학생  
- Transformer: 책 전체를 동시에 펼쳐놓고, 필요한 부분을 자유롭게 참고하는 학생  


</br> </br>

---

## 🏗️ 3. 전체 구조: Encoder & Decoder

<img width="454" height="517" alt="image" src="https://github.com/user-attachments/assets/2758306e-534a-450c-bcac-dc4288efb2b7" />


Transformer는 **Encoder-Decoder** 구조를 기반으로 합니다.

- **Encoder**: 입력 문장을 받아 의미(정보)를 압축함  
- **Decoder**: 압축된 정보를 활용해 출력 문장을 생성
  
`전체적으로 입력 내용을 압축시킨 다음, 다시 펼치는 느낌`


### 🧩 Encoder
- 입력 문장을 처리하는 파트입니다.
- 여러 개의 층으로 쌓여 있고, 각 단어 간의 관계를 **Attention**으로 학습합니다.

### 🧩 Decoder
- Encoder의 출력(=압축된 정보)을 받아서, 새로운 문장(예: 번역문)을 생성하는 파트입니다.
- 디코더도 여러 층으로 되어 있으며, **Attention** 구조를 갖습니다.


### 🧩 압축된 정보
- Encoder 와 Decoder 사이에 있는 압축된 feature vector
- Encoder 의 출력이면서, Decoder 의 입력
- 여러 가지 명칭이 존재
  - Context Vector
    - 입력 문장을 하나의 압축된 벡터로 표현했기 때문에, 문맥(context)을 고려한 벡터라고 할 수 있다.
  - Latent Vector
    - Latent space 는 잠재공간을 의미한다.
    - 우리가 직관적으로 보는 정보가 아니라 압축된 공간에 숨겨져 있는 정보이기 때문에 Latent vector 라고 할 수 있다.
    - 예를 들면 원래의 정보가 [국어, 영어, 수학, 역사, 과학] 에 대해 5개의 점수를 나타낸 vector 였다고 가정해보자.
    - 그러면 latent vector는 [국어,영어,역사의 평균 점수] 와 [수학, 과학의 평균 점수]로 정보를 압축한 vector가 될 수 있다.
  - Feature Vector
    - 넓은 의미에서 압축된 정보도 일종의 feature vector 이다. 다만 정보를 압축했기 때문에 어떤 특징(feature)을 묘사할지는 재구성될 수 있다.

  
</br> </br>

> **이미지** 도메인에서 쓰인 Encoder-Decoder 구조
<img width="1731" height="271" alt="image" src="https://github.com/user-attachments/assets/5417fbc8-62f1-4369-9df2-ece776f8bbfd" />

> **텍스트** 도메인에서 쓰인 Encoder-Decoder 구조
<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/48590b52-cd21-4391-98d7-9b0186bd7bc9" />


</br> </br>

---

## 🧱 4. Transformer의 핵심 구성요소

</br> 

### 🔢 4-1. Positional Encoding
> 각 단어마다 그 단어의 위치 정보까지 추가

Transformer는 단어를 한꺼번에 보기 때문에, **단어 순서 정보**가 사라집니다.  
그래서 각 단어에 **위치 정보를 더해주는 역할**을 합니다.  
→ `나는`과 `는나`는 글자들은 같아도 위치가 다르므로 의미도 달라져야 하니까요!

</br> 

### 🌀 4-2. Self-Attention
> 하나의 시퀀스 내의 단어들이 서로 어떤 관련이 있는지 계산

- "나는 사과를 먹었다"에서 "사과"와 "먹었다"의 연결 관계를 파악  
- 각 단어가 다른 단어를 **참고(참조)** 하며 표현을 강화

예: "나는 학교에 간다"라는 문장에서 "간다"는 "학교"와 더 관련이 깊죠?  
Self-Attention은 **하나의 문장 내에서** 이렇게 **관련성 점수**를 계산해서, **중요한 단어에 더 집중**하게 합니다.

</br> 

### ↔️ 4-3 Cross-Attention
> Decoder가 현재 번역되는 단어와, Encoder가 인코딩한 전체 입력 문장을 연결
 
- 즉, 번역할 때 원문 문장 전체를 다시 참고  


```python
Self-Attention 과 Cross-Attention 의 차이
: 입력된 영어 문장을 한국어 문장으로 번역하는 작업을 가정해봅시다. (예를 들어, "I am a student" → "나는 학생이다.")


- Encoder 에서의 Self-Attention
  - "I am a student."라는 문장 안에서 "student"와의 상관관계를 계산
- Decoder 에서의 Self-Attention
  - "나는 학생이다."라는 문장 안에서 "학생"과의 상관관계를 계산
- Encoder와 Decoder 사이의 Cross-Attention
  - "I am a student."라는 문장 안에서 "학생"과의 상관관계를 계산
```
 
</br> 

### 🧠 4-4. Multi-Head Attention
> Attention을 여러 번 병렬로 다양하게 수행하는 구조

- 여러 시각에서 문장을 해석하는 것과 같아요. 더 풍부한 표현이 가능해집니다.
- (예시) 어떤 Head는 문법에 집중, 다른 Head는 감정에 집중 등

</br> 


### ⚙️ 4-5. Feed Forward Layer
- Attention 결과를 바탕으로 **각 단어를 독립적으로 처리하는 작은 MLP(Multi-Layer Perceptron)** 입니다.
- MLP 란 Multi-Layer Perceptron 을 의미합니다.
  - Perceptron은 인간의 뇌 구조에서 하나의 뉴런을 모방한 것입니다.
  - 이러한 perceptron을 여러 층으로 겹겹이 쌓아놓으면 MLP(Multi-Layer Perceptron)라고 합니다.
  - 주로 여러 번의 비선형적인 연산을 통해, 의미 있는 특징을 추출할 때 쓰입니다.

</br> 

### 🧩 4-6. Add & Norm (잔차 연결과 정규화)
> Residual connection, 잔차 연결 (Add)

- 각 Layer 블록은 **잔차 연결(residual connection)** 을 통해 원래 입력을 더합니다.
  - 원래는 y = f(x) 이지만, 잔차 연결을 통해 y = x + f(x) 로 계산합니다.
  - 이를 통해 학습 안정성이 강화됩니다.
    
> Layer normalization, 정규화 (Norm)

- 그리고 **Layer Normalization**으로 학습 안정성과 속도를 향상시킵니다. (학습 시 값이 튀지 않도록 정규화)

> 🛡️ 비유

Residual은 “원래 답안을 지우지 않고 메모를 추가”하는 것,  
Normalization은 “모든 메모의 크기를 일정하게 정리”하는 것과 같음.


</br> </br>

---

## 📈 5. Transformer 성능 및 트렌드

### ✅ 성능 및 효과
- **빠름**: 병렬 연산 덕분에 GPU 성능 활용이 뛰어남
- **정확함**: 긴 문장도 잘 처리하고, 번역 품질 향상
- **범용성**: NLP뿐 아니라 음성, 이미지까지 다양한 분야에 적용

### 📊 트렌드
- BERT, GPT, T5 등 대부분의 최신 모델은 Transformer 구조 기반
- 심지어 ViT처럼 **이미지 처리 모델**도 Transformer 구조를 사용
- 현재 AI의 **표준 구조**라고 할 수 있음

---

