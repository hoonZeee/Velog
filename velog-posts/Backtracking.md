<h3 id="백트래킹이란">백트래킹이란?</h3>
<blockquote>
<p>해를 찾는 도중 해가 될수 없다고 판단되면, 되돌아가서 해를 찾는 기법.</p>
</blockquote>
<p><em>우선 백트래킹과 DFS의 차이점을 짚고 넘어가보자.</em></p>
<h3 id="dfsdepth-first-search">DFS(Depth-First Search)</h3>
<ul>
<li>깊이 우선 탐색으로 가능한 모든 경로를 탐색한다.</li>
<li>모든 경로를 탐색하는 특징으로 불필요한 경로를 차단하지 않아 많은 경우의 수가 발생한다.</li>
</ul>
<h3 id="백트래킹backtracking">백트래킹(BackTracking)</h3>
<ul>
<li>재귀적으로 접근.</li>
<li>현재 재귀를 통해 확인 중인 상태가 제한 조건에 위배가 되지 않는지 판단하고, 위배되는 경우 해당 상태를 제외하고 다음 단계로 넘어간다.</li>
<li>즉, 해를 찾는 도중 해가 절대 될 수 없다고 판단되면, 되돌아가서 다시 해를 찾는 행위이다.</li>
</ul>
<h3 id="브루트포스brute-force">브루트포스(Brute-Force)</h3>
<ul>
<li>'모든 경우의 수를 찾아보는 것'</li>
<li>즉, a = 1, b = 1, c =1, d = 1 부터 시작하여 a = 100, b = 100, c = 100, d = 100 까지 총 1억개의 경우의 수를 모두 찾아보면서 a + b + c + d = 20 이 만족하는 값을 탐색하는 것</li>
</ul>
<h4 id="백준15649번을-예제로-풀어보자">백준15649번을 예제로 풀어보자.</h4>
<ul>
<li>DFS와 백트래킹방법으로 접근하여 보겠다.(예시 알고리즘)</li>
</ul>
<pre><code>boolean[] visit = new boolean[N];
int[] arr = new int[M];

public static void dfs(int N, int M, int depth) {

    // 재귀 깊이가 M과 같아지면 탐색과정에서 담았던 배열을 출력
    if (depth == M) {
        for (int val : arr) {
            System.out.print(val + &quot; &quot;);
        }
        System.out.println();
        return;
    }


    for (int i = 0; i &lt; N; i++) {

        // 만약 해당 노드(값)을 방문하지 않았다면?
        if (visit[i] == false) {

            visit[i] = true;        // 해당 노드를 방문상태로 변경
            arr[depth] = i + 1;        // 해당 깊이를 index로 하여 i + 1 값 저장
            dfs(N, M, depth + 1);    // 다음 자식 노드 방문을 위해 depth 1 증가시키면서 재귀호출

            // 자식노드 방문이 끝나고 돌아오면 방문노드를 방문하지 않은 상태로 변경
            visit[i] = false;
        }
    }
    return;
}</code></pre><h4 id="정리하여-보면">정리하여 보면..</h4>
<blockquote>
<p>visit[0] = true로 설정함으로써 →
→ 숫자 1을 이미 사용했다는 표시를 해두는 것
→ 다음 반복문/재귀에서는 같은 숫자(예: 1,1) 선택을 차단
그다음엔 재귀를 통해서 모든 경우의 수를 탐색
→ depth를 하나씩 늘려가면서 수열을 완성하고
→ depth == M일 때마다 출력하고 다시 돌아감 (백트래킹)</p>
</blockquote>
<h4 id="15649-전체코드-bufferreaderstringbuilder">15649 전체코드 BufferReader,StringBuilder</h4>
<pre><code>package Level20;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class no15649PR {

    public static boolean[] visit;
    public static int[] arr;
    public static StringBuilder sb = new StringBuilder();

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());

        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        visit = new boolean[N];
        arr = new int[M];

        dfs(N, M ,0);

        System.out.println(sb);



    }

    static void dfs(int N , int M, int depth){
        if(depth == M ){
            for (int val : arr){
                sb.append(val).append(' ');
            }
            sb.append('\n');
            return;
        }

        for (int i = 0; i &lt; N; i++) {
            if(!visit[i]){
                visit[i] = true;
                arr[depth] = i + 1;
                dfs(N, M , depth+1);
                visit[i] = false;
            }
        }
    }
}
</code></pre>