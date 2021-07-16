
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/72414'> 광고 삽입 </a>


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
