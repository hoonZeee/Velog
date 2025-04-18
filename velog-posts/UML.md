<h3 id="umlunified-modeling-language">UML(unified Modeling language)</h3>
<ul>
<li>고객 / 개발자 간 원활한 의사소통을 위해 표준화한 대표적 객체지향 모델링 언어</li>
<li>rumhaugh, booch, jacobson등 객체지향 방법론의 장점 통합</li>
<li>인터페이스 : 클래스/컴포넌트가 구현해야하는 오퍼레이션 세트를 정의하는 모델 요소</li>
</ul>
<p>1) 사물 (Things) : 구조(개념, 물리적 요소) / 행동 / 그룹 /주해</p>
<blockquote>
<p>2) 관계 (relationship)</p>
</blockquote>
<ul>
<li>Assosiation(연관) : 두 개이상의 사물이 서로 관련</li>
<li>Aggregation(집합) : 하나의 사물이 다른 사물에 포함 - 일반적인 포함 </li>
<li>Composition(포함) : 집합 내 한 사물의 변화가 다른 사물에게 영향 -영향을 주는가?</li>
<li>Generalization(일반화) : 한 사물이 다른 사물에 비해 일반/구체적인지 표현 </li>
<li>Dependency(의존) : 사물 간 서로가 영향을 주는가? </li>
<li>Realization(실체화) : 한 객체가 다른 객체에게 오퍼레이션을 수행하도록 지정 (인터페이스 ,클래스 메서드 관계)</li>
</ul>
<blockquote>
<p>3) Diagram</p>
</blockquote>
<p>3-1) 구조,정적 다이어그램</p>
<ul>
<li>Class : 클래스 사이의 관계 및 속성 표현</li>
<li>Object : 인스턴스를 객체와 객체 사이의 관계로 표현</li>
<li>Component : 구현 모델인 컴포넌트 간의 관계 표현</li>
<li>Deployment : 물리적 요소 (HW/SW) 위치/구조 표현</li>
<li>Composite Structure : 클래스 및 컴포넌트의 복합체 내부 구조 표현</li>
<li>Package : UML의 다양한 모델요소를 그룹화<blockquote>
<p>3-2) 행위,동적 다이어그램</p>
</blockquote>
</li>
<li>Usecase : 사용자의 요구사항분석(사용자 관점)</li>
<li>Sequence : 시스템/객체들이 주고받는 메세지 표현 -시스템 객체의 메세지</li>
<li>communication : 객체들이 주고받는 메세지와 연관관계 -객체간의 메세지</li>
<li>state: 다른 객체와의 상호작용에 따라 상태가 어떻게 변하는지</li>
<li>activity : 객체의 처리 로직 조건에 따른 처리의 흐름 순서</li>
<li>timing : 객체 상태 변화와 시간 제약 명시 표현</li>
<li>interaction overview : 상호작용 다이어그램 간 제어 흐름 표현</li>
</ul>
<h4 id="ui설계">UI설계</h4>
<ul>
<li>직관성, 유효성, 학습성, 유연성</li>
<li>CLI : Command line  텍스트</li>
<li>GUI : Graphical 그래픽</li>
<li>NUI : Natural 말/행동</li>
<li>VUI : Voice</li>
<li>OUi : Oragnic 사물과 사용자 상호작용</li>
</ul>
<p>설계도구 </p>
<ul>
<li>Wireframe : 기획 초기의 대략적인 레이아웃설계</li>
<li>Story Board : 최종적 산출문서</li>
<li>Prototype : 동적 형태 모형</li>
<li>Mockup : 정적 형태 모형</li>
<li>usecase : 사용자 측면 다이어그램</li>
</ul>