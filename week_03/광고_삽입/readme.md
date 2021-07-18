
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
그냥 실패
	
``` cpp
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

const int msPerHour = 60 * 60;
const int msPerMin = 60;

int parseTimeSeconds(const string& time)
{
    const int h = stoi(time.substr(0, 2));
    const int m = stoi(time.substr(3, 2));
    const int s = stoi(time.substr(6, 2));

    return h * msPerHour + m * msPerMin + s;
}

string zero_padding(string num) {
    return num.length() == 1 ? '0' + num : num;
}

string convertTimeStamp(int seconds)
{
    string hh = zero_padding(to_string(seconds / msPerHour));
    string mm = zero_padding(to_string(seconds % msPerHour / msPerMin));
    string ss = zero_padding(to_string(seconds % msPerMin));

    return hh + ":" + mm + ":" + ss;
}

bool logCompare(const pair<int, int>& a, const pair<int, int>& b)
{
    // 시간 오름차순  
    if (a.first != b.first)
    {
        return a.first < b.first;
    }
    // 시작 시간이 앞으로 가게
    return a.second > b.second;
}

string solution(string play_time, string adv_time, vector<string> logs)
{
    int answer = 0;

    int playTime = parseTimeSeconds(play_time);
    int advTime = parseTimeSeconds(adv_time);

    // 시간, 상태 (1 == 시작, -1 == 완료)
    vector<pair<int, int>> sortedLogs;
    // Key : 시작시간, Value : 누적시간
    vector<pair<int, int>> accumLogs;

    sortedLogs.push_back({ 0, 1 });
    sortedLogs.push_back({ playTime, -1 });

    for (int i = 0; i < logs.size(); ++i)
    {
        string& line = logs[i];

        int startTime = parseTimeSeconds(line.substr(0, 8));
        int endTime = parseTimeSeconds(line.substr(9, 8));

        sortedLogs.push_back({ startTime, 1 });
        sortedLogs.push_back({ endTime, -1 });
    }

    sort(sortedLogs.begin(), sortedLogs.end(), logCompare);

    int prevTime = 0;
    int viewer = 0;
    
    pair<int, int> advLog = { 0, 0 };

    for (int i = 0; i < sortedLogs.size(); ++i)
    {
        int curTime = sortedLogs[i].first;
        int stepTime = curTime - prevTime;

        for (int i = 0; i < accumLogs.size(); ++i)
        {
            int startTime = accumLogs[i].first;
            int progressTime = startTime + advTime - prevTime;

            if (progressTime > 0)
            {
                accumLogs[i].second += viewer * min(stepTime, progressTime);
                
                if (advLog.second < accumLogs[i].second)
                {
                    advLog = accumLogs[i];
                }
            }
        }

        // 시작이면 +1, 완료면 -1
        if (sortedLogs[i].second == 1)
        {
            ++viewer;
            
            accumLogs.push_back({ curTime, 0 });
        }
        else
        {
            --viewer;
        }

        prevTime = curTime;
    }

    return convertTimeStamp(advLog.first);
}
}

```

</details>
