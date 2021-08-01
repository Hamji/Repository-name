
<a href = 'https://programmers.co.kr/learn/courses/30/lessons/42890'> 후보키 </a>


건희
<details>
<summary>접기/펼치기 버튼</summary>
	
``` python

from itertools import combinations

def solution(relation):
    row = len(relation)
    col = len(relation[0])
    
    candi = []
    # combination으로 키 경우의 수 만들어주기
    for i in range(1, col+1):
        candi.extend(combinations(range(col), i))
    
    # exclusive 가 유일성이래 알고 계셧나요?
    exclu = []
    for ks in candi:
        temp = [tuple([item[k] for k in ks]) for item in relation]
        #print(temp)
        # 추출한 키값의 길이가 row 와 같다면 candidate 의 유일성 만족한다
        if len(set(temp)) == row:
            exclu.append(ks)
    
    # 최소성 검사
    minim = set(exclu)
    for i in range(len(exclu)):
        for j in range(i + 1, len(exclu)):
            if len(exclu[i]) == len(set(exclu[i]).intersection(set(exclu[j]))):
                # print(exclu[i])
                # print(exclu[j])
                # print("---")
                minim.discard(exclu[j])
    
    #print(minim)
    return len(minim)
	
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

테스트 18 〉	통과 (1.12ms, 10.4MB)

from itertools import combinations 

#특정 index가 키가 될 수 있는지 없는지 확인
def checkUnique(relation,checkIndex):
    checkSet = set()
    for row in relation:
        checkList = []
        for i in checkIndex:
            checkList.append(row[i])
        
        if tuple(checkList) in checkSet:
            return False
        checkSet.add(tuple(checkList))
    return True

#특정 index가 후보키가 될 수 있는지 없는지 확인
#set안에 존재하는 index가 subset 이면 불가능
def checkCandi(candiSet,checkIndex):
    count = 0
    for candi in candiSet:
        count = 0
        for i in checkIndex:
            if i in candi :
                count+=1
        if count == len(candi):
            return False
    return True

def solution(relation):
    answer = 0
    items = list(range(len(relation[0])))
    candiSet = set()
    for i in items :
        #가능한 경우의 수를 조합으로 생성
        for t in list(combinations(items, i+1)) :
            #check 함수의 순서를 반대로하면 시간차이가 발생
            #연산의 효율때문
            if checkCandi(candiSet,t) is True :
                if checkUnique(relation,t) is True :
                    candiSet.add(t)
        
    return len(candiSet)
```
</details>
  
문교
<details>
<summary>접기/펼치기 버튼</summary>

</details>
