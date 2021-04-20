# Discovering Relationships between OSDAs and Zeolites through Data Mining and Generative Neural Networks

Zach Jensen, Soonhyoung Kwon, Daniel Schwalbe-Koda, Cecilia Paris, Rafael Gómez-Bombarelli, Yuriy Román-Leshkov, Avelino Corma, Manuel Moliner, and Elsa A. Olivetti

_ACS Cent. Sci._2021

Publication Date:April 16, 2021

[https://doi.org/10.1021/acscentsci.1c00024](https://doi.org/10.1021/acscentsci.1c00024)

자연어 처리를 통해 자동으로 추출된 문헌 데이터에서 WHIM featurization 와 생성 모델링(generative modeling)을 사용하여 OSDA와 zeolite 사이의 상호 작용을 모델링하는 논문

**Abstract**

OSDA는 micro- 및 mesoporous\* 물질의 합성에 중요한 역할을 한다 (특히 제올라이트의 경우). OSDA의 활발하고 방대한 사용 범위에도 불구하고, 연구자들은 유기 분자(organic molecule)가 특정 zeolite에 대한 OSDA로 작용할 수 있는지를 예측하기 위해 합성  heuristic, 또는 계산 비용이 많이 드는 기술에 의존하고있다. 이는 연구자들이 OSDA와 zeolite 프레임워크와의 상호작용을 잘 이해하지 못하고 있음을 의미한다. 본 논문에서는 다공성 물질(porous materials)에 대한 5,663개의 합성 경로로 구성된 포괄적인 데이터베이스를 사용하여 일반화된 OSDA-zeolite 관계에 대한 data-driven 접근법을 취한다. DB 생성을 위해 자연어 처리 및 텍스트 마이닝 기술을 사용하여 1966년에서 2020년 사이에 출판된 과학 문헌에서 OSDA, 제올라이트의 phases 및 gel chemistry 데이터를 추출했고, WHIM(weighted holistic invariant molecular) descriptor를 사용하여 OSDA의 structural featurization을 진행했다.  문헌에서 추출한 정보를 통해 OSDA를 다양한 유형의  cage-based, small-pore zeolites와 연관시킬 수 있었다. 마지막으로, zeolite 구조와 gel chemistry가 주어졌을 때, "잠재적 OSDA"로써 제안할 수 있는 새로운 분자를 생성하는 생성신경망(generative neural network)를 사용했다. 이 생성 모델을 CHA 및 SFW 제올라이트에게 적용하여 현재 실무에서 사용되는 OSDA 후보에 대해 여러 개의 대체 OSDA 후보를 생성한다. 이러한 분자들은 모델이 물리적으로 의미 있는 예측을 생성한다는 것을 보여주기 위해 분자 역학 시뮬레이션을 통해 추가로 검증되었다. 본 논문에서 제안한 모델은 OSDA 공간을 자동으로 탐색하여 새로운 OSDA 후보를 찾는 데 필요한 시뮬레이션 또는 실험 횟수 및 양을 줄일 수 있다.

**Introduction**

Zeolites 및 관련 Zeotype 재료는 다양한 산업 응용 분야에서 광범위하게 사용되는 microporous 재료이다. 250개 이상의 공인된 zeolite 구조가 존재하지만, 제올라이트의 nucleation과 crystallization과 관련된 정확한 메커니즘은 아직 완전히 이해되지 않았으며,  이는 원하는 제올라이트 단계의 사전 예측을 정확하고 어렵게 만드는 원인. 이로 인해 새로운 제올라이트 구조의 발견은 역사적으로 축적된 인간의 지식과 화학적 직관에 의해 안내된 시행착오 합성 방법론에만 기초해왔다. 제올라이트 형성에 영향을 미치는 것으로 알려진 변수로는 types and amounts of framework atoms, mineralizing agents, inorganic/organic structure directing agents가 있다.

유기 구조 지시제(OSDA) 분자는 제올라이트 합성에 중요한 역할을 한다. 특히 OSDA의 크기(size), 유연성(flexibility), 친수성(hydrophilicity) 및 충전은 무엇보다도 zeolite의 crystallization kinetics\*와 phase specificity에 중요한 역할을 한다. 실제로 zeolite 커뮤니티 내의 실험 휴리스틱은 OSDA 크기를 증가시키고 OSDA 경직성을 증가시키는 특수성을 증가시킨다.이러한 휴리스틱에서 OSDA를 설계하는 것은 여전히 어렵지만, 제올라이트 단계의 r형성은 여전히 어렵다. (12) 연구자들은 밀도 함수 이론과 분자 역학 등을 포함한 계산 접근법을 사용하여 특정 제올라이트 구조에 대한 후보 OSDA를 제안했지만, 이러한 접근 방식은 일반적으로 단일 제올라이트 시스템으로 제한된다.m, 계산 비용이 많이 들고 순수한 실리카 시스템에 초점을 맞춘다. 보다 최근에는 산업적으로 관련된 촉매 반응의 전이 상태를 모방하기 위한 OSDA의 "abinitio" 설계가 주목을 받았지만(18,19) 이 기술은 계산 비용이 많이 드는 밀도 함수 이론 계산에 의존하여 광범위한 구현을 방해한다. 의심할 여지 없이, 우리는 OSDA 설계를 진전시키기 위해 더 효율적이고 포괄적인 새로운 모델링 접근법을 개발해야 한다.

\*mesoporous materials: 직경이 2~50nm인 모공을 함유한 물질

\*crystallization kinetics: crystallization kinetics(운동화)는 용액의 결정화 과정에서 발생하는데, 대체로 다음 두 가지 과정에서 발생한다. 1) nucleation kinetics 2) growth kinetics.
