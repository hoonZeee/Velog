<p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/ae337ff2-eb1e-4bb4-bff9-6b95a3e8a716/image.png" /></p>
<p>호스트에 도커를 설치하면 기본적으로 172.17.0.xxx IP를 컨테이너에 순차적으로 할당이 된다.</p>
<p>내부 IP에 아무런 설정을 하지 않았다면 외부에서 접근할 수 없을 것이다.
외부에 IP를 노출하고 싶다면, eth0를 호스트 IP에 바인딩 해주어야한다.</p>
<p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/a42e9922-a08c-490d-8b9e-e41f562a9c74/image.jpeg" /></p>
<p>외부와의 네트워크연결은 컨테이너마다 eth0에 대응되는 veth라는 가상 네트워크 인터페이스를 호스트에 생성함으로써 이루어진다.</p>
<p>즉, eth0에 대응되는 vethXXXX이라는 이름의 veth interface와 브릿지 네트워크에 컨테이너의 interface가 바인딩되는 형태로 통신한다.</p>
<p>veth 인터페이스는 사용자가 직접 생성할 필요는 없고 컨테이너가 생성될 때 도커 엔진이 자동으로 생성한다. 도커 컨테이너가 실행될 때 네트워크 드라이버를 따로 지정하지 않으면 docker0라고 하는 브릿지 네트워크를 default로 사용한다. 이 브릿지 네트워크의 역할은 veth와 호스트의 eth0의 다리 역할을 한다.</p>
<p>veth란?</p>
<blockquote>
<p>리눅스의 virtual ethernet interface, virtual eth를 의미. 랜카드에 연결된 실제 네트워크 인터페이스가 아니라 가상으로 생성한 네트워크 인터페이스이다. 일반적인 네트워크 인터페이스와는 달리 패킷을 전달받으면, 자신에게 연결된 다른 네트워크 인터페이스로 패킷을 보내주는 식으로 동작하기 때문에 항상 쌍(pair)로 생성해줘야 한다. 한 쪽에서 다른 쪽으로 패킷을 전송할 수 있으며, 한 쪽에 다운된 경우 나머지 한 쪽도 정상적으로 기능하지 않는 것이 특징이다. 도커에서는 실행중인 컨테이너 수 만큼 veth로 시작하는 인터페이스가 생성된다.</p>
</blockquote>
<h3 id="도커-컨테이너와-호스트-포트-연결-명령어">도커 컨테이너와 호스트 포트 연결 명령어</h3>
<blockquote>
<p>$ $ docker run -p [HOST IP:PORT]:[CONTAINER PORT] [CONTAINER NAME]</p>
</blockquote>
<hr />
<h2 id="bridge-network">Bridge Network</h2>
<p>포트를 연결해 외부에 노출하는 방식이 바로 이 bridge network이다.</p>
<p>*<em>내부적으로 다양한 IP를 갖게 되는데 가상의 IP를 갖게되면 일어나는 일을 보자.
*</em></p>
<ol>
<li><p>동일한 host 내 두 개의 프로세스 A, B 가 있다고 해보자. A 에서 B 로 http 호출을 보낸다고 했을 때, 이 패킷에는 A와 B가 공유하는 host의 IP가 적혀있을 것이다. 이 경우 호출은 컴퓨터가 기본적으로 내장한 별로의 router 프로세스를 통해 전달된다. A의 호출을 라우터가 읽고 그대로 B로 전달하는 셈이다. 
<img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/5f72052b-b1ef-4d42-abb0-52901d1fd880/image.png" /></p>
</li>
<li><p>반면 가상의 IP를 부여받거나해서 A와 B가 서로 다른 IP를 가졌을 경우엔 호출 패킷엔 서로 다른 Origin/ Destination IP가 적혀진다. 이때의 호출은 라우터로만 처리할 수 없고 네트워크 시스템의 해석을 거쳐야 한다. 쉽게 말해 OSI의 아래 계층을 통과해야 한다. </p>
</li>
</ol>
<p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/c0e806f7-8d5b-41d2-b71e-c297bf8e1bd8/image.png" /></p>
<hr />
<h2 id="도커-내부-통신-체계">도커 내부 통신 체계</h2>
<p> 하나의 host 내 구동되는 컨테이너들은 서로 분리된 망을 지닌다
<img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/b6089ed1-6c23-4cbe-abf9-318fa5c6e286/image.png" /></p>
<p> 이는, 컨테이너만 세 개 띄웠는데 거의 클러스터나 다름 없는 운영이 필요하다. 서로 의존되는 컨테이너와 컨테이너를 일일이 연결해주는 일은 매우 복잡한 일이다. 이런 문제를 해결하고자 기본적으로 같은 bridge를 사용하는 컨테이너들이 공유하는 gateway를 제공한다.  Gateway 주소는 관례상 subnet 범위의 첫번째 값(x.x.0.1)을 사용한다. </p>
<p> <img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/4ce2119d-d874-4eca-97b9-92f43b93cd3e/image.png" /></p>
<p>  이런 Gateway의 존재로 컨테이너간 통신을  원활하게 관리할 수 있다. Gateway를 고려한 컨테이너 간 통신 체계는 다음과 같다. 컨테이너는 서로 직접 연결하는 것이 아니라 이처럼 Gateway 를 통해서 통신한다. 또한 host와 컨테이너간의 통신도 Gateway를 거친다. 컨테이너들이 서로 다른 주소를 사용하고 있음에도 port가 겹치면 안되는 이유가 여기에 있다. 또한 외부에서 컨테이너에 접근할 때 컨테이너의 주소가 아닌 host의 주소를 사용하는 이유도 여기에 있다. 외부에서 컨테이너에 직접 접근은 불가능하다. </p>
<p>과 유연성이 좋음</p>
<hr />
<h3 id="host-네트워크">Host 네트워크</h3>
<ul>
<li>Docker 컨테이너가 <strong>호스트의 네트워크 스택을 그대로 공유</strong></li>
<li>컨테이너가 <strong>Host의 IP, 포트 공간을 그대로 사용</strong></li>
<li>-p 바인딩 없이도 컨테이너에서 포트를 열면 <strong>곧바로 외부에서 접근 가능</strong></li>
<li><strong>veth 인터페이스, docker0 브릿지 등을 사용하지 않음</strong></li>
<li>NAT, 라우팅이 없어서 오버헤드가 적고 <strong>성능이 더 좋음</strong></li>
<li>컨테이너가 호스트 네트워크를 통째로 가져가므로 <strong>보안/포트 충돌 위험 존재</strong></li>
</ul>
<h3 id="비교-표">비교 표</h3>
<table>
<thead>
<tr>
<th>항목</th>
<th>Bridge 모드</th>
<th>Host 모드</th>
</tr>
</thead>
<tbody><tr>
<td>컨테이너 IP</td>
<td>고유한 가상 IP (ex. 172.17.x.x)</td>
<td>Host IP 공유</td>
</tr>
<tr>
<td>격리성</td>
<td>높음</td>
<td>낮음</td>
</tr>
<tr>
<td>외부 노출</td>
<td><code>-p</code> 바인딩 필요</td>
<td>바로 노출됨</td>
</tr>
<tr>
<td>성능</td>
<td>중간 (NAT 경유)</td>
<td>빠름 (NAT 없음)</td>
</tr>
<tr>
<td>포트 충돌</td>
<td>컨테이너끼리는 없음</td>
<td>Host와 충돌 가능</td>
</tr>
<tr>
<td>보안</td>
<td>더 안전</td>
<td>위험 가능성 있음</td>
</tr>
<tr>
<td>사용 목적</td>
<td>일반적 서비스 운영</td>
<td>네트워크 성능 중시, 실험적 환경 등</td>
</tr>
</tbody></table>