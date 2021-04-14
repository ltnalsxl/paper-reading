## **Convolutional Networks on Graphs for Learning Molecular Fingerprints**  

David Duvenaud, Dougal Maclaurin, Jorge Aguilera-Iparragurre, Rafael Gómez-Bombarelli, Timothy Hirzel, Alán Aspuru-Guzik, and Ryan P. Adams.  
  
본 논문에서는 그래프에서 바로 사용이 가능한 CNN(convolutional neural network)구조를 제안한다. 이러한 네트워크를 통해 입력이 임의의 크기와 모양의 그래프인 예측 파이프라인에 대한 end-to-end 학습이 가능해진다. 우리가 제시하는 아키텍처는 circular fingerprints를 기반으로 표준 분자 feature의 추출 방법을 일반화한다. 이렇게 추출해낸 data-driven feature들이 더 해석 가능하며 다양한 작업에서 보다 나은 예측 성능을 낸다는 것을 보여준다.   
  
최근 재료 설계 분야에서는 새로운 분자 property을 예측하기 위해 neural network를 이용하기 시작했다. 하지만 예측 변수, 즉 분자에 대한 입력이 임의의 크기와 모양을 가질 수 있다는 어려움이 뒤따랐다. 현재 대부분의 기계 학습 파이프라인은 고정된 크기의 입력만 처리할 수 있다. 현재 기술 상태는 off-the-shelf fingerprint 소프트웨어를 사용하여 고정 차원을 가지는 벡터를 계산하고 이러한 feature를 FCNN(Fully-connected deep neural network) 또는 다른 일반적인 머신러닝 방법론의 입력(input)으로 사용하는 것이다. 학습(훈련)과정에서 molecular fingerprint는 고정된 길이로 처리되어 온 것이다.   
본 논문에서, 우리는 molecular fingerprint vector를 계산하는 함수인 stack의 하단 계층을, 원래 molecule를 나타내는 graph를 input으로 가지는 차별화된 neural network(신경망)으로 교체한다. 이 그래프에서 꼭지점은 개별 원자(atoms)를 나타내고 가장자리는 결합(bonds)을 나타낸다. 이 네트워크의 하위 계층은 동일한 local filter가 각 원자와 그 neighbor에 적용된다는 점에서 convolution이다. 몇 개의 그러한 layer(층)들 이후에, global pooling 단계는 분자의 모든 원자들의 특징들을 결합한다.  
  
이러한 방법을 사용하는 것은 고정된 fingerprint를 사용하는 것에 비해 다음과 같은 이점들을 제공한다.

-   Predictive performance (예측 성능): Data adopting을 사용하여, 모델에 최적화된 지문은 고정 지문보다 훨씬 나은 예측 성능을 제공할 수 있다. 우리는 neural graph fingerprints이 용해성(solubility), 약물 효과 및 유기 광전지 효율 데이터 세트에서 일반적인 fingerprint의 예측 성능과 일치하거나 비교한다는 것을 보여준다.
-   Parsimony: 모든 가능한 하부 구조를 겹치지 않고 인코딩하려면, fingerprint의 차원이 매우 커야한다. 예를 들어, 거의 발생하지 않는 property들을 제거한 후 43,000 크기의 molecular fingerprint를 사용했다. 본 논문에서 제안하는 'differentiable fingerprints'는 관련 기능만 인코딩하도록 최적화되어 downstream 계산과 정규화 요구 사항을 줄일 수 있다.
-   Interpretability (해석 가능성): 일반적인 fingerprint는 fragment 간의 유사성에 대한 개념 없이, 가능한 각 fragment를 완전히 명확하게 인코딩한다. 이와 다르게 neural graph fingerprint는 각 특성은 유사하지만 별개의 분자 fragment에 대해서만 활성화되어 molecular representation 자체를 의미 있고 해석 가능하게 만든다.
