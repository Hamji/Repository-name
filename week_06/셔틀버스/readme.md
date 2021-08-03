
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/17678'> 셔틀버스 </a>


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
	
테스트 21 〉	통과 (1.22ms, 10.5MB)
	
#최소시간은 9시
#최대시간은 23시59분
minTime = 540
maxTime = 1439

def timeToInt(time):
    return int(time.split(':')[0]) * 60 + int(time.split(':')[1])

def intToTime(num):
    hour = num // 60
    minutes = num % 60
    return ("0"+str(hour))[-2:] + ":" + ("0"+str(minutes))[-2:]
    

def solution(n, t, m, timetable):
    global minTime
    global maxTime
    answer = ''
    maxTime = minTime +(t*(n-1))
    
    #모든 timetable를 int로 변환후 정렬
    for i in range(len(timetable)):
        timetable[i] = timeToInt(timetable[i])
    timetable.sort()
    
    #들어온 사람들을 저장할 list
    timeList = [list() for x in range(n)]
    
    minIndex = 0
    for time in timetable:
        #정렬되어있으므로 한번이라도 범위를 벗어나면 끝.
        if time > maxTime :
            break
        
        #계산으로 index를 찾음
        #index가 0보다 작으면(도착시간이 9시 이전이면) 0으로 보정
        currentIndex = (time - minTime + t-1)//t
        #중복되는 index가 들어오게되면 사람이 m보다 많으면 index++
        if minIndex < currentIndex :
            minIndex = currentIndex
        if len(timeList[minIndex]) >= m :
            minIndex = minIndex + 1
            if minIndex >= len(timeList) :
                break

        timeList[minIndex].append(time)
    
        
    #모든 사람들을 list에 다 넣은 후에 마지막 인덱스를 탐색
    #자리가 남아있다면 그 시각,
    #자리가 남아있지 않다면 맨 마지막사람의 시간보다 1분 빠른 시간
    if len(timeList[-1]) < m :
        answer = intToTime(maxTime)
    else :
        answer = intToTime(timeList[-1][-1]-1)
            
    return answer
	
```  
</details>
