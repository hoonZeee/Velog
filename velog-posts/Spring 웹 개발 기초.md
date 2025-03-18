<ol>
<li>정적 컨텐츠</li>
<li>MVC와 템플릿 엔진</li>
<li>API<br />

</li>
</ol>
<h3 id="정적-컨텐츠">정적 컨텐츠</h3>
<p>원리
1.관련 컨트롤러를 찾아본다.
2. 없으면 resources:static.html이 있는지확인
3. 반환~</p>
<h3 id="mvc와-템플릿-엔진">MVC와 템플릿 엔진</h3>
<p>MVC : Mode, View, Controller</p>
<pre><code>@GetMapping(&quot;hello-mvc&quot;)
    public String helloMvc(@RequestParam(&quot;name&quot;) String name, Model model) {
        model.addAttribute(&quot;name&quot;, name);
        return &quot;hello-template&quot;;
    }</code></pre><pre><code>&lt;p th:text=&quot;'hello ' + ${name}&quot;&gt;hello! empty&lt;/p&gt;
</code></pre><p>외부에서 파라미터를 받겠다. @Request Param
model(name:value --&gt; ${name}
hello!empty는 default값이지만 th:text속성땜에 의미없음(th덮어쓰기)</p>
<h3 id="api">API</h3>
<ul>
<li>JSON</li>
<li>서버끼리 통신</li>
</ul>
<pre><code>@GetMapping(&quot;hello-string&quot;)
    @ResponseBody
    public String helloString(@RequestParam(&quot;name&quot;) String name) {
        return &quot;hello&quot; + name;
    }</code></pre><blockquote>
</blockquote>
<h4 id="responsebody">@ResponseBody</h4>
<p>http body부분에 데이터를 직접 추가해주겠다!는 의미
이전에는 템플릿을 통해 view로 출력해주는 형식이라면
이건 입력한 그대로 보여준다..
<a href="http://localhost:8080/hello-string?name=spring">http://localhost:8080/hello-string?name=spring</a>!! &lt;이런 형태
<br />
이 어노테이션이 적혀있는데  어라..
//public Hello helloApi(@RequestParam(&quot;name&quot;) String name)
객체로 반환된다면??</p>
<ul>
<li>기존에는 viewResolver가 반응했지만 이건 HttpMessageConverter가 반응한다 //default값은 json방식이 된다(기본정책)</li>
</ul>