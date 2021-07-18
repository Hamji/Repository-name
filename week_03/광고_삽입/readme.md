
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/72414'> 광고 삽입 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python

def time_to_int(time):
    h, m, s = time.split(':')
    return int(h) * 3600 + int(m) * 60 + int(s)

def int_to_time(time):
    h = time // 3600
    h = '0' + str(h) if h < 10 else str(h)
    time = time % 3600 
    m = time // 60
    m = '0' + str(m) if m < 10 else str(m)
    time = time % 60
    s = '0' + str(time) if time < 10 else str(time)
    return h + ':' + m + ':' + s

def solution(play_time, adv_time, logs):
    play_time = time_to_int(play_time)
    adv_time = time_to_int(adv_time)
    total = [0 for i in range(play_time + 1)]
    
    # 시작 부분하고 끝부분 표시
    for i in logs:
        start, end = i.split('-')
        start = time_to_int(start)
        end = time_to_int(end)
        total[start] += 1
        total[end] -= 1 
        
    for i in range(1, len(total)):
        total[i] = total[i] + total[i - 1]
## 누적 시간은 어케? 모르겟다 싯팔~
	
```
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>


</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python
테스트 13 〉	통과 (679.10ms, 54.5MB)
	
def toSec(string):
    timeList = string.split(':')
    return (int(timeList[0])*60*60) + (int(timeList[1])*60) + (int(timeList[2]))

def toTime(num):
    h = num // 3600
    m = (num % 3600) // 60
    s = (num % 60)
    return str(h).zfill(2)+':'+str(m).zfill(2)+':'+str(s).zfill(2)


def solution(play_time, adv_time, logs):
    answer = ''
    sec_play_time = toSec(play_time)
    sec_adv_time = toSec(adv_time)
    time = [0]*(sec_play_time+1)
    
    for log in logs :
        startTime = toSec(log.split('-')[0])
        endTime = toSec(log.split('-')[1])
        time[startTime] += 1
        time[endTime] -= 1
        
    for i in range(1,sec_play_time):
        time[i] += time[i-1]
        
    for i in range(1,sec_play_time):
        time[i] += time[i-1]
        
    my_sum = time[sec_adv_time]
    position = 0
    
    for i in range(1,sec_play_time-sec_adv_time):
        temp_sum = time[sec_adv_time+i] - time[i]
        if temp_sum > my_sum:
            my_sum = temp_sum
            position = i+1
            
    return toTime(position)
```
	
  
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>
	
	
</details>
