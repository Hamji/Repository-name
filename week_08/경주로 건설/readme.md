<a href = 'https://programmers.co.kr/learn/courses/30/lessons/67259'> 경주로 건설 </a>


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

``` python

테스트 11 〉	통과 (274.80ms, 11MB)

class point:
    def __init__(self,x,y,direct,value):
        self.x = x
        self.y = y
        self.direct = direct
        self.value = value
    
def solution(board):
    direct = [[0,-1],[0,1],[1,0],[-1,0]]
    answer = 0

    queue = []
    queue.append(point(0,0,-1,0))
    
    while queue:
        current = queue.pop(0)
        if current.y < 0 or current.y >= len(board): continue
        if current.x < 0 or current.x >= len(board[0]): continue
        if board[current.y][current.x] == 1: continue
        if (board[current.y][current.x] != 0 and 
            board[current.y][current.x] < current.value): continue
            
        board[current.y][current.x] = current.value
        for i in range(0,len(direct)):
            corner = (i != current.direct and current.direct != -1)
            newValue = current.value+100 + (500 if corner else 0)
            nextX = current.x+direct[i][0]
            nextY = current.y+direct[i][1]
            queue.append(point(
                nextX,nextY,i,newValue
            ))
    
    # for col in board:
    #     for row in col:
    #         print(str(row).rjust(4,' '),end = ' ')
    #     print()
    
    answer = board[-1][-1]
    return answer


```
  
</details>
