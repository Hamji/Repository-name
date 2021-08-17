<a href = 'https://programmers.co.kr/learn/courses/30/lessons/17680'> 캐시 </a>


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
