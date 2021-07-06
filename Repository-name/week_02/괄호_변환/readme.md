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
<br>
최장시간 
  
  테스트 19 〉	통과 (0.45ms, 10.3MB)
  
``` python

def findUV(ss):
    count = 0
    for i in range(len(ss)) :
        if ss[i] == '(' :
            count += 1
        elif ss[i] == ')' :
            count -= 1
        
        if count == 0 :
            return ss[:i+1],ss[i+1:]
    return '',''
        
def isRight(ss):
    count = 0
    for char in ss:
        if char == '(' :
            count += 1
        elif char == ')' :
            count -= 1
        if count < 0 :
            return False
    return True


def find(p):
    result = ''
    if p == '' :return ''
    UV = findUV(p)
    if not isRight(UV[0]):
        mm = {'(' : ')' , ')' : '('}
        result = '('
        result += find(UV[1])
        result += ')'
        # 괄호 뒤집기가 문자열의 순서를 뒤집는게 아니라 
        # ( -> )로 , ) -> ( 로 바꾸라는 뜻이었음...
        result += ''.join([mm[char] for char in UV[0][1:-1]])
    else :
        result = UV[0]
        result += find(UV[1])
    return result

def solution(p):
    answer = find(p)
    return answer
                    
```

</details>    

문교

<details>
<summary>접기/펼치기 버튼</summary>


</details>    
