<p>스프링부트에서 단위테스를 진행할때 
Object의 isEqual() 을 쓰기보단 
 org.assertj.core.api 에서 제공하는 Assertions 클래스를 사용한다.</p>
<p> 이 클래스의 장점이 뭘까? </p>
<p> 알고리즘 문제를 풀면서 병아리시절(지금도 병아리지만..)에는 
 == 연산자면 끝인줄 알았다.
 하지만, 같은 값 같은데? 왜 같지않다고 하지? 라는 의문에서
 == 연산자는 &quot;주소 값 비교&quot; 라는걸 알게되었고, 
 모든 오브젝트 비교 연산은 isEqual()로 진행하였다.</p>
<p> 근데, Spring 테스트 검증에선 AssertThat이라는 메서드를 사용한다!</p>
<h4 id="왜-쓸까">왜 쓸까?</h4>
<ul>
<li>.equals()는 단순히 값을 비교하고 true/false 반환.
→ 테스트 결과를 알려주지 않고 직접 로직으로 처리한다.</li>
<li>assertThat()은 테스트에서 자동으로 결과를 fail/pass로 판별해줌.
→ 실패 시 이유를 명확히 보여주고 디버깅에 도움을 준다.</li>
</ul>
<h4 id="그럼-다른-메서드는">그럼 다른 메서드는?</h4>
<pre><code>import static org.assertj.core.api.Assertions.assertThat;

@Test
void testExample() {
    String actual = &quot;hello&quot;;
    String expected = &quot;hello&quot;;

    // 값이 같은지 확인
    assertThat(actual).isEqualTo(expected);

    // null이 아닌지 확인
    assertThat(actual).isNotNull();

    // 특정 문자열 포함 여부
    assertThat(actual).contains(&quot;ell&quot;);
}</code></pre><blockquote>
<h3 id="정리">정리!</h3>
<p>역할: 단위 테스트에서 기대 값과 실제 값이 같은지, 조건을 만족하는지 검증.
문법: assertThat(실제값).isEqualTo(기대값);
장점: 가독성 좋음 (문장처럼 읽힘)
실패 시 어떤 값이 다른지 명확하게 출력됨
isNotNull(), contains(), hasSize() 등 다양한 matcher 제공</p>
</blockquote>