# 📌 <span style= "color: lightgreen"> 최소 신장 트리(MST)

---
### 참고자료
```
https://velog.io/@suk13574/알고리즘Java-크루스칼Kruskal-알고리즘
https://gmlwjd9405.github.io/2018/08/28/algorithm-mst.html
https://velog.io/@kimdukbae/크루스칼-알고리즘-Kruskal-Algorithm
https://velog.io/@suk13574/알고리즘Java-프림Prim-알고리즘
https://gmlwjd9405.github.io/2018/08/30/algorithm-prim-mst.html
```

---
## 🌿 신장트리 (ST, Spanning Tree)

**신장트리란 최소한의 간선 수로 그래프 내의 모든 정점을 연결하는 트리 구조를 의미한다.**

<img src= "https://gmlwjd9405.github.io/images/algorithm-mst/spanning-tree.png" style="background-color: white">

#### 특징
  1. DFS, BFS를 사용하여 그래프 내의 신장트리를 찾을 수 있다.
  2. 하나의 그래프는 여러개의 신장 트리가 존재할 수 있다.
  3. 모든 정점들이 연결되어야 하며 싸이클(Cycle)이 포함되면 안된다.
  4. <u>n개의 정점을 연결하는 신장 트리의 간선 수는 (n-1)개이다.</u>

#### 최소신장트리 (MST, Minimum Spanning Tree)
  - **최소신장트리란 신장트리 중 사용된 간선들의 가중치 합이 최소인 트리이다.**
  - 즉, 한 그래프에 있는 모든 정점들을 최소 간선, 최소 비용으로 연결하는 것이다.

---
## 🌿 크루스칼(Kruskal)
###  1-1. 개념
- <span style= "color: lightblue"> Greedy 방법을 이용하여 그래프의 모든 정점을 최소 비용으로 연결하는 알고리즘 </span>
- 간선 선택을 기반으로 동작하는 알고리즘이다.

### 1-2. 동작 단계
- 각 단계마다 싸이클을 구성하지 않는 최소 비용 간선을 선택한다.
- 싸이클을 확인할 때 분리집합(Union-Find) 알고리즘을 사용한다.
- 이전 단게에서 만들어진 신장트리와는 관계없이 각 단계가 독립적으로 최소 간선을 선택한다.

``` markdown
1. 그래프의 간선들을 가중치의 오름차순으로 정렬한다.
2. 정렬된 간선 리스트에서 순서대로 사이클을 형성하지 않는 간선을 선택한다.
    - 분리집합의 Find 알고리즘을 통해 부모노드가 같은지 확인한다.
    - 부모노드가 같다면 한 이미 같은 트리에 포함된 노드이므로 넘어간다.
3. 해당 간선을 현재의 트리에 추가한다.
    - 분리집합의 Union 알고리즘을 통해 부모노드를 통합한다.
4. 모든 노드가 연결될 때까지 반복한다.
```

<details>
<summary style="font-size: larger">크루스칼 동작과정 보기</summary>
<div markdown="1">

<h3> [Step 0] </h3>
그래프의 모든 간선들을 오름차순으로 정렬한다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F619dcf12-0348-40e2-a94c-9aef18dcbbe8%2Fimage.png">

<h3> [Step 1] </h3>
비용이 최소인 간선에 연결된 노드(3,4)끼리 Union을 수행한다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Ff8cef60a-4fbe-4218-aa41-00a3e762e0c9%2Fimage.png">

<h3> [Step 2] </h3>
방문하지 않은 '간선' 중에서 가장 적은 비용인 간선(4,7)을 선택하려 처리한다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F067492c2-8121-4565-81e7-e3c9629e2db5%2Fimage.png">

<h3> [Step 3] </h3>
방문하지 않은 '간선' 중에서 가장 적은 비용인 간선(6,4)을 선택하려 처리한다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Fe7bfd60a-16a7-42ed-ab0b-16fc625feb25%2Fimage.png">

<h3> [Step 4] </h3>
방문하지 않은 '간선' 중에서 싸이클을 형성하는 간선(6,7)은 제외한다. 
그리고 다시 최소 간선(1,2)을 선택하여 처리한다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F5376a3f5-5077-4349-9f51-20b9405cbe21%2Fimage.png">

<h3> [Step 5] </h3>
방문하지 않은 '간선' 중에서 가장 적은 비용인 간선(2,6)을 선택하려 처리한다.
다음 간선은 (2,3)인데 싸이클을 형성하므로 연결하지 않는다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Faaa4fa67-7918-41f8-9cf4-63e5642b3f7f%2Fimage.png">

<h3> [Step 6] </h3>
방문하지 않은 '간선' 중에서 가장 적은 비용인 간선(6,5)을 선택하려 처리한다.
다음 간선은 (1,5)인데 싸이클을 형성하므로 연결하지 않는다.
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2Fb6cdf21d-1472-4f9e-a3be-c5bc1b2d43c9%2Fimage.png">

<h3> [결과] </h3>
<img src="https://velog.velcdn.com/images%2Fkimdukbae%2Fpost%2F24569486-1a01-4e95-a0c6-70935b7c4449%2Fimage.png">
</div>
</details>



### 1-3. 구현 (JAVA)
```java
public class Main {
    
        // 유니온 
	public static void union(int[] parent, int x, int y) {
		x = find(parent, x);
		y = find(parent, y);
		
		if(x < y) parent[y] = x;
		else parent[x] = y;
	}
  
        // 파인드
	public static int find(int[] parent, int x) {
		if(parent[x] == x) return x;
		else return find(parent, parent[x]);
	}
	
        // 크루스칼
	public static void kruskal(int[][] graph, int[] parent) {
		int cost = 0;
		for(int i = 0; i < graph.length;i++) {
			if (find(parent, graph[i][0]) != find(parent, graph[i][1])) {
				cost += graph[i][2];
				union(parent, graph[i][0], graph[i][1]);
			}
		}
            // 최소 신장 트리의 총 가중치 출력
            System.out.println(cost);
	}
	
	public static void main(String[] args) throws IOException {
    	        // 간선 입력 받기, 그래프에 저장
		BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(bf.readLine());
		int m = Integer.parseInt(bf.readLine());
		int[][] graph = new int[m][3];
		
		StringTokenizer st;
		for (int i = 0; i < m; i++) {
			st = new StringTokenizer(bf.readLine());
			graph[i][0] = Integer.parseInt(st.nextToken()); // 간선 나가는 정점
			graph[i][1] = Integer.parseInt(st.nextToken()); // 간선 들어오는 정점
			graph[i][2] = Integer.parseInt(st.nextToken()); // 가중치
		}
		
                // 간선 정렬
		Arrays.sort(graph, (o1, o2) -> o1[2] - o2[2]);
		
                // 부모노드 초기화
		int[] parent = new int[n + 1];
		for (int i = 0; i < parent.length; i++) {
			parent[i] = i;
		}
        
		//크루스칼 알고리즘 수행
		kruskal(graph, parent);
	}
}
```
```java
입력
5
6
1 3 3
1 4 8
4 5 9
1 2 10
2 3 13
2 5 14

출력 결과
30
```

### 1-4. 시간복잡도와 공간복잡도
크루스칼 알고리즘에서 복잡도에 포함되는 과정은 `Union-Find`와 `정렬` 두 가지이다.
정점의 수(V), 간선의 수(E)
- 시간복잡도
  - `간선을 정렬하는 과정` -> O(ElogE)
  - `Union-Find`의 연산 시간복잡도는 O(1)에 가깝다.
  - 따라서 크루스칼 알고리즘의 시간복잡도는 <span style= "color: lightblue">O(ElogE)</span>이다.
- 공간복잡도
  - `Union-Find`에서 사용하는 공간복잡도는 정점의 수 V에 비례하므로 O(V)이다.
  - `정렬`을 위해 추가적인 배열이 사용될 수 있으며 이 역시 간선의 수 E에 비례하므로 O(E)이다.
  - 따라서 크루스칼 알고리즘의 공간복잡도는 <span style= "color: lightblue">O(max(V,E))</span>이다.

### 1-5. 문제 링크
1. <a href = "https://www.acmicpc.net/board/view/108031"> 백준 1197 - 최소 스패닝 트리 </a>
2. <a href = "https://www.acmicpc.net/problem/14950"> 백준 14950 - 정복자 </a>
3. <a href = "https://school.programmers.co.kr/learn/courses/30/lessons/42861"> 프로그래머스 - 섬 연결하기 </a>

---
## 🌿 프림(Prim)

###  1-1. 개념
- <span style= "color: lightblue"> 시작 정점을 기준으로 가장 작은 간선과 연결된 정점을 선택하며 신장 트리를 확장 시키는 알고리즘 </span>
- 정점 선택을 기반으로 동작하는 알고리즘

### 1-2. 동작 단계
``` markdown
1. 시작 정점을 선택한다.
2. 선택한 정점을 트리에 추가하고, 해당 정점과 연결된 간선들을 우선순위 큐에 넣는다.
3. 우선순위 큐에서 가장 작은 비용을 가지는 간선을 꺼낸다.
4. 꺼낸 간선의 반대쪽 정점이 현재 트리에 포함되어 있지 않다면, 해당 간선과 정점을 트리에 추가한다.
5. 우선순위 큐가 비어질 때까지 3-4단계를 반복한다.
```
<details>
<summary style="font-size: larger">프림 동작과정 보기</summary>
<div markdown="1">
<h3> [Step 0] </h3>
시작정점(1)을 정하고 우선순위 큐에 넣는다. 우선순위큐는 가중치를 기준으로 구성한다.
<img src="https://velog.velcdn.com/images/suk13574/post/f156717e-fef0-4499-bd76-e7800f2642d9/image.PNG">

<h3> [Step 1] </h3>
우선순위 큐에서 하나 꺼낸다. 해당 정점(1)을 트리에 추가하고 정점과 연결된 간선들을 모두 탐색하며 아직 방문하지 않은 정점이라면 우선순위 큐에 추가한다.
<img src="https://velog.velcdn.com/images/suk13574/post/e91076ac-887b-4954-84eb-4be6c667f51c/image.PNG">

<h3> [Step 2] </h3>
Step1과 같은 과정을 수행한다. 하지만 이미 방문한 1번 노드는 우선순위큐에 넣지 않는다.
<img src="https://velog.velcdn.com/images/suk13574/post/7e495375-90a8-465c-a13a-3424b54d6cae/image.PNG">

<h3> [Step 3] </h3>
Step1과 같은 과정을 수행한다. 똑같이 방문한 노드는 무시한다.
<img src="https://velog.velcdn.com/images/suk13574/post/e5746d9d-23ab-4815-b3bf-4ff21c2d1569/image.PNG">

<h3> [Step 3] </h3>
<img src="https://velog.velcdn.com/images/suk13574/post/2a832d41-1724-4e01-9cdc-4270732dc195/image.PNG">

<h3> [Step 3] </h3>
<img src="https://velog.velcdn.com/images/suk13574/post/78d27f3e-bade-4dcc-b0f1-ebd755601ae2/image.PNG">

<h3> [Step 3] </h3>
<img src="https://velog.velcdn.com/images/suk13574/post/40cd32f3-e3c0-4cd8-aef4-c0eb65229475/image.PNG">

<h3> [Step 3] </h3>
<img src="https://velog.velcdn.com/images/suk13574/post/c465e08e-763c-4e70-9e6e-8689355daace/image.PNG">

<h3> [결과] </h3>
<img src="https://velog.velcdn.com/images/suk13574/post/7e34542b-2cd9-4a00-bc22-bc67f053ce54/image.PNG">

</div>
</details>

### 1-3. 구현
```java
class Edge implements Comparable<Edge>{
	int w;
	int cost;
	
	Edge(int  w, int cost){
		this.w = w;
		this.cost = cost;
	}
	
	@Override
	public int compareTo(Edge o) {
		return this.cost - o.cost;
	}
}

public class prim_main {
	static List<Edge>[] graph;
	
	public static void prim(int start, int n) {
		boolean[] visit = new boolean[n + 1];
		
		PriorityQueue<Edge> pq = new PriorityQueue<>();
		pq.offer(new Edge(start, 0));
		
		int total = 0;
		while(!pq.isEmpty()) {
			Edge edge = pq.poll();
			int v = edge.w;
			int cost = edge.cost;
			
			if(visit[v]) continue;
            
			visit[v] = true;
			total += cost;
			
			for(Edge e : graph[v]) {
				if(!visit[e.w]) {
					pq.add(e);
				}
			}
		}
		System.out.println(total);
	}

	
	public static void main(String[] args) throws IOException {
   		// 그래프 입력, 저장
		BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
		int n = Integer.parseInt(bf.readLine());
		int m = Integer.parseInt(bf.readLine());
		
        // 그래프 선언, 간선 리스트로 표현
		graph = new ArrayList[n + 1];
		for (int i = 0; i < graph.length; i++) graph[i] = new ArrayList<>();
		
		StringTokenizer st;
		for (int i = 0; i < m; i++) {
			st = new StringTokenizer(bf.readLine());
			int v = Integer.parseInt(st.nextToken());
			int w = Integer.parseInt(st.nextToken());
			int cost = Integer.parseInt(st.nextToken());
			
			graph[v].add(new Edge(w, cost));
			graph[v].add(new Edge(v, cost));
		}
		
        // 프림 알고리즘 수행
		prim(1, n);
	}
}
```
```java
입력
5
6
1 3 3
1 4 8
4 5 9
1 2 10
2 3 13
2 5 14

출력 결과
30
```

### 1-4. 시간복잡도와 공간복잡도
정점의 수(V), 간선의 수(E)
- 시간복잡도
  - 모든 노드에 대해 탐색을 진행하므로 O(V)가 소요된다.
  - 또한 우선순위 큐를 사용하여 매 노드마다 최소 간선을 찾는 시간은 O(logV)이다.
  - 따라서 탐색과정에서는 O(VlogV)가 소요된다.
  - 각 노드의 인접 간선을 찾는 과정에서 모든 간선을 탐색하므로 O(E)가 소요된다.
  - 각 간선에 대해 힙에 넣는 과정에서 O(logV)가 소요된다.
  - 따라서 우선순위 큐를 구성하는 과정에서 O(ElogV)가 소요된다.
  - 따라서 프림 알고리즘의 시간복잡도는  <span style= "color: lightblue">O(VlogV + ElogV) == O(ElogV)</span>이다. 왜냐하면 일반적으로 정점보다 간선의 개수가 크기 때문이다.

- 공간복잡도
  - 각 정점의 방문여부를 저장하는 배열은 O(V)만큼의 크기를 가진다.
  - 우선순위 큐에는 정점들이 들어가기에 최대 크기는 정점의 수와 같아 O(V)의 크기이다.
  - 따라서 프림 알고리즘의 공간복잡도는 O(V)이다.

### 1-5. 문제 링크
1. <a href = "https://www.acmicpc.net/problem/4386"> 백준 4386 - 별자리 만들기 </a>
2. <a href = "https://technote-mezza.tistory.com/103"> 백준 1647 - 도시 분할 계획</a>
3. <a href = "https://www.acmicpc.net/problem/1197"> 백준 1197 - 최소 스패닝 트리</a>

---
## 🌿 크루스칼과 프림의 차이점
1. 크루스칼 알고리즘은 간선을 기반으로 선택하는 알고리즘이며 프림 알고리즘은 정점을 기반으로 선택하는 알고리즘이다.
2. 크루스칼 알고리즘은 싸이클을 확인하는 방법으로 Union-Find를 사용하고 프림 알고리즘은 방문처리 배열을 통해 싸이클을 방지한다.
3. 간선의 수가 정점의 수에 비해 많은 경우에는 프림 알고리즘을 사용하는 것이 좋고 간선의 수가 비교적 적은 경우에는 크루스칼 알고리즘을 사용하면 좋다.
   1. 크루스칼 알고리즘의 시간복잡도는 O(ElogE)이기 때문에 간선이 많아질수록 시간복잡도가 커진다.
   2. 프림 알고리즘의 시간복잡도는 O(VlogV + ElogV)이므로 V가 E보다 크다면 O(VlogV)가 된다.