<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1837'> GPS </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>


</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
	
	
``` cpp
	
테스트 1 〉	통과 (41.49ms, 5.75MB)
#include <vector>
#include <iostream>
#include <set>

#define INF 999999

using namespace std;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int n, int m, vector<vector<int>> edge_list, int k, vector<int> gps_log) {
    int answer = 0;
    vector<vector<int>> dp(k,vector<int>(n + 1,INF));
    vector<set<int>> edgeMap(n+1,set<int>());
    dp[0][gps_log[0]] = 0;
    
    for(int i = 0; i<m; i++){
        if(i<n){
            edgeMap[i+1].insert(i+1);
        }
        edgeMap[edge_list[i][0]].insert(edge_list[i][1]);
        edgeMap[edge_list[i][1]].insert(edge_list[i][0]);
    }
    
    
    
    for(int i = 1; i<k; i++){
        for(int j = 1; j<n+1; j++){
            for(auto it = edgeMap[j].begin(); it != edgeMap[j].end(); it++){
                dp[i][j] = min(dp[i-1][*it],dp[i][j]);
            }
            dp[i][j] += (j == gps_log[i]) ? 0 : 1;
        }
    }
    
    if(dp[k-1][gps_log[k-1]] >= INF) return -1;
    
    return dp[k-1][gps_log[k-1]];
}
	
```
  
</details>
