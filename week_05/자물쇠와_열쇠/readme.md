
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/60059'> 자물쇠와 열쇠 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python
{ % highlight python linenos % }
code_contents
{ % endhighlight % }
# 90 도 시계방향 회전
def rotate(key):
    result = [[0] * len(key) for i in range(len(key))] 
    rev = len(key)
    for i in range(len(key)):
        for j in range(len(key)):
            result [j][rev - 1 - i] = key[i][j]
    
    return result
    
def insert(key, lock, start_c, start_r, end):
    leng = (2 * len(key)) + len(lock) - 2
    key_len = len(key)
    # 배경 행렬
    back = [[0 for i in range(leng)] for j in range(leng)]
    
    # 배경에 key 넣기
    for i in range(key_len):
        for j in range(key_len):
            back[start_c + i][start_r + j] = key[i][j]
    
    for i in range(key_len - 1, end):
        for j in range(key_len - 1, end):
            back[i][j] += lock[i - key_len + 1][j - key_len + 1]
            if back[i][j] != 1:
                return False
    
    return True
    
def solution(key, lock):
    answer = True
    end = len(key) + len(lock) - 1
    for k in range(4):
        for i in range(end):
            for j in range(end):
                start_c = i
                start_r = j
                if insert(key, lock, start_c, start_r, end) == True:
                    return True
        key = rotate(key)
    
    return False
{ % highlight python linenos % }
code_contents
{ % endhighlight % }
```
	
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
	
#include <string>
#include <vector>

using namespace std;

struct Box {
    int minX;
    int minY;
    int maxX;
    int maxY;
    int sizeX;
    int sizeY;
};

vector<vector<int>> board;  // lock
vector<vector<int>> new_key;  // key 실공간

Box getBox(vector<vector<int>> &board, int cond) {
    int minX = 20, minY = 20;
    int maxX = -1, maxY = -1;
    for(int i = 0; i < board.size(); i++) {
        for(int j = 0; j < board.size(); j++) {
            if(board[i][j] == cond) {
                minX = min(minX, i);
                minY = min(minY, j);
                maxX = max(maxX, i);
                maxY = max(maxY, j);
            }
        }
    }
    int sizeX = maxX - minX + 1;    // 세로 길이
    int sizeY = maxY - minY + 1;    // 가로 길이
    return {minX, minY, maxX, maxY, sizeX, sizeY};
}

bool checker(int sx, int ex, int dx, int sy, int ey, int dy) {
   for(int x = sx; x != ex; x = x + dx) {\
       for(int y = sy; y != ey; y += dy) {
           // printf("%d(%d) ", board[x][y], new_key[dx*(x - sx)][dy*(y - sy)]);
           if(board[x][y] == new_key[dx*(x - sx)][dy*(y - sy)]) {
               // printf("\n\n");
               return false;
           }
       }
       // printf("\n");
    }
    // printf("\n");
    return true;
}

bool solution(vector<vector<int>> key, vector<vector<int>> lock) {
    bool answer = true;
    static int s = lock.size()-1;
    board = lock;
    // lock에서 홈 부분 공간을 구한다.
    Box lockB = getBox(lock, 0);
    Box keyB = getBox(key, 1);
    // lock 공간이 key 보다 큰 지 체크(위로 옆으로 한번씩)
    if( (keyB.sizeX < lockB.sizeX || keyB.sizeY < lockB.sizeY) && 
        (keyB.sizeY < lockB.sizeX || keyB.sizeX < lockB.sizeY) ) {
        printf("e");
        return false;
    }
    
    // 실제 사용될 key를 구한다.
    new_key = vector<vector<int>>(keyB.sizeX, vector<int>(keyB.sizeY));
    for(int i = keyB.minX; i <= keyB.maxX; i++) {
        for(int j = keyB.minY; j <= keyB.maxY; j++) {
            new_key[i - keyB.minX][j - keyB.minY] = key[i][j];
        }
    }
    
    // lock 공간이
    if(lockB.minX > 0 && lockB.minY > 0 && 
       lockB.maxX < s && lockB.maxY < s) { // 중심부에 있는 경우
        // key가 lock 과 동일크기인지 체크
        if( (keyB.sizeX != lockB.sizeX || keyB.sizeY != lockB.sizeY) && 
            (keyB.sizeY != lockB.sizeX || keyB.sizeX != lockB.sizeY) ) {
            printf("ms");
            return false;
        }
        // 실제 확인, 정확하기 때문에 회전만 체크
        if( checker(lockB.minX, lockB.maxX+1, +1, lockB.minY, lockB.maxY+1, +1) ||
            checker(lockB.minY, lockB.maxY+1, +1, lockB.maxX, lockB.minX-1, -1) || 
            checker(lockB.maxX, lockB.minX-1, -1, lockB.maxY, lockB.minY-1, -1) || 
            checker(lockB.maxY, lockB.minY-1, -1, lockB.minX, lockB.maxX+1, +1) ){
            printf("mm"); 
            return true;
        }
        return false;
    } else { // 외변, 모서리에 있는 경우
        // 실제 확인, 양 변을 초과된 키의 여분만큼 확인해야함.
        for(int cur_x = 0; keyB.sizeX - cur_x >= lockB.sizeX; cur_x++) {
            for(int cur_y = 0; keyB.sizeY - cur_y >= lockB.sizeY; cur_y++) {
                
                for(int x = max(lockB.minX - cur_x, 0); x <= min(keyB.sizeX - cur_x, s); x++) {
                    for(int y = max(lockB.minX - cur_y, 0); y <= min(keyB.sizeY - cur_y, s); y++) {
                        printf("%d ", lock[x][y]);
                    }
                    printf("\n");
                }
                
                printf("----\n");
            }
        }
        printf("s");
    }
    
    // return false;
    return answer;
}
```
	
</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python
	
테스트 27 〉	통과 (27.14ms, 10.3MB)

N = None
M = None
empty = None

#오른쪽으로 90도 회전
def rotate(key):
    mylist = []
    for j in range(M):
        mylist.append([])
        for i in range(M):
            mylist[j].append(key[M-i-1][j])
    return mylist
            
#해당 범위에서 0인 부분만 카운트
def countEmpty(array):
    N = len(array)
    empty = 0

    for i in range(N):
        for j in range(N):
            if array[i][j] == 0:
                empty += 1
    return empty

#key와 lock을 부르트포스
def fit(y,x,key,lock):
    global N 
    global M
    global empty
    
    count = 0
    for i in range(M):
        for j in range(M):
            #체크할 범위가 lock 보다 클 경우 이 경우는 무시.
            if i+y < 0 or j+x < 0 or i+y >= N or j+x >= N :
                continue
            if(key[i][j] == 1 and lock[i+y][j+x] == 1):
                return False
            if(key[i][j] == 0 and lock[i+y][j+x] == 0):
                return False
            if(key[i][j] == 1):
                count+=1
	
    #이부분은 없어도 될듯? 위의 for문을 통과하면 무조건 True로 봐도 될듯함
    if count == empty :
        return True
    else:
        return False

def solution(key, lock):
    global N 
    global M
    global empty
    
    answer = False
    
    N = len(lock)
    M = len(key)
    empty = countEmpty(lock)
    
    for i in range(4):
        #범위가 -M+1 부터 N 인이유
        #key(0,0) 의 위치가 lock 의 범위를 초과할 수 있기때문
        for i in range(-M+1,N):
            for j in range(-M+1,N):
                answer = fit(i,j,key,lock)
                if answer == True:
                    return answer
        key = rotate(key)
    
    return answer
	
```
  
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
