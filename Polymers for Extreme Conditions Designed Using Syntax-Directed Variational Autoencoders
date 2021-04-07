## **Polymers for Extreme Conditions Designed Using Syntax-Directed Variational Autoencoders**  
 **Rohit Batra\*, Hanjun Dai, Tran Doan Huan, Lihua Chen, Chiho Kim, Will R. Gutekunst, Le Song, and Rampi Ramprasad**  
 **Chem. Mater. 2020, XXXX, XXX, XXX-XXX**  
 **Publication Date:December 14, 2020**  

Subjects: Insulators,Molecular modeling,Materials,Molecules,Polymers

Authors 소개

Members of Georgia Institute of Technology (Atlanta, Georgia 30332, United States) majoring either Materials Science & Engineering/Computational Science & Engineering/Chemistry & Biochemistry.

이번 글에서는 2020년 12월 14일에 Chemistry of Materials 저널에 등재된 논문인 **Polymers for Extreme Conditions Designed Using Syntax-Directed Variational Autoencoders**의 내용을 정리해보도록 하겠습니다. 본 논문에서 가장 핵심적인 두 단어인 'syntax'와 'semantics'의 사전적 의미는 모두 'grammar(문법)'의 부분집합으로 해석되며, syntax는 구문론/구문  semantics는 의미론/의미를 뜻합니다. 본 포스트에서는 편의상 영문 그대로 syntax, semantics라 표기하도록 하겠습니다.

---

그동안 기존 연구에서는 물질(material)이 주어지면 그에 대한 property를 예측하는 supervised learning 형태의 머신러닝 기법들이 사용되어왔습니다. 다만 이러한 방법은 알려진 물질을 가지고 예측의 모든 과정이 진행되기에 어떠한 결과도 인간의 상상력 범위를 넘을 수 없다는 한계점을 가지고 있습니다. 이와 다른 이상적인 접근 방법으로 'inverse type'을 기반으로 한 접근 방법, 즉 정해진 물질에 대한 property를 예측하는 것이 아닌 desired property set을 만족하는 물질을 찾는 방법론이 제안되었고, 이와 관련해서는 두 가지 scheme이 존재합니다. 

1.  Genetic Algorithm과 Evolutionary Searches와 같은 global search algorithm strategies. Target property를 만족할 때까지 후보 물질들을 새로운 형태로 변형하고 (mutate) 혼합.
2.  Deep Generative ML moel을 이용하는 방법. Latent space에서 materials space로의 inverse 매핑(mapping)을 학습함으로써 보다 간단한 솔루션을 제공. 

두 가지 방법 모두 사람이 상상할 수 있는 범위를 넘어선다는 점에서 분자 발견 (혹은 생성)에 있어서 분명한 이점이 존재하지만, 후자 (Deep Generative ML model) 는 더불어 고유한 화학적 물질 특성과 경향을 학습하여, 적절한 물질들 사이에 추가하거나 알려진 최고의 물질 근처를 탐색하는 방법으로 재료 설계를 가능하게 합니다.

최근 들어 VAE (Variational Autoencoder)과 GAN (Generative Adversarial Network)와 같은 생성 모델들은 drug-like 분자(molecule) 뿐만 아니라 고분자(polymer)까지도 생성하는 데에 사용되고 있습니다.

-   논문 Automatic Chemical Design Using a Data-Driven Continuous Representation of Molecules(ACS Cent. Sci. 2018, 4, 2, 268–276)에서는 SMILES를 이용하여 분자의 화학적 구조를 나타냄.
    -   Encoder: 분자 SMILES를 latent space에 투영
    -   Decoder: 주어진 latent representation을 통해 분자 SMILES 복원

다만 이러한 분자 생성 모델을 개발하는 데에 있어서 가장 핵심은, SMILES 언어의 구문(syntax) 또는 문법과 분자(또는 물질)의 화학적 성질에 관한 의미론(semantics)으로 인해 만들어진 연속적인(continuous) latent space와 이산적인(discrete) chemical structure space의 매핑을 학습해야 한다는 것입니다. 만약 VAE 모델에 명확한 syntax나 semantics가 없다면, SMILES를 표현하는 데에 있어서 불필요한 flexibility를 허용하게 되고, 물질의 구조와 SMILES에 내재되어 있는 여러 화학적 규칙들을 학습시켜야 합니다. 이는 결국 더 많은 양의 training data가 필요하다는 것을 의미하며, 뿐만 아니라 학습된 latent space의 품질 하락에도 일조하며 높은 확률로 decoding 과정에서 invalid (유효하지 않은) SMILES을 생성하며 전반적인 분자 생성 과정의 효율이 하락합니다.  다음 논문은 이러한 한계점을 극복하기 위해서 근본적인 개선 방안이 적용된 연구입니다.

-   논문 Grammar Variational Autoencoder (Proceedings of the 34th International Conference on Machine Learning, PMLR 70:1945-1954, 2017.)에서는 context-free-grammar(이하 CFG)라는 아이디어를 이용하여 성공적으로 SMIlES 언어의 구문 또는 구조를 성공적으로 통합
    -   SMILES 언어와 연관된 문법을 사용하여 분자 SMILES를 parse tree로 전환
    -   Parse Tree는 흔히 파싱 트리(parsing tree), 어원 트리(derivation tree), 구체적인 구문 트리(concrete syntax tree)라고도 불림. 올바른 문장에 대해 트리 구조로 나타낸 것으로 언어의 문법을 구체적으로 반영.![](https://blog.kakaocdn.net/dn/bjGupf/btqQBHJgL87/mAbLp98FAabR4y6diXyv81/img.jpg)
    -   Parse Tree는 One-Hot 표현 방식의 형태로 나타나며 VAE를 훈련시키기 위한 input으로 사용된다.

최근 (2020년 11월) VAE 모델에 semantics까지 통합시키는 것에 성공한 syntax-directed VAE라고 불리는 모델이 다음 논문을 통해 소개되었습니다. 

-   논문 Syntax-Directed Variational Autoencoder for Structured Data (arXiv:1802.08786)
    -   생성 모델의 발전에도 불구하고 컴퓨터 프로그램 또는 분자 구조와 같이 formal한 문법(grammar)과 의미론(semantics)를 사용하여 이산 구조의 포착하고 구문론적(syntactically)으로나 의미론적(semantically)으로 올바른 데이터를 생성하는 것은 여전히 어려운 일
    -   새로운 syntax-directed variational autoencoder (SD-VAE) 모델을 제안
    -   출력 공간에 제약을 가하여 출력이 구문적으로 유효할 뿐만 아니라 의미론적으로 합리적이도록
    -   Discrete 생성 모델에 구문 및 의미 제약 조건을 통합하는 효과를 입증

마침내 syntax와 semantics가 결합된 VAE 모델은 의미있는 latent representation을 학습하여 기존 모델보다 훨씬 좋은 성능을 보였습니다. 더욱 중요한 것은 본 모델은 decoding 과정에서 높은 확률로 유효한 분자 SMILES를 생성해낼 수 있었으며, 분자 생성 모델로서 사용될 수 있음을 입증했습니다. 

최근 들어 가장 강력하고도 일반적인 접근인 'SELFIES'가 구문론적 (syntactic) / 의미론적 (semantic) 제약을 모두 '[derivation rules table](https://iopscience.iop.org/article/10.1088/2632-2153/aba947/pdf)'을 통해 통합하며 위에 언급한 모든 방법론들의 성능을 뛰어넘었습니다. 다만 고분자(polymer)에 본 방법론을 적용시키기 위해서는 고분자의 특수한 제약 조건들을 고려해야 하기에 이와 관련하여 확장(개선)되어야 할 부분이 남아있습니다.

그리하여 본 논문에서는 단순히 분자(molecule) 설계를 넘어서 polymer-specific syntax-directed VAE를 개발하여 극도의 열 및 전기 안전성을 가지는 고분자를 찾아내고자 합니다. VAE 이전 연구에서 polymer 발견에 사용된 적은 있지만, SMILES의 문법(grammar)이 설정된 chemical fragments\*로 인해 제한되어 chemical space에 의해 제한되거나, polymer의 대용으로 large molecule의 생성에만 의존하는 경우였습니다.

chemical fragments\*: 화학 물질의 생물학적 또는 물리적 화학적 성질을 모델링하는 데 유용한  화학 구조의 구성 요소

논문에서 제안하는 모델은 기존과 다르게 syntax와 semantics 제한을 모두 통합한 VAE를 polymer 생성에 사용하였습니다. 이러한 모델을 성공적으로 구축하기 위해 기존 SMILES grammar의 중요한 부분들을 수정하였으며, 고분자에 적합한 polymer-specific semantics를 모델에 포함시켰습니다. 결과적으로 VAE가그럴듯한 고분자 SMILES를 생성해낼 수 있도록 디코딩(decoding) 과정에 1) polymer syntax 2) CFG의 parse tree를 사용한 semantics 3) stochastic lazy attributes가 포함되었습니다. 

고분자 VAE 훈련에 잇따르는 또 하나의 문제는 사용 가능한 많은 양의 데이터가 없다는 점입니다. Molecule은 약 250,000개의 데이터가 구비되어있는 반면 고분자 VAE에 사용할 수 있는 데이터는 약 12,000개로, VAE를 훈련시키기에는 충분하지 않은 양입니다. 본 문제를 해결하고자 250,000개의 가상 데이터를 생성하기 위해 retro-synthetic(역합성) 아이디어를 이용하여 우리는 수천 개의 분자 'building blocks' 또는 문헌에서 얻은 12,000개의 합성된 polymer로부터 파생된 조각으로 구성된 약 250,000개의 대표적인 가설 데이터 세트를 생성한다. 

\*Retrosynthetic analysis (역합성 분석) : 목표 분자를 합리적인 유기 합성 반응에 근거를 둔 추론 방법을 통하여 작용기를 변화시키거나 또는 단계적으로 작은 분자 (단순한 구조를 가진 분자)로 분해하여 합성법을 설계하는 수단.

\*building blocks: 가상 분자 조각 또는 실제 화학 화합물을 설명하는 화학 용어

본 접근법의 성능을 입증하기 위해, 이 모델을 3가지 극한의 조건 - 1) 높은 온도 2) 높은 전기장 3) 높은 온도와 높은 전기장 - 하에 안정적인 성능을 보일 것이라 예측되는 수백개의 polymer 후보들을 발견해내는 데에 사용합니다. 이때 높은 온도와 높은 전기장이 polymer의 유리전이온도 ($T\_g$)와 bandgap($E\_g$)과 각각 연관되어 있다는 추가적인 가정을 해봅니다. 특히 구체적으로 우리가 여기서 고려하는 3가지 극한 조건에 대해서 다음과 같은 목표를 설정합니다: 1) $T\_g > 600K$ 2) $E\_g > 65eV$ 3) $T\_g > 500K$ and $E\_g > 4eV$.

<table style="border-collapse: collapse; width: 68.4884%; height: 103px;" border="1"><tbody><tr style="height: 18px;"><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">high temperature</span></td><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">$T_g &gt; 600K$</span></td></tr><tr style="height: 18px;"><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">high electric field</span></td><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">$E_g &gt; 65eV$</span></td></tr><tr style="height: 18px;"><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">high temperature and high electric field</span></td><td style="width: 50%; height: 18px; text-align: center;"><span style="color: #1a5490; font-family: 'Noto Sans Demilight', 'Noto Sans KR';">$T_g &gt; 500K$&nbsp;</span></td></tr></tbody></table>

높은 $T\_g$ polymer는 기계적으로 견고하며, 높은 $E\_g$ polymer는 낮은 유전손실(dielectric loss)에 대해서 광범위한 전기적 안전성을 제공합니다. 최종적인 설계 목표 - 높은 온도와 높은 전기장 안전성 - 는 특히 high-energy capacitor dielectrics에서 중요하기 때문에 위에서 언급된 조건들은 반드시 요구되어야 합니다. Polymer의 $T\_g$와 $E\_g$ 간의 inverse relation 발생 때문에, 이 최종적인 설계 목표를 이루는 것이 material에서 가장 어렵고 힘든 과제입니다.

### **Method**

**1\. Unsupervise****d Learning: Polymer Variational Autoencoder**

VAE는 input 데이터셋 X의 latent variables를 가지고 생성모델을 학습하는 framework입니다. 여기서 설계한 VAE로 decoding이 desired target region에 reshape될 수 있습니다.

**Incorporating Grammar or Syntax**

Decoder로 하여금 synthetically, semantically 유효한 polymer SMILES를 구성할 수 있도록 하는 역할을 해줍니다. NLP에서는 어떤 언어의 문법 (CFG)을 나타내기 위해서 문장이 parse tree로 변환되고, 여기서는 polymer의 SMILES를 그에 맞는 syntactic tree 구조로 변환하기 위해 사용됩니다. 변환된 구조는 VAE의 input과 output으로 사용됩니다. 

예를 들어 폴리케톤(Polyketones)의 SMILES는 \[\*\]CC(==O)\[\*\] 에 대한 parse tree는 다음과 같습니다. 

[##_Image|kage@MmKSe/btqQLkNKHwT/kMIk7BUoYPej8xccGYvXvK/img.jpg|alignCenter|data-origin-width="0" data-origin-height="0" width="411" height="524" data-ke-mobilestyle="widthContent"|Figure 6||_##]

위 parse tree를 만드는 데에 쓰인 Grammar

[##_Image|kage@tM1Ni/btqQE9z4bjD/sZSfSDMnl5CHK4kkKAo2kk/img.gif|alignLeft|data-origin-width="0" data-origin-height="0" width="612" height="NaN" data-ke-mobilestyle="widthContent"|||_##]

Parse tree는 생성규칙의 sequence에 따라서 분해되고, one-hot vector 형태로 변환이 가능합니다. 변환 시에 벡터의 각 차원은 생성 규칙(production rules) 또는 SMILES의 grammar의 terminal symbol을 의미합니다. 그러니까 각 고분자 SMILES는 $X ∈R^{T\*p}$ 형태의 matrix를 갖게 되고, 이때 p는 생성 규칙과 고분자 문법의 terminal symbol의 총 개수이며, T는 SMILES input을 parse하기 위해 적용된 production수입니다. SMILES의 길이가 모두 다르기 때문에, 같은 dimension을 유지하기 위해서 X는 $T = T\_{max}$가 될 때까지  패딩되며 (dummy numbers), 이후 X는 VAE 인코더의 입력(input)으로 사용됩니다. VAE 디코더는 이 matrix X, 또는 생성 규칙(Polymer SMILES를 의미하게 되는 것)의 재구성을 시도합니다. 구문론적으로 (syntactically) 유효한 SMILES가 생성되도록 하기 위해, 디코더는 디코딩 과정에서는 항상 유효한 생성 규칙의 하위 집합에서만 선택을 할 수 있게 됩니다. 폴리케톤(Polyketones)의 SMILES \[\*\]CC(==O)\[\*\]의 syntactic tree를 나타낸 Figure 6를 살펴보면 앞서 설명한 과정에 대한 것을 그림을 통해 이해할 수 있습니다. 이 트리는 위의 Grammar에 포함된 생성 규칙을 모든 leaf node가 terminal symbol을 가질 때까지 재귀적으로 적용시켜 만들어낸 트리입니다.

**Incorporating Semantics**

CFG를 적용시켜 사용하는 것은 구문론적으로 (syntactically) 유효한 SMILES를 생성한ㄴ 것은 보장해주나, 유효한 SMILES를 생산하기 위해서는 정확한 semantics(의미론)과 화학적 규칙 또한 통합되어야 합니다. 예를 들어 SMILES \[\*\]C, \[\*\]C(→O)C\[\*\]와 \[\*\]cccc\[\*\]는 모두 구문론적으로는 맞지만, 두번째 polymer chain end가 비어있다는 점, O 원자와의 비현실적인 삼중 결합, 그리고 각각 1번으로 번호가 매겨진 ringbond가 닫히지 않았다는 점에서 의미론적으로, 즉 화학적으로, 유효하지 않습니다. 

본 논문에서는 이전에 제안된 SD-VAE 모델을 polymer 영역으로 확장합니다. SD-VAE의 주된 아이디어는 속성 문법의 개념을 사용하여, 디코더로 하여금 의미론적으로 유효한 생산 규칙의 더 작은 부분 집합에서 선택하도록 강요함으로써 CFG 트리의 생성을 규제 혹은 제한하는 것입니다.  SD-VAE는 CFG 트리의 생성과 함께 속성 문법 검사를 수행할 수 있는 "lazy attribute"의 개념을 도입하는 일반적인 방법을 제안했다. 본 논문에서는 생성된 SMILES가 두 개의 chain-ends를 갖고, 결합 유형이 일치함을 보장하는 새로운 polymerization semantics 제약 조건을 도입하여 polymer의 경우에도 사용될 수 있도록 모델을 확장하였습니다. 이 모델은 확장을 통해 분자(molecule)과 고분자(polymer)에 모두 쓰이게 할 수 있다.

Polymerization semantics를 실현하기 위해 본 연구에서는 일반적인 SD-VAE 프레임워크에서 다음 구성 요소를 구현하였습니다.

-   Polymer inherited attribute: parent-to-child 방향으로 계산됨. Polymer 생성 중에 특정 결합 유형(예: 단일 결합)으로 하나의 chain end(\[\*\])가 이미 생성되었다면, 다음에 생성된 chain end도 동일한 결합 유형을 가지도록 제약될 것이며, 정확히 두 개의 체인 엔드가 생성되도록 하는 데 사용됨.

-   Polymer synthesized attribute: child-to-parent 방향으로 계산됨. 디코딩 과정에서 첫번째 symbol \[\*\]와 그에 해당하는 결합 유형이 생성되면 이 정보를 해당 리프 노드에서 루트 노드로 전달하기 위해 사용됨.

-   Polymer stochastic lazy attribute: 이 속성은 SD-VAE의 맥락에서 독점적으로 도입된 것. 합성 속성 (synthesized attribute), 즉 \[\*\]와 관련된 결합 유형 계산을 지연시키면서 \[\*\]의 존재만 기억하기 위해 사용. 일단 실제 \[\*\]가 생성되면 어떤 결합 유형인지 알게 될 거고, synthesized attribute은 이 정보를 트리 위로 전달하기 위해 사용될 것. 나중에 inherited attribute는 일치하는 다른 \[\*\] 생성에 사용됨.

위와 같은 고분자 semantics 이외에도 분자 시스템에서 일반적으로 적용되 semantics  또한 포함시켰습니다.   
전체적으로 보면 다음과 같은 semantics(의미론) 집합이 사용되었습니다.

-   Polymer Chain: 체인 끝을 나타내는 정확히 2 \[\*\]가 있어야 하며, 시작과 끝과 관련된 결합 유형\[\*\]도 일치해야 합니다.
-   Valency constraints: 각 원자와 관련된 최대 결합 수를 제한합니다.
-   Ringbond consistency: ringbonds는 쌍으로 구성되어야 하며 서로 겹칠 수 있습니다.
-   Aromatic: 원자 type은 ring의 일부일 때만 방향성을 가질 수 있습니다.

**VAE Architecture**

Encoder: 3-layer one-dimensional convolution neural networks (CNNs)에 이어 fully connected dense layer으로 이루어진 network 사용. output은 출력은 reparametrization trick을 통해 분포 q\_\\theta(z|x)의 평균과 분산을 얻는 데 사용.

Decoder: 3-layer gated recurrent unit (GRU)-based recurrent neural network (RNN)에 이어 p\_{\\theta} (x|z)확률을 내기 위한 softmax activation affine layer 가 사용됨

VAE 코드는 모두 Pytorch로 구현되었습니다.- $T\_{max}$ 값: 512로 설정- 각각 latent 차원이 64, 128인 두 개의 VAE 모델들이 Adam 최적화 알고리즘을 사용하여 train - Epoch 수: 가상 데이터셋에서 얻은 2만개의 SMILES, 즉 validation set에 대한 모델의 성능에 따라 결정됨

**2\. Supervised Learning: Gaussian Process Regression (GPR)**

Polymer VAE를 훈련시킨 후, encoded latent representations를 통해 property들을 학습하기 위해서 GPR을 사용합니다. GPR은 Bayesian framework를 사용하며, 여기서 Gaussian process는 공분산(또는 커널) 함수를 사용하여 통합된 available training set과 이전 확률 분포(Bayesian Prior)를기반으로 functional mapping f(z) → y 을 얻기 위해 사용됩니다. 이 작업을 위해 일반적으로 다양한 재료 문제에 적합한 세 개의 Hyperparameter ($σ\_f$ $σ\_l$,and $σ\_n$)를 가진 Squared Exponential Kernel이 선택되었습니다.

**3\. Training Datasets**

VAE와 같은 심층 생성 모델을 사용할 때는, training 과정이 효과를 보려면 큰 데이터셋이 필요합니다. 분자의 경우 그러한 대규모 데이터 세트가 사용 가능하지만, polymer에 대해서는 문헌에 실험적으로 보고된 12,000개에 대한 정보만 얻을 수 있었습니다. 따라서, 이 초기 데이터 세트를 사용하여 더 큰 가상의 SMILES 데이터셋을 만들기 위해 다양한 polymer "Building Block"을 찾기 위한 역합성 원리의 사용에 기초하여 가상 polymer SMILES 데이터셋을 만들었습니다. 2개에서 7개에 이르는 다양한 수의 빌딩 블록을 결합하여 총 약 250,000개 (25만개)의 데이터셋을 마련하였으며, validation set으로 쓰일 20,000 SMILES를 제외하고는 모두 training data로 사용하였으며, validation error가 가장 낮을 때의 model parameter가 선정되었습니다. 

GPR 속성 예측 모델은 경험적으로 알려진 $T\_g$를 사용하여 훈련되었으며 이론적으로 각각 4997개와 963개의 데이터 포인트를 포함하는 $E\_g$ 데이터 세트를 계산했습니다. $T\_g$ 데이터셋은 실험 보고서에서 수집되었지만, $E\_g$ 데이터셋은 Polymer의 단일 체인에 대해서 DFT로 계산된 bandgap 값들로 구성되었습니다. 이때 polymer에 대한 $E\_g$ 값을 계산하는 것은 중요하지 않은 작업이며 DFT 계산 값은 합리적인 추정치로만 간주할 수 있습니다. 다만 그럼에도 불구하고, 과거의 연구에서 볼 수 있듯 몇몇 일반적인 polymer에 대한 DFT 계산값과 경험적으로 측정된 $E\_g$ 값 사이의 좋은 상관관계를 시사

### **Conclusion**

잠재적인 물질의 공간은 거의 무한대이기 때문에, desired property를 가지는 물질을 찾는 것은 다루기 힘든 문제입니다. 머신러닝의 도입으로 바람직한 특성을 가진 새로운 재료의 설계가 가속화 및 개선되었지만, 여전히 대부분의 방법은 미리 결정된 물질 후보 목록을 선별하여, 이 목록에서 물질과 property의 매핑(materials-to-property mappings)을 학습하는 것에 초점이 맞춰져 있습니다. 이러한 접근 방식은 탐색된 재료 공간의 측면에서 제한적일 뿐만 아니라 비효율적입니다. 이러한 방법을 대신하는 'inverse problem'을 해결하는 방법은, 우리가 원하는 물질에 대한 property를 예측하는 것이 아닌, 반대로 우리가 원하는 property에 대한 물질을 생성하는 방법입니다. 예를 들어 본 연구에서는 각각 세 가지 극한의 상황 - thermal extremes, electrical extremes, thermal and electrical extremes- 에서의 polymer를 설계합니다. 

본 연구는 연속적인 잠재 공간에 대한 폴리머를 인코딩하고 디코딩하기 위해 VAE를 사용하는 것에 기초하고 있으며, desired property를 가진 새로운 polymer를 이 공간에서 검색할 수 있게 합니다. 이때 autoencoder는 polymer SMILES를 가지고 훈련되며, SMILES 문법과 Polymer semantics (의미론), CFG parse trees, 그리고 SD-VAE에서 등장하는 stochastic lazy attributes를 통합하여 autoencoder의 성능을 단번에 향상시킵니다. 이를 통해 의미 있는 latent representation을 구성할 수 있으며, 디코더의 효율성이 개선되어 유효한 SMILES를 생성해내는 데에 도움이 됩니다.

Polymer latent representation을 사용하여$T\_g$.$E\_g$ 두 가지 property에 대한 지도 학습 GPR 모델이 구축되고, 원하는 $T\_g$.$E\_g$ property를 가진 폴리머를 설계하기 위해 열거(enumeration)와 생성(generation) 방식이 적용되었습니다. Enumeration 방식은 GPR property 예측을 통해 설계 기준을 충족하는 polymer 후보를 선별했고, 이 후보들은 GPR 속성 예측 모델을 사용하여 필요한 property에 해당하는 latent points를 찾는 과정에서 발견한 latent neighborhoods를 식별함으로써 생성(generation) 방식을 추진하는데 도움을 주었습니다. 이후 디코더는 관련된 polymer를 찾는 데 사용되었습니다. 두 가지 접근법 중에서 (inverse problem를 해결하는) 생성적 설계 접근법이 훨씬 더 낮은 계산으로 우수한 속성을 가진 polymer 후보를 발견했습니다. 우리가 제안한 방식은 상당히 일반적이며 원하는 기능 세트를 가진 중합체를 발견하는 데 적용될 수 있지만, 이 두 가지 방법을 모두 사용하여, 우리는 다른 유전적 용도를 위한 수백 개의 잠재적 polymer를 제안했습니다. 또한, 보다 복잡한 property set를 가진 polymer의잠재 공간에서 표적 검색(targeted search)을 수행하는 데 사용될 때. 생성 설계의 이점이 더욱 분명해질 것으로 기대됩니다.

Results and Discussion

Polymer Variational Autoencoder

Polymer Design
