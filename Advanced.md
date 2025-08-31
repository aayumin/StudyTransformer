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
   - Flamingo / Gato
   - Gemini / GPT-4o
6. 💡 용어 정리
7. 🗓️ 연도별 주요 모델 요약


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

- 이미지와 텍스트를 **같은 공간에 임베딩**하여 의미 유사도를 비교할 수 있게 학습
- 학습 방식: **Contrastive Learning**  
  → 정답 쌍(이미지-문장)은 가깝게, 틀린 쌍은 멀리 떨어지도록 학습

> 예: "A cat sitting on a sofa"와 고양이 이미지가 가까워지고, "A car driving"과는 멀어지도록 조정

[이미지 part: 텍스트와 이미지가 같은 공간에 매핑되는 벡터 시각화 + 양극단 contrastive loss 설명]

</br>

---

### ✅ Flamingo (2022, DeepMind)

- 텍스트+이미지 입력 → 텍스트 출력
- 예: "이 이미지에 어떤 상황이 벌어지고 있나요?" → 자동 설명 생성

</br>

---

### ✅ Gato (2022, DeepMind)

- 텍스트, 로봇 제어, 게임 등 다양한 작업을 하나의 모델로 처리
- 모든 입력을 **토큰화**해서 Transformer로 처리 → 범용 인공지능(AGI)의 초기 형태

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

[이미지 part: GPT-4o가 실시간으로 듣고, 보고, 말하는 시각적 예시]

</br> </br> </br>

---

## 6. 💡 주요 용어 정리

| 용어 | 설명 |
|------|------|
| **Token** | 문장을 나누는 기본 단위. 하나의 단어가 하나의 토큰이 될 수 있고, 서브워드 단위로 쪼개기도 함. 예: "사과를 먹었다" → ["사", "과", "를", "먹", "었", "다"] |
| **Embedding** | 단순한 텍스트/토큰을 수치화된 **벡터(d차원)**로 바꾸는 작업. 예: "사과"라는 단어 → [0.41, -0.22, ...] 형태의 벡터로 바뀌어, 의미를 수치적으로 담을 수 있게 됨 |
| **Encoder** | 입력을 요약하고, 의미 있는 feature vector(문맥 벡터)로 압축하는 모듈. 번역할 때 원문 이해 담당 |
| **Decoder** | 압축된 벡터를 바탕으로 출력 생성. 번역할 때 번역 결과 생성 담당 |
| **Attention** | 입력 중 어떤 부분에 집중할지 결정하는 가중치 계산 방식 |
| **Self-Attention** | 같은 입력 내에서 각 요소끼리 얼마나 관련 있는지 판단. 예: "그는 사과를 먹었다" → "그"와 "먹었다" 연결 |
| **Cross-Attention** | 인코더와 디코더 사이 정보를 연결할 때 사용. 예: 입력 문장을 보며 번역어 생성 |
| **Positional Encoding** | 순서 정보가 없는 Transformer에 단어의 위치 정보를 추가하는 방법 |
| **Autoregressive** | 이전 출력 결과를 바탕으로 **순차적으로 다음을 예측**하는 방식 (ex: GPT) |
| **Image Captioning** | 이미지를 보고 그 내용을 설명하는 문장을 생성하는 작업 |
| **Contrastive Learning** | **비슷한 쌍은 가깝게, 다른 쌍은 멀게** 학습시키는 방식. CLIP 같은 모델의 학습 전략 |

---

## 7. 🗓️ 연도별 주요 모델 요약

| 모델 | 발표 연도 | 기관 | 특징 |
|------|-----------|------|-------|
| Transformer | 2017 | Google | Attention 도입, 병렬처리 가능 |
| BERT | 2018 | Google | 이해 중심, Encoder-only |
| GPT-2 | 2019 | OpenAI | 텍스트 생성, Decoder-only |
| GPT-3 | 2020 | OpenAI | 대규모, few-shot 학습 |
| ViT | 2020 | Google | 이미지 → 패치 → Transformer |
| CLIP | 2021 | OpenAI | 텍스트+이미지 매칭, Contrastive 학습 |
| Flamingo | 2022 | DeepMind | 멀티모달 텍스트 생성 |
| Gato | 2022 | DeepMind | 범용 AI, 여러 작업 통합 |
| Gemini | 2023 | Google | 멀티모달 + 검색 연동 |
| GPT-4o | 2024 | OpenAI | 실시간 멀티모달 (음성+텍스트+이미지)

