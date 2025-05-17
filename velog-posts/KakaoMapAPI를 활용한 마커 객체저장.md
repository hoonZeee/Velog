<p>kakaoSDK를 활용해서 지도에 마커를 찍는 과정중...
랜더링할때 원인 모를 오류 발생...
분명 마커는 잘찍는데, 널포인트 익셉션도 많이터져서 그냥 강제로 출력해버리고 테스중이였다..
<em>즉, 마커 객체에 직접 데이터를 저장하려고 했는데, 방식이 잘못됐었다..</em></p>
<p>그래서 그냥 구조체를 선언해서 정보를 하나로 묶었음! + 딕셔너리 생성</p>
<pre><code>struct BusStop {
    let id: String
    let name: String
    let x: Double
    let y: Double
}

var busStopDict = [String: BusStop]()

self.busStopDict[markerKey] = busStop</code></pre><p>마커 클릭시 param.poiItem.itemID를 통해서 ID를 얻고 정보를 꺼냈음..</p>
<pre><code>if let busStop = self.busStopDict[poiID] {
    self.busStopName.text = busStop.name
}</code></pre><p>결과!!
<a href="">업로드중..</a></p>
<p>이게 랜더링 되는 와중에 객체를 집으면 바로 다운되던 오류가 해결되었고, 마킹 찍히는속도도 엄청 빨라졌음 ㅎㅎ
UI- 데이터도 분리시켜놔서 유지보수랑 앞으로 확장도 쉬울듯 하다!</p>