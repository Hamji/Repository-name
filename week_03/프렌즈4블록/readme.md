
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/17679'> 프렌즈4블록 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>

통과 (35.76ms, 10.3MB)
	
``` python

def solution(m, n, board):
    answer = 0
    for i in range(len(board)):
        board[i] = list(board[i])
    
    while True:
        remove = [[0]*n for _ in range(m)]
        for i in range(m - 1):
            for j in range(n - 1):
                if board[i][j] != 0 and board[i][j] == board[i][j + 1] and board[i][j] == board[i + 1][j] and board[i][j] == board[i + 1][j + 1]:
                    remove[i][j] = 1
                    remove[i][j + 1] = 1
                    remove[i + 1][j] = 1
                    remove[i + 1][j + 1] = 1
        count = 0
        for i in range(m): 
            count += sum(remove[i])
        answer += count
        if count == 0: 
            break
        # 지워진 블록 채우기
        for i in range(m - 1, -1, -1):
            for j in range(n):
                if remove[i][j] == 1:
                    x = i - 1
                    # x 올리기 remove 가 1이 아닐때까지 쭈우욱 올라간다.
                    while x >= 0 and remove[x][j] == 1: 
                        x -= 1
                    ## 만약 0보다 작다면위에 아무것도 없으니까 그냥 0으로 바꿔버린다.
                    if x < 0:
                        board[i][j] = 0
                    else:
                        board[i][j] = board[x][j]
                        remove[x][j] = 1

    return answer
	
```
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

테스트 5 〉	통과 (2.48ms, 3.95MB)
``` c++
#include <string>
#include <vector>
#include <queue>

using namespace std;

vector<vector<int>> is_block;

bool check_block(int m, int n, vector<string> board) {
    bool has_change = false;
    for(int i = 0; i < m-1; i++) {
        for(int j = 0; j < n-1; j++) {
            if(board[i][j] != 'X' &&
              board[i][j] == board[i][j+1] &&
              board[i][j] == board[i+1][j] &&
              board[i][j] == board[i+1][j+1] ) {
                if(is_block[i][j] == 0) {
                    has_change = true;
                }
                is_block[i][j] = 1;
                is_block[i][j+1] = 1;
                is_block[i+1][j] = 1;
                is_block[i+1][j+1] = 1;
            }
        }
    }
    return has_change;
}
void block_down(int m, int l, vector<string> &board) {
    queue<pair<char, int>> q;
    for(int i = m-1; i >= 0; i--) {
        if(is_block[i][l] == 0) {
            q.push({board[i][l], 0});
        }
    }
    for(int i = m-1; i >= 0; i--) {
        if(is_block[i][l] == 1) {
            q.push({'X', 1});
        }
    }
    for(int i = m-1; i >= 0; i--) {
        board[i][l] = q.front().first;
        is_block[i][l] = q.front().second;
        q.pop();
    }
}

int solution(int m, int n, vector<string> board) {
    int answer = 0;
    is_block = vector<vector<int>>(m, vector<int>(n, 0));
    
    // check block
    while(check_block(m, n, board)) {
        // push down
        for(int line = 0; line < n; line++) {
            block_down(m, line, board);
        }
    }
    // count
    for(int i = 0; i < m; i++) {
        for(int j = 0; j < n; j++) {
            answer += is_block[i][j];
        }
    }
    
    return answer;
}
```
			    

</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
	
테스트 5 〉	통과 (66.45ms, 10.3MB)
``` python
	
#18:04 시작 19:00 중단
#19:30 재시작 19:53 완료

#본인 위치기준 4개의 블럭을 지우기 위한 루프배열
checkList = [[0,1],[1,0],[1,1]]

#삭제할 부분을 체크하기위한 부분
#삭제할 예정일 부분을 소문자로 나타냄
def check(y,x,board):
    char = board[y][x]
    if char == '' :
        return False
    for i,j in checkList :
        if (char != board[y+i][x+j].upper() and
            char != board[y+i][x+j].lower()):
            return False
    
    board[y][x] = char.lower()
    for i,j in checkList :
        board[y+i][x+j] = board[y][x]
    return True
        

def solution(m, n, board):
    answer = 0
    boardList = []
    
    for i in range(m) :
        boardList.append(list(board[i]))
        
    mylist = []
    
    #리스트에서 삭제할때 후처리작업이 필요없게 리스트를 90도 회전하여 새로만듦
    for j in range(n):
        mylist.append([])
        for i in range(m):
            mylist[j].append(board[m-i-1][j])
        
    isCheck = True
    
    
    #하나라도 삭제할 부분이 생기면 루프
    #모든 경우의수를 모두 해보는 부르트포스 사용
    while isCheck is True :
        isCheck = False

        for i in range(n-1) :
            for j in range(m-1) :
                if check(i,j,mylist) is True :
                    isCheck = True
                    
        i = 0
        while i < n :
            j = 0
            while j < m :
                if mylist[i][j] == '' :
                    break
                if mylist[i][j].islower():
                    del mylist[i][j]
                    mylist[i].append('')
                    #i,j 번째를 지우면 다음 순번은 j+1번째가 아닌 다시한번 j번째이므로
                    #j를 1 빼줌
                    j-=1
                    answer += 1
                j+=1
            i+=1
                    
    return answer
```
  
</details>

  
문교
<details>
<summary>접기/펼치기 버튼</summary>

테스트 5 〉	통과 (2.40ms, 3.77MB)
	
``` cpp
	
#include <string>
#include <vector>

using namespace std;

bool canClear(int m, int n, vector<string>& board)
{
    char c = board[m][n];
    
    return c == board[m][n + 1]
        && c == board[m + 1][n]
        && c == board[m + 1][n + 1];
}

int solution(int m, int n, vector<string> board) {
    int answer = 0;

    while (true)
    {
        vector<vector<bool>> clearFlags(m, vector<bool>(n, false));

        bool finishFlag = true;

        for (int i = 0; i < m - 1; ++i)
        {
            for (int j = 0; j < n - 1; ++j)
            {
                if (board[i][j] == ' ' )
                    continue;

                if (canClear(i, j, board))
                {
                    clearFlags[i][j] = true;
                    clearFlags[i][j + 1] = true;
                    clearFlags[i + 1][j] = true;
                    clearFlags[i + 1][j + 1] = true;

                    finishFlag = false;
                }
            }
        }

        if (finishFlag)
            break;

        for (int i = 0; i < m; ++i)
        {
            for (int j = 0; j < n; ++j)
            {
                if (clearFlags[i][j] == true)
                {
                    board[i][j] = ' ';
                    ++answer;
                }
            }
        }

        for (int i = m - 1; i >= 0; --i)
        {
            for (int j = n - 1; j >= 0; --j)
            {
                if (board[i][j] != ' ')
                    continue;

                for (int k = i - 1; k >= 0; k--) 
                {
                    if (board[k][j] != ' ') 
                    {
                        board[i][j] = board[k][j];
                        board[k][j] = ' ';
                        break;
                    }
                }
            }
        }

    }

    return answer;
}
	
```
	
</details>
