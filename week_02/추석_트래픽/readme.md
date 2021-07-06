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

테스트 3 〉	통과 (1833.13ms, 11MB)
	
``` python
	
from datetime import datetime, timedelta

def solution(lines):
    answer = 1
    i = 0
    count = 1
    #이전에 확인 했었던 항목은 다음 항목에도 무조건 속하기 때문에
    #이를 체크하기 위한 list
    dpList = list(range(len(lines)))
    while i < len(lines)-1 :
        count -= 1
        time = datetime.strptime(lines[i].split(' ')[1],"%H:%M:%S.%f") + timedelta(seconds = 0.999)
        
        num = 0
        while num < len(dpList) :
            target = (datetime.strptime(lines[dpList[num]].split(' ')[1],"%H:%M:%S.%f")
                      - timedelta(seconds = float(lines[dpList[num]].split(' ')[2].split('s')[0])-0.001)
                     )
            limit = (datetime.strptime(lines[dpList[num]].split(' ')[1],"%H:%M:%S.%f")
                      - timedelta(seconds = 3)
                     )
            
            #3초가 넘는 경우가 생기면 이 이후는 확인할 필요없음
            #이 이후는 모두 3초가 넘을 것이기 때문
            if time <= limit: break
            if time >= target:
                del dpList[num]
                count += 1
            else : 
                num+=1
        answer = count if answer < count else answer
        i+=1
    
    return answer
					
```
	
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

	
</details>
