<a href = 'https://programmers.co.kr/learn/courses/30/lessons/17680'> 캐시 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>

테스트 11 〉	통과 (66.85ms, 23.8MB)
	
``` python

def solution(cacheSize, cities):
    time = 0
    cache = []
    cities = [i.lower() for i in cities]
    if cacheSize != 0:
        for c in cities:
            if c in cache:
                cache.pop(cache.index(c))
                cache.append(c)
                time += 1
            else:
                if len(cache) < cacheSize:
                    cache.append(c)
                    time += 5
                else:
                    cache.pop(0)
                    cache.append(c)
                    time += 5
    else:
        time += len(cities) * 5
    
    return time
	
```
	
</details>
    
정영
<details>
<summary>접기/펼치기 버튼</summary>

``` cpp
테스트 11 〉	통과 (26.07ms, 12.2MB)
	
#include <string>
#include <vector>
#include <queue>
#include <unordered_set>
#include <algorithm>

using namespace std;

int solution(int cacheSize, vector<string> cities) {
    int answer = 0;
    int bufferSize = 0;
    deque<string> dq;
    unordered_set<string> s;
    if(cacheSize == 0) return cities.size() * 5;
    
    for(int i = 0; i < cities.size(); i++) {
        transform(cities[i].begin(), cities[i].end(), cities[i].begin(), ::toupper);
        // 버퍼가 비어있음. cache miss
        if(bufferSize < cacheSize && s.find(cities[i]) == s.end()) {
            answer+=5;
            bufferSize++;
            s.insert(cities[i]);
            dq.push_back(cities[i]);
        // 버퍼는 차있는데 cache miss
        } else if(s.find(cities[i]) == s.end()) {
            answer+=5;
            s.insert(cities[i]);
            dq.push_back(cities[i]);
            s.erase(s.find(dq.front()));
            dq.pop_front();
        // cache hit
        } else {
            answer+=1;
            for(auto it = dq.begin(); it != dq.end(); it++) {
                if(*it == cities[i]) {
                    dq.erase(it);
                    break;
                }
            }
            dq.push_back(cities[i]);
        }
    }
    return answer;
}
```
	
</details>
    
건률
<details>
<summary>접기/펼치기 버튼</summary>

``` python

테스트 11 〉	통과 (105.10ms, 17.7MB)

def cacheCheck(cache,cacheSize,city):
    if cacheSize == 0 : return False
    city = city.upper()
    if city in cache :
        cache.remove(city)
        cache.append(city)
        return True
    else:
        if len(cache) == cacheSize :
            del cache[0]
        cache.append(city)
        return False
            

def solution(cacheSize, cities):    
    answer = 0
    
    cache = []
    for city in cities :
        result = cacheCheck(cache,cacheSize,city)
        answer += 1 if result == True else 5
    
    return answer

```
  
</details>
