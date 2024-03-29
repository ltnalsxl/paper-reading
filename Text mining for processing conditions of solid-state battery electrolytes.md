# Text mining for processing conditions of solid-state battery electrolytes
Rubayyat Mahbub, Kevin Huang, Zach Jensen, Zachary D.Hood, Jennifer L.M.Rupp, Elsa A.Olivetti
Keywords: Lithium battery, Solid-state electrolyte, Li7La3Zr2O12, Text mining, Natural language processing, Material informatics

보다 안전한 차세대 리튬이온배터리를 찾는 것은 넓은 Electrochemical window와 높은 이온 전도성(10-3~10-4Scm-1), 광범위한 고용량 전극 소재로 양호한 화학적 안정성 때문에 solid-state electrolytes 전해질(SSE) 개발에 동기를 부여했다. 그럼에도 불구하고, complete cell assembly의 성능을 희생시키지 않고 SSE의 처리 조건을 최적화하는 것은 여전히 어려운 일이다. 과학 문헌에서 추출한 인사이트를 통해 SSE의 처리 프로토콜 최적화를 가속화할 수는 있지만 수천 개의 journal articles에 흩어져 있는 정보를 모두 끌어모으고 분석하는 과정은 지루하고 시간이 많이 소요된다. 본 연구에서는 황화물과 산화물 기반 LISE의 처리에 정보를 얻는 머신러닝과 자연어 처리 기술을 사용하여 수만 개의 학술 출판물에 걸쳐 재료 합성 매개 변수를 자동으로 컴파일하는 텍스트 마이닝( text mining)의 역할을 보여준다. 또한 SSE를 cell assembly에 통합하는 동안 인터페이스 복잡성을 줄일 수 있는 Li7La3Zr2O12(LLZO) 등 높은 잠재적 산화 기반 Ligarnet 전해액의 저온 합성에 대한 통찰력을 얻는다. 이 연구는 실험 설계 중 가설을 통해 all-solid-state Li 금속 배터리의 개발을 촉진하는 데에 텍스트 마이닝과 데이터 마이닝이 사용되는 것을 보여준다.

- SSE 합성 매개 변수를 컴파일하기 위해 자동화된text mining pipeline 이 적용된다.
- 황화물과 산화제 기반 SSE의 처리 조건에 대한 인사이트를 분석한다.
- SSE의 처리 온도를 낮추기 위한 dopants의 트렌드를 조사한다.

### 1. Introduction 
가전·자동차 산업의 수요 증가 -> 최근 몇 년 사이 리튬이온배터리(LIB)의 성능과 안전성 개선이 큰 관심을 끌고 있다.
LIB에서 액체 전해질을 대체하기 위해 많은 고체 상태의 Li-ion 도체가 연구됨. 유기 액체 전해질을 유사한 Li-ion 전도성을 가지며 잠재적으로 더 넓은 전기 화학적 안정성 창을 가진 비인화성 고체 전해질(SSE)로 대체하면 광범위한 고에너지 밀도 음극과 순수 리튬 양극에 걸쳐 호환성을 높인 안전한 Li-ion 배터리로 이어질 수 있다. 특히 SSE는 모두 §10-4 Scm-1의 실내 온도 이온 전도성을 가지고 있으며, 높은 에너지 밀도를 가진 광범위한 전극 재료와 양호한 전기 화학적 호환성을 보여주며, 다양한 전자 및 자동차 응용 분야에 매력적인 소재이다. 
SSE의 처리 프로토콜 최적화에 대한 대부분의 문헌 보고서는 처리에 관한 과학 문헌의 수동 해부와 함께 직관 및 시행착오 연구를 기반으로 한다.이러한 접근 방식은 전해질 음극의 단일 구성 요소 및 재료 표준에 대한 세라믹 합성의 느린 최적화와 문헌의 데이터를 수동으로 컴파일하는 어려움 때문에 제한적이다. 우리는 이전에 발표된 합성 프로토콜을 추출하고 구성하면 SSE(특히 Ligarnet)에 대한 저온 처리를 지향하는 더 많은 정보에 입각한 실험이 가능하고 음극-전자의 공동 조립을 위한 최상의 프로토콜을 향하도록 안내할 수 있다는 가설을 세웠다. 사실, 과학 문헌에는 SSE에 대한 수천 개의 기사가 포함되어 있지만, 아직 어떤 종류의 기능 데이터베이스에도 색인되거나 정리되지 않았다.
실험 및 처리 매개 변수(예: 작동 단계, 온도 등)의 데이터베이스를 컴파일하는 데 필요한 문헌을 채굴하기 위해 자연어 처리(NLP)와 기계 학습(ML) 기술을 사용한다. 본 연구에서는 차세대 배터리 애플리케이션을 위해 SSE의 저온 합성에 대한 이해를 개선하기 위해 과학 출판물에서 표적 세라믹 처리 정보를 자동으로 추출하고 구조화한다. 본 연구의 초점은 배터리 SSE를 위한 유사한 데이터베이스를 개발하는 것이다.

### 2. Methods
관련 저널 기사의 corpus는SSE 재료 시스템에 대한 합성 데이터베이스를 생성하기 위해 자동화된 text mining pipeline을 통해 얻을 수 있다. 이 pipeline으로 다운로드된 저널 기사로부터 일반 텍스트를 추출하고 규칙 기반(rule based) 및 기계 학습 접근 방식을 사용하여 실험 합성 섹션을 자동으로 식별한다. 다음으로, 각 레시피 단락의 단어들은 문장의 각 단어에 대한 중요한 합성 키워드(예: 재료 이름, 작업 이름, 양, 조건 등)를 예측하기 위해 학습된 신경망을 사용하여 토큰화되고 분류된다. 다음으로 분류된 토큰은 데이터베이스 개체로 조립되며, 이는 합성 trend를 추출하기 위해 추가로 데이터 마이닝 작업을 거치게 된다.

### 4. Conclusion
텍스트 마이닝은 SSE의 낮은 온도 프로세스를 달성하는 데 사용되는 전구체뿐만 아니라 처리 온도, 특정 프로세스 모두를 신속하게 요약하기 위한 실행 가능한 프레임워크를 제공할 수 있다. 이로 인해 SSE 처리의 열 예산 감소에 대한 지침을 제공하며, 이 지침은 특히 세라믹/세라믹 결합을 형성하기 위해 세심한 제어가 필요한 새로운 캐소드를 ASSB에 통합하는 방법을 열 수 있다. 향후 연구는 차세대 ASSB에서 전기 화학적 안정성 창, 질량 적재 및 기타 성능 기반 측정 기준을 신속하게 식별할 수도 있게 될 것이다.
