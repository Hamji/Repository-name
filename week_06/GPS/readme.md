
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1837'> GPS </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    int answer = 0;
    
    /*
     * 각 점에서 다른 점으로 가는데 필요한 비용이 얼마인지 구해놓는다.
     */
    vector<vector<int>> edge_vec(n+1, vector<int>(n+1, 0));
    deque<vector<int>> edge_queue;
    // 기본 경로 추가
    for(auto edge : edge_list) {
        edge_vec[edge[0]][edge[1]]++;
        edge_vec[edge[1]][edge[0]]++;
    }
    // 한 단계씩 이동 비용을 증가시킨다.
    for(int cost = 2; cost < n; cost++) {
        // 전체 보드를 체크한다
        for(int i = 1; i <= n; i++) {
            for(int j = 1; j <= n; j++) {
                if(i == j) continue;

                if(edge_vec[i][j] > 0) { // 이동할 수 있는 지점이라면
                    for(int cur = 1; cur <= n; cur++) {
                        if(i == cur) continue;
                        if(edge_vec[i][cur] == 0 && edge_vec[j][cur] > 0) { // 연결 가능한 경로 추가
                            edge_queue.push_back({i, cur, edge_vec[j][cur] + edge_vec[i][j]});
                        }
                    }
                }
                
            }
        }
        // 현재 비용으로 갈 수 있게 된 경로 추가
        for(auto q : edge_queue) {
            if(edge_vec[q[0]][q[1]] == 0) {
                edge_vec[q[0]][q[1]] = q[2];
            } else {
                edge_vec[q[0]][q[1]] = min(edge_vec[q[0]][q[1]], q[2]);
            }
            edge_queue.pop_front();
        }
    }
}
```

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
</details>
