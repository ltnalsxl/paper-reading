# Discovering Relationships between OSDAs and Zeolites through Data Mining and Generative Neural Networks

Zach Jensen, Soonhyoung Kwon, Daniel Schwalbe-Koda, Cecilia Paris, Rafael Gómez-Bombarelli, Yuriy Román-Leshkov, Avelino Corma, Manuel Moliner, and Elsa A. Olivetti

_ACS Cent. Sci._ 2021

Publication Date:April 16, 2021

[https://doi.org/10.1021/acscentsci.1c00024](https://doi.org/10.1021/acscentsci.1c00024)

자연어 처리를 통해 자동으로 추출된 문헌 데이터에서 WHIM featurization 와 생성 모델링(generative modeling)을 사용하여 OSDA와 zeolite 사이의 상호 작용을 모델링하는 논문

#### **Abstract**

OSDA는 micro- 및 mesoporous\* 물질의 합성에 중요한 역할을 한다 (특히 제올라이트의 경우). OSDA의 활발하고 방대한 사용 범위에도 불구하고, 연구자들은 유기 분자(organic molecule)가 특정 zeolite에 대한 OSDA로 작용할 수 있는지를 예측하기 위해 합성  heuristic, 또는 계산 비용이 많이 드는 기술에 의존하고있다. 이는 연구자들이 OSDA와 zeolite 프레임워크와의 상호작용을 잘 이해하지 못하고 있음을 의미한다. 본 논문에서는 다공성 물질(porous materials)에 대한 5,663개의 합성 경로로 구성된 포괄적인 데이터베이스를 사용하여 일반화된 OSDA-zeolite 관계에 대한 data-driven 접근법을 취한다. DB 생성을 위해 자연어 처리 및 텍스트 마이닝 기술을 사용하여 1966년에서 2020년 사이에 출판된 과학 문헌에서 OSDA, 제올라이트의 phases 및 gel chemistry 데이터를 추출했고, WHIM(weighted holistic invariant molecular) descriptor를 사용하여 OSDA의 structural featurization을 진행했다.  문헌에서 추출한 정보를 통해 OSDA를 다양한 유형의  cage-based, small-pore zeolites와 연관시킬 수 있었다. 마지막으로, zeolite 구조와 gel chemistry가 주어졌을 때, "잠재적 OSDA"로써 제안할 수 있는 새로운 분자를 생성하는 생성신경망(generative neural network)를 사용했다. 이 생성 모델을 CHA 및 SFW 제올라이트에게 적용하여 현재 실무에서 사용되는 OSDA 후보에 대해 여러 개의 대체 OSDA 후보를 생성한다. 이러한 분자들은 모델이 물리적으로 의미 있는 예측을 생성한다는 것을 보여주기 위해 분자 역학 시뮬레이션을 통해 추가로 검증되었다. 본 논문에서 제안한 모델은 OSDA 공간을 자동으로 탐색하여 새로운 OSDA 후보를 찾는 데 필요한 시뮬레이션 또는 실험 횟수 및 양을 줄일 수 있다. 

#### **Introduction**

Zeolites 및 관련 Zeotype 재료는 다양한 산업 응용 분야에서 광범위하게 사용되는 microporous 재료이다. 250개 이상의 공인된 zeolite 구조가 존재하지만, 제올라이트의 nucleation과 crystallization과 관련된 정확한 메커니즘은 아직 완전히 이해되지 않았으며,  이는 원하는 제올라이트 단계의 사전 예측을 정확하고 어렵게 만드는 원인. 이로 인해 새로운 제올라이트 구조의 발견은 역사적으로 축적된 인간의 지식과 화학적 직관에 의해 안내된 시행착오 합성 방법론에만 기초해왔다. 제올라이트 형성에 영향을 미치는 것으로 알려진 변수로는 types and amounts of framework atoms, mineralizing agents, inorganic/organic structure directing agents가 있다.

유기 구조 지시제(OSDA) 분자는 제올라이트 합성에 중요한 역할을 한다. 특히 OSDA의 크기(size), 유연성(flexibility), 친수성(hydrophilicity) 및 충전은 무엇보다도 zeolite의 crystallization kinetics\*와 phase specificity에 중요한 역할을 한다. 그래서 연구자들은 특정 제올라이트 구조에 대한 후보 OSDA를 제안하기 위해 밀도 함수 이론과 분자 역학 등을 포함한 계산 접근 방식 (computational approach)을 사용해 왔지만, 이러한 접근 방식은 일반적으로 단일 제올라이트(single zeolite) system으로 제한될 뿐만 아니라 계산 비용이 많이 들고 pure silica systems에 초점을 맞춘다는 한계점이 있었다. 이에 산업적으로 관련된 촉매 반응의 전이 상태를 모방하기 위한 OSDA의 ab initio 설계를 포함하는 전략이 최근 들어 등장했지만, 이 기법 역시 계산 비용이 많이 드는 밀도 함수 이론 계산에 의존하여 광범위한 구현을 방해한다. 기존 전략법들이 이와 같은 한계점을 지니기 때문에 OSDA 설계를 진전시키기 위해서는 더 효율적이고 포괄적인 새로운 모델링 접근법을 개발해야 한다.

데이터 기반 접근 방식은 다공성 물질을 연구하기 위해 사용되어 왔지만, 데이터 기반 제올라이트 합성 연구는 범위가 제한적이며 시스템의 복잡성을 파악할 수 없는 지나치게 단순한 OSDA-zeolite 상호 작용에 의존한다. 기계 학습(ML)과 데이터 마이닝은 화학 공간의 제한된 영역, OSDA 없는 제올라이트 시스템 및 인터제올라이트 변환을 포함한 OSDA-zeolite 상호 작용을 명시적으로 모델링할 필요가 없는 연구에 사용되어 왔다. 이러한  OSDA-zeolite 상호 작용을 모델링하여 시도된 연구들을 통해서, OSDA 표현을 분자량과 같은 기본 속성으로 단순화하거나 단일 zeolite 구조로 제한하여 OSDA-zeolite 관계를 더 잘 모델링하기 위해 더 진보된 ML 기술과 더 큰 데이터 세트가 필요하다는 것을 알 수 있었다.

문헌 데이터로부터 OSDA-zeolite 쌍의 포괄적인 데이터 세트를 얻을 수 있고, 최근 들어 문헌으로부터 화학적인 정보를 추출하는 자연어 처리(NLP) 프레임워크가 활발하게 개발되었기 때문에 문헌 추출 데이터와 OSDA-zeolite relationship을 모델링하기 위해 고급 ML 기법이 결합되면 데이터 기반 zeolite 연구의 범위가 확대될 수 있다. 뿐만 아니라 ML은 또한 다공성 물질과 유기 분자에 대한 inverse design(역설계)을 가능하게 한다.Generative neural network를 이용한 역설계를 통해 이 분야에서의 약물 발견, property optimization, 합성 예측 및 분자 설계를 포함한 많은 응용 분야에서 성공한 바 있다. 이러한 모델은 일반적으로 훈련 데이터를 다차원 가우스 분포로 압축하고 샘플링된 벡터로부터 재구성함으로써 organic molecule들의 latent representation을 학습한다. 그런 다음 latent space를 탐색하여 support distribution와 유사한 새로운 organic molecule을 생성할 수 있다. 이 새로운 샘플들은 SMILES 형식과 같은 표준 분자 표현으로 변환된다. 최근의 모델들은 분자의 physical and chemical properties로부터 직접 분자를 생성하도록 train되어 왔다. 이 과정에서 연구원들은 원하는 property을 입력하고 그것들을 가진 분자를 생성할 수 있게 된다. 이 모델은 특정 제올라이트 구조와 gel chemistry에서 유기 분자를 생성하도록 모델을 훈련시킴으로써 제올라이트 데이터에 이 모델을 fit할 수 있다.

본 논문에서는 데이터 기반 접근 방식(data-driven approach)을 사용하여 OSDA,  qualitative gel chemistry 및 resulting zeolite 구조 사이의 관계를 조사한다. NLP 및 텍스트 마이닝을 통해 추출한 OSDA, 제올라이트(zeolite) 및 정성적 합성 데이터셋을 제시한다. OSDA의 structural descriptors를 사용하여 화학적 공간의 차원을 줄이고 특정 제올라이트의 crystallization에서 발견되는 추세를 시각화한다. 마지막으로, 추출된 데이터셋에 대해 훈련된 생성 신경망 모델(generative neural network model)을 적용하여 특정 제올라이트 구조와 합성 조건에 따라 condition된 잠재적인 OSDA 분자를 제안한다. 데이터, 모델 및 결과 분석은 신속한 제올라이트 연구 진행, 그리고 제올라이트(zeolite) 연구 파이프라인 개발을 위한 중요한 첫 단계를 밟을 수 있는 연구 기회를 제공한다.

#### **Results and Discussion**

**Extacted Dataset**

본 논문에서는 자동화된 기법으로 전체 zeolite 문헌에서 OSDA, 화학 및 zeolite 단계의 데이터 세트를 추출한다. 이 데이터 세트는 1966년부터 2020년까지 15개 이상의 출판사와 140개 저널에서  추출한 데이터이다. 이 데이터 세트에는 758개의 개별 OSDA 분자 및 205개의 제올라이트 단계에 대한 정보가 포함되어 있으며 다양한 합성 경로 중 3,085개는 전통적인 제올라이트(순수 Si, Si/Al, Si/B 프레임워크), 1,274개의 데이터 포인트는 알루미늄 인산(AlPO) 타입의 물질, 나머지 1,304개의 데이터 포인트는 게르마늄 기반 및 금속 함유(Ti, Sn 또는 Zroric 구조)를 포함한 추가적인 zeotypes를 설명한다. 

![자동으로 추출된 데이터셋 overview](https://user-images.githubusercontent.com/68283760/115491861-b679d380-a29b-11eb-822d-bdddd3a22f23.png)

(a-c) 데이터 집합의 모든 OSDA에 대한 평균 분자량, OSDA 특이성 및 전하 분포. (a)는 데이터 세트에서 OSDA의 평균 분자량 분포. 더 큰 OSDA가 AlPO형 물질 대신 제올라이트 합성과 관련이 있음을 보여준다. (b)는 대부분의 OSDA는 높은 특수성을 가지고 있어 5개 미만의 제올라이트 phase를 생성하는 반면, 소수의 특이치는 20개 이상의 phase를 만들 수 있다는 것을 보여준다. 

(d) 제올라이트 구조를 가장 많이 만드는 것으로 알려진 5개의 OSDA

(e) 가장 유기적인 분자를 사용하여 실험적으로 얻은 제올라이트는 MFI, MTW, \*BEA, CHA, MOR

OSDA 내의 이온 전하의 수와 분포는 nucleation and crystallization  과정에 중요한 역할을 하며, 알칼리화의 유무와 함께 음전하의 이질체를 특정 프레임워크 위치에 배치하는 데 중요하다. 

**Literature-Mined OSDA와 Zeolite의 상관관계(Correlations)**

**OSDA와 zeolite의 관계를 설명하기 위한 보다 유용한 structural descriptors의 필요성 제기**

OSDAs와 resulting zeolite framework의 복잡한 상호작용(interactions)으로 인해, molecular volumes나 flexibility같은 단순한 parameter들은 특정 OSDA와 제올라이트 구조 사이의 연결을 설명하기에 충분하지 않다. 우리는 OSDA의 nonstructural property들과 제올라이트 구조에 대한 영향도 고려하지만 이러한 특징들은 OSDA-zeolite 관계를 적절하게 설명해주지는 못한다. 분자 모양 일치를 포착하려면 분자의 크기(size)뿐만 아니라 folding and charge distributions와 같은 다른 structural 특징도 포착할 수 있는 보다 유용한 structural descriptors가 필요하다. 

**WHIM Representation 사용 및 PCA를 이용한 차원 축소**

WHIM descriptor는 분자의 3차원적 적합성에 의존하는 크기, 모양, 대칭 및 원자 분포에 대한 정보를 포함하고, 분자의 유연성에 따라 다른 적합성들은 대폭적으로 다른 WHIM 표현을 가질 수 있다. 예를 들어, 긴 선형 분자는 뻗어나가거나 접을 수 있으며, 두 개의 다른 3차원 표현을 제공한다. RDkit을 통해 얻은 geometries를 사용하여 평균 적합성 WHIM 설명자를 계산하여 다른 적합성에 기초하여 각 분자의 다양한 3차원 표현이 가능하다. 이때 WHIM은 고차원 descriptor이기 때문에 공간 차원을 줄이고 데이터 세트의 모든 OSDA를 시각화하기 위해 주성분 분석(PCA)을 사용한다. 첫 번째 주성분(PCA 1)은 분자의 가장 긴 축차원과 전역 차원에 해당하는 WHIM 특성을 포함하기 때문에 분산의 58%를 차지하고 분자의 부피와 상관관계가 있다. 두 번째 주성분(PCA 2)은 분산의 15%를 차지하며 전역 차원 및 대칭 특징으로 구성된다. 세 번째 주성분(PCA3)은 분산의 13%를 차지하며 세 개의 axial dimensions 와 global dimensions OSDA의 제한된 화학적 공간을 강조하기 위해 OSDA를 전체 유기 공간의 표본과 비교하는 PCA WHIM 시각화를 진행한다. 이러한 차원 축소된 WHIM descriptors를 통해 OSDA와 zeolite 간의 관계를 볼 수 있다.

WHIM descriptor featurization과 PCA 분석을 통해 OSDA-zeolite 상관 관계를 평가하기 위해 5개의 cage-based small-pore zeolites(LEV, CHA, AEI, LTA, AFX)를 선택한다. cage-based 제올라이트는 OSDA의 3차원 구조와 cage의 형태 사이에 강한 상관관계를 가지고 있어 분석 대상으로 적합하다. Gel composition은 OSDA와 제올라이트 사이의 관계에도 영향을 미치기 때문에 추출된 qualitative synthesis gel information을 사용하여 선택된 제올라이트용 기존의 제올라이트 chemistry versions만 포함하도록 데이터 세트를 필터링한다.

![5개의 cage-based small-pore zeolites 시스템에서 사용되는 OSDA 분자의 주성분 분석(PCA) WHIM 벡터 표현. PCA 1, 2, 3은 처음 세 개의 주요 구성 요소 축을 나타내고  회색 점은 문헌에서 추출한 모든 OSDA를 나타낸다. ](https://user-images.githubusercontent.com/68283760/115491900-c5f91c80-a29b-11eb-9ef9-0b5ac45325de.png)

생성 모델링(Generative Modeling)을 통한 새로운 후보 OSDA 제안

생성 신경망 모델을 적응시켜 OSDA로 사용하기 위한 대체 유기 분자를 제안한다. 이 모델은 추출된 문헌 데이터에 대해 훈련되어 zeolite phase 및 gel chemistry가 입력으로 주어진 OSDA 분자에 대한 SMILES 문자열을 출력한다.  이 모델을 사용하면 문헌에서 특정 제올라이트 구조에 대한 새로운 OSDA를 발견하는 프로세스로 관계를 마이닝할 수 있다. 모델을 사용하기 위해서는 대용량 데이터가 필요하며, 이 모델은 추출된 데이터 세트의 크기로 활성화된다.

생성된 OSDA 분자 필터링

생성된 OSDA 분자를 필터링하기 위해 CHA, N, N, N-트리메틸라다만탐모늄(TMADA)에 대해 현재 산업에서 사용되는 OSDA와 비교한다. 우리는 TMADA의 PCA 감소 WHIM 좌표를 취하고 처음 세 개의 주성분 축을 따라 범위의 5%를 차지하는 점을 중심으로 타원체를 생성한다. 그림을 살펴보면 408개의 생성된 CHA 분자 중 57개는 TMADA 타원체 내에 있다. 앞서 CHA용 문헌에 보고된 11개 OSDA와 기타 토폴로지에 대해 보고된 24개 OSDA도 이 범위에 속한다. 타원체 내의 유기 분자는 TMADA와 구조적으로 유사할 것으로 예상되므로 아래에서 자세히 살펴보면서 적절한 대체 OSDA가 될 수 있다. 생성된 분자는 잠재적 OSDA 후보를 지능적으로 예측할 수 있는 방식으로 도메인 및 데이터 정보 화학 noise 를 OSDA 공간에 추가하는 모델의 능력을 보여준다.

![Chazeolite의 문헌 OSDA와 생성된 OSDA 비교. WHIM 공간 내에서 강조 표시된 점은 세 PCA 차원에서 모두 타원체 내에 포함되는 OSDA를 나타내고, 생성된 OSDA는 CHA 합성에 사용된 문헌에서 발견된 OSDA와 유사한 많은 특징을 포함하고 있다. ](https://user-images.githubusercontent.com/68283760/115491935-d3160b80-a29b-11eb-8d70-c9658dfa571f.png)

(a) PCA WHIM 공간에 나머지 OSDA와 비교하여 TMADA(파란색 별과 함께 표시됨)의 위치를 표시

(b) 이를 둘러싼 타원형을 볼 때 확대된 파란색 사각형은 CHADA를 포함

(c) (d) 주황색 정사각형에는 타원 내에 속하는 CHA에 대해 생성된 OSDA의 예가 포함

또한 본 연구에서는 CHA보다 덜 연구된 zeolite에 대해 생성된 OSDA 후보를 평가한다. 이때는 molecular mechanic simulations를 사용하여 생성된 각 분자의 결합 에너지를 SFW 프레임워크로 계산하여 CHA와 같이 알려진 분자와 비교하지 않고 모델의 예측 능력을 측정했을 때, 모델에 의해 생성된 많은 분자가 SFW에 적합한 OSDA 후보임을 보여주었다. 생성된 분자 중 60%는 문헌 OSDA 범위 내에서 결합 에너지를 가지고 있었고, 흥미롭게도 7%의 추가 결합 에너지는 알려진 OSDA보다 낮다. 

![문헌에서 얻어서 우리의 모델에 의해 생성된 SFW에 대한 OSD로, 차원축소된 WHIM공간에서 SFW를 위한 분자의 생성 결과를 보여주는 그림.](https://user-images.githubusercontent.com/68283760/115491963-dc9f7380-a29b-11eb-88f3-724192bde6bf.png)

(a) SFW(파란색)를 만드는 것으로 알려진 세 개의 OSDA와 우리의 모델에 의해 생성된 다섯 개의 선택된 분자(주황색)에 대한 PCA가 감소된 WHIM 위치.

(b) 3개의 문헌 OSDA에 대해 SFW로 최소 conformer 결합에너지\*

(c) 5개의 선택된 생성 분자에 대해 SFW 결합 에너지

이 모델이 SFW zeolite에 대해 물리적으로 의미 있는 제안을 생성할 수 있지만 성능 한계가 있다. 화학성분이 모델에 미치는 영향을 비교하기 위해 SFW zeolite chemistry를 사용하여 LAU OSDA를 생성해보았고, 예상한 바와 같이 유사한 화학성분이 있으면 생성된 분포가 서로 더 가깝게 이동하는 것을 볼 수 있었다. 또한 전체 zeolite 문헌에서 생성된 OSDA와 OSDA의 SFW 결합 에너지를 비교했을 때 분포가 매우 유사하다는 것을 확인했으며 모델이 각 zeolite 시스템에 특정한 OSDA를 예측하는 능력이 제한적일 수 있음을 보여준다.  이 모델은 적절한 OSDA로 알려진 분자를 포함하는 문헌 분포와 일치할 수 있지만, 이러한 결과는 화학 noise를 OSDA 공간에 주입함에도 불구하고 문헌을 통해 알려진 OSDA의 성능과 일치시킴으로써 다른 OSDA 제안을 생성할 수 있는 모델의 능력을 보여준다.이 결과는 생성된 분자가 구조적으로 유사한 여러 제올라이트 시스템에 대해 OSDA로서 잠재력을 가질 수 있음을 나타낸다. 이 모델을 결합 에너지 시뮬레이션과 결합하면 예측 OSDA를 선택하는 데 도움이 될 수 있다.

**Conclusion**

본 연구에서는 zeolite 문헌에서 OSDA, zeolite phase 및 gel chemistry에 대한 데이터를 추출하고 featurization을 진행했으며 결과적으로 zeolite 합성 parameter에 대한 크고 포괄적인 데이터 세트를 생성했다. WHIM이라는 3차원 feature을 사용하여 OSDA structure과 zeolite phase 사이의 관계를 밝혀내기 위해 이 문헌 데이터를 추출하고, 생성 신경망(generative neural network)을 사용하여 OSDA, zeolite gel chemistry 간의 상호 작용을 모델링하였다.

**추후 연구 진행: gel chemistry 정량적 data 추출,  atomistic simulations**

이 모델은 결합 에너지를 가진 새로운 유기 분자를 제안할 수 있으며 알려진 문헌과 비교할 수 있다. 본 논문에서 추출한 모든 화학 데이터는 정성적이지만, gel chemistry에 대한 정량적 정보가 제올라이트 합성에 대한 더 자세한 열역학적 및 운동학적 연구를 가능하게 할 수 있기 때문에, 이 정보를 추출하여 추후 연구가 진행된다면 더 좋을 것. 또한 추가적인 원자론적 시뮬레이션은 실험적으로 대상 제올라이트를 형성할 가능성이 가장 큰 OSDA를 선택하는 데 도움이 될 수 있다. 위 두 가지 데이터 확보와 시뮬레이션 방법론을 통해 본 연구보다 진보된 신속한 시뮬레이션 기법 및 실험 최적화가 결합되어 높은 처리량 제올라이트 합성 파이프라인을 개발할 수 있을 것이다.

## Experimental Section

**Data Extraction, Processing, and Validation**

-   350만 건 이상의 화학 및 재료 과학 저널들 중 “zeolite”, “osda”, “aluminophosphate”, “molecular sieve”를 포함한  제올라이트 물질과 관련된 키워드를 스캔하여 약 9만 건의 논문 채택
-   이 corpus들로부터 정규식과 도메인별 키워드 일치를 사용하여 각 논문의 표와 합성 섹션에서 OSDA 이름, 제올라이트 구조, 합성 젤 구성 요소를 추출
-   회수율이 매우 높은 원시 제올라이트 데이터를 추출하는 데는 효과적
-   여러 실험 샘플을 포함하는 논문의 경우 특정 OSDA-제올라이트-합성 시스템을 결정하기는 어렵다는 한계점 -> 그래서 추출된 합성 경로의 무결성과 정확성을 보장하기 위해 수동으로 검사됨

**Data Normalization and Featurization**

-   OSDA 분자와 제올라이트 구조를 설명하기 위해 다양한 화학 이름을 사용하기 때문에 추출된 텍스트 데이터를 정규화해야 다른 naming 체계가 최종 representation에 영향을 미치지 않는다
-   OSDA: CIRpy(Cemical Identifier Resolver) Python 패키지가 IUPAC Name과 SMILES 문자열을 결정하는 데 사용됨. 만약 OSDA 이름이 논문에서 주어지지 않았다면, 한 화학 전문가가 정확한 IUPAC Name과 SMILES 결정
-   각 제올라이트 재료는 알려진 재료 목록을 통해 국제 제올라이트 협회(IZA) 코드로 표준화됨. IZA 데이터베이스에 없는 자료에는 올바른 세 개의 문자 코드가 수동으로 할당
-   OSDA molecules featurization에 RDKit 사용. SMILES 표현 외에도 molecular volume, surface area, charge, and WHIM descriptors 등 유기 분자의  physical and chemical properties
-   Average WHIM descriptors는 모든 conformer의 WHIM descriptors로부터 계산됨
-   WHIM vectors에 대한 PCA 차원축소는 WHIM feature이 모두 정규화된 후에 scikit-learn을 통해 진행
-   Zeolite 구조는 IZA database로부터 얻어낸 데이터를 사용하여 featurization 진행: framework density, maximum ring size, channel dimensionality, maximum included volume of a sphere, accessible volume, maximum channel area, and minimum channel area
-   Qualitative gel chemistry는 one-hot encoding됨 (Si, Al, Ge, P, Ti, B, Ga, Fe, Na, K, F, additional framework elements, additional cations, extra solvents in addition/instead of water, acid used in the synthesis, and other synthesis components)

Generative OSDA Model

> Kotsias, P.-C.; Arús-Pous, J.; Chen, H.; Engkvist, O.; Tyrchan, C.; Bjerrum, E. J.Direct steering of de novo molecular generation with descriptor conditional recurrent neural networks. _Nature Machine Intelligence_2020, _2_, 254– 265,  DOI: 10.1038/s42256-020-0174-5

위 논문에서 사용한 모델 차용. 

-   유기 분자 descriptors를 사용하는 대신, 제올라이트 및 합성 gel properties를 input으로 사용
-   추출된 각 합성 경로에 대해, 제올라이트 및 합성은 featurization되고 입력 벡터에 연결된 반면, OSDA의 SMILES 문자열은 출력에 해당
-   데이터 확대:  생성 모델의 정확도를 높이기 위해

_모델(Neural Network Model)_

6 dense layers of 256 units with ReLU activation

input: unidirectional LSTM layers consisting of 256 units

output: a feedforward dense layer with 35 units having a softmax activation

Batch normalization used on the first dense layers and LSTM layers

Keras v2.2.4 with TensorFlowGPU v2.0.0 backend

trained using  two NVIDIA Titan Xp GPUs

_데이터 분할(Data Split)_

a variety of train/test splits: 4 different training and testing splits

  1) 8:2 random split

  2) 결과적으로 CHA가 나온 모든 데이터를 test  set서 분리하도록

  3) 앞선 2)와 동일하지만 AEI 사용

  4) 최종 모델은 테스트 세트 없이 전체 데이터 세트에 대해 훈련됨

1, 2, 3은 모델의 성능을 평가하는 데 사용, 4는 특정 제올라이트 시스템인 CHA와 SFW를 살펴보는 데 사용

_훈련 (train)_

100 epochs

Adam optimizer

batch size = 128

learning rate scheduler (initial rate of 10–3 for 50 epochs)

each epoch was exponentially decayed down to 10–6

  
_Molecular mechanics simulations_

General Utility Lattice Program (GULP) 사용

Dreiding force field는 제올라이트와 OSDA 사이의 상호작용을 모델링하는 데 사용

 SFW 제올라이트에 대한 초기 구조는 국제 Zeolite Association 데이터베이스에서 검색되고 Sanders-Leslie-Catlow force 필드를 사용하여 최적화됨

SFW에서 OSDA 도킹은 기본 매개 변수를 사용하여 VOIE 패키지를 사용하여 수행됨

Pose optimization은 volume이 일정한 상태에서 수행되었고, 결합 에너지는 frozen pose method에 따라 계산됨

\*mesoporous materials: 직경이 2~50nm인 모공을 함유한 물질

\*crystallization kinetics: crystallization kinetics(운동화)는  용액의 결정화 과정에서 발생하는데, 대체로 다음 두 가지 과정에서 발생한다. 1) nucleation kinetics 2) growth kinetics.

\*binding energy(결합에너지): 분자의 결합을 끊어 구성입자로 분리하는 데 필요한 에너지
