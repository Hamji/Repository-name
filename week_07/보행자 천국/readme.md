<a href = 'https://programmers.co.kr/learn/courses/30/lessons/1832'> 보행자 천국 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` cpp
	#include <vector>

using namespace std;

int MOD = 20170805;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int m, int n, vector<vector<int>> city_map) {
    int answer = 0;
    // 500개 딱 맞게하면 넘어가서 out of range라서 [3][-1]
    int right[501][501] = {0};
    int down[501][501] = {0};
    
    // 탐색 
    for (int i = 1; i <= m; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            // 맨 처음의 경우, 시작점
            if (i == 1 && j == 1)
            {
                right[i][j] = 1;
                down[i][j] = 1;
            }
            // 둘다 갈 수 있는 경우 왼쪽에서 올 경우와 위에서 올 경우를 더해준다
            else if(city_map[i-1][j-1] == 0)
            {
                right[i][j] = (right[i][j-1] + down[i-1][j]) % MOD;
                down[i][j] = (right[i][j - 1] + down[i - 1][j]) % MOD;
            }
            // 통행 금지구역 
            else if(city_map[i-1][j-1] == 1)
            {
                right[i][j] = 0;
                down[i][j] = 0;
            }
            // 회전이 불가능 하므로 down은 위에 놈만 
            // right 는 왼쪽 놈만
            else
            {
                right[i][j] = right[i][j-1];
                down[i][j] = down[i-1][j];
            }
        }
    }
    
    // down 으로 하나 right 로 하나 상관없다 
    return down[m][n];
}
``` 
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
테스트 1 〉	통과 (41.60ms, 7.43MB)
	
#include <vector>
#include <stdio.h>

using namespace std;

int MOD = 20170805;
vector<vector<int>> city_path;

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
	
``` cpp

테스트 1 〉	통과 (131.73ms, 7.37MB)
	
#include <vector>
#include <iostream>

using namespace std;

int MOD = 20170805;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int solution(int m, int n, vector<vector<int>> city_map) {
    int answer = 0;
    //예외처리의 간편화를 위해 배열에 왼쪽과 위쪽에 여백 추가
    city_map.insert(city_map.begin(),vector<int>(n,0));
    for(int i = 0; i<=m; i++){
        city_map[i].insert(city_map[i].begin(),0);
    }
    vector<vector<int>> new_map(m+1,vector<int>(n+1,0));
    new_map[0][1] = 1;
    for(int i = 1; i<=m; i++){
        for(int j = 1; j<=n; j++){
            int left = 0;
            int up = 0;
            //접근 불가지역이면 도달불가(0)
            if(city_map[i][j] == 1) continue;
            //이전 y가 회전불가면 회전불가가 아닌 맨 위 블럭찾기
            if(city_map[i-1][j] == 0){
                up = new_map[i-1][j];
            }
            else if(city_map[i-1][j] == 2) {
                for(int y = i-1; y > 0; y--){
                    if(city_map[y][j] == 1) break;
                    if(city_map[y][j] == 0) {
                        up = new_map[y][j];
                        break;
                    }
                }
            }
            //이전 x가 회전불가면 회전불가가 아닌 맨 왼 블럭찾기
            if(city_map[i][j-1] == 0){
                left = new_map[i][j-1];
            }
            else if(city_map[i][j-1] == 2) {
                for(int x = j-1; x > 0; x--){
                    if(city_map[i][x] == 1) break;
                    if(city_map[i][x] == 0) {
                        left = new_map[i][x];
                        break;
                    }
                }
            }
            new_map[i][j] = (left + up)%20170805;
        }
    }
    
    return new_map[m][n];
}
  
```
</details>
