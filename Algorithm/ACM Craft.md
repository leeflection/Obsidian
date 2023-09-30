#위상정렬 
## 문제
![[Pasted image 20230927120409.png]]
### 풀이과정
- 순서가 중요한 탐색이기 때문에 **위상정렬**을 떠올렸다.
- 하지만 탐색하다 보면 상관없는 노드도 포함되기 때문에 이를 제외하는데에 고민
- 연결된 노드라면
	- dp[현재노드] = MAX(dp[이전노드] + 현재노드에 필요한 시간값)
	- 이부분을 큐에 넣음과 동시에 진행하다보니 문제가 발생
- 차수를 하나씩 줄이면서 0이되면 q에 추가
### 코드

```java
package baekjoon;  
  
import java.io.BufferedReader;  
import java.io.InputStreamReader;  
import java.util.ArrayDeque;  
import java.util.ArrayList;  
import java.util.Queue;  
import java.util.StringTokenizer;  
  
public class BJ1005_ACMCraft {  
    static ArrayList<Integer>[] list;  
    public static void main(String[] args) throws Exception{  
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));  
        int T = Integer.parseInt(br.readLine());  
        for(int t=0; t<T; t++){  
            StringTokenizer st = new StringTokenizer(br.readLine());  
            int n = Integer.parseInt(st.nextToken());  
            int k = Integer.parseInt(st.nextToken());  
            int[] time = new int[n+1];  
            int[] indegree = new int[n+1];  
            list = new ArrayList[n+1];  
  
            for(int i=1; i<n+1; i++){  
                list[i] = new ArrayList<Integer>();  
            }  
  
            st = new StringTokenizer(br.readLine());  
            for(int i=1; i<n+1; i++){  
                int a = Integer.parseInt(st.nextToken());  
                time[i] = a;  
            }  
  
            for(int i=0; i<k; i++){  
                st = new StringTokenizer(br.readLine());  
                int a = Integer.parseInt(st.nextToken());  
                int b = Integer.parseInt(st.nextToken());  
                list[a].add(b);  
                indegree[b]++;  
            }  
            int fin = Integer.parseInt(br.readLine());  
  
            for(int i=1; i<indegree.length; i++){  
                System.out.print(indegree[i]+" ");  
            }  
  
            System.out.println(topology(time,indegree,fin));  
        }  
    }  
  
    private static int topology(int[] time, int[] indegree, int fin) {  
        Queue<Integer> q = new ArrayDeque<>();  
        int[] result = new int[indegree.length];  
        for(int i=1; i<indegree.length; i++){  
            if(indegree[i] == 0){  
                result[i] = time[i];  
                q.add(i);  
            }  
        }  
  
        while(!q.isEmpty()){  
            int prev = q.poll();  
  
            for(int i : list[prev]){  
            //이 부분에서 dp 연산을 해야하는 이유
            //차수가 2라면 향하는 값이 2이기 때문에 2번연산되야 한다. 
            //차수가 0일때만 초기화하면안됨
                result[i] = Math.max(result[i],result[prev] + time[i]);  
                indegree[i]--;  
                if(indegree[i] == 0){  
                    q.add(i);  
                }  
            }  
        }  
        return result[fin];  
    }  
}
```