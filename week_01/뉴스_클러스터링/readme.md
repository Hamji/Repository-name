건희
```
copy here
```
정영
```
copy here
```
건률(파이썬)
<details>
<summary>접기/펼치기 버튼</summary>
  
### 06-28 23:50 완료

``` python
def inDict(text):
    dic = {}
    for i in range(len(text)-1) :
        if not ('A' <= text[i] <= 'Z' and 'A' <= text[i+1] <= 'Z') :
            continue
        if text[i:i+2] in dic :
            dic[text[i:i+2]] += 1
        else :
            dic[text[i:i+2]] = 1
    return dic

def unionDict(dict1, dict2):
    dic = {}
    for key,value in dict1.items() :
        dic[key] = value
    for key,value in dict2.items() :
        if key in dic :
            dic[key] = value if value > dic[key] else dic[key]
        else :
            dic[key] = value
    return dic


def interDict(dict1, dict2):
    temp = {}
    dic = {}
    for key,value in dict1.items() :
        temp[key] = value
    for key,value in dict2.items() :
        if key in temp :
            dic[key] = value if value < temp[key] else temp[key]
    return dic


def solution(str1, str2):
    str1 = str1.upper()
    str2 = str2.upper()
    answer = 0
    
    dict1 = inDict(str1)
    dict2 = inDict(str2)
    
    unionSum = 0
    interSum = 0
    
    for key,value in unionDict(dict1,dict2).items():
        unionSum += value
    for key,value in interDict(dict1,dict2).items():
        interSum += value
    
    if unionSum == 0 :
        return 65536
    else :
        return int((interSum/unionSum)*65536)
    
```
</details>
  
문교
```
copy here
```
