<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1832'> 보행자 천국 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
</details>
    
정영
테스트 1 〉	통과 (41.60ms, 7.43MB)
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
#include <vector>
#include <stdio.h>

using namespace std;

int MOD = 20170805;
vector<vector<int>> city_path;

// city_path[i+1][j]이 0 city_path[i-1][j] 통행
int get_path(int i, int j, int dx, int dy) {
    while(city_path[i-dx][j-dy] == -2) {
        i-=dx;
        j-=dy;
    }
    if(city_path[i-dx][j-dy] == -1) return 0;
    
    return city_path[i-dx][j-dy];
}

int get_row_path(int i, int j) {
    return get_path(i, j, 1, 0);
}
int get_col_path(int i, int j) {
    return get_path(i, j, 0, 1);
}

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int m, int n, vector<vector<int>> city_map) {
    int answer = 0;
    city_path = vector<vector<int>>(m+2, vector<int>(n+2, -1));
    
    // city_map으로 city_path초기화
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            city_path[i+1][j+1] = -city_map[i][j];
        }
    }
    
    // 경로 계산
    for(int i = 1; i < m+1; i++) {
        for(int j = 1; j < n+1; j++) {
            if(i == 1 && j == 1) {
                city_path[1][1] = 1;
                continue;
            }
            if(city_map[i-1][j-1] == 0) {
                city_path[i][j] = (get_row_path(i, j) + get_col_path(i, j)) % MOD;
            }
        }
    }
    
    return city_path[m][n];
}
```

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
</details>
