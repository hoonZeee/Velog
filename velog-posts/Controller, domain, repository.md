<p>Spring MVC에서 코드를 유지보수하기 위한 패키지 구조</p>
<h4 id="controller">controller</h4>
<ul>
<li>사용자의 요청을 받고, 처리한 후 응답을 반환하는 역할</li>
<li>클라이언트(웹, 모바일)와 직접 소통하는 계층</li>
<li>@Controller, @RestController 사용</li>
<li>보통 Service와 Repository를 호출해서 데이터를 처리한 후, 결과를 View(HTML)나 JSON으로 반환함.</li>
<li><em>Controller는 직접 데이터를 저장하거나 조회하지 않고, Service를 통해 처리함!</em></li>
</ul>
<h4 id="domain">domain</h4>
<ul>
<li>애플리케이션에서 다루는 &quot;핵심 데이터&quot;를 관리하는 패키지</li>
<li>Entity, VO, DTO 같은 객체가 들어감</li>
<li>보통 데이터베이스와 연결되는 @Entity 클래스를 포함함</li>
<li>이 패키지에 있는 클래스는 데이터를 표현하는 역할을 하고, 일부는 비즈니스 로직을 포함할 수도 있음</li>
<li><em>도메인 패키지는 데이터 모델을 정의하고, 서비스와 저장소에서 활용하는 역할을 함!</em></li>
</ul>
<h4 id="repository">repository</h4>
<ul>
<li>데이터를 저장하고 조회하는 역할</li>
<li>Spring Data JPA, JDBC, MyBatis 등의 기술을 사용해 DB와 연결</li>
<li>@Repository 또는 JpaRepository를 사용</li>
<li>데이터를 다루는 CRUD(Create, Read, Update, Delete) 기능을 제공</li>
</ul>
<blockquote>
<p>Controller - Service - Repository - Domain 흐름</p>
</blockquote>
<ol>
<li>사용자가 회원가입 요청 (<code>POST /members/new?name=Jihoon</code>)</li>
<li>Controller가 요청을 받음 (<code>MemberController.create()</code>)</li>
<li>Service가 비즈니스 로직 실행 (<code>MemberService.join()</code>)</li>
<li>Repository가 DB에 데이터 저장 (<code>MemberRepository.save()</code>)</li>
<li>응답을 반환하고, 화면에 출력됨</li>
</ol>
<h4 id="코드-흐름">코드 흐름</h4>
<pre><code>// Controller
@PostMapping(&quot;/members/new&quot;)
public String create(@RequestParam String name) {
    Member member = new Member();
    member.setName(name);
    memberService.join(member);  // Service 호출
    return &quot;redirect:/members&quot;;
}

// Service
public Long join(Member member) {
    memberRepository.save(member);  // Repository 호출
    return member.getId();
}

// Repository
public Member save(Member member) {
    store.put(member.getId(), member);  // 데이터 저장
    return member;
}
</code></pre><h4 id="요약">요약</h4>
<p>✔ Controller → 사용자의 요청을 받아 Service에 전달
✔ Service → 비즈니스 로직 실행 &amp; Repository 호출
✔ Repository → DB와 연결해서 데이터 저장/조회
✔ Domain → 데이터를 표현하는 객체 (Entity, DTO, VO)</p>