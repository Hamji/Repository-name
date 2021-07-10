건희
<details>
<summary>접기/펼치기 버튼</summary>

	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>
// 09:45 ~ 10:00 + 13:30 ~ 14:00 + 08:30 ~ 09:00 + 11:00 ~ 13:00

테스트 18 〉 통과 (3.67ms, 4.19MB)
	
``` cpp
#include <string>
#include <vector>
#include <algorithm>

#define SEC_ITV 1000
#define MIN_ITV (1000 * 60)
#define HOR_ITV (1000 * 60 * 60)
#define TRAFFIC_TERM 1000

using namespace std;

int parse_time(string time) {
    string hh = time.substr(0, 2);
    string mm = time.substr(3, 2);
    string ss = time.substr(6, 2);
    string nn = time.substr(9, 3);
    return stoi(hh)*HOR_ITV + stoi(mm)*MIN_ITV + stoi(ss)*SEC_ITV + stoi(nn);
}
int parse_term(string term) {
    string ss = term.substr(0, 1);
    string nn = term.length() > 2 ? term.substr(2, term.length()-1) : "0";
    
    static const int nitv[5] = { 1, 10, 100 , -1, 1};
    return stoi(ss)*SEC_ITV + stoi(nn) * nitv[6 - term.length()];
}
int solution(vector<string> lines) {
    vector<pair<int, int>> times;
    for(auto it = lines.begin(); it != lines.end(); ++it) {
    // get time
        string time = it->substr(11, 12);
        string term = it->substr(24, it->length());
    // get point
        int itime = parse_time(time);
        int iterm = parse_term(term);
        times.push_back({max(itime - iterm + 1, 0), +1});
        times.push_back({itime, -1});
    }
    sort(times.begin(), times.end());
    // get max
    int max_traffic = 0, bef_traffic = 0;
    for(auto event = times.begin(); event != times.end(); ++event) {
        // add before traffic
        int traffic = bef_traffic;
        // add after traffic
        for(auto it = event;
            it != times.end() && it->first < event->first + TRAFFIC_TERM; ++it) {
            if(it->second == +1) traffic++;
        }
        // calc max
        max_traffic = max(max_traffic, traffic);
        // calc bef
        bef_traffic += event->second; // +1 or -1
    }
    
    return max_traffic;
}
```

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
