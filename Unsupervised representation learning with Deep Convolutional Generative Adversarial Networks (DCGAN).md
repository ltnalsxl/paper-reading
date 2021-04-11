# Unsupervised representation learning with Deep Convolutional Generative Adversarial Networks (DCGAN)

### 1. Introduction
본 연구에서는 DCGAN이라고 불리는 CNN class를 소개하고, 이들이 이미지 데이터 세트에 대한 교육을 통해 deep convolutional adversarial pair (Generator와 Discriminator) 모두에서 object 부분부터 scene까지 hierarchy 구조를 학습한다는 설득력 있는 증거를 제시한다. 또한, DCGAN을 새로운 task에 적용하여 일반적인 이미지 representation으로서 활용될 가능성을 보여준다.


### 2. Related Work
2.1 Representation Learning from Unlabeled Data
비지도 학습은 컴퓨터 비전 관련 분야의 연구에서 활발하게 연구된 주제이다. 일반적인 접근법들로는 다음과 같은 예시들을 들 수 있다.
  - Clustering: K-means (Clustering의 대표적인 예시) 사용 후 Cluster를 활용하여 분류 성능을 개선, 이미지 표현 학습을 위해 계층적 클러스터링을 사용.
  - Auto-Encoder: 이미지를 인코딩하고, 가능한 정확하게 이미지를 재구성하기 위해 디코딩 진행.

2.2 Generative Image Models
생성 이미지 모델은 두 범주로 나뉘게 된다: 1) parametric 2) nonparametric
Nonparametric 모델들은 주로 texture synthesis, super-resolution, in-painting과 같은 분야에 사용되었다. Parametric 모델은 광범위하게 연구된 바 있으나 실제 세계의 자연스러운 이미지를 만드는 것은 아직까지도 큰 성공을 거두지는 못했다. 
  - Kingma & Welling의 접근 방식: 어느 정도 성공했지만 샘플이 흐릿한 것으로 고생했다.
  - Sohl-Dickstein의 접근 방식: 반복적인 전방 확산 프로세스를 사용하여 이미지를 생성한다.
  - GAN(Goodfellow et al., 2014): noisy and incomprehensible 이미지가 생성되어 어려움을 겪었다.
  - Denton et al.의 접근 방식: GAN보다 더 높은 품질의 이미지를 보여주었지만, 여전히 noise로 인해 이미지의 물체가 흔들려 보이는 어려움을 겪었다.
  - Gregor et al.과 Dosovitskiy et al.의 접근 방식에서 최근 자연 이미지 생성에 어느 정도 성공했지만, supervised learning에 generator를 사용하지 않았다.

2.3 Visualizing the Internals of CNNs
CNN은 안에서 무슨 일이 벌어지고 있는지 모르는 ‘블랙박스’ 방법이라는 것에 대한 비판이 끊이지 않는다. 이에 Zeiler et. al.은 deconvolution을 사용하고 최대 활성화를 필터링함으로써 네트워크에서 각 컨볼루션 필터의 대략적인 목적을 찾을 수 있음을 보여주었다. 또한 Mordvintsev et al.은 gradient descent를 input에 적용시켜 이상적인 이미지를 찾아낼 수 있음을 보여주었다. 


### 3. Approach and Model Architecture
그동안 CNN을 사용하여 이미지를 모델링하려는 시도에서 어려움을 겪었다. 특히 일반적으로 사용되는 CNN 아키텍처를 사용하여 GAN을 scale하는 것.
본 논문에서는 다양한 데이터에 대한 안정적인 학습 및 고해상도의 심층적인 생성 모델을 학습하는 방법론을 제안하였다. 이 접근 방식의 핵심은 최근 CNN 구조에 대해 입증된 세 가지 변경 사항을 수정하는 것이다.
  1) maxpooling과 같은 spatial pooling functions를 사용하지 않고 strided convolutions을 사용한다. 이 방법을 generator에 적용하여, generator가 스스로 spatial upsampling과 discriminator를 학습할 수 있도록 한다.
  2) fully connected layers를 제거한다. 대표적인 예시는 global average pooling. global average pooling은 모델 안전성에는 도움이 되나 convergence speed에는 부정적인 영향을 미친다. GAN의 첫번째 layer를 fully connected라고 볼 수도 있지만, 결과적으로 4차원 tensor로 재구성되어 convolution stack의 시작으로 사용된다. Discriminator의 경우네는 마지막 layer가 flatten된 뒤 단일 sigmoid output으로 출력된다. 
  3) Batch Normalization: input을 normalize하여 평균 및 단위 분산을 0으로 설정한다. 이는 학습 안정화하고 잘못된 초기화 때문에 발생하는 훈련 문제를 처리하는 데 도움이 된다. 다만 모든 layer에 직접적으로 batch normalization을 설정하는 것은 sample oscillation과 모델 불안정성을 야기하기 때문에, generator의 output layer와 discriminator의 input layer에는 batch normalization을 적용하지 않았다. 

안정적인 DCGAN을 구축하기 위해 다음과 같은 사항들을 적용 및 고려하였다.
 
 ![image](https://user-images.githubusercontent.com/68283760/114290604-c6cfc880-9abb-11eb-9639-58b94b3aa1f2.png)
![image](https://user-images.githubusercontent.com/68283760/114290605-c8998c00-9abb-11eb-9455-52c371ce2dde.png)

	- 모든 pooling layers를 discriminator의 경우 strided convolutions로 generator의 경우 fractional-strided convolutions로 대체한다.
	- Generator와 discriminator에 모두 batch normalization을 적용한다 (앞서 언급했듯 G의 출력 layer아 D의 input layer는 예외)
	- 더 깊은 모델을 구축하기 위해 fully connected layer는 제거한다.
	- Generator의 경우에는 마지막 layer에는 tanh activation을, 나머지 layer들에는 ReLU activation을 사용한다. 


### 4. Details of Adversarial Training
본 연구에는 세 가지 데이터셋에 DCGAN을 학습시켜보았다. 1) Large-scale Scene Understanding (LSUN) 2) Imagenet-1k 3) a newly assembled Faces dataset
	- 모든 데이터셋들은 mini-batch stochastic gradient descent (SGD)를 사용하여 train
	- mini-batch size = 128
	- 모든 모델 가중치를 평균=0, 표준편차=0.02의 정규 분포에서 랜덤하게 초기화한다. (weight initialization)
	- LeakyReLU에서 slope of the leak는 0.2로 설정
	- 기존 GAN은 momentum 사용 -> Adam optimizer로 변경 (hyperparameters 최적화된 상태)
	- 기존 learning rate 0.001이 너무 높다고 판단하여 0.0002로 낮춤
	- β_1의 값 0.9가 training 과정에서 불안정성을 유발한다고 판단하여 이를 안정화하기 위해 0.5로 값을 낮췄다. 

다음은 사용한 3개의 데이터셋들에 대한 논문의 간략한 설명이다.

4.1 LSUN 
이미지 생성 모델의 수준이 많이 올라왔음에도 불구하고 아직까지 1) overfitting과 2) memorization of training samples 문제가 남아있다. 본 논문의 모델이 더 많은 데이터를 가지고 고해상도로 생성해낼 수 있다는 걸 보여주기 위해 LSUN 데이터(3백만개)를 활용했다. Generator가 input sample들을 memorizing 할 가능성을 더 감소시키기 위해서 본 방법론에서는 이미지 중복 제거 과정을 수행한다. 이를 “image de-duplication”이라고 하고, 32x32 훈련 이미지에 3072-128-3072 de-noising dropout regularized RELU autoencoder를 fit하는 방식으로 진행된다. 결과적으로 ReLU activation을 통해 code layer의 activation들이 이진화되며 효과적으로 정보를 보존함과 동시에 편리한 형태를 제공한다.

4.2 Faces
사람의 이름과 얼굴이 포함된 10,000명에 대한 3백만개의 이미지 데이터. 이를 train data로 활용하기 위해 OpenCV face detector를 사용하여 고해상도를 유지하고 약 35만 개의 face boxes를 얻어냈다. Data augmentation은 적용되지 않았다.

4.3 Imagenet-1k
Unsupervised learning (비지도학습)을 위한 이미지 training set으로 Imagenet-1k를 사용했다. 32×32 사이즈에 train했으며 이 역시 data augmentation은 적용하지 않았다.


### 5. Empirical Validation of DCGANs Capabilities
5.1 Classifying CIFAR-10 using GANs as a feature extractor
 
DCGAN은 CIFAR-10에 pre-trained 된 것이 아니라 Imagenet-1k에서 학습된 후 feature들이CIFAR-10 이미지를 분류하는 데 사용되었다. 

5.2 Classifying SVHN digits using GANs as a feature extractor
StreetView House Numbers dataset (SVHN)에 대해서는 labeled data가 부족한 경우 DCGAN discriminator의 기능을 supervised 목적으로 사용한다. 또한 DCGAN에 사용된 CNN 아키텍처가 동일한 데이터에 대해 동일한 아키텍처를 가진 supervised CNN을 훈련시키고 64개의 하이퍼파라미터를 무작위로 시도하여 이 모델을 최적화함으로써 이게 모델 성능의 핵심 기여 요소가 아님을 검증한다. 


### 6. Investigating and Visualizing the Internals of the Networks
 
1000개의 label들을 가지고 SCHN 분류를 진행했을 때, DCGAN (본 논문의 모델의 error rate가 가장 낮은 것을 확인할 수 있다.

6.1 Walking in the Latent Space
Latent space에서 이미지 생성에 대한 의미 있는 변화가 발생할 경우 모델이 관련되고 흥미로운 표현을 학습했다고 추론할 수 있다. 

6.2 Visualizing the Discriminator Features
본 논문은 대규모 이미지 데이터 세트에 대해 훈련된 비지도 DCGAN이 feature들의 흥미로운 계층 구조(hierarchy)도 배울 수 있음을 보여준다. 역전파(backpropagation)를 사용하여, 우리는 판별기(discriminator)에 의해 학습된 feature가 침대와 창문과 같은 침실의 일반적인 부분에서 활성화됨을 아래 그림을 통해 알 수 있다.
 

6.3 Manipulating the Generator representation
6.3.1 Forgetting to Draw certain objects
Generator에서 창문을 완전히 제거하려는 실험을 수행했다. 150개 샘플에 대해서 52개의 window bounding boxes를 수동으로 그렸고, 흥미롭게도 결과적으로 네트워크는 침실에 창을 그리는 것을 잊어버리고 다른 객체로 교체되는 것을 발견할 수 있었다.
아래 그림에서 가장 상단 행을 보면, 공간의 모든 이미지가 침실처럼 보이는 것을 보여준다. 6번째 줄에는 창문이 없는 방이 천천히 거대한 창문이 있는 방으로 바뀌는 것을 볼 수 있으며, 10번째 줄에서는 TV로 보이는 것이 창으로 서서히 변환되는 것을 볼 수 있다.
 
6.3.2 Vector Arithmetic on face Samples
Face Samples를 이용해서 이미지 간 벡터 연산이 가능하다는 것을 보여준다.

### 7. Conclusion and Future Work
본 논문은 GAN을 훈련하기 위해서 보다 안정적인 구조를 제안하고 adversarial network(적대적 네트워크)가 지도 학습 및 생성 모델링을 위한 이미지의 좋은 representation을 학습한다는 증거를 제시했다. 다만 모델 불안정성 문제는 여전히 몇 가지 형태에서 남아 있기 때문에 더 많은 연구가 필요하다. 본 논문에서 제안하는 framework를 비디오와 오디오로 확장하는 것, 학습된 잠재 공간의 특성에 대한 추가 연구도 흥미로울 것이라 생각한다.
