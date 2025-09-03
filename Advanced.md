# 🧠 최신 Transformer 모델 학습자료

## 📚 목차

1. Transformer 간단 복습
2. Transformer의 확장: 다양한 도메인 적용
3. 🌐 언어 모델 시리즈
   - BERT
   - GPT
4. 🖼️ 이미지 모델 (Vision Transformer)
5. 🧠 멀티모달 모델
   - CLIP
   - Flamingo
   - Gemini
   - GPT-4o
6. 📋 멀티모달 주요 Task 
7. 💡 용어 정리


</br> </br> </br>

---

## 1. 🔁 Transformer 간단 복습

Transformer는 2017년 구글이 발표한 모델로, **기존 RNN의 한계를 극복**하기 위해 만들어졌어요.  
RNN은 순차적으로 데이터를 처리했기 때문에 속도와 정보 전달력에서 한계가 있었죠.

Transformer는 **모든 단어를 동시에 고려하는 Attention** 구조를 통해 병렬처리 가능하고, 긴 문장도 잘 이해하게 되었어요.

---

## 2. 🌱 Transformer의 확장: 다양한 도메인 적용


처음엔 자연어 처리(NLP)에만 쓰였지만, 이후 이미지, 오디오, 비디오, 로봇 제어 등 다양한 분야로 확장되었어요.  

Transformer는 이제 모든 인공지능 분야에서 **기본 골격**처럼 사용되고 있어요.

이후 설명할 ViT, CLIP, GPT-4o 같은 모델들이 모두 Transformer 기반이에요.

</br> </br> </br>

---

## 3. 🌐 언어 모델 시리즈

</br>

### ✅ BERT (2018, Google)

- **B**idirectional **E**ncoder **R**epresentations from **T**ransformers
- 문장의 좌우 **양방향 문맥**을 모두 이해하는 모델 (좌 → 우 / 우 → 좌 동시에)
- **Masked Language Modeling (MLM)**: 입력 문장에서 일부 단어를 [MASK] 처리하고, 나머지 부분을 통해 [MASK]를 예측

> 📖 예시: "나는 [MASK]를 먹었다" → "사과", "학교", "컴퓨터" 등을 예측


<img width="664" height="232" alt="image" src="https://github.com/user-attachments/assets/687a6f66-091d-462c-bb85-38e86cbf11d4" />

</br> </br>

- 구조적으로는 **Transformer의 Encoder만 사용**함  
  → **이해 중심(task)** 작업에 특화 (문장 분류, 문장 유사도, 개체명 인식 등)

> 📦 비유: 전체 문장을 앞뒤로 동시에 훑으며 정답을 추리하는 독해 고수

<img width="431" height="530" alt="image" src="https://github.com/user-attachments/assets/5dca5e0d-a006-4701-9b9b-f4798d4e54d6" />



</br>

---

### ✅ GPT (2018, OpenAI)

- **Generative Pre-trained Transformer**
- 왼쪽에서 오른쪽으로 순차적으로 단어를 예측  
  → **Autoregressive 방식**

> 📖 예시: "오늘은 날씨가" → 다음 단어: 맑다 / 흐리다 / 덥다 등

</br> </br>

- 구조적으로는 **Transformer의 Decoder만 사용**  
  → **생성 중심(task)** 작업에 특화 (텍스트 생성, 요약, 번역, 질문답변 등)

> 📦 비유: 지금까지 말한 내용을 바탕으로 다음 문장을 예측하는 소설가

<img width="367" height="575" alt="image" src="https://github.com/user-attachments/assets/5f2578cb-dd96-46b1-a2a2-66026c8c14d6" />


</br> </br> </br>

---

## 4. 🖼️ 이미지 모델 - Vision Transformer (ViT)

### ✅ ViT (2020, Google)

- **이미지를 패치로 쪼개어 Transformer에 입력**
- 예: 512x512 이미지를 32x32로 자르면 16x16 = 총 256개의 패치 → 각 패치는 1개의 토큰처럼 사용
- 각 패치는 flatten(2d 이미지를 1차원으로 펼침) 후 Linear Projection(특징을 추출함)을 거쳐 d-차원 벡터로 변환됨
- 위치 정보 보완을 위해 **Positional Encoding** 추가

> 📦 비유: 큰 퍼즐을 조각으로 잘라서 각각 의미를 이해한 후, 조합해서 전체 의미를 파악함

<img width="1205" height="223" alt="image" src="https://github.com/user-attachments/assets/681dc49c-d319-4306-b0d1-def41ea3a203" />


</br> </br> </br>

---

## 5. 🧠 멀티모달 모델

### ✅ CLIP (2021, OpenAI)

- 이미지와 텍스트를 **같은 공간에 임베딩**하여, 의미 유사도를 비교할 수 있게 학습
- 학습 방식: **Contrastive Learning**  
  → 정답 쌍(이미지-문장)은 가깝게, 틀린 쌍은 멀리 떨어지도록 학습

> 예: "A cat sitting on a sofa"와 고양이 이미지가 가까워지고, "A bicycle on the street"과 고양이 이미지는 멀어지도록 조정

- 아래는 contrastive learning 을 설명하는 그림

<img width="897" height="306" alt="image" src="https://github.com/user-attachments/assets/9a3da1f2-2b10-4b9f-9e2f-18e2b9c13af3" />

</br> </br>

- 아래는 CLIP의 전반적인 구조

<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/ba592cc0-9a3d-4306-9503-9157b88dfaa9" />

</br>

---

### ✅ Flamingo (2022, DeepMind)

- **입력**: 텍스트 + 이미지  
- **출력**: 텍스트  
- 멀티모달(텍스트와 이미지 융합) + few-shot (아주 소량의 예시만으로 학습 가능) 학습에 특화된 구조


<img width="75%" height="75%" alt="image" src="https://github.com/user-attachments/assets/a4d5e37b-3f00-4230-9a92-3fe5fd48c94f" />

</br>

**`구성 요소`**

1. **Vision Encoder (이미지 인코더)**: 이미지를 고차원 벡터로 변환
2. **Perceiver Resampler**: 이미지 벡터를 고정된 개수의 토큰으로 요약
3. **LM block (언어 모델)**: 텍스트를 생성하는 디코더 기반 언어 모델

=> Flamingo는 **이미지를 텍스트 사이에 자연스럽게 삽입된 토큰**처럼 처리합니다.


</br>

**`few-shot 수행 방식`**

=> Flamingo는 **텍스트와 이미지가 섞인 일련의 예시들**을 프롬프트로 받아, 새로운 입력에 대해 적절한 텍스트를 생성합니다.

<img width="1004" height="405" alt="image" src="https://github.com/user-attachments/assets/99d11d58-12f5-496e-9a1c-24d2637a3046" />


</br>

---

### ✅ Gemini (2023, Google)

- GPT 스타일 + 멀티모달 구조의 통합
- 텍스트, 이미지, 코드, 문서 요약, 표 등 다양한 입력을 동시에 처리
- **웹 검색과 연결되어 최신 정보 반영 가능**
  
</br>

---

### ✅ GPT-4o (2024, OpenAI)

- o는 **omni**의 뜻: 텍스트, 이미지, 음성까지 **실시간 통합**
- 실제 대화하듯 자연스러운 상호작용 가능

</br> </br> </br>

---


## 6. 📋 멀티모달 주요 Task

`멀티모달 모델들은 이미지와 텍스트를 동시에 이해하고 처리할 수 있도록 설계되어, 다양한 실용적인 작업에 활용됩니다.`


**1) Image Captioning (이미지 캡셔닝)**

- **설명**: 모델이 이미지를 보고 그에 대한 설명 문장을 생성합니다.  
- **예시**:  
  - 입력 이미지: 고양이가 창가에 앉아 있는 사진  
  - 출력 텍스트: `"A cat sitting by the window watching outside."`


**2) Visual Question Answering (VQA)**

- **설명**: 주어진 이미지에 대한 질문을 이해하고, 그에 적절한 답변을 생성합니다.  
- **예시**:  
  - 입력:  
    - 이미지: 아이가 사과를 들고 있는 사진  
    - 질문: `"What is the child holding?"`  
  - 출력: `"An apple"`


**3) Text-conditioned Object Detection**

- **설명**: `"고양이를 찾아줘"` 같은 명령에 따라 이미지에서 해당 객체의 위치를 찾아줍니다.  
- **예시**:  
  - 입력:  
    - 이미지: 한 아이가 자신이 키우는 고양이와 강아지와 함께 노는 사진
    - 질문: `"Find the cat"`  
  - 출력: 고양이에 해당하는 부분에 네모박스 (ex: 좌상단 좌표와 우하단 좌표)



</br> </br> </br>

---

## 7. 💡 주요 용어 정리

| 용어 | 설명 |
|------|------|
| **Token** | 문장을 나누는 기본 단위. 하나의 단어가 하나의 토큰이 될 수 있고, 서브워드 단위로 쪼개기도 함. 예: "사과를 먹었다" → ["사", "과", "를", "먹", "었", "다"] |
| **Embedding** | 단순한 텍스트/토큰을 수치화된 **벡터(d차원)**로 바꾸는 작업. 예: "사과"라는 단어 → [0.41, -0.22, ...] 형태의 벡터로 바뀌어, 의미를 수치적으로 담을 수 있게 됨 |
| **Encoder** | 입력을 요약하고, 의미 있는 context vector(문맥 벡터)로 압축하는 모듈. 번역할 때 원문 이해 담당 |
| **Decoder** | 압축된 벡터를 바탕으로 출력 생성. 번역할 때 번역 결과 생성 담당 |
| **Attention** | 입력 중 어떤 부분에 집중할지 결정하는 가중치 계산 방식 |
| **Self-Attention** | 같은 입력 문장 내에서 각 요소끼리 얼마나 관련 있는지 판단. 예: "그는 사과를 먹었다" → "그"와 "먹었다" 연결 |
| **Cross-Attention** | 인코더와 디코더 사이 정보를 연결할 때 사용. 예: 입력 문장을 보며 번역어 생성 |
| **Positional Encoding** | 순서 정보가 없는 Transformer에 단어의 위치 정보를 추가하는 방법 |
| **Autoregressive** | 이전 출력 결과를 바탕으로 **순차적으로 다음을 예측**하는 방식 (ex: GPT) |
| **Contrastive Learning** | **비슷한 쌍은 가깝게, 다른 쌍은 멀게** 학습시키는 방식. CLIP 같은 모델의 학습 전략 |

---

