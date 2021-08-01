
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/60059'> 자물쇠와 열쇠 </a>


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

테스트 27 〉	통과 (222.02ms, 10.2MB)

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
        #범위가 -M+1 부터 N+M-1 인이유
        #key 의 위치가 lock 의 범위를 초과할 수 있기때문
        for i in range(-M+1,N+M-1):
            for j in range(-M+1,N+M-1):
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
