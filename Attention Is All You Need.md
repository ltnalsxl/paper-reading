# Attention Is All You Need

Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Lukasz Kaiser, Illia Polosukhin
 
**새로운 architecture 제안**
Recurrent 모델의 제약 사항들을 피하고 입력과 출력 사이에 전역 의존성을 이끌어내기 위해 RNN과 CNN을 제거하고 오로지 attention mechanism에만 기초한 새롭고 간단한 network 구조인 Transformer를 제안. 

Machine translation작업에 대한 실험 결과를 보면, 우리의 모델은 병렬 처리가 가능하고 학습 시간이 훨씬 덜 소요되는 동시에 품질이 우수한 것을 확인했고, 우리는 Transformer가 크고 제한된 train 자료로 영어 문법 해석에 성공적으로 적용됨으로써 다른task들에서도 잘 일반화됨을 확인했다.

#### Encoder
encoder는 N = 6개의 동일한 layer로 구성되어 있다. 이때 각 층은 2개의 sub-layer (multi-head self attention, fully connected feed-forward network)가 있다. 각 sub-layer의 output은 LayerNorm(x + Sublayer(x)) residual connection을 사용한다. 이때 embedding layer를 포함한 모델의 모든 sub-layer들은 dimension의 output (d_model)이 512를 생성한다.
#### Decoder
Decoder는 각 encoder layer의 2개의 sub-layer에 이어서 세번째 sub-layer를 넣는다. Decoder는 순차적으로 결과를 만들어내야 하기 때문에, self-attention을 변형한다. 바로 masking을 해주는 것이죠. masking을 통해, position i에 대한 예측은 미리 알고 있는 output들에만 의존하게 됩니다. 

### 3.2.2 Multi-Head Attention
d_model dimension의 key, value, query들로 single attention function을 수행하는 것보다 각각 학습된 keys, values, query들에 linear projection을 h번 계산하는 것, 즉, d_k, d_k and d_v차원에 각각 다른 weight matrix W를 곱해주는 것이 더 좋다고 한다. 이유는 벡터들의 크기가 줄고 병렬 처리가 가능해지기 때문에. 
 
Transformer는 이러한 multi-head attention을 세가지 방법으로 이용한다.
	encoder-decoder attention layers에서는 쿼리들이 그 이전 decoder layer로부터 오고, memory key와 value들이 encoder의 output으로부터 온다.
	Encoder가 self-attention layers를 포함한다. 
	decoder에 있는 self-attention layer도 마찬가지.

### 3.5 Positional Encoding
우리 모델은 recurrence와 convolution을 포함하지 않기 때문에 sequence를 모델에 사용하기 위해서는 단어의 position에 대한 정보를 추가해줘야 한다. 그래서 encoder와 decoder의 input embedding에 positional encoding을 추가했다. positional encoding은 embedding의 차원과 같은 차원을 갖기 때문에 둘 사이의 덧셈이 가능하다.

### 4. Why Self-Attention
Self-attention 사용의 이점 3가지: recurrent와 convolutional layers와 비교
	Layer 별 총 계산 복잡도, 즉 계산량이 감소한다. 
	병렬 처리 가능한 양이 증가한다. 
	장거리 dependency가 학습 가능해진다. long-range dependencies를 학습하는 것은 많은 sequence transduction task에서 가장 어렵고 중요한 부분. RNN과 비교했을 때, 먼 거리에 있는 sequence에 대한 학습이 가능함이 확인되었다. 
