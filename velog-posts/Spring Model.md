<p><em>_spring입문.. model객체의 의문점이 생겼다.
고놈 참 HashMap이랑 하는 일이 똑같네. 크게 뭔차이가 있을까?</em>
<br /><br /></p>
<ul>
<li><p>Spring Model객체는 HashMap과 매우 유사하다.
기능 : Controller --&gt; View로 데이터를 전달할때 쓰는 객체이다.</p>
</li>
<li><p>내부적으로 Map&lt;String, Object&gt; 구조를 가짐</p>
</li>
</ul>
<pre><code>@GetMapping(&quot;hello-mvc&quot;)
public String helloMvc(@RequestParam(&quot;name&quot;) String name, Model model) {
    model.addAttribute(&quot;name&quot;, name);  // &quot;name&quot;이라는 key에 값 저장
    return &quot;hello-template&quot;;  // hello-template.html로 데이터 전달
}
</code></pre><pre><code>&lt;p th:text=&quot;'hello ' + ${name}&quot;&gt;&lt;/p&gt;
</code></pre><blockquote>
<p><a href="http://localhost:8080/hello-mvc?name=Jihoon">http://localhost:8080/hello-mvc?name=Jihoon</a></p>
</blockquote>
<p>hello Jihoon</p> 

<ul>
<li><p>Spring이 자동으로 관리하며, 뷰 템플릿(Thymeleaf, JSP 등)에서 쉽게 사용할 수 있도록 보장해 줌.</p>
</li>
<li><p>model.addAttribute(&quot;key&quot;, value); 를 사용하여 데이터를 추가하고,
뷰에서 ${key}로 접근할 수 있음.</p>
</li>
<li><p>Spring이 자동으로 관리하므로 뷰와 쉽게 연결되고 유지보수하기 좋음</p>
</li>
</ul>
<p>결론 :  즉, Model은 Spring이 자동으로 관리하는 HashMap 같은 객체지만, 더 편리하고 강력하게 동작한다!</p>