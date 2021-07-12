
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/17679'> 프렌즈4블록 </a>


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
	
	
</details>
