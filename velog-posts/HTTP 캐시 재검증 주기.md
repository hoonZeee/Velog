<h4 id="http-캐시의-동작">HTTP 캐시의 동작</h4>
<ol>
<li>클라이언트(브라우저)가 서버로 리소스 요청</li>
<li>응답에 캐시 관련 헤더가 함께 옴 (예: Cache-Control, ETag, Last-Modified)</li>
<li>브라우저는 해당 리소스를 캐시에 저장함</li>
<li>이후 다시 요청할 때:</li>
</ol>
<ul>
<li>캐시가 유효하면 재사용 (200 OK (from disk cache) 같은 응답)</li>
<li>유효성 확인이 필요하면, 서버에 재검증 요청을 보냄 → 아래 흐름</li>
</ul>
<h4 id="재검증이란">재검증이란?</h4>
<blockquote>
<p>클라이언트 캐시에 저장된 리소스가 여전히 최신인지 서버에 확인하는 과정</p>
</blockquote>
<p>조건부 요청으로 이루어진다.</p>
<ul>
<li>브라우저가 서버에 다음과 같은 요청을 보낸다.<pre><code>If-None-Match: &quot;etag-value&quot;
If-Modified-Since: Tue, 15 Jul 2025 13:00:00 GMT</code></pre></li>
</ul>
<p>서버응답</p>
<pre><code>304 Not Modified → 변경 없음 → 캐시 재사용 OK
200 OK + 새로운 콘텐츠 → 변경됨 → 새로 저장</code></pre><h3 id="재검중-주기가-0초라면">재검중 주기가 0초라면?</h3>
<h4 id="의미">의미</h4>
<ul>
<li><p>캐시를 절대 믿지 말고 항상 서버에 확인해라!</p>
</li>
<li><p>브라우저는 리소스를 캐시에 저장하더라도, 매번 서버에 재검증 요청을 보냄</p>
</li>
<li><p>결과적으로 → 트래픽은 늘고, 정확도는 매우 높음</p>
</li>
</ul>
<h4 id="단점">단점</h4>
<ul>
<li><p>서버에 매번 요청하므로 성능 저하</p>
</li>
<li><p>네트워크 비용 증가</p>
</li>
</ul>
<h4 id="사용-예">사용 예</h4>
<ul>
<li><p>민감한 데이터 (예: 로그인 정보, 잦은 업데이트가 있는 리소스 등)</p>
</li>
<li><p>API 응답이 자주 바뀌는 경우</p>
</li>
</ul>
<table>
<thead>
<tr>
<th>헤더 설정</th>
<th>의미</th>
<th>재검증 필요 여부</th>
</tr>
</thead>
<tbody><tr>
<td><code>max-age=60</code></td>
<td>60초간 캐시 사용 가능</td>
<td>X (60초 내엔 재검증 안 함)</td>
</tr>
<tr>
<td><code>max-age=0, must-revalidate</code></td>
<td>항상 재검증 필요</td>
<td>매번 서버 확인</td>
</tr>
<tr>
<td><code>no-store</code></td>
<td>캐시 자체를 하지 말 것</td>
<td>저장조차 안 함</td>
</tr>
</tbody></table>
<p><br /><br /></p>
<h4 id="재검증-주기가-0초면-매번-서버에-확인한다면-그냥-캐시-안-하는-것no-store이랑-뭐가-다를까">&quot;재검증 주기가 0초면 매번 서버에 확인한다면, 그냥 캐시 안 하는 것(no-store)이랑 뭐가 다를까??</h4>
<p>결론부터 말하면 : </p>
<ul>
<li>재검증 결과가 &quot;안 바뀜(304)&quot;이면,
브라우저는 이미 가지고 있는 캐시 리소스를 그대로 재사용하기 때문에
네트워크도 아끼고 렌더링도 빠르다!</li>
</ul>
<h3 id="비교-요약">비교 요약</h3>
<table>
<thead>
<tr>
<th>항목</th>
<th><code>Cache-Control: no-store</code></th>
<th><code>Cache-Control: max-age=0, must-revalidate</code></th>
</tr>
</thead>
<tbody><tr>
<td>저장</td>
<td>안 함</td>
<td>함</td>
</tr>
<tr>
<td>다음 요청</td>
<td>정상 GET</td>
<td><strong>조건부 GET</strong> (<code>If-None-Match</code> 등)</td>
</tr>
<tr>
<td>서버 응답</td>
<td>200 OK + 모든 바디</td>
<td><strong>304 Not Modified</strong> (바디 X)</td>
</tr>
<tr>
<td>속도</td>
<td>느린 편</td>
<td>빠른 편 (모든 데이터 전송 X)</td>
</tr>
</tbody></table>
<hr />
<h3 id="테스트-예시">테스트 예시</h3>
<h4 id="no-store"><code>no-store</code>:</h4>
<ul>
<li>매 요청마다 서버에서 갱신 정보 담은 것 받음 → 모든 데이터 자력적 전송</li>
</ul>
<h4 id="max-age0-must-revalidate"><code>max-age=0, must-revalidate</code>:</h4>
<ul>
<li>서버에 &quot;이것 없나?&quot; 물어보고,</li>
<li>304 받으면 캐시를 그대로 사용 → 빠른 회신 + 한가지 협조 조건 결정</li>
</ul>