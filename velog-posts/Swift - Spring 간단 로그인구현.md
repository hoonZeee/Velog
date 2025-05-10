<p>ios 앱도전기 3일차..</p>
<p>유튜브 swift강의를 통해서 빠르게 기본문법만 익히고 간단한 로그인 기능을 구현 도전!</p>
<p>기초설계</p>
<ul>
<li>spring으로 백엔드 설계 </li>
<li>DB는 mysql</li>
<li>front는 swift</li>
</ul>
<p>일단 springBoot Dependency 설정!</p>
<ul>
<li>Spring Web</li>
<li>Spring Data JPA</li>
<li>MySQL Driver</li>
<li>Spring Boot DevTools</li>
<li>Lombok </li>
</ul>
<p>간단한 테스트니 편의성을 위해 이정도만..ㅎㅎ</p>
<p>intellij가 참좋은게 디비 연결이 정말 편합니다..
Query도 직접 날려 볼 수있고 환경설정도 정말 편하고..후후</p>
<p>암튼 MVC설계 바로!
일단 디렉터리 구조는 요롷게
<img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/ffce80bb-f817-4a20-87a7-d5195e9c7088/image.png" /></p>
<p>매우간단한 로그인 기능과 테스트코드까지 작성!</p>
<pre><code>package com.example.loginapi.service;

import com.example.loginapi.domain.User;
import com.example.loginapi.repository.UserRepository;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import java.util.Optional;
import java.util.HashMap;
import java.util.Map;

import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;



public class UserServiceTest {

    private UserRepository userRepository;
    private UserService userService;

    @BeforeEach
    void setUp() {
        userRepository = mock(UserRepository.class);
        userService = new UserService(userRepository);
    }

    @Test
    void 회원가입_성공() {
        when(userRepository.findByUsername(&quot;jihun&quot;)).thenReturn(Optional.empty());

        boolean result = userService.register(&quot;jihun&quot;, &quot;1234&quot;);

        assertTrue(result);
        verify(userRepository).save(any(User.class));
    }

    @Test
    void 회원가입_중복() {
        when(userRepository.findByUsername(&quot;jihun&quot;)).thenReturn(Optional.of(new User(&quot;jihun&quot;, &quot;1234&quot;)));

        boolean result = userService.register(&quot;jihun&quot;, &quot;1234&quot;);

        assertFalse(result);
        verify(userRepository, never()).save(any(User.class));
    }

    @Test
    void 로그인_성공() {
        when(userRepository.findByUsername(&quot;jihun&quot;)).thenReturn(Optional.of(new User(&quot;jihun&quot;, &quot;1234&quot;)));

        boolean result = userService.login(&quot;jihun&quot;, &quot;1234&quot;);

        assertTrue(result);
    }

    @Test
    void 로그인_실패_비밀번호불일치() {
        when(userRepository.findByUsername(&quot;jihun&quot;)).thenReturn(Optional.of(new User(&quot;jihun&quot;, &quot;wrong&quot;)));

        boolean result = userService.login(&quot;jihun&quot;, &quot;1234&quot;);

        assertFalse(result);
    }

    @Test
    void 로그인_실패_없는아이디() {
        when(userRepository.findByUsername(&quot;unknown&quot;)).thenReturn(Optional.empty());

        boolean result = userService.login(&quot;unknown&quot;, &quot;1234&quot;);

        assertFalse(result);
    }
}
</code></pre><p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/b6ed049c-e7f0-4fb7-9bd3-8368e9e1c3a4/image.png" />
순조롭다.. </p>
<p>이제 swift랑 api로 연결만 해주면된다.!!</p>
<p>login.swift코드는 아래와 같이!</p>
<pre><code>//
//  ViewController.swift
//  springLogin
//
//

import UIKit

class ViewController: UIViewController {


    @IBOutlet weak var usernameField: UITextField!

    @IBOutlet weak var passwordField: UITextField!



    @IBAction func signupButtonTapped(_ sender: Any) {
        let username = usernameField.text ?? &quot;&quot;
        let password = passwordField.text ?? &quot;&quot;
        signup(username: username, password: password)
    }

    func signup(username: String, password: String) {
        guard let url = URL(string: &quot;https://로컬주소/api/signup&quot;) else { return }


            var request = URLRequest(url: url)
            request.httpMethod = &quot;POST&quot;
            request.setValue(&quot;application/json&quot;, forHTTPHeaderField: &quot;Content-Type&quot;)

            let body = [&quot;username&quot;: username, &quot;password&quot;: password]
            request.httpBody = try? JSONSerialization.data(withJSONObject: body)

            URLSession.shared.dataTask(with: request) { data, response, error in
                if let data = data {
                    let result = String(data: data, encoding: .utf8)
                    print(&quot;회원가입 응답: \(result ?? &quot;&quot;)&quot;)
                }
            }.resume()
        }




    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
    }


}

</code></pre><p>일단 무사히 빌드도 됐고 매우 순조롭다고 생각했던 찰나..
무수한 오류와 함께 통신이 안되고있었다..
(오류 로그도 올리고싶지만 다시 오류내기 귀찮음 양해부탁합니다 ㅎㅎ..)</p>
<p>암튼! 원인을 살펴보니
swift가 루프백으로 접속시도하고있었고
지스스로 내부 연결시도중이였음.
진짜 별에 별짓 다해보다가</p>
<p><strong>결론은 ngrok으로 서버를 외부에서 접근 가능하게 만들기로 했습니다..!</strong></p>
<p>여차저차 네트워크 대역을 얻어서 그주소로 접속한결과~</p>
<p><img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/aae99da8-ab69-4278-9adc-570d0c5bbcca/image.png" /></p>
<p>아주 고퀄리티 ui와 함께..<img alt="" src="https://velog.velcdn.com/images/hoonzeee/post/6979e649-ce82-4ea9-8e56-a75f5a674297/image.png" /></p>
<p>따란~~
우여곡절 끝에...아주 성공적인 실험 마무리..ㅋㅋㅋ</p>
<p>로컬에서 돌렸지만..지금 계획한 프로젝트를 배포하기까지 구현하고 서버로 바꿔 돌리고 까지 어후..과연 성공할 수있을까 (watchUI도 써야되서..ㅎㅎ)ㅠ ㅠ ㅠ</p>
<p>아무튼! 환경설정 끝! (솔직히 환경설정이 제일빡센거 인정하시죠?...코린이 기준)</p>
<p>아무도 안보겠지만 읽는 누군가가 있다면 끝까지 읽어주셔서 감사합니다 ㅎ ㅎ </p>