# Attention 메커니즘 학습자료

---

## 0. 사전지식

- **Feature Vector**란, 특정 데이터의 **특징을 숫자로 표현한 벡터**입니다. 예를 들어, 이미지에서는 각 픽셀의 값들이 Feature Vector로 표현될 수 있고, 텍스트에서는 단어들의 수치화된 표현이 Feature Vector가 됩니다.
- **두 벡터 간의 유사성**을 측정하는 방법으로는 여러 가지가 있습니다:
  - **내적 (Dot Product)**: 두 벡터의 내적은 유사성을 간단하게 측정할 수 있는 방법입니다. 내적이 크면 두 벡터의 방향이 비슷하고, 작으면 방향이 다르다는 것을 의미합니다.
  - **코사인 유사도 (Cosine Similarity)**: 두 벡터 간의 각도를 계산하여, 1이면 완전히 같은 방향, 0이면 직각, -1이면 반대 방향을 나타냅니다.


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

<img width="1228" height="792" alt="image" src="https://github.com/user-attachments/assets/7def7333-7db0-4f27-9c2f-298b4f63e04f" />

</br> </br> 


### 🔍 RNN 과의 비교

- RNN 은 각 단어를 **앞에서부터 하나씩 차례대로** 처리한다.
  - 즉 앞의 단어가 처리되어야, 그 다음 단어를 처리할 수 있다.
- RNN 은 **장기 의존성 문제**가 자주 나타난다.
  - 순차적으로 내부 상태를 업데이트 하면서 각 단어를 처리하기 때문에, 가장 최근에 본 단어는 정보가 많이 남아있지만, 아주 예전에 본 단어는 시간이 갈수록 희미해진다.

<img width="972" height="731" alt="image" src="https://github.com/user-attachments/assets/d8a45f47-0c82-4450-bcb5-8f72f4f62293" />


</br> </br>   </br> 

---

## 2. Attention의 구체적인 동작 방식

- Attention 메커니즘은 크게 **세 가지**로 나눌 수 있습니다:
  1. **Query (Q)**: 우리가 찾고자 하는 정보. 예를 들어, "번역하고 싶은 단어"가 될 수 있습니다.
  2. **Key (K)**: 다른 데이터의 정보. 예를 들어, 번역하려는 문장의 각 단어가 될 수 있습니다.
  3. **Value (V)**: 실제로 계산에 사용되는 값. Key에 해당하는 데이터의 값들이 됩니다.

- Attention은 **Query**와 **Key**의 **유사성**을 계산하여, **Value**에 가중치를 부여합니다. 이때 계산은 보통 **내적**으로 이루어집니다. 유사성이 높은 **Key**에 해당하는 **Value**는 더 큰 가중치를 부여받고, 유사성이 낮은 것은 낮은 가중치를 받습니다.

- **Self-Attention**: 이는 **자기 자신에 대한 주의**입니다. 예를 들어, 문장 내에서 각 단어가 다른 단어들과 얼마나 관계가 있는지를 계산하여, 각 단어에 주어지는 "주의"의 정도를 결정합니다.

---

용어 정리

- **Query (Q)**: 원하는 정보나 질문을 나타내는 벡터.
- **Key (K)**: 입력 데이터에서 정보의 키워드나 특징을 나타내는 벡터.
- **Value (V)**: Key에 해당하는 실제 값.
- **Score**: Query와 Key 간의 유사도를 나타내는 값. 보통 **내적**이나 **코사인 유사도**를 사용하여 계산.
- **Softmax**: Score를 확률 값으로 변환하여 가중치를 계산합니다. 각 가중치는 합이 1이 됩니다.
- **Attention Weight**: Value에 곱해지는 가중치. 이 가중치가 높을수록 해당 정보는 더 중요하다는 뜻입니다.

</br> </br>

---

## 3. 파이썬 예시 코드

```python
import torch
import torch.nn.functional as F

# Query, Key, Value 예시
Q = torch.tensor([[1, 0], [0, 1]], dtype=torch.float32)  # Query 벡터
K = torch.tensor([[1, 0], [1, 1]], dtype=torch.float32)  # Key 벡터
V = torch.tensor([[1, 1], [0, 0]], dtype=torch.float32)  # Value 벡터

# Query와 Key의 내적을 계산하여 점수 계산
scores = torch.matmul(Q, K.T)  # Q와 K의 내적
print("Scores:", scores)

# Softmax를 사용하여 가중치 계산
attention_weights = F.softmax(scores, dim=-1)
print("Attention Weights:", attention_weights)

# Attention 가중치를 Value에 곱해 최종 결과 계산
output = torch.matmul(attention_weights, V)
print("Output:", output)
```

이 코드는 self-attention을 계산하는 간단한 예시입니다.

Q, K, V는 각각 Query, Key, Value 벡터를 의미하고, 이들 간의 내적을 통해 어떤 정보에 주의를 기울일지 결정합니다. 

마지막으로 Softmax를 사용하여 가중치를 계산하고, 이를 Value에 곱해 최종 출력을 만듭니다.
