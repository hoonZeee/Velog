<h4 id="http-캐시">HTTP 캐시</h4>
<p>브라우저 또는 중간 프록시에서 HTTP 응답을 저장하여 재사용하는 방식</p>
<h4 id="server-캐시">Server 캐시</h4>
<p>서버 내부 또는 다수 서버 간 데이터 캐시</p>
<p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/4f867a43-dc71-4219-950c-56b0c24a0cf6/image.jpeg" /></p>
<h3 id="http-캐시-예시">HTTP 캐시 예시</h3>
<h4 id="private-cache">private Cache</h4>
<blockquote>
<p>Client가 <a href="https://jihoon.kr/logo.png%EC%97%90">https://jihoon.kr/logo.png에</a> 접속했을 때,
브라우저는 이 이미지를 받아오고 다음 요청부터는 로컬에 저장된 이미지를 그대로 사용함.</p>
</blockquote>
<ul>
<li>캐시 제어 방식 <ul>
<li>응답 Header에 Cache-Control: private, max-age=86400 → 1일간 브라우저에 캐싱</li>
<li>혹은 ETag를 사용해서 변경 여부 확인 후 재사용</li>
</ul>
</li>
</ul>
<h4 id="shared-cache-프록시-캐시--cdn-캐시">Shared Cache (프록시 캐시 / CDN 캐시)</h4>
<blockquote>
<p><a href="https://jihoon.kr/main.css">https://jihoon.kr/main.css</a> 파일은 Cloudflare CDN이 중간에서 대신 응답함.
같은 파일을 여러 사용자들이 요청할 경우, 서버에 매번 가지 않고 CDN에서 복사본 반환.</p>
</blockquote>
<ul>
<li>캐시 제어 방식<ul>
<li>Cache-Control: public, max-age=31536000
→ 1년간 CDN에서 보관 가능</li>
</ul>
</li>
</ul>
<h3 id="server-캐시-예시">Server 캐시 예시</h3>
<h4 id="local-cache">local Cache</h4>
<blockquote>
<p>백엔드 서버에서 정류장 리스트를 자주 가져올 때,
Java 서버에서 Guava Cache로 메모리에 저장해두고 DB를 매번 조회하지 않음.</p>
</blockquote>
<pre><code>LoadingCache&lt;String, BusStop&gt; busStopCache = CacheBuilder.newBuilder()
    .expireAfterWrite(10, TimeUnit.MINUTES)
    .build(...);</code></pre><h4 id="global-cache">Global Cache</h4>
<blockquote>
<p>서비스가 EC2 여러 대로 스케일 아웃 됐을 때,
어느 서버든 동일하게 Redis에 저장된 정류장 정보를 꺼내 씀.</p>
</blockquote>
<pre><code>// Redis에 캐싱
redisTemplate.opsForValue().set(&quot;busStop:화명&quot;, json, Duration.ofMinutes(30));</code></pre><table>
<thead>
<tr>
<th>구분</th>
<th>사용자 단</th>
<th>서버 단</th>
</tr>
</thead>
<tbody><tr>
<td><strong>Private Cache</strong></td>
<td>브라우저 내 이미지, JS, CSS 캐시</td>
<td>❌</td>
</tr>
<tr>
<td><strong>Shared Cache</strong></td>
<td>CDN에서 파일 응답</td>
<td>❌</td>
</tr>
<tr>
<td><strong>Local Cache</strong></td>
<td>❌</td>
<td>서버 메모리 캐싱</td>
</tr>
<tr>
<td><strong>Global Cache</strong></td>
<td>❌</td>
<td>Redis 등 여러 서버 공유 캐싱</td>
</tr>
</tbody></table>
<p>그렇다면 GavaCache랑 Redis (로컬과 글로벌 캐시의 차이점은?)</p>
<table>
<thead>
<tr>
<th>구분</th>
<th>Guava Cache</th>
<th>Redis</th>
</tr>
</thead>
<tbody><tr>
<td>위치</td>
<td><strong>서버 메모리</strong></td>
<td><strong>별도 서버 (네트워크)</strong></td>
</tr>
<tr>
<td>속도</td>
<td>빠름 (RAM)</td>
<td>느릴 수 있음 (네트워크 I/O)</td>
</tr>
<tr>
<td>범위</td>
<td>해당 서버만</td>
<td>여러 서버에서 공유 가능</td>
</tr>
<tr>
<td>사용 예</td>
<td>단일 서버 / 임시 데이터</td>
<td>세션, 글로벌 데이터 공유 등</td>
</tr>
</tbody></table>
<p>병행전략</p>
<ul>
<li>Reids만 쓰면 네트워크 IO가 생길 수 있다.</li>
<li>Guava만 쓰면 → 서버 1대에서만 유효하다.(스케일 불가)</li>
</ul>
<p>그렇다면 Guava에서 로컬 캐시를 먼저 확인하고 Redis로 글로벌 캐시를 공유하면 해결할 수 있다. &gt; 속도 + 일관성</p>
<h4 id="코드-예시">코드 예시</h4>
<pre><code>@Autowired
private RedisTemplate&lt;String, BusStop&gt; redisTemplate;

private LoadingCache&lt;String, BusStop&gt; localCache = CacheBuilder.newBuilder()
    .expireAfterWrite(10, TimeUnit.MINUTES)
    .build(new CacheLoader&lt;&gt;() {
        @Override
        public BusStop load(String key) throws Exception {
            // 1. Redis 확인
            BusStop fromRedis = redisTemplate.opsForValue().get(key);
            if (fromRedis != null) return fromRedis;

            // 2. DB 조회
            BusStop fromDb = dbRepo.findById(key);
            if (fromDb != null) {
                redisTemplate.opsForValue().set(key, fromDb, Duration.ofMinutes(30)); // Redis 저장
            }
            return fromDb;
        }
    });

// 호출
BusStop result = localCache.get(&quot;busStop:화명&quot;);
</code></pre>