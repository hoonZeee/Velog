<h4 id="판다스는-파이썬에서-데이터-조각-작업을-단순하고-효율적으로-수행할-수-있도록-지원하는-강력한-라이브러리이다">판다스는 파이썬에서 데이터 조각 작업을 단순하고 효율적으로 수행할 수 있도록 지원하는 강력한 라이브러리이다.</h4>
<p><em>웨스 맥키니(Wes McKinney)가 2008년에 개발하였다고함.</em></p>
<blockquote>
<p>판다스의 주요 기능</p>
</blockquote>
<ul>
<li>데이터셋에 포함된 결측값을 쉽게 처리할 수 있다.</li>
<li><strong>데이터 객체에 레이블을 부여하고, 레이블을 활용하여 데이터를 다룰 수 있다.</strong>
(레이블 : 숫자가 아닌 의미 있는 값)</li>
<li>데이터를 그룹화하거나, 그룹별로 집계하는 작업이 가능하다.</li>
<li>판다스 객체를 CSV파일, 엑셀파일, 데이터베이스 형식으로 저장 가능</li>
<li>시계열(Time Series) 데이터를 다루는데 유용한 기능 제공</li>
</ul>
<p>시리즈(Series) : 1차원 배열로서 다일 변수 데이터를 저장하는데 사용
데이터프레임(DataFrame) : 2차원 배열로서 다중 변수 데이터를 저장하는데 사용</p>
<p>판다스에선 행렬 구별자가 index이다.</p>
<h4 id="판다스-시리즈에서-객체-생성하기">판다스 시리즈에서 객체 생성하기</h4>
<pre><code>import pandas as pd

#판다스를 이용해 배열생성
# 25를 '25'처럼 감싸면 String이 아닌 object타입이 된다.(python은 string이 없음)
age= pd.Sereis([25,34,19,45,60]) 
age
type(age)

data = ['spring,'summer','fall','winter'] #object type
season = pd.Series(data) #배열로 전환
season
# spyder에서 print()없이 작성해도 마지막한줄은 print()형태로 출력된다..신기
season.iloc[2]</code></pre><h4 id="판다스에서-데이터프레임-객체-생성하기">판다스에서 데이터프레임 객체 생성하기</h4>
<pre><code>score = pd.DataFrame([[85,96,40,95],[73,69,45,80],[78,50,60,90]])
score
type(score) #score의 데이터 타입 출력

score.index # 행 방향 인덱스
score.columns # 열방향 인덱스

score.iloc[1,2] # 인덱스 1행 2열의 값</code></pre><h4 id="넘파이-2차원-배열---판다스-데이터프레임">넘파이 2차원 배열 &lt; &gt; 판다스 데이터프레임</h4>
<pre><code>import pandas as pd
import numpy as np

s_np = np.array([[1,2,3,4,5],[6,7,8,9,10],[11,12,13,14,15]])
s_np
score2 = pd.DataFrame(s_np) #넘파이 배열을 데이터 프레임으로
score2
score_np = score2.to_numpy() #데이터 프레임을 넘파이 배열로
score_np</code></pre><h4 id="판다스의-인덱싱-시스템">판다스의 인덱싱 시스템</h4>
<p>값의 절대 위치에 의한 인덱싱 : iloc[] 메서드 사용
값의 레이블에 의한 인덱싱 : loc[] 메서드 사용</p>
<h4 id="행과-열에-레이블을-부여하는-방법">행과 열에 레이블을 부여하는 방법</h4>
<pre><code>import pandas as pd

#시리즈에 레이블 부여

age = pd.Series([25,34,19,45]) # 레이블 부여 전
age
age.index = ['John','Jane','Tom','Luka'] #행에 레이블 부여
age

age.iloc[2] # 절대 위치 인덱싱
age.loc[Tom] # 레이블 인덱싱</code></pre><h4 id="중복된-레이블-부여">중복된 레이블 부여</h4>
<ul>
<li>절대 위치 인덱스는 중복되지 않으며, 시스템에 의해 자동 관리됨</li>
<li>레이블 인덱스는 사용자가 임의로 지정할 수 있으며, 중복 존재 가능<pre><code>age = pd.Series([25,34,19,45,60])
age.index = ['John','Jane','Tom','Michle','Tom']
age.loc['Tom']
</code></pre></li>
</ul>
<p>#output
Out[73]: 
Tom    19
Tom    60
dtype: int64</p>
<pre><code>#### 실습
![](https://velog.velcdn.com/images/hoonzeee/post/583057b2-0d57-4cb9-b674-846cce5c5638/image.png)
</code></pre><p>score = pd.DataFrame(
    [[-0.1, 0.0, -0.1, -0.2],
     [1.8, 2.0, 1.6, 1.6],
     [6.4, 6.8, 5.8, 5.9],
     [12.3, 12.9, 11.5, 11.5],
     [17.9, 18.5, 17.1, 17.1],
     [22.2, 22.8, 21.6, 21.5]])</p>
<p>score.index = ['1월','2월','3월','4월','5월','6월']
score.columns = ['전북','전주','군산','부안']</p>
<p>print(score.iloc[2,1])
print(score.iloc[3,3])
print(score.loc['1월','군산'])
print(score.loc['6월','전북'])</p>
<pre><code>
후기 : 
레이블이 참 편하다.. iloc로 접근할려다가 눈빠지겠다.
파이썬 라이브러리는 공부하기 참 좋다.. 자바 메서드에 비하면 되게 친절한느낌..?</code></pre>