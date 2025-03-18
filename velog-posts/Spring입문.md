<p>알고리즘은 JAVA로 공부하지만 실상 프로젝트는 js로 진행하고 있었다.
이미 nodeJS에 익숙해져서 잘모르는 문법은 GPT를 통해 넘어가는 딜레마..
그래서 앞으로는 스프링을 공부해서 자바 웹개발자가 되기 위해 노력하자!!</p>
<blockquote>
<p><a href="https://start.spring.io/">spring Installer</a>
스프링이 기본으로 제공하는 프레임워크를 다운받을 수 있다.</p>
</blockquote>
<ul>
<li>add dependencies를 통해 외장 라이브러리를 받을 수 있다.</li>
<li>나는 웹개발을 하기 위해 다음 라이브러리를 설치했다
<em>&emsp;Spring Web
&emsp;thymeleaf</em></li>
<li>프로젝트명과 언어를 설정하면 아주 쉽게 생성 성공!</li>
</ul>
<h4 id="스프링-어노테이션">스프링 어노테이션</h4>
<blockquote>
<p>스프링은 어노테이션이 필수다.
주석용도로만 어노테이션을 써봤지만..
스프링에서는 코드사이에서 특별한 의미,기능을 부여해준다.
어노테이션의 순서</p>
</blockquote>
<ol>
<li>어노테이션 정의</li>
<li>클래스에 어노테이션 배치</li>
<li>코드 실행 도중 reflection을 통해 추가 정보 획득 실시</li>
</ol>
<h4 id="thymeleaf">Thymeleaf</h4>
<blockquote>
<p>Thymeleaf란?</p>
</blockquote>
<ul>
<li>jsp,Freemarker처럼 사용되는 프레임워크이다.</li>
<li>백엔드에서 HTML을 동적으로 랜더링하는 용도로 사용된다.</li>
<li>타임리프는 태그에 th속성을 지정하는 방식으로 동작한다.<pre><code>@Controller
public class HelloController {
  @GetMapping(&quot;hello&quot;)
  public String hello(Model model){
      model.addAttribute(&quot;data&quot;, &quot;hello!!&quot;);
      return &quot;hello&quot;;
  }
}</code></pre></li>
<li>return의 기본경로는 resources:templates/ +{ViewName}+.html</li>
<li>data에 해당하는 value(hello)가 ${data}로 치환된다.</li>
<li>Controller의 메서드는 Model이라는 타입의 객체를 파라미터로 받을수 있다.<pre><code>&lt;body&gt;
&lt;p th:text=&quot;'안녕하세요. ' +${data}&quot; &gt;안녕하세요!&lt;/p&gt;
&lt;/body&gt;</code></pre></li>
</ul>
<h4 id="서버에서-빌드하는법">서버에서 빌드하는법</h4>
<p>jar파일 집어넣고 실행만 시키면된다..</p>
<blockquote>
<ol>
<li>프로젝트로 이동</li>
<li>./gradlew build</li>
<li>cd build</li>
<li>cd libs</li>
<li>ls -arlth</li>
<li>ex) java -jar 프로젝트명0.0.1-SNAPSHOT.jar</li>
</ol>
</blockquote>