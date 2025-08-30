# Attention 메커니즘 학습자료

---

## 0. 사전지식

- **Feature Vector**란, 특정 데이터의 **특징을 숫자로 표현한 벡터**입니다. 예를 들어, 이미지에서는 각 픽셀의 값들이 Feature Vector로 표현될 수 있고, 텍스트에서는 단어들의 수치화된 표현이 Feature Vector가 됩니다.
  - 예를 들어, 얼굴을 묘사하는 Feature Vector 를 만든다고 하면, [눈썹의 길이(cm), 쌍커풀 유무(0 또는 1), 코의 높이(cm), 턱선의 각도(degree)] 를 계산해서 길이가 4인 Vector를 만들 수 있습니다.
  - 이때 위의 Feature Vector 의 실제 값으로 [4.2,  1,  2.1,  135], [3.8,  0,  2.3,  140] 등이 있을 수 있습니다.
  
- **두 벡터 간의 유사성**을 측정하는 방법으로는 여러 가지가 있습니다:
  - **내적 (Dot Product)**: 두 벡터의 내적은 유사성을 간단하게 측정할 수 있는 방법입니다. 내적이 크면 두 벡터의 방향이 비슷하고, 작으면 방향이 다르다는 것을 의미합니다.
  - **코사인 유사도 (Cosine Similarity)**: 두 벡터 간의 각도를 계산하여, 1이면 완전히 같은 방향, 0이면 직각, -1이면 반대 방향을 나타냅니다.

- **은닉 레이어 (Hidden layer)** 는 입력과 출력 사이에서 데이터의 특징을 추출하고 표현하는 신경망의 중간 단계입니다.

  
- **Query, Key, Value** 는 Attention 메커니즘을 이해하기 위해 필수적인 부분입니다.
  - Key 와 Value 는 하나의 쌍을 이룹니다. (예를 들면, key가 상품명이고 value 는 상품의 가격)
  - query 는 요청이나 질문의 느낌으로 이해하면 됩니다.
  - 비유하자면, [고래밥, 포테이토칩, 콘칩, 칸쵸] 등의 상품명을 나타내는 key가 있고 각각에 대응하는 상품가격 value가 있을 때 "홈런볼"이라는 query가 들어왔다고 가정해보자.
  - 이때 "홈런볼"이라는 query를 모든 key와 비교해서 유사도를 계산한다.
  - 만약 똑같이 초콜릿 맛이 나는 "홈런볼"과 "칸쵸"가 유사도가 가장 높았고, "포테이토칩"과는 유사도가 가장 낮았다고 해보자.
  - 그러면 최종 결과값으로 홈런볼의 value(여기서는 가격)를 결정할 때, "칸쵸"라는 key에 대응되는 "2500원"이라는 value 가 가장 많이 반영되고, "포테이토칩"의 가격은 가장 적게 반영된다.

- **Softmax 연산**은 임의의 여러 값들이 있을 때 이 값들을 확률처럼 표현해주는 연산을 말합니다.
  - Softmax 특징으로는 변환된 각각의 값들은 모두 0~1 사이의 값을 가집니다. (확률처럼)
  - 변환된 모든 값들을 합하면 정확히 1.0 이 됩니다. (확률처럼)
  - 원래의 값에서의 대소 관계는 변환된 후에도 대소 관계가 그대로 유지됩니다.
  - 예를 들면, 어떤 이미지를 [고양이, 강아지, 사자] 중에서 어디에 속하는지 예측한 결과값이 원래 [2, 1, 0.1] 이라면 Softmax 를 적용하여 변환된 값은 [0.66, 0.24, 0.1] 이 됩니다.
  - 그러면 그 이미지가 고양이일 확률은 66%, 강아지일 확률은 24%, 사자일 확률은 10% 라고 해석할 수 있습니다.

</br> </br>

---

## 1. Attention이란

- **Attention**은 **중요한 정보에 집중**하는 메커니즘입니다. 주어진 입력 데이터 중에서 중요한 부분에 더 많은 **가중치를 부여**하여 학습하는 방식입니다.
- 예를 들어, **기계 번역**에서 문장의 각 단어는 문장의 전체 의미를 이해하는 데 있어서 중요도가 다를 수 있습니다. Attention은 이 **차이를 반영하여** 더 중요한 단어에 집중하고 덜 중요한 단어는 덜 신경 씁니다.

<img width="802" height="205" alt="image" src="https://github.com/user-attachments/assets/e5828a0c-eec8-4bea-9988-251912b9e2b2" />

</br> </br> </br>

 ### 📚 Attention을 책 읽기에 비유하기
  - **책을 읽을 때**, 우리는 글의 전체 내용을 읽지만, **중요한 부분에 더 집중**하거나 **자주 반복해서 읽는** 경향이 있습니다.
  - 예를 들어, **사전에서 단어를 찾을 때** 전체 단어 목록을 읽지 않지만, **찾고자 하는 단어에만 집중**하여 검색합니다. 
  - 마찬가지로 **Attention 메커니즘은 중요한 정보에만 집중**하여, 필요한 부분에 **더 많은 "주의"를 기울입니다**.

</br> </br> </br>



### ⚠️ 기존 방식의 문제
  - 전통적인 순차적 모델은 입력 데이터를 순차적으로 처리합니다. 이 방식은 **긴 시퀀스**에서 중요한 정보가 **시간에 따라 사라지거나 희석되는** 문제를 겪습니다. 예를 들어, 긴 문장에서 처음 나온 단어와 끝에 나온 단어의 관계를 잘 이해할 수 없습니다.

<img width="867" height="152" alt="image" src="https://github.com/user-attachments/assets/a313bd98-e003-4c80-82d1-92967fcd15a7" />


</br> </br> </br>

### ✅ Attention의 장점
  - **긴 거리의 의존성(long-range dependencies)** 을 잘 처리할 수 있습니다.
    - 중요한 정보만 더욱 집중해서 보기 때문에 멀리 있는 단어라도 중요한 단어라면 집중해서 봅니다.
  - **병렬 처리**가 가능합니다.
    - 꼭 앞에서부터 하나씩 차례대로 처리할 필요가 없습니다. 각 단어를 처리하는 것은 개별적/독립적으로 수행됩니다.
    - 하나씩 순서대로 할 필요가 없기 때문에, 모든 단어를 한꺼번에 병렬적으로 처리할 수 있습니다.
  - **중요한 정보에 집중**할 수 있어, **기억을 효율적으로 활용**할 수 있습니다.

<img width="70%" height="70%" alt="image" src="https://github.com/user-attachments/assets/7def7333-7db0-4f27-9c2f-298b4f63e04f" />

</br> </br> 


### 📊 RNN 과의 비교

- RNN 은 각 단어를 **앞에서부터 하나씩 차례대로** 처리한다.
  - 즉 앞의 단어가 처리되어야, 그 다음 단어를 처리할 수 있다.
- RNN 은 **장기 의존성 문제**가 자주 나타난다.
  - 순차적으로 내부 상태를 업데이트 하면서 각 단어를 처리하기 때문에, 가장 최근에 본 단어는 정보가 많이 남아있지만, 아주 예전에 본 단어는 시간이 갈수록 희미해진다.

<img width="70%" height="70%" alt="image" src="https://github.com/user-attachments/assets/d8a45f47-0c82-4450-bcb5-8f72f4f62293" />

</br> </br>   </br> 

---

## 2. Attention의 구체적인 동작 방식

<img width="381" height="232" alt="image" src="https://github.com/user-attachments/assets/87df9b3d-d6f4-471c-a237-620788edb8fa" />

- Attention 메커니즘의 입력은 크게 **세 가지**로 나눌 수 있습니다:
  1. **Query (Q)**: 우리가 현재 보고 있는 정보. 예를 들어, "번역하고 싶은 단어"가 될 수 있습니다.
  2. **Key (K)**: 다른 데이터의 정보. 예를 들어, 문장을 구성하는 각 단어들이 될 수 있습니다.
  3. **Value (V)**: 실제로 계산에 사용되는 값.

<img width="745" height="326" alt="image" src="https://github.com/user-attachments/assets/58a3565e-e597-4369-8546-5d25cfeb6b7a" />

- Attention은 **Query**와 **Key**의 **유사성**을 계산하여, 해당 Key와 대응되는 **Value**에 유사한 정도만큼 가중치를 부여합니다. 유사성이 높은 **Key**에 해당하는 **Value**는 더 큰 가중치를 부여받고, 유사성이 낮은 것은 낮은 가중치를 받습니다.

</br> </br>   </br> 


### 🔍 Attention 차근차근 알아보기

 </br> 
 
Step 1️⃣: 유사한 정도에 따라 Attention Score 계산
```text
현재 관심 있는 query 단어를 나머지 단어들의 key와 비교 → 얼마나 유사하고 중요한지 "점수"로 매김
```

```python
for i in range(num_words_in_sentence):
  attention_score[i] = calculate_similarity(query, key[i])
```

- 아래 그림에서 "it_"이 query 인 상태이다.
  
<img width="40%" height="40%" alt="image" src="https://github.com/user-attachments/assets/47bb585a-2bce-449f-a55c-c8667160f13c" />
 
</br> 

Step 2️⃣: 소프트맥스(SOFTMAX) 적용

```text
다양한 값을 가지고 있는 Attention Score → Softmax 적용 → 확률화된 Attention Weight
```

- 값들을 0~1 사이로 바꾸고 **총합이 1이 되도록 조정**
 
</br> 

Step 3️⃣: 최종 값인 Attention Value 계산

```text
각 단어들의 value × 각 단어의 가중치 → 모두 더함 (가중합)
   ==>  최종 값 계산 ("Attention Value" 또는 "Context Vector" 라고 부른다.)
```
- 여기서 가중치는 (앞에서 유사도를 계산하고 softmax를 적용해서) 확률화된 attention weight 를 의미함.
- 중요도가 높은 단어의 정보는 **더 크게 반영**
 
</br> 

<img width="60%" height="60%" alt="image" src="https://github.com/user-attachments/assets/9aaa381b-5c1d-40e3-bc59-6f6bb5ab96b4" />

- 위 그림은 가볍게 참고만 하기.
 
</br> 

Step 4️⃣: 예측

```text
지금까지 계산된 Attention Value 를 잘 활용하여 다음 단어를 예측
```
- 기존 RNN보다 **더 문맥에 맞는 단어**를 예측할 수 있음



</br> </br>   </br> 

---



## 3. 파이썬 예시 코드

```python
import torch
import torch.nn.functional as F

# 가상의 Q, K, V 벡터 (샘플 개수: 1, 문장에 포함된 단어의 수: 4, 각 단어를 표현하는 Feature Vector의 차원: 5)
QUERY = torch.randn(1, 1, 5)   # (1, 1, feature_dim)
KEY = torch.randn(1, 4, 5)   # (1, sequence_length, feature_dim)
VALUE = torch.randn(1, 4, 5)   # (1, sequence_length, feature_dim)

# 1) Query와 Key의 내적을 계산하여 점수 계산
scores = torch.matmul(QUERY, KEY.transpose(-2, -1)) / torch.sqrt(torch.tensor(feature_dim))

# 2) Softmax 로 값을 확률화
attention_weights = F.softmax(scores, dim=-1)

# 3) Attention weights 와 values 를 곱해서 최종 결과 계산
output = torch.matmul(attention_weights, VALUE)

print("Attention Weights:", attention_weights)
print("Output:", output)
```


