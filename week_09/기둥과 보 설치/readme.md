<a href = 'https://programmers.co.kr/learn/courses/30/lessons/60061/'> 기둥과 보 설치 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>

테스트 17 〉	통과 (46.09ms, 10.6MB)
	
``` python
	
def unableBoGi(arr):
    GI, BO = 0, 1
    for x, y, i in arr:
        # 맨 밑에, 왼쪽에 보가 있을떄, 설치지점에 보가 있을 때 를 제외하면 설치 X
        if i == GI:
            if y != 0 and (x, y-1, GI) not in arr and (x-1, y, BO) not in arr and (x, y, BO) not in arr:
                return True
        # 설치 아래 지점에 기둥, 설치 아래 오른쪽에 기둥, 양옆에 보가 있는 경우 를 제외하면 설치 X
        else:
            if (x, y -1, GI) not in arr and (x+1, y-1, GI) not in arr and \
            not ((x-1, y, BO) in arr and (x+1, y , BO) in arr):
                return True
    return False

def solution(n, build_frame):
    answer = set()
    
    for x, y, i, j in build_frame:
        item = (x, y, i)
        if j == 1: # 설치인경우 
            # 일단 추가
            answer.add(item)
            # valid 한지 검사후 valid 하지 않다면 삭제
            if unableBoGi(answer):
                answer.remove(item)
            # 일단 삭제
        elif j == 0 and item in answer:
            answer.remove(item)
            # valid 한지 검사 후 않다면 다시 추가
            if unableBoGi(answer):
                answer.add(item)
    
    answer = map(list, answer)
    answer = list(answer)
    answer.sort(key = lambda x : (x[0], x[1], x[2]))
    return answer
	
```

</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
테스트 13 〉	통과 (1.18ms, 4.58MB)
#include <string>
#include <vector>
#include <algorithm>

#define COLUMN  1   // 0은 기둥+1
#define BEAM    2   // 1은 보+1

using namespace std;

vector<vector<int>> world;

vector<vector<int>> solution(int n, vector<vector<int>> build_frame) {
    vector<vector<int>> answer;
    world = vector<vector<int>>(n+1+4, vector<int>(n+1+4, 0));
    
    // build
    for(auto frame : build_frame) {
        int x = frame[0]+2;
        int y = frame[1]+2;
        int type = frame[2];
        int op = frame[3];
        
        // 기둥
        if(type == 0) {
            // insert
            if(op == 1) {
                // 바닥이거나 아래 기둥이 있거나 같은 층에 보가 있는 경우
                if(y == 2 || world[y-1][x] == COLUMN || world[y-1][x] == COLUMN+BEAM ||
                   world[y][x] == BEAM || world[y][x-1] == BEAM || world[y][x-1] == COLUMN+BEAM ){
                    world[y][x] += COLUMN;
                }
            // delete
            } else if(op == 0) {
                // 왼쪽 보가 없음
                if(world[y+1][x-1] == 0 || world[y+1][x-1] == COLUMN) {
                    // 오른쪽 보가 없음
                    if(world[y+1][x] == 0) {
                        world[y][x] -= COLUMN;
                    // 오른쪽 보가 있음
                    } else if(world[y+1][x] == BEAM || world[y+1][x] == COLUMN+BEAM){
                        if(world[y][x+1] == COLUMN || world[y][x+1] == COLUMN+BEAM) {
                            world[y][x] -= COLUMN;
                        }
                    }
                // 왼쪽 보가 있음
                } else {
                    // 오른쪽 보가 없음
                    if(world[y+1][x] == 0 || world[y+1][x] == COLUMN) {
                        if(world[y][x-1] == COLUMN || world[y][x-1] == COLUMN+BEAM) {
                            world[y][x] -= COLUMN;
                        }
                    // 오른쪽 보가 있음
                    } else {
                        bool left = false;
                        if(world[y][x-1] == COLUMN || world[y][x-1]   == COLUMN+BEAM ||
                           world[y+1][x-2] == BEAM || world[y+1][x-2] == COLUMN+BEAM ){
                            left = true;
                        }
                        bool right = false;
                        if(world[y][x+1] == COLUMN || world[y][x+1]   == COLUMN+BEAM ||
                           world[y+1][x+1] == BEAM || world[y+1][x+1] == COLUMN+BEAM ){
                            right = true;
                        }
                        if(left && right) {
                            world[y][x] -= COLUMN;
                        }
                    }
                }
            }
        // 보
        } else if(type == 1) {
            // insert
            if(op == 1) {
                // 아래 기둥이 있거나 양옆에 보가 있는 경우
                if(world[y-1][x]   == COLUMN || world[y-1][x]   == COLUMN+BEAM ||
                   world[y-1][x+1] == COLUMN || world[y-1][x+1] == COLUMN+BEAM ||
                  (world[y][x-1] != 0 && world[y][x-1] != COLUMN &&
                   world[y][x+1] != 0 && world[y][x+1] != COLUMN)){
                    world[y][x] += BEAM;
                }
            // delete
            } else if(op == 0) {
                // 기둥에 이상 없는지 확인
                bool COLUMN_FINE = false;
                // 왼쪽에 기둥 없음
                if(world[y][x] == BEAM) {
                    // 오른쪽에 기둥 없음
                    if(world[y][x+1] == 0 || world[y][x+1] == BEAM) {
                        COLUMN_FINE = true;
                    // 오른쪽에 기둥 있음
                    } else {
                        if(world[y-1][x+1] == COLUMN || world[y-1][x+1] == COLUMN+BEAM ||
                           world[y][x+1] == COLUMN+BEAM) {
                            COLUMN_FINE = true;
                        }
                    }
                // 왼쪽에 기둥 있음
                } else {
                    // 오른쪽에 기둥 없음
                    if(world[y][x+1] == 0 || world[y][x+1] == BEAM) {
                        if(world[y-1][x] == COLUMN || world[y-1][x] == COLUMN+BEAM ||
                           world[y][x-1] == BEAM   || world[y][x-1] == COLUMN+BEAM ){
                            COLUMN_FINE = true;
                        }
                    // 오른쪽에 기둥 있음
                    } else {
                        bool left = false;
                        if(world[y-1][x] == COLUMN || world[y-1][x] == COLUMN+BEAM ||
                           world[y][x-1] == BEAM   || world[y][x-1] == COLUMN+BEAM ){
                            left = true;
                        }
                        bool right = false;
                        if(world[y-1][x+1] == COLUMN || world[y-1][x+1] == COLUMN+BEAM ||
                           world[y][x+1] == COLUMN+BEAM) {
                            right = true;
                        }
                        if(left && right) {
                            COLUMN_FINE = true;
                        }
                    }
                }
                // 보에 이상 없는지 확인
                bool BEAM_FINE = false;
                // 왼쪽에 보 없음
                if(world[y][x-1] == 0 || world[y][x-1] == COLUMN) {
                    // 오른쪽에 보 없음
                    if(world[y][x+1] == 0 || world[y][x+1] == COLUMN) {
                        BEAM_FINE = true;
                    // 오른쪽에 보 있음
                    } else {
                        if(world[y-1][x+1] == COLUMN || world[y-1][x+1] == COLUMN+BEAM || 
                           world[y-1][x+2] == COLUMN || world[y-1][x+2] == COLUMN+BEAM ){
                            BEAM_FINE = true;
                        }
                    }
                // 왼쪽에 보 있음
                } else {
                    // 오른쪽에 보 없음
                    if(world[y][x+1] == 0 || world[y][x+1] == COLUMN) {
                        if(world[y-1][x]   == COLUMN || world[y-1][x]   == COLUMN+BEAM || 
                           world[y-1][x-1] == COLUMN || world[y-1][x-1] == COLUMN+BEAM ){
                            BEAM_FINE = true;
                        }
                    // 오른쪽에 보 있음
                    } else {
                        bool left = false;
                        if(world[y-1][x]   == COLUMN || world[y-1][x]   == COLUMN+BEAM || 
                           world[y-1][x-1] == COLUMN || world[y-1][x-1] == COLUMN+BEAM ){
                            left = true;
                        }
                        bool right = false;
                        if(world[y-1][x+1] == COLUMN || world[y-1][x+1] == COLUMN+BEAM || 
                           world[y-1][x+2] == COLUMN || world[y-1][x+2] == COLUMN+BEAM ){
                            right = true;
                        }
                        BEAM_FINE = left && right;
                    }
                }
                
                if(COLUMN_FINE && BEAM_FINE) {
                    world[y][x] -= BEAM;
                }
            }
        }
        if(world[x][y] > 3 || world[x][y] < 0) throw "error"; 
    }
    
    // check
    for(int i = 0; i < world.size(); i++) {
        for(int j = 0; j < world.size(); j++) {
            if(world[i][j] == COLUMN || world[i][j] == BEAM) {
                answer.push_back({j-2, i-2, world[i][j]-1});
            } else if(world[i][j] == COLUMN+BEAM) {
                answer.push_back({j-2, i-2, COLUMN-1});
                answer.push_back({j-2, i-2, BEAM-1});
            }
        }
    }
    
    // sort
    struct {
        bool operator()(vector<int> a, vector<int> b) const {
            if(a[0] == b[0]) {
                if(a[1] == b[1]) {
                    return a[2] < b[2];
                } else {
                    return a[1] < b[1];
                }
            } else {
                return a[0] < b[0];
            }
        }
    } customLess;
    sort(answer.begin(), answer.end(), customLess);
    return answer;
}
```

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
  
``` python

테스트 22 〉	통과 (10.28ms, 10.5MB)

def check(table,x,y):
    #기둥도 보도 아니면 그냥 빈 공간이므로 체크 불필요
    if table[y][x] != 0 and table[y][x] != 1 :
        return True
    
    #기둥일 경우
    if table[y][x] == 0:
        if y == 0 :
            return True
        if table[y-2][x] == 0:
            return True
        if table[y-1][x-1] == 1 or table[y-1][x+1] == 1:
            return True
    #보일 경우
    if table[y][x] == 1:
        if table[y][x-2] == 1 and table[y][x+2] == 1:
            return True
        if table[y-1][x-1] == 0 or table[y-1][x+1] == 0:
            return True
        
    #모든조건에 실패하면 잘못된 위치
    return False


def build(table,size,cmd):
    x = cmd[0]*2
    y = cmd[1]*2
    if cmd[2] == 1:
        x+=1
        y-=1
        
    if cmd[3] == 1:
        #먼저 설치 한 후에 적절한 위치인지 판단
        table[y][x] = cmd[2]
        #적절하지 않은 위치면 설치 취소
        if check(table,x,y) is False:
            table[y][x] = ' '
            
    if cmd[3] == 0:
        #먼저 제거한 후에 주변 블록이 적절한지 판단
        table[y][x] = ' '
        for i in range(y-3,y+3):
            if i < 0 or i > size :
                continue
            for j in range(x-3,x+3):
                if j < 0 or j > size :
                    continue
                
                #적절하지 않은 블록이 발견되면 제거 취소
                if check(table,j,i) is False:
                    table[y][x] = cmd[2]
                    return
                
def solution(n, build_frame):
    answer = []
    
    #중복된 좌표에 보와 기둥이 동시에 존재하기 때문에 혼동을 방지하기 위해 좌표를 2배로 늘리고 보와 기둥을 격자무늬로 배치
    table = [[' ' for j in range(0,n*2+2)] for i in range(0,n*2+2)]
    for cmd in build_frame:
        build(table,n*2+1,cmd)
        
    
    for y,row in enumerate(table):
        for x,char in enumerate(row):
            if char != ' ':
                answer.append([x//2,(y+1)//2,char])
            
    answer.sort()
    
    return answer

```
</details>
