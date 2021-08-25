<a href = 'https://programmers.co.kr/learn/courses/30/lessons/60061/'> 기둥과 보 설치 </a>


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
